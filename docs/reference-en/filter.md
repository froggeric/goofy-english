​

# Filter

Methods for filtering elements.

| Method | Result Type | Short Description |
| --- | --- | --- |
| [dedupArtists](/reference/filter?id=dedupartists) | - | Remove duplicate artists. |
| [dedupTracks](/reference/filter?id=deduptracks) | - | Remove duplicate tracks. |
| [getDateRel](/reference/filter?id=getdaterel) | Date | Calculate the date by offset in days relative to today. |
| [detectLanguage](/reference/filter?id=detectlanguage) | - | Determine the main language of track performance based on text. |
| [getLastOutRange](/reference/filter?id=getlastoutrange) | Array | Find out which tracks did not pass the last filtering by [rangeTracks](/reference/filter?id=rangetracks) |
| [match](/reference/filter?id=match) | - | Remove tracks that do not meet the regular expression. |
| [matchExcept](/reference/filter?id=matchexcept) | - | Wrapper for [match](/reference/filter?id=match) with inversion. |
| [matchExceptMix](/reference/filter?id=matchexceptmix) | - | Remove tracks containing the words _mix_ and _club_. |
| [matchExceptRu](/reference/filter?id=matchexceptru) | - | Remove tracks that contain Cyrillic in the title. |
| [matchLatinOnly](/reference/filter?id=matchlatinonly) | - | Remove all tracks except those containing Latin characters in the title. |
| [matchOriginalOnly](/reference/filter?id=matchoriginalonly) | - | Remove non-original versions of tracks. |
| [rangeDateAbs](/reference/filter?id=rangedateabs) | - | Select elements that fall within a period by absolute dates of addition or playback. |
| [rangeDateRel](/reference/filter?id=rangedaterel) | - | Select elements that fall within a period by relative dates of addition or playback. |
| [rangeTracks](/reference/filter?id=rangetracks) | - | Select tracks that fall within the range of metadata. |
| [removeArtists](/reference/filter?id=removeartists) | - | Exclude artists from an array. |
| [removeTracks](/reference/filter?id=removetracks) | - | Exclude tracks from an array. |
| [removeUnavailable](/reference/filter?id=removeunavailable) | - | Exclude unplayable tracks. |
| [replaceWithSimilar](/reference/filter?id=replacewithsimilar) | - | Replace tracks with similar ones. |

## dedupArtists

Remove duplicate artists by _id_. Only one element from the same artist will remain.

### Arguments :id=dedupartists-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `items` | Array | Tracks or artists in which it is necessary to remove duplicate artists. |

### Return :id=dedupartists-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=dedupartists-examples {docsify-ignore}

1. Remove duplicates of artists in tracks.

```js
let tracks = Source.getTracks(playlistArray);
Filter.dedupArtists(tracks);
```

1. Remove duplicate artists from an array of artists.

```js
let relatedArtists = Source.getRelatedArtists(artists);
Filter.dedupArtists(relatedArtists);
```

## dedupTracks

Remove duplicate tracks by _id_ and _name_.

### Arguments :id=deduptracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | Tracks in which it is necessary to remove duplicates. |
| `offsetDurationMs` | Number | Offset in milliseconds, at which identical tracks by name are considered the same. By default 2000 milliseconds (2 seconds). [More details](https://github.com/Chimildic/goofy/discussions/116). |

### Return :id=deduptracks-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=deduptracks-examples {docsify-ignore}

1. Remove duplicates. Offset can be specified as optional.

```js
let tracks = Source.getTracks(playlistArray);
Filter.dedupTracks(tracks);
```

## getDateRel

Calculate the date by offset in days relative to today.

### Arguments :id=getdaterel-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `days` | Number | Offset in days relative to today. |
| `bound` | String | Time boundary. At `startDay` 00:00, at `endDay` 23:59. If not specified, the value from the moment of method execution will remain. |

### Return :id=getdaterel-return {docsify-ignore}

`date` (date) - the desired date after offsetting.

### Examples :id=getdaterel-examples {docsify-ignore}

Example in template [favorite and forgotten](/template?id=Любимо-и-забыто).

## detectLanguage

Determine the main language of track execution by text.

!> Requires setting the `MUSIXMATCH\_API\_KEY` parameter, [more information](/config). The service limits the number of requests per day. Use this function only after reducing the array of tracks with other filters.

### Arguments :id=detectlanguage-arguments {docsify-ignore}

| Name | Type | Description |
|-----|-----|----------|
| `tracks` | Array | Tracks for which you need to determine the main language of execution. |
| `params` | Object | Filter parameters. |

#### Filter Parameters :id=detectlanguage-params {docsify-ignore}

| Name | Type | Description |
|-----|-----|----------|
| `isRemoveUnknown` | Boolean | Action when the language is unknown (when it is not in the `musixmatch` database or it is a dance language). When `true`, deletes such tracks, when `false` leaves them. Default is `false`. |
| `include` | Array | Languages to keep. |
| `exclude` | Array | Languages to exclude. |

?> Accepts two-letter language designations in lowercase. [List of languages](https://ru.wikipedia.org/wiki/Коды_языков).

### Return :id=detectlanguage-return {docsify-ignore}

No return value. Modifies the input array when filter parameters are specified.

The `lyrics` object is added to the tracks, which contains the language designation `lang` and a short excerpt of text `text`.

### Examples :id=detectlanguage-examples {docsify-ignore}

1. Take [Germany's top hits](https://open.spotify.com/playlist/37i9dQZEVXbJiZcmkrIHGU?si=33fdf90a2b854fc8) and leave only German.

```js
let tracks = Source.getPlaylistTracks('', '37i9dQZEVXbJiZcmkrIHGU');
Filter.detectLanguage(tracks, {
  isRemoveUnknown: true,
  include: ['de'],
});
```

2. Similar situation, exclude Russian.

```js
let tracks = Source.getPlaylistTracks('', '37i9dQZEVXbL8l7ra5vVdB');
Filter.detectLanguage(tracks, {
  isRemoveUnknown: true,
  exclude: ['ru'],
});
```

3. Find out which languages are in the array.

```js
let tracks = Source.getPlaylistTracks('', '37i9dQZEVXbL8l7ra5vVdB');
Filter.detectLanguage(tracks, { isRemoveUnknown: true });
console.log(Array.from(new Set(tracks.map(t => t.lyrics.lang))).join('\n'));
```

?> The [mineTracks](/reference/source?id=minetracks) search function is suitable for a set of tracks in different languages.

## getLastOutRange

Find tracks that did not pass the last filtering [rangeTracks](/reference/filter?id=rangetracks).

### Arguments :id=getlastoutrange-arguments {docsify-ignore}

No arguments.

### Return :id=getlastoutrange-return {docsify-ignore}

`outRangeTracks` (array) - tracks that did not pass the filtering.

### Examples :id=getlastoutrange-examples {docsify-ignore}

1. Get tracks that did not pass the filtering.

```js
let tracks = Source.getTracks(playlistArray);
Filter.rangeTracks(tracks, args);
let outRangeTracks = Filter.getLastOutRange();
```

​

## match

Remove tracks that do not meet the regular expression by track name, album and artist.

### Arguments :id=match-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `items` | Array | Tracks or artists to check with a regular expression. |
| `strRegex` | String | Value of the regular expression |
| `invert` | Boolean | If `true`, inverts the result. By default, `false`. |

### Return :id=match-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=match-examples {docsify-ignore}

1. Remove tracks containing the words _cover_ or _live_ in their name.

```js
let tracks = Source.getTracks(playlistArray);
Filter.match(tracks, 'cover|live', true);
```

## matchExcept

Wrapper for [match](/reference/filter) with `invert = true`.

### Arguments :id=matchexcept-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `items` | Array | Tracks or artists to check with a regular expression. |
| `strRegex` | String | Value of the regular expression |

### Return :id=matchexcept-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=matchexcept-examples {docsify-ignore}

1. Remove tracks containing the words _cover_ or _live_ in their name.

```js
let tracks = Source.getTracks(playlistArray);
Filter.matchExcept(tracks, 'cover|live');
```

## matchExceptMix

Remove tracks containing the words _mix_ and _club_. Wrapper for [matchExcept](/reference/filter?id=matchexcept) with argument `strRegex = 'mix|club'`.

### Arguments :id=matchexceptmix-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | Tracks to check with a regular expression. |

### Return :id=matchexceptmix-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=matchexceptmix-examples {docsify-ignore}

1. Remove tracks containing the words _mix_ or _club_ in their name.

```js
let tracks = Source.getTracks(playlistArray);
Filter.matchExceptMix(tracks);
```

## matchExceptRu

Remove tracks containing Cyrillic in the title. Wrapper for [matchExcept](/reference/filter?id=matchexcept) with argument `strRegex = '[а-яА-ЯёЁ]+'`.

> For filtering by track language, use [detectLanguage](/reference/filter?id=detectlanguage).

### Arguments :id=matchexceptru-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | Tracks to check with a regular expression. |

### Return :id=matchexceptru-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=matchexceptru-examples {docsify-ignore}

1. Remove tracks containing Cyrillic in their name.

```js
let tracks = Source.getTracks(playlistArray);
Filter.matchExceptRu(tracks);
```

## matchLatinOnly

Remove all tracks except those containing Latin characters in the title. Wrapper for [match](/reference/filter?id=match) with argument `strRegex = '^[a-zA-Z0-9 ]+$'`.

> For filtering by track language, use [detectLanguage](/reference/filter?id=detectlanguage).

### Arguments :id=matchlatinonly-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | Tracks to check with a regular expression. |

### Return :id=matchlatinonly-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=matchlatinonly-examples {docsify-ignore}

1. Leave only tracks with Latin characters in their name.

```js
let tracks = Source.getTracks(playlistArray);
Filter.matchLatinOnly(tracks);
```

## matchOriginalOnly

Remove non-original versions of tracks. Wrapper for [matchExcept](/reference/filter?id=matchexcept) with argument `strRegex = 'mix|club|radio|piano|acoustic|edit|live|version|cover|karaoke'`.

### Arguments :id=matchoriginalonly-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | Tracks to check with a regular expression. |

### Return :id=matchoriginalonly-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=matchoriginalonly-examples {docsify-ignore}

1. Leave only original versions of tracks.

```js
let tracks = Source.getTracks(playlistArray);
Filter.matchOriginalOnly(tracks);
```

## rangeDateAbs

Select elements that fall within a period by absolute dates. The date of addition or playback is checked.

> For filtering by album release date, use [rangeTracks](/reference/filter?id=rangetracks).

### Arguments :id=rangedateabs-arguments {docsify-ignore}

| Name | Type | Description |
|------|------|-------------|
| `items` | Array | Elements to check. |
| `startDate` | Date | Start of the absolute period. |
| `endDate` | Date | End of the absolute period. |

The date format is `YYYY-MM-DDTHH:mm:ss.sss`, where
- `YYYY-MM-DD` - year, month, day
- `T` - separator to indicate time. Specify if time is added.
- `HH:mm:ss.sss` - hours, minutes, seconds, milliseconds

### Return :id=rangedateabs-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=rangedateabs-examples {docsify-ignore}

1. Tracks added between 1 and 3 September.

```js
let tracks = Source.getTracks(playlistArray);
let startDate = new Date('2020-09-01');
let endDate = new Date('2020-09-03');
Filter.rangeDateAbs(tracks, startDate, endDate);
```

2. Tracks added from 1 August 15:00 to 20 August 10:00.

```js
let tracks = Source.getTracks(playlistArray);
let startDate = new Date('2020-08-01T15:00');
let endDate = new Date('2020-08-20T10:00');
Filter.rangeDateAbs(tracks, startDate, endDate);
```

3. Tracks added from 1 September to the current date and time.

```js
let tracks = Source.getTracks(playlistArray);
let startDate = new Date('2020-09-01');
let endDate = new Date();
Filter.rangeDateAbs(tracks, startDate, endDate);
```

## rangeDateRel

Select elements that fall within a period by relative dates. The date of addition or playback is checked.

> If an element does not contain a date, it is set to 01.01.2000. This may happen if the track was added to Spotify a long time ago and the source is [getTopTracks](/reference/source?id=gettoptracks), or this is playlists such as "My Mix of the Day #N" or several other sources.

### Arguments :id=rangedaterel-arguments {docsify-ignore}

| Name | Type | Description |
|------|------|-------------|
| `items` | Array | Elements to check. |
| `sinceDays` | Number | Start of the relative period. |
| `beforeDays` | Number | End of the relative period. |

In the diagram, an example for `sinceDays = 7` and `beforeDays = 2`. That is, get elements added to the playlist from September 3, 00:00 to September 8, 23:59 relative to today, September 10.

![Example of using sinceDays and beforeDays](../img/DaysRel.png ':size=60%')

> The mechanism for obtaining the start date of tracking an artist is described [on the forum](https://github.com/Chimildic/goofy/discussions/98)

### Return :id=rangedaterel-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=rangedaterel-examples {docsify-ignore}

1. Tracks added in the last 5 days and today.

```js
let tracks = Source.getTracks(playlistArray);
Filter.rangeDateRel(tracks, 5);
// equivalent to Filter.rangeDateRel(tracks, 5, 0);
```

2. Tracks for the last 7 days excluding today.

```js
let tracks = Source.getTracks(playlistArray);
Filter.rangeDateRel(tracks, 7, 1);
```

3. Tracks for one day that was 14 days ago.

```js
let tracks = Source.getTracks(playlistArray);
Filter.rangeDateRel(tracks, 14, 14);
```

4. Tracks only for today.
```js
let tracks = Source.getTracks(playlistArray);
Filter.rangeDateRel(tracks);
// equivalent to Filter.rangeDateRel(tracks, 0, 0);
```

## rangeTracks

Select tracks that fall within the metadata range.

### Arguments :id=rangetracks-arguments {docsify-ignore}

| Name | Type | Description |
|------|-----|-------------|
| `tracks` | Array | Tracks to check. |
| `params` | Object | Selection parameters. |

> The function requests additional data. To reduce the number of requests, use it after reducing the array of tracks by other means (e.g., [rangeDateRel](/reference/filter?id=rangedaterel), [match](/reference/filter?id=match) and others). Obtained data is cached for **current** execution. Repeated function calls or sorting with the same categories using [sort](/reference/order?id=sort) do not send new requests.

#### Selection Parameters :id=rangetracks-params {docsify-ignore}

Below is an example of a `params` object with all possible conditions for checking. [Parameter description](/reference/desc?id=Описание-параметров-объектов).

```js
let params = {
    meta: {
        popularity: { min: 0, max: 100 },
        duration_ms: { min: 0, max: 10000 },
        explicit: false,
    },
    artist: {
        popularity: { min: 0, max: 100 },
        followers: { min: 0, max: 100000 },
        genres: ['indie'],
        ban_genres: ['rap', 'pop'],
        isRemoveUnknownGenre: false,
    },
    features: {
        acousticness: { min: 0.0, max: 1.0 },
        danceability: { min: 0.0, max: 1.0 },
        energy: { min: 0.0, max: 1.0 },
        instrumentalness: { min: 0.0, max: 1.0 },
        liveness: { min: 0.0, max: 1.0 },
        loudness: { min: -60, max: 0 },
        speechiness: { min: 0.0, max: 1.0 },
        valence: { min: 0.0, max: 1.0 },
        tempo: { min: 30, max: 210 },
        key: 0,
        mode: 0,
        time_signature: 1,


        // calculated <https://github.com/Chimildic/goofy/discussions/87>
        anger: { min: 0.0, max: 1.0 },
        happiness: { min: 0.0, max: 1.0 },
        sadness: { min: 0.0, max: 1.0 },  

        // duplicates args.meta.duration_ms, only one is sufficient (choice depends on the category)
        duration_ms: { min: 0, max: 10000 },
    },
    album: {
        popularity: { min: 30, max: 70 },
        album_type: ['single', 'album'],
        release_date: { sinceDays: 6, beforeDays: 0 },
        // or release_date: { startDate: new Date('2020.11.30'), endDate: new Date('2020.12.30') },
    },
};
```

### Return :id=rangetracks-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=rangetracks-examples {docsify-ignore}

1. Remove tracks of rap genre.

```js
let tracks = Source.getTracks(playlistArray);
Filter.rangeTracks(tracks, {
    artist: {
      ban_genres: ['rap'],
    }
});
```

2. Select tracks in the indie and alternative genres.

```js
let tracks = Source.getTracks(playlistArray);
Filter.rangeTracks(tracks, {
    artist: {
        genres: ['indie', 'alternative'],
    },
});
```

3. Select unpopular tracks by little-known artists.

```js
let tracks = Source.getTracks(playlistArray);
Filter.rangeTracks(tracks, {
    meta: {
      popularity: { min: 0, max: 49 },
    },
    artist: {
      followers: { min: 0, max: 9999 },
    },
});
```

​

## removeArtists

Exclude artists from the array. Matching is determined by the track artist's _id_.

### Arguments :id=removeartists-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `original` | Array | Checked tracks or artists. |
| `removable` | Array | Tracks or artists to exclude. |
| `invert` | Boolean | Inversion of the result. If `true`, delete all except `removable`. By default, `false`. |
| `mode` | String | Artist selection mode. With `every`, check each one; with `first`, only the first (usually main) one. Default is `every`. |

### Return :id=removeartists-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=removeartists-examples {docsify-ignore}

1. Get playlist tracks and exclude favorite track artists.

```js
let sourceArray = Source.getTracks(playlistArray);
let removedArray = Source.getSavedTracks();
Filter.removeArtists(sourceArray, removedArray);
```

## removeTracks

Exclude tracks from the array. Matching is determined by the track's _id_ and its name along with the artist.

> There is a possibility of encountering the [relink problem](https://github.com/Chimildic/goofy/discussions/99) when deleting playback history.

### Arguments :id=removetracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `original` | Array | Checked tracks. |
| `removable` | Array | Tracks to exclude. |
| `invert` | Boolean | Inversion of the result. If `true`, delete all except `removable`. By default, `false`. |
| `mode` | String | Artist selection mode. With `every`, check each one; with `first`, only the first (usually main) one. Default is `every`. |

### Return :id=removetracks-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=removetracks-examples {docsify-ignore}

1. Get playlist tracks and exclude favorite tracks.

```js
let sourceArray = Source.getTracks(playlistArray);
let removedArray = Source.getSavedTracks();
Filter.removeTracks(sourceArray, removedArray);
```

## removeUnavailable

Exclude unplayable tracks. Does not replace the original with an analogue. That is, there is no redirection to another track, more details in [the relink problem](https://github.com/Chimildic/goofy/discussions/99). Makes additional requests (1 for 50 tracks) if a track is in an undefined state.

> It's acceptable to apply the filter to tracks from `Cache`, passed through the [compressTracks](/reference/cache?id=compresstracks) method. If the method was not applied, the state is determined by the value in the cached track.

### Arguments :id=removeunavailable-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | Checked tracks. |
| `market` | String | Country in which the track availability is checked. By default, the country of the account. |

### Return :id=removeunavailable-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=removeunavailable-examples {docsify-ignore}

1. Delete unavailable tracks in Russia from a playlist.

```js
let tracks = Source.getPlaylistTracks('', 'id');
Filter.removeUnavailable(tracks, 'RU');
```

## replaceWithSimilar

Replace tracks with similar ones. For one replacement N random tracks are taken from the results of [getRecomTracks](/reference/source?id=getrecomtracks). Recommendations are requested with `target_*` parameters from the original track. When replacements are not available, the track is deleted.

### Arguments :id=replacewithsimilar-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `params` | Object | Replacement parameters. |

### Replacement Parameters :id=replacewithsimilar-params {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `origin` | Array | Checked tracks. |
| `replace` | Array | Tracks to replace. |
| `count` | Number | Number of recommended tracks per original. Default is 1, maximum is 100. The output amount may be less due to the lack of recommendations or filtering. |
| `isRemoveOriginArtists` | Boolean | If `true`, removes artists from recommendations that appear in `origin`. If `false`, leaves them. By default, `false`. |

### Return :id=replacewithsimilar-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=replacewithsimilar-examples {docsify-ignore}

1. Replace recently played tracks and likes from a playlist with similar analogs.

```js
let tracks = Source.getPlaylistTracks('', 'id');
Filter.replaceWithSimilar({
    origin: tracks,
    replace: [RecentTracks.get(2000), Source.getSavedTracks()],
    count: 3
});
```

2. Get recommendations for favorite tracks without involving the original artists.

```js
let tracks = Source.getSavedTracks();
Filter.replaceWithSimilar({
  origin: tracks,
  replace: tracks,
  isRemoveOriginArtists: true,
})
```
