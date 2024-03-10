# RecentTracks

Methods for working with the playback history.

| Method | Result Type | Brief Description |
| --- | --- | --- |
| [appendTracks](/reference-en/filter?id=appendtracks) | - | Add an array of tracks to the playback history file. |
| [compress](/reference-en/filter?id=compress) | - | Delete insignificant data about tracks in accumulative files of the playback history. |
| [get](/reference-en/filter?id=get) | Array | Get the tracks from the playback history. |

## appendTracks

Add an array of tracks to the playback history file. The `added_at` date of adding a track to the playlist becomes the `played_at` date of listening. If there are no dates, the current date is set as the date of execution of the function. Sorts by the date of listening from fresh to older ones.

!> Note the limitation of 60 thousand tracks for the playback history. The limit can be increased in the `config` file.

### Arguments :id=appendtracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `filename` | String | Name of the playback history file: `SpotifyRecentTracks` or `LastfmRecentTracks`. |
| `tracks` | Array | Tracks to add. |

### Return :id=appendtracks-return {docsify-ignore}

No return value.

### Examples :id=appendtracks-examples {docsify-ignore}

1. Add all favorite tracks to the playback history

```js
let tracks = Source.getSavedTracks();
RecentTracks.appendTracks('SpotifyRecentTracks', tracks);
```

## compress

Delete insignificant data about tracks in accumulative files of the playback history based on [parameters](/config). A copy of the file is created beforehand.

?> Used for compatibility with previous versions of the library. It's enough to run it once to compress the playback history files. New tracks from the history are compressed automatically.

### Arguments :id=compress-arguments {docsify-ignore}

No arguments.

### Return :id=compress-return {docsify-ignore}

No return value.

## get

Get the tracks from the playback history based on [parameters](/config). Sorts by the date of listening from fresh to older ones.

### Arguments :id=get-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `limit` | Number | If specified, limit the number of returned tracks. If not, all available. |

#### Playback History Parameters

| Enabled Parameter | Returned Array |
|-|-|
| `ON_SPOTIFY_RECENT_TRACKS` | Only Spotify playback history. File `SpotifyRecentTracks`. Duplicates are not removed. |
| `ON_LASTFM_RECENT_TRACKS` | Only Lastfm playback history. File `LastfmRecentTracks`. Duplicates are not removed.  |
| `ON_SPOTIFY_RECENT_TRACKS` and `ON_LASTFM_RECENT_TRACKS` | Combination of both sources with removal of duplicates. File `BothRecentTracks`. |

### Return :id=get-return {docsify-ignore}

`tracks` (array) - tracks from the playback history.

### Examples :id=get-examples {docsify-ignore}

1. Get an array of tracks from the playback history. The source of tracks depends on parameters.

```js
let tracks = RecentTracks.get();
```

2. Get 100 tracks from the playback history.

```js
let tracks = RecentTracks.get(100);
```
