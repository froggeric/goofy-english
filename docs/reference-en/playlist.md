# Playlist

Methods for creating and managing playlists.

### getDescription

Returns a string in the format: `Artist 1, Artist 2... and more`.

Arguments
- (array) `tracks` - tracks from which artists are randomly selected.
- (number) `limit` - number of randomly selected artists. Default is 5.

Example 1 - Create a playlist with description
```js
let tracks = Source.getTracks(playlistArray);
Playlist.saveWithReplace({
    id: 'abcd',
    name: 'Big Mix of the Day',
    tracks: tracks,
    description: Playlist.getDescription(tracks),
});
```

### removeTracks

Removes tracks from a playlist.

Arguments
- (string) `id` - id of the playlist.
- (array) `tracks` - array of tracks.

Example 1 - Remove likes from playlist
```js
let savedTracks = Source.getSavedTracks();
Playlist.removeTracks('id', savedTracks);
```

### saveAsNew

Creates a new playlist every time. Returns the `id` of the created playlist.

Arguments
- (object) `data` - data for creating a playlist.

Format of data for creating a playlist
- (string) `name` - name of the playlist, required.
- (array) `tracks` - array of tracks, required.
- (string) `description` - description of the playlist. Up to 300 characters.
- (boolean) `public` - if `false`, the playlist will be private. Default is `true`.
- (string) `sourceCover` - direct link to cover art (up to 256 kb). If specified, `randomCover` is ignored.
- (string) `randomCover` - adds a random cover with the value of `once`. Without using it, the standard mosaic from Spotify is used.

Example 1 - Create a public playlist with favorite tracks without description and with a random cover
```js
let tracks = Source.getSavedTracks();
Playlist.saveAsNew({
  name: 'Copy of Favorite Tracks',
  tracks: tracks,
  randomCover: 'once',
  // sourceCover: tracks[0].album.images[0].url,
});
```

Example 2 - Create a private playlist with recent listening history and description without cover art
```js
let tracks = RecentTracks.get(200);
Playlist.saveAsNew({
  name: 'Listening History',
  description: '200 recently listened to tracks'
  public: false,
  tracks: tracks,
});
```

### saveWithAppend

Adds tracks to an existing playlist and updates other data (name, description). If the playlist does not exist yet, creates a new one. Returns the `id` of the playlist where the tracks were added.

Arguments
- (object) `data` - data about the playlist. Format of data about the playlist according to the [saveWithReplace](/reference/playlist?id=savewithreplace) description.
- (string) `position` - place to add tracks: beginning `begin` or end `end`. Default is `end`.

Example 1 - Add tracks to the beginning of a playlist
```js
let tracks = Source.getTracks(playlistArray);
Playlist.saveWithAppend({
    id: 'fewf4t34tfwf4',
    name: 'Mix of the Day',
    tracks: tracks,
});
```

Example 2 - Add tracks to the end of a playlist and update the name and description
```js
let tracks = Source.getTracks(playlistArray);
Playlist.saveWithAppend({
    id: 'fewf4t34tfwf4',
    name: 'New Name',
    description: 'New Description',
    tracks: tracks,
});
```

Note! If you update the playlist name without specifying `id`, a new playlist will be created. This is because the search does not find a playlist with the new name.

### saveWithReplace

Replaces the tracks in a playlist and updates other data (name, description). If the playlist does not exist yet, creates a new one. Returns the `id` of the playlist where the tracks were added.

Arguments
- (object) `data` - data about the playlist.

Format of data about the playlist
- (string) `id` - [playlist identifier](#identifier).
- (string) `name` - name of the playlist, required.
- (array) `tracks` - array of tracks, required.
- (string) `description` - description of the playlist. Up to 300 characters.
- (boolean) `public` - if `false`, the playlist will be private. Default is `true`.
- (string) `sourceCover` - direct link to cover art (up to 256 kb). If specified, `randomCover` is ignored.
- (string) `randomCover` - if `once` adds a random cover. With `update`, updates the cover every time. Without using it, the standard mosaic from Spotify is used.

Note! It is recommended to always specify `id`. If `id` is not specified, search by name. If such a playlist does not exist, a new one is created.

Example 1 - Update the contents of the playlist and cover art
```js
let tracks = Source.getTracks(playlistArray);
Playlist.saveWithReplace({
    id: 'fewf4t34tfwf4',
    name: 'Mix of the Day',
    description: 'Description of the playlist',
    tracks: tracks,
    randomCover: 'update',
    // sourceCover: tracks[0].album.images[0].url,
});
```

Example 2 - Update the contents of the playlist from example 1 by searching for it by name
```js
let tracks = RecentTracks.get();
Playlist.saveWithReplace({
    name: 'History',
    description: 'New Description of the Playlist',
    tracks: tracks,
    randomCover: 'update',
});
```

### saveWithUpdate

Updates a playlist or creates a new one. Tracks that are in the array but not in the playlist are added. Tracks that are not in the array but exist in the playlist are removed. What exists both there and there is preserved. The date of track addition is not affected. It is impossible to apply sorting where old and new tracks are mixed (acts as `saveWithAppend`). Returns the `id` of the playlist where the tracks were added.

Arguments
- (object) `data` - data about the playlist, corresponds to [saveWithReplace](/reference/playlist?id=savewithreplace).

Additional mode for `data`
- (string) `position` - place to add tracks: beginning `begin` or end `end`. Default is `end`.
