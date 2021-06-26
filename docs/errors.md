# Ошибки

Если на странице нет вашей ошибки или ее не удалось решить, создайте новую тему [на форуме](https://github.com/Chimildic/goofy/discussions). При этом опишите ваши действия и добавьте код.

## Авторизация

| Описание | Решение |
|-|-|
| Access not granted or expired | Нет доступа к Spotify. Повторите шаги из установки: вызовите `setProperties` и пройдите авторизацию (`начать развертывание` - `пробное развартывание`). </br> Если ошибка возникает при попытке вызвать `setProperties`, в одном из ваших файлов есть код вне тела функции, найдите и удалите его. |
| Client ID is required | В сохраненных параметрах отсутствует Client ID от Spotify. Вызовите функцию `setProperties` из файла `config`. При этом у параметров `CLIENT_ID` и `CLIENT_SECRET` должны быть значения из кабинента разработчика Spotify (не текст `вашеЗначение`) |

## Запросы

| Номер | Описание |
|-|-|
| 400 | Сервер получил некорректный запрос. Напишите на форум, если ошибка стабильно воспроизводится. |
| 403 | Запрос составлен корректно, но отсутствует доступ. [Обновите права доступа](/install?id=Обновить-права-доступа). |
| 404 | Запрашиваемые данные не найдены. Например, указано не существующее `id` плейлиста. |
| 413 | Известен случай возникновения при загрузке обложки плейлиста. При кодировании размер данных превысил 256 кб. Поэтому Spotify отклонил запрос. Используйте другую обложку. При `randomCover` со значением `update`, обложка обновится в следующий раз. |
| 429 | За единицу времени совершено много запросов. Сервер перестал отвечать контентом. Как правило, автоматически выжидается пауза в несколько секунд и процесс возобновляется. |
| 500 </br> 503 | Внутреняя ошибка на стороне сервера Spotify или Lastfm. В обычном случае, пропадет через несколько минут. Во время критических сбоев может продолжаться неопределенное время. Наиболее часто встретится при запросе рекомендаций Spotify или обращению к Lastfm множество раз за короткое время. |