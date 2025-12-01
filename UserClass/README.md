# Документация: Привязка ученика к классу/направлению (class_user)

## 1. Общая структура

Привязка ученика к классу/направлению хранится в таблице **class_user**.  
Эта таблица отражена в модели:

```
modules/v1/models/ClassUser.php
```

Основные поля:
- `class_id` — класс / направление
- `user_id` — ученик
- `amount` — стоимость доступа
- `first_price` — первоначальная стоимость
- `discount` — скидка
- `next_payment_date` — следующая дата платежа
- `status_payment` — статус оплаты
- `deleted_at` — поле для мягкого удаления

Поле `class` в API — это вложенный объект, приходящий из связи `getClass()`.

---

## 2. Soft Delete

Колонка `deleted_at` добавлена миграцией:

```
migrations/m241015_221738_add_deleted_at_column_to_class_user_table.php
```

Мягкое удаление реализовано через:
- `TimestampBehavior` на событие **beforeDelete**
- метод `softDelete()` в `ClassUser`  
  Он устанавливает `deleted_at = NOW()`.

---

## 3. Создание привязки

### 3.1. Основной путь (внутренние действия)

Запись создаётся через:

```
modules/v1/models/ClassUserForm.php (line 79)
```

Метод **attachChildren()**:
1. Создаёт новую запись `class_user` для пары `class_id/user_id`.
2. Проставляет:
   - `amount`
   - `discount`
   - `first_price`
   - `next_payment_date`
   - `status_payment`
3. Деактивирует старые активные связи ученика (ставит `deleted_at = NOW()`).
4. Отключает старые `direction_orders`.
5. Создаёт новые `direction_orders` по `ClassGroup` (`is_primary=1`).
6. Если используется one_pay или входящая ссылка:
   - создаёт `PolymorphPayment` с привязкой к ClassUser.

Метод вызывается из:
```
ClassesController::actionAttachChildren()
modules/v1/controllers/ClassesController.php (lines 523, 676, 964)
```

---

### 3.2. Внешние входы

Некоторые интеграции создают ClassUser напрямую:

```
modules/v1/controllers/ExternalController.php:1210,1331,1663
```

Используются в сценариях:
- активация внешних оплат
- вебхуки
- интеграции с внешними сервисами

Могут устанавливать:
- `next_payment_date`
- `discount`
- `amount`

---

## 4. Удаление / Отвязка ученика

Отвязка выполняется методом:

```
ClassUserForm::detachChildren()  
modules/v1/models/ClassUserForm.php (line 351)
```

Алгоритм:
1. Находит активную связь (где `deleted_at IS NULL`).
2. Удаляет связанные `direction_orders` через:

```
DirectionOrderService::deleteOrder()
```

3. Чистит связанные записи расписания через:

```
TimetableBetaService
```

4. Вызывает:

```
$classUser->softDelete()
```

Класс `ClassUser` содержит soft delete поведение и метод `softDelete()`:

```
modules/v1/models/ClassUser.php (line 32, line 80)
```

---

## 5. Хранение и миграции

Таблица `class_user` создана миграциями:

```
m240319_141500_create_class_user_table.php
m240328_101400_add_columns_into_class_user_table.php
m241015_221738_add_deleted_at_column_to_class_user_table.php
```

---

## 6. Использование в системе

### 6.1. Фильтрация активных связей

Активная привязка = `deleted_at IS NULL`.

Применяется в:
- `ClassInfoPaymentResource`
- списках в `ClassesController`
- `UserController`
- `DirectionController`

---

### 6.2. Продление оплаты

Используется метод:

```
PolymorphPaymentService::extendClassUser()
modules/v1/services/PolymorphPaymentService.php
```

Функционал:
- продление `next_payment_date`
- синхронизация `direction_orders`

---

### 6.3. Использование в CRM и отчётах

Контроллеры:
- `ReportsController`
- `CrmController`
- `CrmLeadPaymentController`

Читают:
- сумму платежа
- срок доступа
- статус оплаты

---

## 7. Схема жизненного цикла ClassUser

1. **attachChildren()** → создаёт новую привязку  
2. Ставятся платежные поля  
3. Создаются direction_orders  
4. Старая привязка помечается удалённой (`deleted_at`)  
5. При оплатах используется PolymorphPayment  
6. При продлении вызывается extendClassUser  
7. При отвязке — softDelete + очистка расписания + удаление direction_orders

---

## 8. Ключевые файлы

```
modules/v1/models/ClassUser.php
modules/v1/models/ClassUserForm.php
modules/v1/controllers/ClassesController.php
modules/v1/controllers/ExternalController.php
modules/v1/services/PolymorphPaymentService.php
modules/v1/services/DirectionOrderService.php
modules/v1/services/TimetableBetaService.php
migrations/m240319_141500_create_class_user_table.php
migrations/m240328_101400_add_columns_into_class_user_table.php
migrations/m241015_221738_add_deleted_at_column_to_class_user_table.php
```

---

## 9. Краткое резюме

`class_user` — это база всей логики доступа ученика к классу:

- создание через **ClassUserForm**
- soft delete через `deleted_at`
- продление через **PolymorphPaymentService**
- интеграции напрямую создают записи
- старые связи отключаются перед созданием новых
- direction_orders полностью синхронизируются с жизненным циклом class_user

Эта модель является центральной точкой для:
- доступа
- оплаты
- аналитики
- CRM
- расписания
