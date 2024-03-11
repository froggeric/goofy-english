# Best Pratices

Tips for working with goofy and Apps Script

## Branching Algorithm

The algorithm does not have to be linear. If for some reason it is not necessary to update the playlist, the function can be terminated.

Let's say there is a daily playlist that doesn't need to be updated on Wednesdays. To implement this, we will write a condition check, thereby creating branching of the code. 

?> Read about how the `if` condition works separately [here](https://itchief.ru/javascript/%D1%81onditional-and-logical-operators)

The keyword `return` is used to return a value from a function. The code after this command will never be executed. Using `return` without a return value is a special case, in which the receiving party receives the value `undefined`. In the case of running with a trigger, this simply means that the function has completed.

The example demonstrates that if today is Wednesday - the function is terminated. The code after `return` will not be executed. But if today is another day of the week, execution will not enter the `if` branch, i.e., it will not execute the `return` command, and will continue to the playlist update logic.
```js
if (Selector.isDayOfWeekRu('среда')){
    console.log('Сегодня среда. Плейлист не будет обновлен.');
    return;
}

// Playlist update logic
let tracks = Source.getPlaylistTracks('', 'id');
// ...
```

Another situation. We have a playlist with 40 tracks. Periodically, we listen to it. Let's add several branches depending on the number of listened tracks. For example, if less than 10 inclusive are listened to, update all tracks. From 10 to 40 not inclusive, remove listened tracks from the playlist. Leave the playlist untouched at 40.

```js
const ID_TARGET = 'id результата';

let tracks = Source.getPlaylistTracks('', ID_TARGET);
Filter.removeTracks(tracks, RecentTracks.get());

if (tracks.length == 40) {
    return;
} else if (tracks.length > 10 && tracks.length < 40) {
    Playlist.saveWitUpdate({
        id: ID_TARGET,
        tracks: tracks,
    });
} else {
    Playlist.saveWithReplace({
        id: ID_TARGET,
        tracks: Source.getPlaylistTracks('', 'id источника'),
    });
}
```

!> Be careful with the word `length`. It is very easy to make a typo in it.

## Debugging Execution

Debugging allows you to stop code execution at any point and analyze values up to that moment.

For example, you need to get all track names. But what key does this field have: `title`, `name`, `label`, `trackname`? It's long and inconvenient to iterate through them. By running the debugger, you will see all available keys for tracks and other elements.

?> Spotify documentation can be found [here](https://developer.spotify.com/documentation/web-api/reference-en/)

Set a breakpoint by clicking next to the line number, select the function and press `debug`.

![Debugging](/img/fp-debug.gif)

As a result, the debugger will open on the right side where you can see intermediate values of variables, as well as continue execution step by step. At the top there are 4 buttons: start (to the next point or to the end), step with bypassing, step inside and step outside. Try it yourself to understand in practice.

![Debugger](/img/debuger.png)

When running a function through a trigger, you may need different information. For example, how many tracks were before and after filters. To output such messages, use the `console.log` function.
```js
let tracks = Source.getSavedTracks();
console.log('Количество любимых треков', tracks.length);
```

![Example logs](/img/example-log.png)

Solving the problem of getting track names, output them as follows:
```js
let tracks = Source.getSavedTracks();
tracks.forEach(track => console.log(track.name));
```

In addition, [hotkeys](https://github.com/Chimildic/goofy/discussions/112) for quickly navigating to the function description directly from the code editor will be useful.

## Multiple Accounts

One Google account (Apps Script) can manage multiple Spotify accounts (starting with version goofy 1.6.1).

To set up goofy for an additional Spotify account, repeat the steps [installation](https://chimildic.github.io/goofy/#/install), but at the same time:

1. Copy the goofy project as you did on [step 4 during installation](https://chimildic.github.io/goofy/#/install).
2. Use the same values `CLIENT_ID` and `CLIENT_SECRET` in the _config_ file (i.e., skip steps 1-2).
3. When granting access on step 10, log in through a new Spotify account.

## Command Palette

The command palette is a list of actions available to the code editor. Here are some useful ones:

Place the cursor on any line of code and press the <kbd>F1</kbd> button or call up the context menu with the right mouse button and select the palette. At the top there is a field for quick search of commands. To the right of the name, hotkeys are indicated if they are available. For example, enter the word `font`.

![Command Palette - Font](/img/cmdp-font.gif)

* Quick copy. Place the cursor anywhere in the line. Press and hold the <kbd>Shift</kbd><kbd>Alt</kbd> keys and click <kbd>↓</kbd>. The same effect will occur with a selected fragment of code.
  
  ![Quick Copy](/img/cmdp-fast-copy.gif)

* Vertical selection. Place the cursor in the required place. Press and hold the <kbd>Shift</kbd><kbd>Alt</kbd> keys and drag the mouse to the opposite corner.
  
  ![Vertical Selection](/img/cmdp-vertical-select.gif)

* Vertical selection without a mouse. Get to the desired position with the arrows. Press and hold <kbd>Ctrl</kbd><kbd>Alt</kbd> and click the up or down arrow. Now press and hold <kbd>Ctrl</kbd><kbd>Shift</kbd> and click the right or left arrow.
  
  ![Vertical Selection without Mouse](/img/cmdp-vertical-select-no-mouse.gif)

* Renaming. Select a word, for example a variable, and press <kbd>F2</kbd>. All mentions will be replaced with a new name.
  
  ![Renaming Variable](/img/cmdp-rename-f2.gif)

* Moving the line. Place the cursor anywhere in the line. Press and hold <kbd>Alt</kbd> and click the up or down arrow. Similarly for selection.
  
  ![Moving Line](/img/cmdp-move-code.gif)

* Action with comment. Place the cursor anywhere in the line, press <kbd>Ctrl</kbd><kbd>/</kbd>, the line will be commented out. Pressing again will remove the comment. Similarly for a selected fragment.
  
  For multi-line comments, use the combination <kbd>Shift</kbd><kbd>Alt</kbd><kbd>A</kbd>
  
  ![Multi-line Comment](/img/cmdp-fast-comment.gif)

* Combination for quick formatting <kbd>Shift</kbd><kbd>Alt</kbd><kbd>F</kbd>
- Also useful will be folding and unfolding all the code. There are no combinations, look for them in the palette.

## Search Value

There are several ways to find an element's value. For example, artist genres or minimum energy threshold of a track.

### Spotify Console

The simplest way is the Spotify console. An add-on that calls API methods with given parameters. 

1. Go to [console](https://developer.spotify.com/console/) and find the necessary API method. For example, [artist by _id_](https://developer.spotify.com/console/get-artist/).
2. You need a token to make a request. Click the `get token` button. In the opened list, pay attention to the note `not require a specific scope`. If it is there, simply click the `request token` button. Otherwise, below the note will be a list of checkboxes that you need to click on. The bottom large list should not be touched.
3. Add artist _id_ in the field and click the `try it` button.

In the right part there will be a response that gives all available attributes of the artist. For example, genres. It is these that should be used in [rangeTracks](/reference-en/filter?id=rangetracks) for `genres` or `ban_genres`.
```js
"genres": [
    "candy pop",
    "emo",
    "pixie",
    "pop emo",
    "pop punk"
]
```

### Step-by-step debugging

A less convenient way. It depends largely on what data needs to be found. 

[Running the debugger](/best-practices-en?id=Выполнение-отладки), set a breakpoint after the line of interest. For example, after getting the list of artists. The debugger will show an array of elements. It is not convenient to search for the value of a specific artist in it.

![Find Artist Genres](/img/find-genres.png)

### Output to log

Elements are iterated in a loop, and the necessary information is output to the log. This method will not work for searching for data hidden inside the library. For example, [track features](/reference-en/desc?id=Особенности-трека-features) are not directly given out, and functions for obtaining them are not documented. Of course, modification of the library code is allowed, but this method is not reflected here.

```js
// Artist and their genres
artists.forEach(a => console.log(a.name, a.genres));

// List view: artist - track
console.log(tracks.map(t => `${t.artists[0].name} - ${t.name}`).join('\n'));
```

## Advanced Trigger

In the section [trigger economy](/best-practices-en?id=Экономика-триггеров), a way to reduce triggers by combining functions with **similar schedules** is described. For example, updating daily playlists. That is, instead of the scheme "1 trigger = 1 function" switch to "1 trigger = 2+ functions". In this section, the method "1 trigger = all functions", regardless of the similarity of the schedule, is described.

As an illustration, the `runTasks_` function. It is activated by one trigger every 15 minutes, but performs three functions (tasks) at different times thanks to Clerk: updating playback history every 15 minutes, adding new likes to cache every day and rewriting the cache of likes in case of track deletion every week.

Clerk is a software module `Clerk` with two functions:
- `runOnceAfter` - perform the task once a day after the specified time of day.
- `runOnceAWeek` - perform the task on a certain day of the week after the specified time of day.

Description of functions in [documentation](/reference-en/clerk)

### Example Analysis {docsify-ignore}

According to the notes in the comments:

1. A trigger is set every 15 minutes for the `runTasks_` function.
2. The `RecentTracks.update()` function runs every 15 minutes without additional conditions and checks.
3. Checking the time condition. If the `updateSavedTracks` function was launched, then `isUpdatedSavedTracks` contains `true`, otherwise it was not launched and `false`.
4. If the `updateSavedTracks` function was not launched - launch `appendSavedTracks` under the specified time condition. Otherwise, do not launch.

```js
// Trigger: every 15 minutes (1)
function runTasks_() {
    RecentTracks.update() // runs every 15 minutes (2)
    let isUpdatedSavedTracks = Clerk.runOnceAWeek('monday', '01:00', updateSavedTracks) // (3)
    !isUpdatedSavedTracks && Clerk.runOnceAfter('01:00', appendSavedTracks) // (4)


    function updateSavedTracks(tracks) {
      // runs every Monday after 1 am
    }


    function appendSavedTracks() {
      // runs every day after 1 am
    }
}
```

## Hiding Functions

There are two ways to hide functions. Such a function cannot be run directly in the code editor and is not available for trigger.

The first way. Useful for reducing the list of selectable functions when creating triggers. In addition, an example of use in [trigger economy](/best-practices-en?id=Экономика-триггеров).

Add an underscore at the end of the function name. In the example, the `create` function is available for execution, and the `update_` function is not. 

```js
function create(){}

function update_(){}
```

* The `runTasks_` function is hidden this way (the trigger for it is created programmatically).
- It is not recommended to hide the `doGet` function. It is needed for authorization and [management from a phone](/addon-en?id=Управление-с-телефона).
- The `setProperties` function can be hidden this way. But when updating parameters, you need to return the usual name to get the possibility of running in the editor.

The second way. Useful for highlighting repeating blocks. For example, when different sources require the same set of filters.  

JavaScript allows defining a function inside another one, thereby reducing its scope. In the example, the `get` function is available for calling inside `append`, but not visible inside `update`. 

```js
function update(){}

function append(){

    function get(){}
}
```

## Request Economy

Apps Script gives a daily quota of sending requests - 20 thousand. When the limit is reached, it becomes impossible to receive tracks and change playlists. The exact time of updating the quota is unknown.

Functions that make many requests in one call have a corresponding note in [their description](/reference-en/index). Simpler functions can also take too much. The main reason is the number of tracks. For example, for 1 thousand favorite tracks, 10 requests are needed. For 10 thousand already 100 requests. Projecting onto the quota, this is an insignificant amount for one day. Therefore, it makes no sense to implement a mechanism for accumulation through `Cache`. But it's easy to make logical mistakes.

Let's say you need to delete favorite tracks from the source and accidentally select a dozen favorite tracks for the playlist.
```js
// Correct variant
let topTracks = Source.getTopTracks('long');
let savedTracks = Source.getSavedTracks();

Filter.removeTracks(topTracks, savedTracks);
Selector.keepRandom(savedTracks, 10);
```

It is possible to make several mistakes. Examples are not made up, they have been encountered in algorithms from goofy users.
```js
// Do not create a variable for savedTracks
// Error: the same tracks are requested twice
Filter.removeTracks(topTracks, Source.getSavedTracks());
let tracks = Selector.sliceRandom(Source.getSavedTracks(), 10);

// Call Selector earlier than Filter
// Error: not all will be removed from topTracks (if this is not needed specifically)
Selector.keepRandom(savedTracks, 10);
Filter.removeTracks(topTracks, savedTracks);

// Try to fix the previous error
// Error: again unnecessary requests
Selector.keepRandom(savedTracks, 10);
Filter.removeTracks(topTracks, Source.getSavedTracks());
```

1. When an algorithm requires participation of one and the same set of elements, try to change the order. As it is done in the _correct variant_ above. That is, use the full set everywhere where necessary and only then modify it.
2. If this cannot be done, create a copy instead of new requests. 
```js
let savedTracks = Source.getSavedTracks();
let copySavedTracks = Selector.sliceCopy(savedTracks);
```

The `getCountRequest` function returns the value corresponding to the number of requests that were made from the start of the launch to the call of the function. The value is not cached. Each time you run, the count starts from zero. Add the following line of code at the end of your function. Thus, you will be able to compare the quality of the optimization carried out. 
```js
console.log('Number of requests', CustomUrlFetchApp.getCountRequest());
```

At the same time, consider the influence of functions that can select a _random_ number of elements. For example, each artist has their own number of albums. Therefore, in the future this will affect the number of requests made with each new launch.

## Trigger Economy

According to [limitations](/details-en?id=Ограничения), one project (copy of the library) gets 20 triggers. At the same time, one is always occupied by updating playback history.

Suppose there are 3 daily playlists. Each function is called by a separate trigger. 

```js
function createSavedAndForgot(){}

function createDailyMix(){}

function createRecom(){}
```

If the launch time does not matter, combine functions. Thus saving 2 triggers.
- [hide functions](/best-practices-en?id=Скрытие-функции)
- delete previous triggers
- create a new trigger for the combining function `createEveryDayPlaylists`

```js
function createEveryDayPlaylists(){
    createSavedAndForgot_();
    createDailyMix_();
    createRecom_();
}

function createSavedAndForgot_(){}
function createDailyMix_(){}
function createRecom_(){}
```

?> At the same time, it is important to keep in mind another limitation - execution time (6 minutes). For correct operation, the total execution time of the combined functions should not exceed this limit.

## Google Drive

### Path to File

From version 1.6.1, creating files in folders is supported. All data continues to be placed in the root folder `Goofy Data`. Scripts from previous versions do not require changes.

Created folders are divided into two types:
- _Account Folders_. They are created automatically in the root with the name of the Spotify account _id_.
- _User Folders_. These are created by you when using the [Cache](/reference-en/cache) module.

?> The described examples of working with folders apply to all `Cache` functions

Example 1 - To create a file, specify only its name. It will be located in the account folder.
```js
Cache.write('MySavedTracks.json', []);
```

Example 2 - To create a _user_ folder, specify the folder name, slash `/`, and file name in the string. Below is an example of creating a `example.json` file in the `test` folder, which will be located in the account folder.
```js
Cache.write('test/example.json', []);
```

Example 3 - A new _user_ folder can be created in the root `Goofy Data`. To do this, specify the reserved word `root` and add a slash `/`. Below is an example of creating a file `example.json`, which will be located in the `shared` folder in the root `Goofy Data`.
```js
Cache.write('root/shared/example.json', []);

// Analogously
Cache.write('../shared/example.json', []);
```

Example 4 - If you don't want to get confused with locations, explicitly specify the reserved words `root` and `user`, in order to distinguish the beginning of the path.
```js
// Folder shared in root Goofy Data
Cache.write('root/shared/example.json', []);
Cache.write('../shared/example.json', []);

// Folder myfolder in account folder
Cache.write('user/myfolder/example.json', []);
Cache.write('./myfolder/example.json', []);
```

Example 5 - The depth of the folders is not limited. 
```js
Cache.write('root/shared/radio/rock/lastfm.json', []);
Cache.write('user/private/radio/pixie.json', []);
```

Example 6 - If you have multiple Spotify accounts using goofy projects on one Google account, you can create _common files_. For example, one project collects tracks for radio and saves them in a common folder, and the second account simply reads this file without spending its time on such a search.

?> You cannot place a file in the root `Goofy Data` folder. They will always be moved to the account folder. This is done so that most people do not have to manually change the Disk structure. Be sure to specify at least one folder, which will be located at the same level as the account folders. 

```js
// First project writes to file
Cache.write('root/shared/myradio.json', []);

// Second project reads it
let radioTracks = Cache.read('root/shared/myradio.json');
```

### Version Management

Files from [Cache](/reference-en/cache) are stored on [Google Drive](https://drive.google.com/). Including [playback history](/details-en?id=История-прослушиваний). Each file has up to 100 versions. Each record in the same file creates a new version. 

If you need to roll back to a previous version of the file:
1. Go to the `Goofy Data` folder on [Google Drive](https://drive.google.com/)
2. Right-click on the file and select the `manage versions` item
3. Find the version by date modified and download it from the menu with three dots
4. In the same window, click the `upload new version` button and select the previously downloaded file

?> When rolling back playback history, do not leave an empty file. It should contain at least an empty array `[]`.
