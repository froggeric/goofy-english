# Library

Methods for managing likes and subscriptions.

| Method | Return type | Brief description |
| --- | --- | --- |
| [checkFavoriteTracks](/reference-en/library?id=checkfavoritetracks) | - | Check each track for presence in favorites (likes). |
| [deleteAlbums](/reference-en/library?id=deletealbums) | - | Delete albums from the library (remove likes on albums). |
| [deleteFavoriteTracks](/reference-en/library?id=deletefavoritetracks) | - | Remove tracks from favorites (remove likes). |
| [followArtists](/reference-en/library?id=followartists) | - | Subscribe to artists. |
| [followPlaylists](/reference-en/library?id=followplaylists) | - | Subscribe to playlists. |
| [saveAlbums](/reference-en/library?id=savealbums) | - | Save albums in the library. |
| [saveFavoriteTracks](/reference-en/library?id=savefavoritetracks) | - | Add tracks to favorites (set likes). |
| [unfollowArtists](/reference-en/library?id=unfollowartists) | - | Unsubscribe from artists. |
| [unfollowPlaylists](/reference-en/library?id=unfollowplaylists) | - | Unsubscribe from playlists. |

## checkFavoriteTracks

Check each track for presence in favorites (likes).

### Arguments :id=checkfavoritetracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | Tracks to check. Only _id_ is significant. |

### Return :id=checkfavoritetracks-return {docsify-ignore}

No return value.

A boolean value `isFavorite` is added to the tracks, indicating whether or not the track is in favorites.

### Examples :id=checkfavoritetracks-examples {docsify-ignore}

1. Leave only playlist tracks without likes.

```js
let tracks = Source.getPlaylistTracks('', 'id')
Library.checkFavoriteTracks(tracks);
tracks = tracks.filter(t => !t.isFavorite);
```

## deleteAlbums

Delete albums from the library (remove likes on albums).

### Arguments :id=deletealbums-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `albums` | Array | Albums to delete. Only _id_ is significant. |

### Return :id=deletealbums-return {docsify-ignore}

No return value.

## deleteFavoriteTracks

Remove tracks from favorites (remove likes).

### Arguments :id=deletefavoritetracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | Tracks to delete. Only _id_ is significant. |

### Return :id=deletefavoritetracks-return {docsify-ignore}

No return value.

### Examples :id=deletefavoritetracks-examples {docsify-ignore}

1. Clear all likes

```js
let savedTracks = Source.getSavedTracks();
Library.deleteFavoriteTracks(savedTracks);
```

## followArtists

Subscribe to artists.

### Arguments :id=followartists-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `artists` | Array | Artists to subscribe to. Only _id_ is significant. |

### Return :id=followartists-return {docsify-ignore}

No return value.

## followPlaylists

Subscribe to playlists.

### Arguments :id=followplaylists-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `playlists` | Array/String | Playlists (only _id_ is significant) or a string with _id_, separated by commas. |

### Return :id=followplaylists-return {docsify-ignore}

No return value.

## saveAlbums

Save albums in the library.

### Arguments :id=savealbums-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `albums` | Array | Albums to add. Only _id_ is significant. |

### Return :id=savealbums-return {docsify-ignore}

No return value.

## saveFavoriteTracks

Add tracks to favorites (set likes).

### Arguments :id=savefavoritetracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | Tracks to add. Only _id_ is significant. |

### Return :id=savefavoritetracks-return {docsify-ignore}

No return value.

## unfollowArtists

Unsubscribe from artists.

### Arguments :id=unfollowartists-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `artists` | Array | Artists to unsubscribe from. Only _id_ is significant. |

### Return :id=unfollowartists-return {docsify-ignore}

No return value.

## unfollowPlaylists

Unsubscribe from playlists.

### Arguments :id=unfollowplaylists-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `playlists` | Array/String | Playlists (only _id_ is significant) or a string with _id_, separated by commas. |

### Return :id=unfollowplaylists-return {docsify-ignore}

No return value.
