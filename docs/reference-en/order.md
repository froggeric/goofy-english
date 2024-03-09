# Order

Sorting methods.

| Method | Return type | Brief description |
| --- | --- | --- |
| [reverse](/reference-en/order?id=reverse) | - | Sort in reverse order. |
| [separateArtists](/reference-en/order?id=separateartists) | - | Separate tracks by the same artist. |
| [separateYears](/reference-en/order?id=separateyears) | Object | Distribute tracks by release year. |
| [shuffle](/reference-en/order?id=shuffle) | - | Shuffle array elements randomly. |
| [sort](/reference-en/order?id=sort) | - | Sort the array by metadata. |

## reverse

Sort in reverse order.

### Arguments :id=reverse-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `array` | Array | The array to be sorted. |

### Return :id=reverse-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=reverse-examples {docsify-ignore}

1. Reverse sorting for an array of numbers.

```js
let array = [1, 2, 3, 4, 5, 6];

Order.reverse(array);
// result: 6, 5, 4, 3, 2, 1

Order.reverse(array);
// result: 1, 2, 3, 4, 5, 6
```

2. Reverse sorting for an array of tracks.

```js
let tracks = Source.getTracks(playlistArray);
Order.reverse(tracks);
```

## separateArtists

Separate tracks by the same artist. Tracks that cannot be placed will be excluded.

### Arguments :id=separateartists-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | The tracks to be sorted. |
| `space` | Number | Minimum spacing. |
| `isRandom` | Boolean | If `true`, performs a random sort of the original array, which will affect the order when separating artists. If `false`, without random sorting. Then the result with identical input tracks will also be the same. By default, `false`. |

### Return :id=separateartists-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=separateartists-examples {docsify-ignore}

1. Example of separation

```js
let array = ['cat', 'cat', 'dog', 'lion']
Order.separateArtists(array, 1, false);
// result: cat, dog, cat, lion

array = ['cat', 'cat', 'dog', 'lion']
Order.separateArtists(array, 1, false);
// repeated call, same result: cat, dog, cat, lion

array = ['cat', 'cat', 'dog', 'lion']
Order.separateArtists(array, 1, true);
// repeated call and random sort: cat, lion, dog, cat
```

2. Separate the same artist by at least two others.

```js
let tracks = Source.getTracks(playlistArray);
Order.separateArtists(tracks, 2);
```

## separateYears

Distribute tracks by release year. Tracks are not sorted by date.

### Arguments :id=separateyears-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `tracks` | Array | The tracks to be sorted. |

### Return :id=separateyears-return {docsify-ignore}

`tracksByYear` (object) - arrays of tracks distributed by years.

### Examples :id=separateyears-examples {docsify-ignore}

1. Get tracks released only in 2020

```js
let tracks2020 = Order.separateYears(tracks)['2020'];
```

2. An error may occur where there are no tracks for the specified year among the tracks. Choose one of: use [pickYear](/reference-en/selector?id=pickyear), replace with an empty array, or check conditionally.

```js
// Replace with an empty array if there are no tracks for the specified year
let tracks2020 = Order.separateYears(tracks)['2020'] || [];

// Check using a condition
let tracksByYear = Order.separateYears(tracks);
if (typeof tracksByYear['2020'] != 'undefined'){
    // tracks exist
} else {
    // no tracks
}
```

## shuffle

Shuffle array elements randomly.

### Arguments :id=shuffle-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `array` | Array | The array to be sorted. |

### Return :id=shuffle-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=shuffle-examples {docsify-ignore}

1. Random sorting for an array of numbers.

```js
let array = [1, 2, 3, 4, 5, 6];

Order.shuffle(array);
// result: 3, 5, 4, 6, 2, 1

Order.shuffle(array);
// result: 6, 1, 2, 3, 5, 4

Order.shuffle(array);
// result: 6, 5, 2, 3, 1, 4
```

2. Random sorting for an array of tracks.

```js
let tracks = Source.getTracks(playlistArray);
Order.shuffle(tracks);
```

## sort

Sort the array by metadata.

> The function makes additional requests. To reduce the number of requests, use it after maximally reducing the array of tracks in other ways. More details in [rangeTracks](/reference-en/filter?id=rangetracks).

?> If multiple sources of tracks are mixed, not all have the specified key. For example, `played_at` is only present in listening history.

### Arguments :id=sort-arguments {docsify-ignore}

| Name | Type | Description |
| --- | --- | --- |
| `source` | Array | The array to be sorted. |
| `pathKey` | String | Sort key. |
| `direction` | String | Sort direction: _asc_ ascending, _desc_ descending. Default is _asc_. |

#### Sort Key

The key has the format `category.data`. [Description of object parameters](/reference-en/desc?id=Описание-параметров-объектов).

| Category | Data |
| --- | --- |
| meta | name, popularity, duration\_ms, explicit, added\_at, played\_at |
| features | acousticness, danceability, energy, instrumentalness, liveness, loudness, speechiness, valence, tempo, key, mode, time\_signature, duration\_ms |
| artist | popularity, followers, name |
| album | popularity, name, release\_date |

?> When using the `features` category, any stream of tracks becomes more pleasant.

### Return :id=sort-return {docsify-ignore}

No return value. Modifies the input array.

### Examples :id=sort-examples {docsify-ignore}

1. Sort by decreasing artist popularity.

```js
Order.sort(tracks, 'artist.popularity', 'desc');
```

2. Sort by increasing energy.

```js
Order.sort(tracks, 'features.energy', 'asc');
```
