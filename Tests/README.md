# 🧪 Тестирование проекта (Yii2 + Codeception)

Мы используем **Codeception** для модульных, функциональных, API и acceptance-тестов.

---

## 📦 Подготовка окружения

Все тесты выполняются внутри Docker-контейнера **impulse-api-php-1**.

Перед запуском убедитесь, что контейнеры работают:
```bash
docker ps
```

Если контейнеры не запущены:
```bash
docker compose up -d
```

---

## 🚀 Запуск тестов

### Войти в контейнер PHP
```bash
docker exec -it impulse-api-php-1 bash
```

### Запустить все тесты
```bash
vendor/bin/codecept run
```

### Запустить тесты по типам

| Тип тестов       | Команда                                      | Назначение                                   |
|------------------|----------------------------------------------|----------------------------------------------|
| Unit             | `vendor/bin/codecept run unit`               | Проверка логики моделей и сервисов.          |
| Functional       | `vendor/bin/codecept run functional`         | Проверка действий контроллеров.              |
| API              | `vendor/bin/codecept run api`                | Тестирование REST API.                       |
| Acceptance       | `vendor/bin/codecept run acceptance`         | Поведенческие UI-тесты (Selenium/Chrome).    |

---

## ⚙️ Полезные параметры

| Опция | Пример | Описание |
|-------|---------|----------|
| `--steps` | `vendor/bin/codecept run unit --steps` | Показать все шаги тестов. |
| `-v` | `vendor/bin/codecept run -v` | Расширенный вывод. |
| `--debug` | `vendor/bin/codecept run --debug` | Отладочная информация. |
| `--coverage` | `vendor/bin/codecept run --coverage` | Покрытие кода (при наличии xdebug). |
| `--html` | `vendor/bin/codecept run --html` | Сформировать HTML-отчёт. |

---

## 🧩 Примеры

### Запуск конкретного теста
```bash
vendor/bin/codecept run unit tests/unit/models/PolymorphPaymentServiceTest.php
```

### Запуск конкретного метода теста
```bash
vendor/bin/codecept run unit tests/unit/models/PolymorphPaymentServiceTest.php:testCreatePolymorphPayment
```

---

## 📊 Пример отчёта

```
Acceptance Tests (5)
E AboutCest: Ensure that about works
E ContactCest: Ensure that contact page works
E ContactCest: Contact form can be submitted
E HomeCest: Ensure that home page works
E LoginCest: Ensure that login works

Api Tests (4)
E CreateMobileAppUserCest: Test can fetch course link
✔ UserCest: Try to test
E UserCest: Auth user
✔ UserCest: Success to get all user

Functional Tests (10)
✔ ContactFormCest: Open contact page
✔ ContactFormCest: Submit empty form
✔ ContactFormCest: Submit form with incorrect email
E ContactFormCest: Submit form successfully
✔ LoginFormCest: Open login page
E LoginFormCest: Internal login by id
E LoginFormCest: Internal login by instance
✖ LoginFormCest: Login with empty credentials
✖ LoginFormCest: Login with wrong credentials
✖ LoginFormCest: Login successfully

Unit Tests (42)
✔ AddHomeWorkTest: Success full
E CreateChildrenTest: Validate empty
E CreateChildrenTest: Validate correct
E PolymorphPaymentServiceTest: Create polymorph payment
✔ AlertTest: Multiple success messages
...
```

✅ — тест пройден  
✖ / E — ошибка или исключение  

---

## 🧰 Структура тестов

```
tests/
├── unit/          # Модульные тесты (модели, сервисы)
├── functional/    # Тесты контроллеров
├── api/           # Тесты API эндпоинтов
├── acceptance/    # UI-тесты (Selenium)
└── _bootstrap.php # Инициализация окружения Yii2
```

---

## 💡 Советы

- **Unit-тесты** можно запускать быстрее всего: они не требуют базы или веб-сервера.  
- **Functional и API** используют тестовую БД (`@tests/_data` или отдельный `db-test`).  
- **Acceptance-тесты** требуют запущенного веб-сервера (обычно `nginx` в контейнере).  
- Для ускорения: `vendor/bin/codecept run unit --skip-group slow`.

---

## 🧹 Очистка окружения

После выполнения тестов можно удалить временные данные:
```bash
docker exec -it impulse-api-php-1 php yii fixture/flush-all
```
или перезапустить контейнер:
```bash
docker compose restart impulse-api-php-1
```

---

## 🧾 Отчёты

Создать HTML-отчёт о тестах:
```bash
vendor/bin/codecept run --html
```
Отчёт будет сохранён в:
```
tests/_output/report.html
```
