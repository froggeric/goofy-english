# Source

Methods for obtaining Spotify elements.

### craftTracks

Returns an array of tracks obtained from [getRecomTracks](/reference-en/source?id=getrecomtracks) for each group of five source tracks. Duplicate source tracks are ignored, and recommended ones are removed. The limitation to five elements is dictated by the Spotify API for the recommendation function. You can partially influence the formed groups of elements. Apply one of the `Order` sorts before calling the function.

Arguments:
- (array) `tracks`: tracks for which recommendations will be obtained. When `key` equals `seed_artists`, an array of artists is allowed.
- (object) `params`: additional parameters.

Description of parameters:
- (string) `key`: determines the key by which recommendations are made. Allowed: `seed_tracks` and `seed_artists`. By default, `seed_tracks`.
- (object) `query`: optional parameter, all keys from [getRecomTracks](/reference-en/source?id=getrecomtracks) are available except for the one specified in `key`.

> In `query`, you can specify two of: `seed_tracks`, `seed_artists`, `seed_genres`. The third is chosen based on `key`. Thus, it is possible to set static track/performer/genre (up to 4 values for all). The remaining free places will be filled in based on `key`.

Example 1 - Get recommendations for all favorite tracks by their performers
```js
let tracks = Source.getSavedTracks();
let recomTracks = Source.craftTracks(tracks, {
    key: 'seed_artists',
    query: {
        limit: 20, // default and maximum 100
        min_energy: 0.4,
        min_popularity: 60,
        // target_popularity: 60,
    }
});
```

Example 2 - Recommendations with a specified static genre and track. The remaining three places are occupied by `seed_artists`.
```js
let recomTracks = Source.craftTracks(tracks, {
    key: 'seed_artists',
    query: {
        seed_genres: 'indie',
        seed_tracks: '6FZDfxM3a3UCqtzo5pxSLZ'
    }
});
```

Example 3 - You can specify only an array of tracks. Then recommendations will be made by the `seed_tracks` key.
```js
let tracks = Source.getSavedTracks();
let recomTracks = Source.craftTracks(tracks);
```

### getAlbumsTracks

Returns an array of tracks from all albums.

Arguments:
- (array) `albums`: album list
- (number) `limit` - if specified, randomly selects tracks up to the specified quantity in each album separately.

Example 1 - Get tracks from top-10 Lastfm albums
```js
let albums = Lastfm.getTopAlbums({ user: 'login', limit: 10 });
let tracks = Source.getAlbumsTracks(albums);
```

### getAlbumTracks

Returns an array of tracks for a specified album.

Arguments:
- (object) `album`: object of one album
- (number) `limit` - if specified, randomly selects tracks up to the specified quantity.

Example 1 - Get tracks from the first album in the array
```js
let albums = Source.getArtistsAlbums(artists, {
    groups: 'album',
});
let albumTracks = Source.getAlbumTracks(albums[0]);
```

Example 2 - Get tracks from all albums
```js
let albums = Source.getArtistsAlbums(artists, {
    groups: 'album',
});
let tracks = [];
albums.forEach((album) => Combiner.push(tracks, Source.getAlbumTracks(album)));
```

### getArtists

Returns an array of artists according to the specified `paramsArtist`.

Arguments:
- (object) `paramsArtist`: criteria for selecting artists. The object corresponds to the description from [getArtistsTracks](/reference-en/source?id=getartiststracks) in terms of artist.

Example 1 - Get an array of tracked artists
```js
let artists = Source.getArtists({
    followed_include: true,
});
```

### getArtistsAlbums

Returns an array with all albums of the specified artists.

Arguments:
- (array) `artists`: artist array
- (object) `paramsAlbum` - criteria for selecting albums. The object corresponds to the description from [getArtistsTracks](/reference-en/source?id=getartiststracks) in terms of album.

Example 1 - Get an array of singles from one artist
```js
let artist = Source.getArtists({
    followed_include: false,
    include: [ 
        { id: 'abc', name: 'Avril' }, 
    ],
});
let albums = Source.getArtistsAlbums(artist, {
    groups: 'single',
    // isFlat: false, // group by artist
});
```

### getArtistsTopTracks

Returns an array of top tracks from the artist up to 10 tracks per artist.

Arguments:
- (array) `artists` - artist array. Only `id` is significant.
- (boolean) `isFlat` - if `false`, the result is grouped by artists. If `true`, all tracks are in one array. By default, `true`.

Example 1 - `isFlat = true`
```js
let tracks = Source.getArtistsTopTracks(artists);
tracks[0]; // first track of the first artist
tracks[10]; // first track of the second artist if there are 10 tracks from the first one
```

Example 2 - `isFlat = false`
```js
let tracks = Source.getArtistsTopTracks(artists, false);
tracks[0][0]; // first track of the first artist
tracks[1][0]; // first track of the second artist
```

### getArtistsTracks

Returns an array of tracks from artists according to the specified `params`.

> Many albums are included in the selection. Especially with a large number of tracked artists (100+). To reduce execution time, use filters for artist and album. You can specify random selection of N-quantity.

> Spotify API has a bug. Despite explicit indication of the country from the token, some albums may be duplicated (for different countries). Use duplicate removal for tracks.

Arguments:
- (object) `params` - criteria for selecting artists and their tracks

| Key | Type | Description |
| --- | --- | --- |
| isFlat | boolean | If `false`, the result is grouped by artists. By default, `true`. |
| followed\_include | boolean | If `true`, includes tracked artists. If `false`, artists are taken only from `include` |
| include | array | Selection of artists by `id` to get albums. The `name` key is for convenience and not required.  |
| exclude | array | Selection of artists by `id` to exclude artists from the selection. Use in combination with `followed_include` |
| popularity | object | Artist popularity range |
| followers | object | Range of number of followers of an artist |
| genres | array | List of genres. If at least one is present, the artist passes the filter.  |
| ban\_genres | array | List of genres to block. If at least one is present, the artist is removed from the selection. |
| groups | string | Type of album. Allowed: `album`, `single`, `appears_on`, `compilation` |
| release\_date | object | Album release date. Relative period with `sinceDays` and `beforeDays`. Absolute period with `startDate` and `endDate` |
| \_limit | number | If specified, selects a random quantity of the specified elements (artist, album, track) |

Example object `params` with all keys
```js
{
    isFlat: true,
    artist: {
        followed_include: true,
        popularity: { min: 0, max: 100 },
        followers: { min: 0, max: 100000 },
        artist_limit: 10,
        genres: ['indie'],
        ban_genres: ['rap', 'pop'],
        include: [
            { id: '', name: '' }, 
            { id: '', name: '' },
        ],
        exclude:  [
            { id: '', name: '' }, 
            { id: '', name: '' },
        ],
    },
    album: {
        groups: 'album,single',
        release_date: { sinceDays: 6, beforeDays: 0 },
        // release_date: { startDate: new Date('2020.11.30'), endDate: new Date('2020.12.30') },
        album_limit: 10,
        track_limit: 1,
    }
}
```

Example 1 - Get tracks from singles of tracked artists released in the last week including today. Exclude several artists.
```js
let tracks = Source.getArtistsTracks({
    artist: {
        followed_include: true,
        exclude:  [
            { id: 'abc1', name: '' }, 
            { id: 'abc2', name: '' },
        ],
    },
    album: {
        groups: 'single',
        release_date: { sinceDays: 7, beforeDays: 0 },
    },
});
```

Example 2 - Get tracks from albums and singles of ten tracked artists selected randomly. Artists with no more than 10 thousand subscribers. Only one track from an album.
```js
let tracks = Source.getArtistsTracks({
    artist: {
        followed_include: true,
        artist_limit: 10,
        followers: { min: 0, max: 10000 },
    },
    album: {
        groups: 'album,single',
        track_limit: 1,
        release_date: { sinceDays: 7, beforeDays: 0 },
    },
});
```

Example 3 - Get tracks from albums and singles of specified artists
```js
let tracks = Source.getArtistsTracks({
    artist: {
        followed_include: false,
        include:  [
            { id: 'abc1', name: '' }, 
            { id: 'abc2', name: '' },
        ],
    },
    album: {
        groups: 'album,single',
    },
});
```

### getCategoryTracks

Returns an array of tracks from playlists in the specified category. Sorting playlists by popularity. [List of categories](/reference-en/desc?id=Категории-плейлистов).

Arguments:
- (string) `category_id` - name of the category.
- (object) `params` - additional parameters.

Description of `params`:
- (number) `limit` - limit the number of selected playlists. Maximum 50, by default 20.
- (number) `offset` - skip the specified number of tracks. By default 0.
- (string) `country` - name of the country in which to view category playlists. For example, `RU` or `AU`.

Example 1 - Get tracks from the second ten playlists of the 'focus' category from Australia.
```js
let tracks = Source.getCategoryTracks('focus', { limit: 10, offset: 10, country: 'AU' });
```

Example 2 - Get tracks from 20 playlists in the party category.
```js
let tracks = Source.getCategoryTracks('party');
```

### getFollowedTracks

Returns an array of tracks from tracked playlists and/or personal playlists of a specified user.

> If you need to perform different actions on the source, create a copy of the array [sliceCopy](/reference-en/selector?id=slicecopy) instead of new requests to Spotify through getFollowedTracks.

Arguments:
- (object) `params` - arguments for selecting playlists.

Description of keys:
- (string) `type` - type of selected playlists. By default, `followed`.
- (string) `userId` - [user identifier](#идентификатор). If not specified, the `userId` of the authorized user is set, i.e., yours.
- (number) `limit` - if used, playlists are selected randomly.
- (array) `exclude` - list of playlists to exclude. Significant only `id`. The value `name` is optional and needed only for understanding which playlist it is. You can do without it by using a comment.
- (boolean) `isFlat` - if `false`, the result is grouped by artists. If `true`, all tracks are in one array (each contains the key `origin` with data about the playlist). By default, `true`.

|type|Selection|
| --- | --- |
| owned | Only personal playlists |
| followed | Only tracked playlists |
| all | All playlists |

Full object `params`
```js
{
    type: 'followed',
    userId: 'abc',
    limit: 2,
    exclude: [
        { name: 'playlist 1', id: 'abc1' },
        { id: 'abc2' }, // playlist 2
    ],
}
```

Example 1 - Get tracks only from my tracked playlists.
```js
// All values are default, arguments are not specified
let tracks = Source.getFollowedTracks();


// Same with explicit type of playlists
let tracks = Source.getFollowedTracks({
    type: 'followed',
});
```

Example 2 - Get tracks only from two randomly selected personal playlists of user `example`, excluding several playlists by their id. 
```js
let tracks = Source.getFollowedTracks({
    type: 'owned',
    userId: 'example',
    limit: 2, 
    exclude: [
        { id: 'abc1' }, // playlist 1
        { id: 'abc2' }, // playlist 2
    ],
});
```

> It is advisable to avoid users with too many playlists. For example, `glennpmcdonald` almost has 5 thousand playlists. The limitation is related to the quota for execution in Apps Script. In the allotted time, it will not be possible to obtain such a volume of tracks. More details in [description of limitations](/details?id=Ограничения).

### getListCategory

Returns an array of allowed categories for [getCategoryTracks](/reference-en/source?id=getcategorytracks).

Arguments:
- (object) `params` - parameters for selecting categories.

Description of `params`:
- (number) `limit` - limit the number of selected categories. Maximum 50, by default 20.
- (number) `offset` - skip the specified number of categories. By default 0. Used to get categories after 50+.
- (string) `country` - name of the country in which to view categories. For example, `RU` or `AU`. If not present, globally available. However, an error may occur. To avoid errors, specify the same `country` for the list of categories and the request for playlists. 

Example 1 - Get tracks from 10 playlists in a random category
```js
let listCategory = Source.getListCategory({ limit: 50, country: 'RU' });
let category = Selector.sliceRandom(listCategory, 1);
let tracks = Source.getCategoryTracks(category[0].id, { limit: 10, country: 'RU' });
```

### getPlaylistTracks

Returns an array of tracks from one playlist. Similar to [getTracks](/reference-en/source?id=gettracks) with one playlist.

Arguments:
- (string) `name` - name of the playlist.
- (string) `id` - [playlist identifier](/reference-en/desc?id=Плейлист).
- (string) `user` - [user identifier](/reference-en/desc?id=Пользователь). By default, yours.
- (number) `count` - number of tracks to select.
- (boolean) `inRow` - selection mode. If not present or `true`, the first `count` elements are selected, otherwise random selection

Example 1 - Get tracks from one playlist
```js
let tracks = Source.getPlaylistTracks('Заблокированный треки', 'abcdef');
```

Example 2 - Random selection of 10 tracks
```js
// playlist name and user can be left empty if the playlist id is known
let tracks = Source.getPlaylistTracks('', 'id', '', 10, false);
```

### getRecomTracks

Returns an array of recommended tracks based on specified parameters (up to 100 tracks). For new or little-known artists/tracks, there may not be enough accumulated data for generating recommendations. 

Arguments:
- (object) `queryObj` - parameters for selecting recommendations.

Allowed parameters:
- limit - number of tracks. Maximum 100.
- seed\* - up to **5 values** in any combinations:
  - seed\_artists - [artist identifiers](/reference-en/desc?id=Идентификатор), separated by a comma.
  - seed\_tracks - [track identifiers](/reference-en/desc?id=Идентификатор), separated by a comma.
  - seed\_genres - genres, separated by a comma. Allowed values can be found [here](/reference-en/desc?id=Жанры-для-отбора-рекомендаций).
- max\* - maximum value of one of the [track features](/reference-en/desc?id=Особенности-трека-features).
- min\* - minimum value of one of the [track features](/reference-en/desc?id=Особенности-трека-features).
- target\* - target value of one of the [track features](/reference-en/desc?id=Особенности-трека-features). Tracks are selected that are closest in value.

> In addition, the `features` key has access to the `populatiry` key. For example, `target_popularity`. This is hidden in the API documentation for Spotify.

!> When a specific genre is specified in `seed_genres`, it is not necessary that tracks of this genre will come. Such a seed is a starting point for recommendations.

Example object with parameters:
```js
let queryObj = {
    seed_artists: '',
    seed_genres: '',
    seed_tracks: '',
    max_*: 0,
    min_*: 0,
    target_*: 0,
};
```

Example 1 - Get recommendations for the indie and alternative genres with a positive mood:
```js
let tracks = Source.getRecomTracks({
    seed_genres: 'indie,alternative',
    min_valence: 0.65,
});
```

Example 2 - Get recommendations in the rock and electronic genres based on 3 random favorite artists (up to 5 values).
```js
let savedTracks = Source.getSavedTracks();
Selector.keepRandom(savedTracks, 3);


let artistIds = savedTracks.map(track => track.artists[0].id);


let tracks = Source.getRecomTracks({
    seed_artists: artistIds.join(','),
    seed_genres: 'rock,electronic'
});
```

Example 3 - Substitution of artists' genres. In all `craftTracks` requests, the genres will be randomly selected once. The next time they will already be different.
```js
let artists = Source.getArtists({ followed_include: false });
let genres = Array.from(new Set(artists.reduce((genres, artist) => {
  return Combiner.push(genres, artist.genres);
}, [])));
let recomTracks = Source.craftTracks(artists, {
  key: 'seed_artists',
  query: {
    seed_genres: Selector.sliceRandom(genres, 3).join(','),
    min_popularity: 20,
  }
});
```

### getRelatedArtists

Returns an array of similar artists based on Spotify data.

Arguments:
- (array) `artists` - list of artists for which to get similar ones. Significant only `id`.
- (boolean) `isFlat` - if `false`, the result is grouped by artists. If `true`, all artists are in one array. By default, `true`.

Example 1 - `isFlat = true`
```js
let relatedArtists = Source.getRelatedArtists(artists);
relatedArtists[0]; // first artist
relatedArtists[10]; // 11th artist
```

Example 2 - `isFlat = false`
```js
let relatedArtists = Source.getRelatedArtists(artists, false);
relatedArtists[0][0]; // first artist, similar to the first from the source
relatedArtists[1][0]; // first artist, similar to the second from the source
```

### getReleasesByArtists

Returns an array of tracks from albums released during a specified period.

> It is not recommended to specify a long period for hundreds of artists. Try to limit one of the parameters to a small value.

Arguments:
- (object) `params` - selection parameters
  - (array) `artists` - artists of sought albums. Each element has significant `id` and `name`.
  - (object) `date` - relative or absolute period of time (`sinceDays` and `beforeDays` or `startDate` and `endDate`).
  - (array) `type` - allowed type of album (`single`, `album`).
  - (boolean) `isFlat` - if `false`, the result is grouped by artists. If `true`, all tracks are in one array. By default, `true`.

Example 1 - Releases from tracked artists over the last week
```js
let weekReleases = Source.getReleasesByArtists({
  artists: Source.getArtists({ followed_include: true }),
  date: { sinceDays: 7, beforeDays: 0 },
  type: ['album', 'single'],
});
```

Example 2 - Releases over the year by absolute date from manually selected artists
```js
let artists = Source.getArtists({ 
    followed_include: false,
    include: [
        { id: '4er5NZNuc83Cev96LA28ID' }, // Halflives
        { id: '53XhwfbYqKCa1cC15pYq2q' }, // Imagine Dragons
    ] 
});
let yearReleases = Source.getReleasesByArtists({
  artists: artists,
  date: { startDate: new Date('2017.01.01'), endDate: new Date('2017.12.31') },
  type: ['album', 'single'],
});
```

### getSavedAlbums

Returns an array of saved albums, each containing up to 50 tracks. Albums are sorted from recent to old by save date.

No arguments.

Example 1 - Get tracks from the last saved album
```js
let albums = Source.getSavedAlbums();
let tracks = albums[0].tracks.items;
```

### getSavedAlbumTracks

Returns an array of tracks from all saved albums. You can specify a random selection of albums.

Arguments:
- (number) `limit` - if used, albums are selected randomly up to the specified value.

Example 1 - Get tracks from three random albums
```js
let tracks = Source.getSavedAlbumTracks(3);
```

Example 2 - Get tracks from all saved albums
```js
let tracks = Source.getSavedAlbumTracks();
```

### getSavedTracks

Returns an array of favorite tracks (likes). Sorting is from new to old. If likes were added using automatic means, the date will be the same. Therefore, the final sorting of the array and the Spotify interface may differ.

> To reduce the number of requests, use `Cache.read('SavedTracks.json')`. The cache is updated daily.

Argument:
- (number) `limit` - if specified, limits the number of tracks and requests to obtain them. If not specified, all are returned.

> If you have many favorite tracks and need to perform different actions on them in the script, create a copy of the array [sliceCopy](/reference-en/selector?id=slicecopy) instead of new requests to Spotify.

Example 1 - Get an array of favorite tracks.
```js
let tracks = Source.getSavedTracks();
```

Example 2 - Get the last 5 likes.
```js
let tracks = Source.getSavedTracks(5);
```

### getTopArtists

Returns a list of top artists for the selected period. Up to 98 artists. 

Arguments:
- (string) `timeRange` - period. By default, `medium`. Possible values are given in [getTopTracks](/reference-en/source?id=gettoptracks).

Example 1 - Get top tracks from the top 10 artists
```js
let artists = Source.getTopArtists('long');
Selector.keepFirst(artists, 10);
let tracks = Source.getArtistsTopTracks(artists);
```

### getTopTracks

Returns an array of top tracks by number of plays for the selected period. Up to 98 tracks.

Arguments:
- (string) `timeRange` - period. By default, `medium`.

|timeRange|Period|
| --- | --- |
| short | Approximately last month |
| medium | Approximately last 6 months |
| long | For several years |

!> Such tracks do not contain information about the addition date. When using [rangeDateRel](/reference-en/filter?id=rangedaterel) or [rangeDateAbs](/reference-en/filter?id=rangedateabs), they are assigned a date of 01.01.2000

Example 1 - Get top for the last month.
```js
let tracks = Source.getTopTracks('short');
```

Example 2 - Get top for several years.
```js
let tracks = Source.getTopTracks('long');
```

### getTracks

Returns an array of tracks from one or more playlists.

Arguments:
- (array) `playlistArray` - one or more playlists. 

Format for *one* playlist:
- `id` - [playlist identifier](/reference-en/desc?id=Плейлист).
- `userId` - [user identifier](/reference-en/desc?id=Пользователь).
- `name` - name of the playlist.
- `count` - number of tracks to select.
- `inRow` - selection mode. If not present or `true`, the first `count` elements are selected, otherwise random selection

| id | name | userId | Action |
| --- | --- | --- | --- |
| ✓ | ☓ | ☓ | Take playlist with specified id |
| ☓ | ✓ | ☓ | Search for a playlist by name among your own |
| ☓ | ✓ | ✓ | Search for a playlist by name from the user |

> It is recommended to always specify `id` and `name`. The fastest and most convenient way.

!> If only `name` is specified without `id`, tracks will be returned from the first found playlist with such a name. When the playlist is not found, an empty array will be returned.

Example 1 - Get tracks from two playlists by `id`. The value of `name` is optional and specified for convenience.
```js
let tracks = Source.getTracks([
  { name: 'Главные хиты', id: '37i9dQZF1DX12G1GAEuIuj' },
  { name: 'Кардио', id: '37i9dQZF1DWSJHnPb1f0X3' },
]);
```

Example 2 - Get tracks from personal playlists The Best and Soundtracks.
```js
let tracks = Source.getTracks([
  { name: 'The Best' },
  { name: 'Саундтреки' },
]);
```

Example 3 - Get tracks from the mint playlist by user spotify.
```js
let tracks = Source.getTracks([
  { name: 'mint', userId: 'spotify' },
]);
```

### getTracksRandom

Returns an array of tracks from one or more playlists. Playlists are selected randomly. 

Arguments:
- (array) `playlistArray` - one or more playlists. Similar to [getTracks](/reference-en/source?id=gettracks).
- (number) `countPlaylist` - number of randomly selected playlists. By default, one.

Example 1 - Get tracks from one randomly selected playlist from three.
```js
let tracks = Source.getTracksRandom([
  { name: 'Главные хиты', id: '37i9dQZF1DX12G1GAEuIuj' },
  { name: 'Кардио', id: '37i9dQZF1DWSJHnPb1f0X3' },
  { name: 'Темная сторона', id: '37i9dQZF1DX73pG7P0YcKJ' },
]);
```

Example 2 - Get tracks from two randomly selected playlists from three.
```js
let playlistArray = [
  { name: 'Главные хиты', id: '37i9dQZF1DX12G1GAEuIuj' },
  { name: 'Кардио', id: '37i9dQZF1DWSJHnPb1f0X3' },
  { name: 'Темная сторона', id: '37i9dQZF1DX73pG7P0YcKJ' },
];
let tracks = Source.getTracksRandom(playlistArray, 2);
```

### mineTracks

Returns an array of tracks found by searching for playlists, albums, or tracks by keywords. Duplicates are removed from the result.

Arguments:
- (object) `params` - search parameters.

Description of `params`:
- (string) `type` - type of search. Allowed: `playlist`, `album`, `track`. By default, `playlist`. When `track`, you can use [extended search](https://support.spotify.com/by-ru/article/search/).
- (array) `keyword` - list of keywords for searching elements.
- (number) `requestCount` - number of requests per keyword. One request returns 50 elements if they exist. Maximum 40 requests. By default, one.
- (number) `itemCount` - number of selected elements from all found for one keyword. By default, three.
- (number) `skipCount` - number of skipped elements from the beginning for one keyword. By default, zero. 
- (boolean) `inRow` - if not specified or `false`, elements are selected randomly. If `true`, the first `N` elements are taken (based on the value of `itemCount`).
- (number) `popularity` - minimum popularity value of a track. By default, zero.
- (object) `followers` - range of number of followers of a playlist (boundaries included). Filter before selecting `itemCount`. Use only with a small amount of `requestCount` when `type = playlist`. 

> It is necessary to observe the balance of values in `params`. Several large values can take up a lot of time and make many requests. Determine acceptable combinations in practice.

> You can output the number of completed requests. Add the following line at the end of the function: 
> `console.log('Number of requests', CustomUrlFetchApp.getCountRequest());`

Example 1 - Selection of 5 random playlists for each keyword with track popularity from 70. With a limited number of followers of playlists.
```js
let tracks = Source.mineTracks({
    keyword: ['synth', 'synthpop', 'rock'],
    followers: { min: 2, max: 1000 },
    itemCount: 5,
    requestCount: 3,
    popularity: 70,
});
```

Example 2 - Selection of the first 10 playlists for a keyword with any track popularity
```js
let tracks = Source.mineTracks({
    keyword: ['indie'],
    itemCount: 10,
    inRow: true,
});
```

Example 3 - Selection of tracks from random albums
```js
let tracks = Source.mineTracks({
    type: 'album',
    keyword: ['winter', 'night'],
});
```

Example 4 - Selection of tracks in the indie genre for 2020 year
```js
let tracks = Source.mineTracks({
    type: 'track',
    keyword: ['genre:indie + year:2020'],
});
```
