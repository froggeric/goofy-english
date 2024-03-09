

# Clerk {docsify-ignore}

Methods for executing multiple functions at different times with a single trigger. More details can be found in the [useful experience](/best-practices?id=Advanced-Trigger) section.

| Method | Return Type | Brief Description |
| --- | --- | --- |
| [runOnceAfter](/reference-en/clerk?id=runonceafter) | Boolean | Execute a task every day after a specified time. |
| [runOnceAWeek](/reference-en/clerk?id=runonceaweek) | Boolean | Execute a task on a specific day of the week. |

## runOnceAfter

Execute a task every day after a specified time.

### Arguments :id=runonceafter-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `timeStr` | String | Time after which the task is launched once. |
| `callback` | Function | The function to be executed. The name of the function must be unique among all functions called by _Clerk_. |

### Return :id=runonceafter-return {docsify-ignore}

`isRun` (boolean) - `true` if the function was executed, `false` if it was not launched.

### Examples :id=runonceafter-examples {docsify-ignore}

1. Execute three functions at different times with a single trigger

```js
// Trigger every hour
function updatePlaylists() {
    updateEveryHour() // every hour
    Clerk.runOnceAfter('15:00', updateInDay) // once a day
    Clerk.runOnceAfter('21:00', updateInEvening) // once a day


    function updateEveryHour() {
        // ...
    }


    function updateInDay() {
        // ...
    }


    function updateInEvening() {
        // ...
    }
}
```

## runOnceAWeek

Execute a task on a specific day of the week.

### Arguments :id=runonceaweek-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `dayStr` | String | Day of the week in English. |
| `timeStr` | String | Time after which the task is launched once. |
| `callback` | Function | The function to be executed. The name of the function must be unique among all functions called by _Clerk_. |

### Return :id=runonceaweek-return {docsify-ignore}

`isRun` (boolean) - `true` if the function was executed, `false` if it was not launched.

### Examples :id=runonceaweek-examples {docsify-ignore}

1. Execute three functions at different times with a single trigger

```js
// Trigger every 15 minutes
function updatePlaylists() {
    update15() // every 15 minutes
    Clerk.runOnceAWeek('monday', '12:00', updateMonday) // every Monday after 12
    Clerk.runOnceAWeek('saturday', '16:00', updateSaturday) // every Saturday after 16


    function update15() {
        // ...
    }


    function updateMonday() {
        // ...
    }


    function updateSaturday() {
        // ...
    }
}
```
