# Selector

Methods for selection and branching.

### isDayOfWeek

Returns a boolean value: `true` if today is the day of the week `strDay`, and `false` if not.

Arguments
- (string) `strDay` - day of the week.
- (string) `locale` - locale for the day of the week. Default is `en-US`, which accepts values: `sunday`, `monday`, `tuesday`, `wednesday`, `thursday`, `friday`, `saturday`.

Example usage
```js
if (Selector.isDayOfWeek('friday')){
    // today is friday
} else {
    // another day of the week
}
```

### isDayOfWeekRu

Returns a boolean value: `true` if today is the day of the week `strDay`, and `false` if not. The day of the week value is in Cyrillic.

Arguments
- (string) `strDay` - day of the week. Acceptable values: `понедельник`, `вторник`, `среда`, `четверг`, `пятница`, `суббота`, `воскресенье`.

Example usage
```js
if (Selector.isDayOfWeekRu('понедельник')){
    // today is monday
} else if (Selector.isDayOfWeekRu('среда')) {
    // today is wednesday
} else {
    // another day of the week
}
```

### isWeekend

Returns a boolean value: `true` if today is Saturday or Friday, and `false` if not.

Arguments: none.

Example usage
```js
if (Selector.isWeekend()){
    // today is the weekend
} else {
   // weekdays
}
```

### keepAllExceptFirst / sliceAllExceptFirst

Modifies / returns an array consisting of all elements of the `array` except for the first `skipCount`.

> Difference between functions `keep*` and `slice*`:
> 
> - `keep*` modifies the contents of the original array,
> - `slice*` returns a new array without changing the original.

Arguments
- (array) `array` - array from which elements are taken.
- (number) `skipCount` - number of skipped elements.

Example 1 - Get all tracks except for the first 10.
```js
let tracks = Source.getTracks(playlistArray);
tracks = Selector.sliceAllExceptFirst(tracks, 10);
```

### keepAllExceptLast / sliceAllExceptLast

Modifies / returns an array consisting of all elements of the `array` except for the last `skipCount`.

Arguments
- (array) `array` - array from which elements are taken.
- (number) `skipCount` - number of skipped elements.

Example 1 - Get all tracks except for the last 10.
```js
let tracks = Source.getTracks(playlistArray);
tracks = Selector.sliceAllExceptLast(tracks, 10);
```

### keepFirst / sliceFirst

Modifies / returns an array consisting of the first `count` elements of the `array`.

Arguments
- (array) `array` - array from which elements are taken.
- (number) `count` - number of elements.

Example 1 - Get the first 100 tracks.
```js
let tracks = Source.getTracks(playlistArray);
tracks = Selector.sliceFirst(tracks, 100);
```

### keepLast / sliceLast

Modifies / returns an array consisting of the last `count` elements of the `array`.

Arguments
- (array) `array` - array from which elements are taken.
- (number) `count` - number of elements.

Example 1 - Get the last 100 tracks.
```js
let tracks = Source.getTracks(playlistArray);
tracks = Selector.sliceLast(tracks, 100);
```

### keepNoLongerThan / sliceNoLongerThan

Modifies / returns an array of tracks with a total duration not exceeding `minutes` minutes.

Arguments
- (array) `tracks` - source array of tracks.
- (number) `minutes` - number of minutes.

Example 1 - Get tracks with a total duration not exceeding 60 minutes.
```js
let tracks = Source.getTracks(playlistArray);
tracks = Selector.sliceNoLongerThan(tracks, 60);
```

Example 2 - To calculate the duration of tracks from an array, use one of the options
```js
let tracks = Source.getPlaylistTracks('', '37i9dQZF1DX5PcuIKocvtW');
let duration_ms = tracks.reduce((d, t) => d + t.duration_ms, 0); // milliseconds
let duration_s = tracks.reduce((d, t) => d + t.duration_ms, 0) / 1000; // seconds
let duration_min = tracks.reduce((d, t) => d + t.duration_ms, 0) / 1000 / 60; // minutes
let duration_h = tracks.reduce((d, t) => d + t.duration_ms, 0) / 1000 / 60 / 60; // hours
```

### keepRandom / sliceRandom

Modifies / returns an array consisting of randomly selected elements from the original array.

Arguments
- (array) `array` - array from which elements are taken.
- (number) `count` - number of randomly selected elements.

Example 1 - Get 20 random tracks.
```js
let tracks = Source.getTracks(playlistArray);
tracks = Selector.sliceRandom(tracks, 20);
```

### pickYear

Returns an array of tracks that were released in the specified year. If there are no such tracks, the closest year is selected.

Arguments
- (array) `tracks` - tracks among which to choose from.
- (string) `year` - release year.
- (number) `offset` - acceptable offset for the nearest year. Default is 5.

Example 1 - Select favorite tracks released in 2020
```js
let tracks = Selector.pickYear(savedTracks, '2020');
```

### sliceCopy

Returns a new array that is a copy of the original array.

> Use copying if different actions need to be performed on the source in one script. Allows you to speed up execution time and not send the same requests twice.

Arguments
- (array) `array` - source array, a copy of which needs to be created.

Example 1 - Create an array copy.
```js
let tracks = Source.getTracks(playlistArray);
let tracksCopy = Selector.sliceCopy(tracks);
```
