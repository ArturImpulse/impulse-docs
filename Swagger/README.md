# Swagger API (External / v1)

Документация по Swagger-спецификациям и их использованию во фронтенде и API.

---

## 1. Расположение спецификаций

В проекте используются следующие Swagger/OpenAPI файлы:

- `modules/v1/docs/v1.yaml` — основная спецификация External API v1  
- `docs/swagger-email-confirmation.yaml` — спецификация для email-подтверждений

Во фронтенде используется собранная спецификация:

- `public/swagger.yaml`

---

## 2. Swagger UI во фронтенде

### Доступ

Swagger UI доступен **только** для ролей:

- `admin`
- `junior-admin`

### URL

```
<frontend_host>/swagger
```

Примеры:
- Dev: http://localhost:3000/swagger
- Prod: https://<frontend-domain>/swagger

---

## 3. Как работает Swagger UI

### Роутинг

- Роут описан в:  
  `src/routes/list.js` (строка ~1481)
- Путь: `/swagger`

### Компонент

- Страница Swagger UI:  
  `src/components/pages/Swagger/index.jsx`
- Используется стандартный **Swagger UI**

### Источник спецификации

- Загружается файл:  
  `public/swagger.yaml`

### Сервер API

- Сервер подставляется динамически из переменной окружения:

```env
REACT_APP_URL_API=http://localhost/v1
```

- Если переменная не задана, используется значение по умолчанию:
  ```
  http://localhost/v1
  ```

> ⚠️ В YAML-файле сервер указан как `/v1`, фактический хост всегда берётся из `REACT_APP_URL_API`.

---

## 4. Ссылка на Swagger в админке

На главной странице админки отображается баннер со ссылкой на Swagger.

Источник:
- `src/components/pages/Main/Admin.jsx`

Формирование ссылки:
```js
linkButton: `${document.location.href}swagger`
```

---

## 5. Структура API (External)

Все эндпоинты начинаются с префикса:

```
/external
```

Полный базовый путь:
```
{REACT_APP_URL_API}/external
```

---

## 6. Классы

### GET `/external/class-folders`
Список папок с классами  
Ответы: `200`, `500`

### POST `/external/class`
Создание класса  

Обязательные поля:
- `title`
- `one_pay`
- `amount`
- `folder_id`

Ответы: `200`, `400`, `500`

### GET `/external/class/{id}`
Ответы: `200`, `404`, `500`

### PUT `/external/class/{id}`
Ответы: `200`, `400`, `404`, `500`

### DELETE `/external/class/{id}`
Ответы: `200`, `404`, `500`

---

## 7. Пользователи

### GET `/external/users`
Query: `page`, `per-page`  
Ответы: `200`, `405`, `500`

### GET `/external/user-by-id/{id}`
Ответы: `200`, `404`, `500`

### GET `/external/user-by-custom-id`
Обязательное поле: `custom_id`  
Ответы: `200`, `500`

### GET `/external/find-user`
Ответы: `200`, `400`, `500`

### POST `/external/create-child`
Создание ученика и родителя  
Ответы: `200`, `400`, `404`, `422`, `500`

### POST `/external/remove-child-from-class`
Ответы: `200`, `400`, `404`

### POST `/external/remove-child-from-course`
Ответы: `200`, `400`, `404`

### POST `/external/delete-user`
Ответы: `200`, `400`, `404`

### POST `/external/block-user`
Ответы: `200`, `400`, `404`

---

## 8. Курсы

### POST `/external/course`
Ответы: `200`, `400`

### GET `/external/course/{id}`
Ответы: `200`, `404`

### PUT `/external/course/{id}`
Ответы: `200`, `400`, `404`

### DELETE `/external/course/{id}`
Ответы: `200`, `404`, `500`

---

## 9. Роли и зачисление

### POST `/external/assign-role`
Ответы: `200`, `400`, `404`, `422`

### POST `/external/add-child-to-class`
Ответы: `200`, `400`, `404`, `409`, `500`

### POST `/external/add-child-to-classes-by-tag`
Ответы: `200`, `400`, `404`, `500`

---

## 10. CRM / Лиды

### GET `/external/leads`
Ответ: `200`

### POST `/external/lead`
Ответы: `200`, `201`, `400`, `422`

### GET `/external/lead-fields`
Ответ: `200`

### GET `/external/managers`
Ответ: `200`

### GET `/external/lead-statuses`
Ответ: `200`

### GET `/external/lead-states`
Ответ: `200`

---

## 11. Примечания

- Swagger предназначен для внутреннего использования
- Все изменения API должны отражаться в `v1.yaml`
- После обновления спецификаций необходимо пересобрать `public/swagger.yaml`
