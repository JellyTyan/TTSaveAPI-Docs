# TTSaveAPI: Документация

**TTSaveAPI** — это мощный инструмент для скачивания и стриминга медиа с TikTok. API поддерживает обработку URL, скачивание контента, стриминг файлов и обработку ошибок.

## Базовая информация
-	**Базовый URL**:
```
http://45.13.225.104:4347/api/v2/
```

-	Версия API: v2
-	Формат ответов: JSON

## Эндпоинты

#### 1. POST /download

Скачивает медиа по указанному URL TikTok и возвращает ссылки на загруженные файлы.

Тело запроса:
| Параметр  | Тип	| Обязательный | Описание |
| ------------- | ------------- | ------------- | ------------- |
| url  | string | да  | URL видео или фото из TikTok. |
| download_content_type  | string | да  | Тип контента: VIDEO_ONLY или ORIGINAL. |


Пример запроса:
```http
POST /download HTTP/1.1
Content-Type: application/json

{
  "url": "https://www.tiktok.com/@author123/video/312132123123132",
  "download_content_type": "VIDEO_ONLY"
}
```
Пример ответа:

```json
{
  "files": [
    {
      "name": "312132123123132.mp4",
      "url": "/stream/312132123123132.mp4"
    }
  ]
}
```

Коды ошибок:
-	400: Некорректный запрос (например, отсутствует url).

Пример ответа:
```json
{ "detail": "URL is required" }
```

- 500: Ошибка сервера (например, не удалось скачать файл).
Пример ответа:

```json
{ "detail": "Failed to save the file" }
```

#### 2. GET /stream/{filename}

Стримит ранее загруженный файл по имени.

Параметры:

| Параметр  | Тип	| Обязательный | Описание |
| ------------- | ------------- | ------------- | ------------- |
| filename  | string | да  | Имя файла для стриминга (например, 312132123123132.mp4). |


Пример запроса:
```http
GET /stream/312132123123132.mp4 HTTP/1.1
```
Пример ответа:

- Файл стримится как бинарный поток.

Коды ошибок:
- 404: Файл не найден.

Пример ответа:
```json
{ "detail": "File not found" }
```
#### 3. POST /ping

Проверка работоспособности сервера.

Пример запроса:
```http
POST /ping HTTP/1.1
```
Пример ответа:
```
pong
```
## Обработка ошибок

TTSaveAPI имеет встроенные обработчики ошибок, чтобы гарантировать ясность ответов при возникновении проблем.

### Типы ошибок:

### 1. Глобальные ошибки

Возникают при неожиданных проблемах.

Пример ответа:
```json
{
  "message": "Internal Server Error",
  "detail": "Error: <подробности ошибки>"
}
```
#### 2. Ошибки валидации

Возникают при некорректных или отсутствующих параметрах в запросе.

Пример ответа:
```json
{
  "message": "Validation Error",
  "detail": [
    {
      "loc": ["body", "url"],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```
## Как начать использовать API
#### 1.	Скачивание медиа:
Отправьте POST-запрос на /download, указав URL TikTok. В ответе получите список файлов с их ссылками.
#### 2.	Стриминг медиа:
Используйте имя файла из ответа /download для стриминга через /stream/{filename}.
#### 3.	Проверка сервера:
Отправьте запрос на /ping, чтобы проверить доступность API.
