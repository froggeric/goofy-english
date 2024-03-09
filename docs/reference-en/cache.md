​

# Cache {docsify-ignore}

Methods for managing Google Drive data.

By default, without specifying a file extension, `json` is assumed. Text format `txt` is supported when explicitly specified.

| Method | Result Type | Short Description |
| --- | --- | --- |
| [append](/reference-en/cache?id=append) | Number | Append data to an array from a file. |
| [compressArtists](/reference-en/cache?id=compressartists) | - | Remove insignificant artist data. |
| [compressTracks](/reference-en/cache?id=compresstracks) | - | Remove insignificant track data. |
| [copy](/reference-en/cache?id=copy) | String | Create a copy of the file in the source folder. |
| [read](/reference-en/cache?id=read) | Array/Object/String | Read data from a file. |
| [remove](/reference-en/cache?id=remove) | - | Move the file to the Google Drive trash. |
| [rename](/reference-en/cache?id=rename) | - | Rename the file. |
| [write](/reference-en/cache?id=write) | - | Write data to a file. |

## append

Append data to an array from a file. Creates a file if it does not exist.

### Arguments :id=append-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `filepath` | String | [Path to the file](/best-practices?id=Путь-до-файла). |
| `content` | Array | Data to add. |
| `place` | String | Connection point: `begin` - beginning, `end` - end. Default is `end`. |
| `limit` | Number | Limit the number of array elements after adding new data. Default is 200 thousand elements. |

### Return :id=append-return {docsify-ignore}

`contentLength` (number) - the number of elements after addition.

### Examples :id=append-examples {docsify-ignore}

1. Append playlist tracks to the beginning of a file. Limit the array to 5 thousand tracks after appending.

```js
let tracks = Source.getPlaylistTracks('playlist name', 'id');
Cache.append('filename.json', tracks, 'begin', 5000);
```

2. Append playlist tracks to the end of a file.

```js
let tracks = Source.getPlaylistTracks('playlist name', 'id');
Cache.append('filename.json', tracks);
```

## compressArtists

Remove insignificant artist data. Use before saving to a file to reduce its size.

### Arguments :id=compressartists-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `artists` | Array | Artists whose insignificant data needs to be removed. |

### Return :id=compressartists-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=compressartists-examples {docsify-ignore}

1. Reduce the size of a file with an artist array.

```js
let filename = 'artists.json';
let artists = Cache.read(filename);
Cache.compressArtists(artists);
Cache.write(filename, artists);
```

## compressTracks

Remove insignificant track data. Use before saving to a file to reduce its size. Includes the [compressArtists](/reference-en/cache?id=compressartists) call.

### Arguments :id=compresstracks-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | Tracks in which insignificant data needs to be removed. |

### Return :id=compresstracks-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=compresstracks-examples {docsify-ignore}

1. Reduce the size of a file with an artist array.

```js
let filename = 'tracks.json';
let tracks = Cache.read(filename);
Cache.compressTracks(tracks);
Cache.write(filename, tracks);
```

## copy

Create a copy of the file in the source folder.

### Arguments :id=copy-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `filepath` | String | [Path to the file](/best-practices?id=Путь-до-файла). |

### Return :id=copy-return {docsify-ignore}

`filecopypath` (string) - path to the created copy.

### Examples :id=copy-examples {docsify-ignore}

1. Create a copy of the file and read its data.

```js
let filename = 'tracks.json';
let filecopyname = Cache.copy(filename);
let tracks = Cache.read(filecopyname);
```

## read

Read data from a file.

### Arguments :id=read-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `filepath` | String | [Path to the file](/best-practices?id=Путь-до-файла). |

### Return :id=read-return {docsify-ignore}

`content` (array/object/string) - data from the file.

If the file does not exist, the extension in the `filepath` string is checked. If it is missing or equal to _json_, an empty array will be returned. In other cases, an empty string will be returned.

### Examples :id=read-examples {docsify-ignore}

1. Read data from a file and add it to the playlist.

```js
let tracks = Cache.read('tracks.json');
Playlist.saveAsNew({
    name: 'Tracks from file',
    tracks: tracks,
});
```

## remove

Move the file to the Google Drive trash. Data in the trash is deleted after 30 days.

### Arguments :id=remove-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `filepath` | String | [Path to the file](/best-practices?id=Путь-до-файла). |

### Return :id=remove-return {docsify-ignore}

No return value.

### Examples :id=remove-examples {docsify-ignore}

1. Move the file to the trash

```js
Cache.remove('filepath.json');
```

## rename

Rename the file.

> Do not use names `SpotifyRecentTracks`, `LastfmRecentTracks`, `BothRecentTracks`. They are needed for the [listening history](/details?id=История-прослушиваний) accumulation mechanism.

### Arguments :id=rename-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `filepath` | String | [Path to the file](/best-practices?id=Путь-до-файла). |
| `newFilename` | String | New file name (not path) |

### Return :id=rename-return {docsify-ignore}

No return value.

### Examples :id=rename-examples {docsify-ignore}

1. Rename the file.

```js
Cache.rename('filename.json', 'newname.json');
```

## write

Write data to a file. Creates a file if it does not exist. Overwrites the file if it exists.

### Arguments :id=write-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `filepath` | String | [Path to the file](/best-practices?id=Путь-до-файла). |
| `content`  | Array | Data for writing.                                 |

### Return :id=write-return {docsify-ignore}

No return value.

### Examples :id=write-examples {docsify-ignore}

1. Write favorite tracks to a file.

```js
let tracks = Sourct.getSavedTracks();
Cache.write('liked.json', tracks);
```
