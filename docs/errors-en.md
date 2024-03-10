# Errors

If your error is not on this page, please write to the [forum](https://github.com/Chimildic/goofy/discussions). Describe your actions and add code.

## Apps Script

| Error | Solution |
| --- | --- |
| Exceeded maximum execution time | The function's execution time has exceeded 6 minutes (/details-en?id=Limits). When each run takes several minutes, review the algorithm, optimize according to documentation notes and [useful experience](/best-practices-en). In rare cases, it is observed in functions that usually process in a matter of seconds. Currently explained by platform lag, as it is impossible to reproduce the error for debugging. |
| Limit exceeded | One of the platform's limits has been exceeded. The classic version is described [here](/details-en?id=Limits). If you are experimenting with other features of the platform, there are also [others](https://developers.google.com/apps-script/guides/services/quotas). |
| Service invoked too many times <br> Service using too much computer time for one day | A special case of previously described errors. The exact difference is unknown. In the best case, it will pass after a short pause. In the worst case, upon updating the quota, i.e., in a day. |
| Access not granted or expired | No access to Spotify. Repeat the installation steps: call `setProperties` and go through authorization (`start deployment` - `test deployment`). <br> If the error occurs when trying to call `setProperties`, there is code outside of the function body in one of your files, find and delete it. |
| Client ID is required | The saved parameters do not contain a Spotify Client ID. Call the function `setProperties` from the file `config`. At the same time, the parameters `CLIENT_ID` and `CLIENT_SECRET` must have values from the Spotify developer cabinet (not the text `yourValue`) |
| Address unavailable | The cause of the error is unknown. An automatic pause is waited out and a repeat request is sent. As a rule, errors do not occur on the second try. |
| Ошибка службы: Диск | The cause of the error is unknown. Starting from version 1.5.4, an attempt is made to catch the error and retry the operation. |
| ReferenceError: Cheerio is not defined | Add the [Cheerio](https://github.com/Chimildic/goofy/discussions/91#discussioncomment-1931923) library. |

## Requests

| Status | Description |
| --- | --- |
| 400 | The server received an incorrect request. Write to the forum if the error is reproducible. |
| 401 <br> 403 | The request was made correctly, but access is missing. [Update access rights](/tuning-en?id=Обновить-права-доступа). Note that functions [Player](/reference/player-en) are only available with an active subscription (Spotify limitation). |
| 404 | Requested data was not found. For example, a non-existent `id` of the playlist was specified. |
| 413 | Known to occur when uploading a playlist cover. During encoding, the size of the data exceeded 256 kb. Therefore, Spotify rejected the request. Use another cover. With `randomCover` with the value `update`, the cover will be updated next time. |
| 429 | Too many requests were made in one unit of time. The server stopped responding with content. As a rule, an automatic pause of several seconds is waited out and the process resumes. |
| 500 <br> 503 | An internal error on the Spotify or Last.fm server side. In the normal case, it will disappear in a few minutes. During critical failures, it can last for an indefinite period of time. Most often encountered when requesting Spotify recommendations or accessing Last.fm many times in a short amount of time. |
| 504 | The response time from the server has been exceeded. An attempt will be made to repeat the request. |
