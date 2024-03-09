​

# Lastfm

Methods for interacting with Last.fm.

> Equivalent tracks are found by searching Spotify for the best match. If there is no match, the track is ignored. One Last.fm track equals one search request. Be mindful of [limits](/details?id=Limits) on the number of requests per day and execution time.

| Method | Result Type | Brief Description |
| --- | --- | --- |
| [convertToSpotify](/reference-en/lastfm?id=converttospotify) | Array | Find Spotify elements by Last.fm data. |
| [getAlbumsByTag](/reference-en/lastfm?id=getalbumsbytag) | Array | Get albums by tag. |
| [getArtistsByTag](/reference-en/lastfm?id=getartistsbytag) | Array | Get artists by tag. |
| [getCustomTop](/reference-en/lastfm?id=getcustomtop) | Array | Get a custom top from the user. |
| [getLibraryStation](/reference-en/lastfm?id=getlibrarystation) | Array | Get tracks from the _library_ radio station. |
| [getLovedTracks](/reference-en/lastfm?id=getlovedtracks) | Array | Get Last.fm likes. |
| [getMixStation](/reference-en/lastfm?id=getmixstation) | Array | Get tracks from the _mix_ radio station. |
| [getNeighboursStation](/reference-en/lastfm?id=getneighboursstation) | Array | Get tracks from the _neighbors_ radio station. |
| [getRecentTracks](/reference-en/lastfm?id=getrecenttracks) | Array | Get a user's Last.fm listening history. |
| [getRecomStation](/reference-en/lastfm?id=getrecomstation) | Array | Get tracks from the _recommendations_ radio station. |
| [getSimilarArtists](/reference-en/lastfm?id=getsimilarartists) | Array | Get similar artists. |
| [getSimilarTracks](/reference-en/lastfm?id=getsimilartracks) | Array | Get similar tracks. |
| [getTopAlbums](/reference-en/lastfm?id=gettopalbums) | Array | Get a user's top albums. |
| [getTopAlbumsByTag](/reference-en/lastfm?id=gettopalbumsbytag) | Array | Get top albums by tag. |
| [getTopArtists](/reference-en/lastfm?id=gettopartists) | Array | Get a user's top artists. |
| [getTopArtistsByTag](/reference-en/lastfm?id=getTopArtistsByTag) | Array | Get top artists by tag. |
| [getTopTracks](/reference-en/lastfm?id=gettoptracks) | Array | Get a user's top tracks. |
| [getTopTracksByTag](/reference-en/lastfm?id=gettoptracksbytag) | Array | Get top tracks by tag. |
| [getTracksByTag](/reference-en/lastfm?id=gettracksbytag) | Array | Get tracks by tag. |
| [rangeTags](/reference-en/lastfm?id=rangetags) | - | Select tracks by tags. |
| [removeRecentArtists](/reference-en/lastfm?id=removerecentartists) | - | Remove artists from Last.fm listening history data. |
| [removeRecentTracks](/reference-en/lastfm?id=removerecenttracks) | - | Remove tracks from Last.fm listening history data. |

## convertToSpotify

Find Spotify elements by Last.fm data.

### Arguments :id=converttospotify-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `items` | String | Elements in Last.fm format. For example, obtained from [getCustomTop](/reference-en/lastfm?id=getcustomtop) with `isRawItems = true`. |
| `type` | String | Search type: `track`, `artist`, or `album`. Default is `track`. |

### Return :id=converttospotify-return {docsify-ignore}

`items` (array) - search results.

## getAlbumsByTag

Get albums by tag. Parses album names from the tag page, [for example](https://www.last.fm/tag/indie/albums).

### Arguments :id=getalbumsbytag-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tag` | String | Tag name. |
| `limit` | Number | Maximum number of albums. |

### Return :id=getalbumsbytag-return {docsify-ignore}

`albums` (array) - search results for albums in Spotify.

### Examples :id=getalbumsbytag-examples {docsify-ignore}

1. Get albums by tag.

```js
let albums = Lastfm.getAlbumsByTag('indie', 40);
```

## getArtistsByTag

Get artists by tag. Parses artist names from the tag page, [for example](https://www.last.fm/tag/pixie/artists).

### Arguments :id=getartistsbytag-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tag` | String | Tag name. |
| `limit` | Number | Maximum number of artists. |

### Return :id=getartistsbytag-return {docsify-ignore}

`artists` (array) - search results for artists in Spotify.

### Examples :id=getartistsbytag-examples {docsify-ignore}

1. Get artists by tag.

```js
let artists = Lastfm.getArtistsByTag('pixie', 40);
```

## getCustomTop

Get a custom top from the user.

> There is a detailed example of usage on the forum, allowing you to get _recommendations from the past_

### Arguments :id=getcustomtop-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `params` | Object | Selection parameters. |

#### Selection Parameters :id=getcustomtop-params {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `user` | Object | Last.fm user login. By default, the value from the `config-file` is used. |
| `from` | Date/String/Number | Start of period. |
| `to` | Date/String/Number | End of period. |
| `type` | String | Type: `track`, `artist`, or `album`. Default is `track`. |
| `count` | Number | Number of elements. Default is 40. |
| `offset` | Number | Skip the first N elements. Default is 0. |
| `minPlayed` | Number | Minimum number of plays, inclusive. Default is 0. |
| `maxPlayed` | Number | Maximum number of plays, inclusive. Default is 100 thousand. |
| `isRawItems` | Boolean | When not specified or `false`, search for the element by name in Spotify. If `true`, the result from lastfm-elements. Ignores `count` and `offset`. May be needed for independent filtering. Then use the [convertToSpotify](/reference-en/lastfm?id=converttospotify) function. |

### Return :id=getcustomtop-return {docsify-ignore}

`items` (array) - selection result, sorted by number of plays (if `isRawItems = false`).

An additional key `countPlayed` with the value of the number of plays will be added to the objects in the result.

### Examples :id=getcustomtop-examples {docsify-ignore}

1. Select the top 40 tracks for 2015

```js
let topTracks = Lastfm.getCustomTop({
    from: '2015-01-01', // or new Date('2015-01-01'),
    to: '2015-12-31', // or new Date('2015-12-31').getTime(),
});
```

2. Select the top 10 artists for the first half of 2014

```js
let topArtists = Lastfm.getCustomTop({
    type: 'artist',
    from: '2014-01-01',
    to: '2014-06-30',
    count: 10,
});
```

## getLibraryStation

Get tracks from the _library_ radio station on Last.fm. Contains only scrobbled tracks. Note the warning in [getRecentTracks](/reference-en/lastfm?id=getrecenttracks).

### Arguments :id=getlibrarystation-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `user` | String | Last.fm user login. By default, the value from the `config-file` is used. |
| `countRequest` | Number | Number of requests to Last.fm. One request gives approximately 20 to 30 tracks. |

### Return :id=getlibrarystation-return {docsify-ignore}

`tracks` (array) - search results for tracks by name in Spotify.

### Examples :id=getlibrarystation-examples {docsify-ignore}

1. Get tracks from the _library_ radio station.

```js
let tracks = Lastfm.getLibraryStation('login', 2);
```

## getLovedTracks

Get Last.fm likes. Note the warning in [getRecentTracks](/reference-en/lastfm?id=getrecenttracks). Includes the date added, which can be used for filtering by date.

### Arguments :id=getlovedtracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `user` | String | Last.fm user login. By default, the value from the `config-file` is used. |
| `limit` | Number | Limit the number of tracks. |

### Return :id=getlovedtracks-return {docsify-ignore}

`tracks` (array) - search results for tracks by name in Spotify.

### Examples :id=getlovedtracks-examples {docsify-ignore}

1. Get Last.fm likes.

```js
let tracks = Lastfm.getLovedTracks('login', 200);
```

## getMixStation

Get tracks from the _mix_ radio station on Last.fm. Contains scrobbled tracks and Last.fm recommendations. Note the warning in [getRecentTracks](/reference-en/lastfm?id=getrecenttracks).

### Arguments :id=getmixstation-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `user` | String | Last.fm user login. By default, the value from the `config-file` is used. |
| `countRequest` | Number | Number of requests to Last.fm. One request gives approximately 20 to 30 tracks. |

### Return :id=getmixstation-return {docsify-ignore}

`tracks` (array) - search results for tracks by name in Spotify.

### Examples :id=getmixstation-examples {docsify-ignore}

1. Get tracks from the _mix_ radio station.

```js
let tracks = Lastfm.getMixStation('login', 2);
```

## getNeighboursStation

Get tracks from the _neighbors_ radio station on Last.fm. Contains tracks that users with similar musical tastes listen to on Last.fm. Note the warning in [getRecentTracks](/reference-en/lastfm?id=getrecenttracks).

### Arguments :id=getneighboursstation-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `user` | String | Last.fm user login. By default, the value from the `config-file` is used. |
| `countRequest` | Number | Number of requests to Last.fm. One request gives approximately 20 to 30 tracks. |

### Return :id=getneighboursstation-return {docsify-ignore}

`tracks` (array) - search results for tracks by name in Spotify.

### Examples :id=getneighboursstation-examples {docsify-ignore}

1. Get tracks from the _neighbors_ radio station.

```js
let tracks = Lastfm.getNeighboursStation('login', 2);
```

## getRecentTracks

Get a user's Last.fm listening history. In account settings, privacy must be disabled.

### Arguments :id=getrecenttracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `user` | String | Last.fm user login. By default, the value from the `config-file` is used. |
| `count` | Number | Limit the number of tracks. |

### Return :id=getrecenttracks-return {docsify-ignore}

`tracks` (array) - search results for tracks by name in Spotify.

### Examples :id=getrecenttracks-examples {docsify-ignore}

1. Get 200 recently played tracks.

```js
let tracks = Lastfm.getRecentTracks('login', 200);
```

## getRecomStation

Get tracks from the _recommendations_ radio station on Last.fm. Note the warning in [getRecentTracks](/reference-en/lastfm?id=getrecenttracks).

### Arguments :id=getrecomstation-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `user` | String | Last.fm user login. By default, the value from the `config-file` is used. |
| `countRequest` | Number | Number of requests to Last.fm. One request gives approximately 20 to 30 tracks. |

### Return :id=getrecomstation-return {docsify-ignore}

`tracks` (array) - search results for tracks by name in Spotify.

### Examples :id=getrecomstation-examples {docsify-ignore}

1. Get tracks from the _recommendations_ radio station.

```js
let tracks = Lastfm.getRecomStation('login', 2);
```

## getSimilarArtists

Get similar artists.

### Arguments :id=getsimilarartists-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `items` | Array | Tracks or artists. Only the `name` field is significant. |
| `match` | Number | Minimum similarity value for an artist between _0.0_ and _1.0_. |
| `limit` | Number | Number of similar artists per original artist. |
| `isFlat` | Boolean | If `false`, the result is grouped by artists. If `true`, all artists are in one array. Default is `true`. |

### Return :id=getsimilarartists-return {docsify-ignore}

`artists` (array) - search results for artists in Spotify.

### Examples :id=getsimilarartists-examples {docsify-ignore}

1. Get artists similar to the ones being tracked.

```js
let artists = Source.getArtists({ followed_include: true, });
let similarArtists = Lastfm.getSimilarArtists(artists, 0.65, 20);
```

## getSimilarTracks

Get similar tracks.

### Arguments :id=getsimilartracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | Tracks or artists. Only the `name` field is significant. |
| `match` | Number | Minimum similarity value for a track between _0.0_ and _1.0_. |
| `limit` | Number | Number of similar tracks per original track. |
| `isFlat` | Boolean | If `false`, the result is grouped by input tracks. If `true`, all tracks are in one array. Default is `true`. |

### Return :id=getsimilartracks-return {docsify-ignore}

`tracks` (array) - search results for tracks in Spotify.

### Examples :id=getsimilartracks-examples {docsify-ignore}

1. Get tracks similar to the tracks in a playlist.

```js
let playlistTracks = Source.getPlaylistTracks('name', 'id');
let similarTracks = Lastfm.getSimilarTracks(playlistTracks, 0.65, 30);
```

## getTopAlbums

Get a user's top albums.

### Arguments :id=gettopalbums-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `params` | Object | Selection parameters. Similar to the parameters for [getTopTracks](/reference-en/lastfm?id=gettoptracks). |

### Return :id=gettopalbums-return {docsify-ignore}

`albums` (array) - search results for albums in Spotify.

### Examples :id=gettopalbums-examples {docsify-ignore}

1. Get the top 10 albums for half a year.

```js
let albums = Lastfm.getTopAlbums({
  user: 'login',
  period: '6month',
  limit: 10
});
```

## getTopAlbumsByTag

Get top albums by tag.

### Arguments :id=gettopalbumsbytag-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `params` | Object | Selection parameters. |

#### Selection Parameters :id=gettopalbumsbytag-params {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tag` | String | Tag name. |
| `limit` | Number | Number of albums per page. Default is 50. Can be more, but in this case Last.fm sometimes gives a different amount. |
| `page` | Number | Page number, used to shift the result. Default is 1. |

### Return :id=gettopalbumsbytag-return {docsify-ignore}

`albums` (array) - search results for albums in Spotify.

### Examples :id=gettopalbumsbytag-examples {docsify-ignore}

1. Get albums 51-100 by the rock tag

```js
let albums = Lastfm.getTopAlbumsByTag({
    tag: 'rock',
    page: 2,
})
```

## getTopArtists

Get a user's top artists.

### Arguments :id=gettopartists-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `params` | Object | Selection parameters. Similar to the parameters for [getTopTracks](/reference-en/lastfm?id=gettoptracks). |

### Return :id=gettopartists-return {docsify-ignore}

`artists` (array) - search results for artists in Spotify.

### Examples :id=gettopartists-examples {docsify-ignore}

1. Get the top 10 artists for half a year.

```js
let artists = Lastfm.getTopArtists({
  user: 'login',
  period: '6month',
  limit: 10
});
```

## getTopArtistsByTag

Get top artists by tag.

### Arguments :id=gettopartistsbytag-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `params` | Object | Selection parameters. Similar to the parameters for [getTopAlbumsByTag](/reference-en/lastfm?id=gettopalbumsbytag). |

### Return :id=gettopartistsbytag-return {docsify-ignore}

`artists` (array) - search results for artists in Spotify.

### Examples :id=gettopartistsbytag-examples {docsify-ignore}

1. Get the second ten artists from the indie top.

```js
let artists = Lastfm.getTopArtistsByTag({
    tag: 'indie',
    limit: 10,
    page: 2,
})
```

## getTopTracks

Get a user's top tracks.

### Arguments :id=gettoptracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `params` | Object | Selection parameters. |

#### Selection Parameters :id=gettoptracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `user` | String | Last.fm user login. By default, the value from the `config-file` is used. |
| `period` | String | One of the values: _overall_, _7day_, _1month_, _3month_, _6month_, _12month_. |
| `limit` | Number | Limit the number of tracks. |

### Return :id=gettoptracks-return {docsify-ignore}

`tracks` (array) - search results for artists in Spotify.

### Examples :id=gettoptracks-examples {docsify-ignore}

1. Get the top 40 tracks for half a year.

```js
let tracks = Lastfm.getTopTracks({
  user: 'your login',
  period: '6month',
  limit: 40
});
```

## getTopTracksByTag

Get top tracks by tag.

### Arguments :id=gettoptracksbytag-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `params` | Object | Selection parameters. Similar to the parameters for [getTopAlbumsByTag](/reference-en/lastfm?id=gettopalbumsbytag). |

### Return :id=gettoptracksbytag-return {docsify-ignore}

`tracks` (array) - search results for tracks in Spotify.

### Examples :id=gettoptracksbytag-examples {docsify-ignore}

1. Get the top 20 tracks by the pop tag.

```js
let tracks = Lastfm.getTopTracksByTag({
    tag: 'pop',
    limit: 20,
})
```

## getTracksByTag

Get tracks by tag. Parses track names from the tag page, [for example](https://www.last.fm/ru/tag/vocal/tracks?page=1).

### Arguments :id=gettracksbytag-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tag` | String | Tag name. |
| `limit` | Number | Maximum number of tracks. |

### Return :id=gettracksbytag-return {docsify-ignore}

`tracks` (array) - search results for tracks in Spotify.

### Examples :id=gettracksbytag-examples {docsify-ignore}

1. Get tracks by tag.

```js
let tracks = Lastfm.getTracksByTag('vocal', 40);
```

2. Get tracks by several tags.

```js
let tracks = ['rock', 'indie', 'pixie'].reduce((tracks, tag) => 
    Combiner.push(tracks, Lastfm.getTracksByTag(tag, 100)), []);
```

## rangeTags

Select tracks by tags.

### Arguments :id=rangetags-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `spotifyTracks` | Array | Tracks to check. |
| `params` | Object | Selection parameters. |

### Selection Parameters :id=rangetags-params {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `include` | Array | Tag objects. If at least one is present in a track, it is kept. |
| `exclude` | Array | Tag objects. If at least one is present in a track, it is removed. |
| `isRemoveUnknown` | Boolean | If `true`, tracks without tags are removed. If `false`, they remain. Default is `false`. |

### Return :id=rangetags-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=rangetags-examples {docsify-ignore}

1. Keep only rock from recent favorites, excluding indie. Since tags are set by users, they have a popularity indicator (up to 100). `minCount` is the minimum value of the indicator inclusive. If it is less, the track is removed regardless of the presence of the tag.

```js
let tracks = Source.getSavedTracks(20);
Lastfm.rangeTags(tracks, {
  isRemoveUnknown: true,
  include: [
    { name: 'rock', minCount: 10 },
  ],
  exclude: [
    { name: 'indie', minCount: 10 },
  ]
});
```

2. Add tags to tracks, remove unknown ones.

```js
let tracks = Source.getSavedTracks(20);
Lastfm.rangeTags(tracks, {
  isRemoveUnknown: true,
});
```

## removeRecentArtists

Remove artists from Last.fm listening history data.

### Arguments :id=removerecentartists-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `original` | Array | Tracks in which elements need to be removed. |
| `user` | String | Last.fm user login. By default, the value from the `config-file` is used. |
| `count` | Number | Number of tracks in listening history. Default is 600. |
| `mode` | String | Selection mode for artists. If `every`, check each one. If `first`, only check the first (usually main). Default is `every`. |

### Return :id=removerecentartists-return {docsify-ignore}

No return value. Modifies the input array.

## removeRecentTracks

Remove tracks from Last.fm listening history data.

### Arguments :id=removerecenttracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `original` | Array | Tracks in which elements need to be removed. |
| `user` | String | Last.fm user login. By default, the value from the `config-file` is used. |
| `count` | Number | Number of tracks in listening history. Default is 600. |
| `mode` | String | Selection mode for artists. If `every`, check each one. If `first`, only check the first (usually main). Default is `every`. |

### Return :id=removerecenttracks-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=removerecenttracks-examples {docsify-ignore}

1. Create a playlist with favorite tracks that have not been played in the last thousand scrobbles.

```js
let savedTracks = Source.getSavedTracks();
Lastfm.removeRecentTracks(savedTracks, 'login', 1000)
Playlist.saveAsNew({
  name: 'Long time no listen',
  tracks: savedTracks,
});
```
