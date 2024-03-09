# Player

Methods for controlling the player.

| Method | Return Type | Brief Description |
| --- | --- | --- |
| [addToQueue](/reference-en/player?id=addtoqueue) | - | Add tracks to the playback queue. |
| [getAvailableDevices](/reference-en/player?id=getavailabledevices) | Array | Get a list of available devices. |
| [getPlayback](/reference-en/player?id=getplayback) | Object | Get player data, including the currently playing track. |
| [next](/reference-en/player?id=next) | - | Skip to the next track in the queue. |
| [pause](/reference-en/player?id=pause) | - | Pause playback of the current player. |
| [previous](/reference-en/player?id=previous) | - | Go back to the previous track in the queue. |
| [resume](/reference-en/player?id=resume) | - | Resume playing the current queue or create a new one. |
| [setRepeatMode](/reference-en/player?id=setrepeatmode) | - | Set repeat mode. |
| [toggleShuffle](/reference-en/player?id=toggleshuffle) | - | Toggle shuffle mode for the queue. |
| [transferPlayback](/reference-en/player?id=transferplayback) | - | Transfer the current playback to another device. |

## addToQueue

Add tracks to the playback queue. Equivalent to _play next_ in the Spotify interface.

### Arguments :id=addtoqueue-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `items` | Array/Object | An array of tracks or a track object to add to the queue. Only _id_ is significant. |
| `deviceId` | String | Device identifier. Optional if playback is active. |

### Return :id=addtoqueue-return {docsify-ignore}

No return value.

### Examples :id=addtoqueue-examples {docsify-ignore}

1. Play the last saved track next.

```js
let tracks = Source.getSavedTracks(1);
Player.addToQueue(tracks[0]);
```

## getAvailableDevices

Get a list of available devices (connected to Spotify at this moment). Use it to obtain the _id_ of the device. The value from `getPlayback` becomes empty quite quickly when paused.

### Arguments :id=getavailabledevices-arguments {docsify-ignore}

No arguments.

### Return :id=getavailabledevices-return {docsify-ignore}

`devices` (array) - available devices. [Example array](https://developer.spotify.com/documentation/web-api/reference-en/#endpoint-get-a-users-available-devices).

### Examples :id=getavailabledevices-examples {docsify-ignore}

1. Select a device by type. `Smartphone` for phone, `Computer` for PC.

```js
let device = Player.getAvailableDevices().find(d => d.type == 'Smartphone');
// device.id
```

## getPlayback

Get player data, including the currently playing track. When paused, it becomes empty quite quickly.

### Arguments :id=getplayback-arguments {docsify-ignore}

No arguments.

### Return :id=getplayback-return {docsify-ignore}

`playback` (object) - player data. [Example object](https://developer.spotify.com/documentation/web-api/reference-en/#endpoint-get-information-about-the-users-current-playback).

### Examples :id=getplayback-examples {docsify-ignore}

[Example usage](<https://github.com/Chimildic/goofy/discussions/102>)

## next

Skip to the next track in the queue.

### Arguments :id=next-arguments {docsify-ignore}

No arguments.

### Return :id=next-return {docsify-ignore}

No return value.

## pause

Pause playback of the current player.

### Arguments :id=pause-arguments {docsify-ignore}

No arguments.

### Return :id=pause-return {docsify-ignore}

No return value.

## previous

Go back to the previous track in the queue.

### Arguments :id=previous-arguments {docsify-ignore}

No arguments.

### Return :id=previous-return {docsify-ignore}

No return value.

## resume

Resume playing the current queue or create a new one.

### Arguments :id=resume-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `params` | Object | Queue parameters. |

#### Queue Parameters :id=resume-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `deviceId` | String | Device identifier. Optional if playback is active. |
| `context_uri` | String | Play by URI, for example a playlist or album. |
| `tracks` | Array | Create a new queue with tracks. Either `context_uri` or `tracks` should be used. |
| `position_ms` | Number | Set the track progress in milliseconds. |
| `offset` | Number | Set the active track in the queue `{ "position": 5 }`. Counted from zero. |

### Return :id=resume-return {docsify-ignore}

No return value.

### Examples :id=resume-examples {docsify-ignore}

1. Resume playback after pause.

```js
Player.pause();
Utilities.sleep(5000);
Player.resume();
```

2. Create a queue from favorite tracks

```js
let tracks = Source.getSavedTracks();
Player.resume({
    tracks: tracks
});
```

3. Play a playlist by URI

```js
let playlistId = '37i9dQZF1DWYmDNATMglFU';
Player.resume({
    context_uri: `spotify:playlist:${playlistId}`,
});
```

## setRepeatMode

Set repeat mode.

### Arguments :id=setrepeatmode-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `state` | String | Repeats the current track when set to `track`, repeats the current queue when set to `context`, and turns off repeat mode when set to `off`. |
| `deviceId` | String | Device identifier. Optional if playback is active. |

### Return :id=setrepeatmode-return {docsify-ignore}

No return value.

## toggleShuffle

Toggle shuffle mode for the queue.

### Arguments :id=toggleshuffle-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `state` | String | Enables shuffle when set to `true`, and disables it when set to `false`. |
| `deviceId` | String | Device identifier. Optional if playback is active. |

### Return :id=toggleshuffle-return {docsify-ignore}

No return value.

## transferPlayback

Transfer the current playback to another device (i.e., queue and currently playing track, [getPlayback](/reference-en/player?id=getplayback)).

### Arguments :id=transferplayback-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `deviceId` | String | _id_ of the new device. Available values can be obtained, for example, through [getAvailableDevices](/reference-en/player?id=getavailabledevices). |
| `isPlay` | Boolean | Starts playback on the new device when set to `true`. When not specified or `false`, the state remains the same as on the previous device. |

### Return :id=transferplayback-return {docsify-ignore}

No return value.

### Examples :id=transferplayback-examples {docsify-ignore}

[Example usage](https://github.com/Chimildic/goofy/discussions/126)
