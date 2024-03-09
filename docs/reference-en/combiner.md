# Combiner {docsify-ignore}

Methods for combining elements.

| Method | Result Type | Brief Description |
| --- | --- | --- |
| [alternate](/reference/combiner?id=alternate) | Array | Alternating elements of arrays. One step - one element of the array. |
| [mixin](/reference/combiner?id=mixin) | Array | Alternating elements of two arrays. One step - one or more elements from one array. |
| [mixinMulti](/reference/combiner?id=mixinmulti) | Array | Alternating elements of an unlimited number of arrays. One step - one or more elements from one array. |
| [push](/reference/combiner?id=push) | Array | Adding the elements of the second array to the end of the first array and so on. |

## alternate

Alternating elements of arrays. One step - one element of the array.

### Arguments :id=alternate-arguments

| Name | Type | Description |
| --- | --- | --- |
| `bound` | String | Boundary for alternation. With `min`, alternation ends when one of the sources is exhausted. With `max`, alternation continues until there are no untouched elements left. |
| `...arrays` | Arrays | Sources of elements to alternate. |

### Return :id=alternate-return

`resultArray` (array) - a new array in which the source elements are interleaved.

### Examples :id=alternate-examples

1. Interleave elements from three arrays.

```js
let firstArray = [1, 3, 5];
let secondeArray = [2, 4, 6, 8, 10];
let thirdArray = [100, 200, 300];
let resultArray = Combiner.alternate('max', firstArray, secondeArray, thirdArray);
// result: 1, 2, 100, 3, 4, 200, 5, 6, 300, 8, 10
```

2. Interleave the top tracks for the month and favorite tracks.

```js
let topTracks = Source.getTopTracks('short'); // assume there are 50 tracks
let savedTracks = Source.getSavedTracks(20); //assume there are 20 tracks
let resultArray = Combiner.alternate('min', topTracks, savedTracks);
// the result contains 40 tracks
```

## mixin

Alternating elements of two arrays. One step - one or more elements from one array. Includes a call to [mixinMulti](/reference/combiner?id=mixinmulti).

### Arguments :id=mixin-arguments

| Name | Type | Description |
| --- | --- | --- |
| `xArray` | Array | First source. |
| `yArray` | Array | Second source. |
| `xRow` | Number | Number of consecutive elements from the first source. |
| `yRow` | Number | Number of consecutive elements from the second source. |
| `toLimitOn` | Boolean | Elements are alternated until the proportion can be maintained. If `true`, excess elements are not included in the result. If `false`, they are added to the end of the result. Default is `false`. |

### Return :id=mixin-return

`resultArray` (array) - a new array in which the elements of two sources are interleaved in the specified proportion.

### Examples :id=mixin-examples

1. Alternate tracks from playlists and favorite tracks in a ratio of 5 to 1, discarding excess.

```js
let tracks = Source.getTracks(playlistArray);
let savedTracks = Source.getSavedTracks();
let resultArray = Combiner.mixin(tracks, savedTracks, 5, 1, true);
```

## mixinMulti

Alternating elements of an unlimited number of arrays. One step - one or more elements from one array.

### Arguments :id=mixinmulti-arguments

| Name | Type | Description |
| --- | --- | --- |
| `params` | Object | Parameters for alternation. |

#### Alternation parameters :id=mixinmulti-params

| Name | Type | Description |
| --- | --- | --- |
| `source` | Array | Source arrays. |
| `inRow` | Array | Elements that set the number of consecutive elements for each source. |
| `toLimitOn` | Boolean | Elements are alternated until the proportion can be maintained. If `true`, excess elements are not included in the result. If `false`, they are added to the end of the result. Default is `false`. With `toLimitOn = true`, the first iteration checks the number of elements. If there are fewer elements than specified by the ratio, an empty array will be returned. |

### Return :id=mixinmulti-return

`resultArray` (array) - a new array in which the source elements are interleaved in the specified proportion.

### Examples :id=mixinmulti-examples

1. Alternate elements in a ratio of 1:1:1, keeping all elements.

```js
let x = [1, 2, 3, 4, 5];
let y = [10, 20, 30, 40];
let z = [100, 200, 300];
let result = Combiner.mixinMulti({
    source: [x, y, z],
    inRow: [1, 1, 1],
});
// 1, 10, 100, 2, 20, 200, 3, 30, 300, 4, 40, 5
```

2. Alternate elements in a ratio of 2:4:2 as long as the sequence can be maintained.

```js
let x = [1, 2, 3, 4, 5];
let y = [10, 20, 30, 40];
let z = [100, 200, 300];
let result = Combiner.mixinMulti({
    toLimitOn: true,
    source: [x, y, z],
    inRow: [2, 4, 2],
});
// 1, 2, 10, 20, 30, 40, 100, 200
```

3. Alternate recommendations, favorite tracks, and listening history in a ratio of 4:1:1 as long as the sequence can be maintained.

```js
let recom = Source.getRecomTracks();
let saved = Source.getSavedTracks();
let recent = RecentTracks.get();
let tracks = Combiner.mixinMulti({
    toLimitOn: true,
    source: [recom, saved, recent],
    inRow: [4, 1, 1],
});
```

## push

Add the elements of the second array to the end of the first array and so on.

### Arguments :id=push-arguments

| Name | Type | Description |
| --- | --- | --- |
| `sourceArray` | Array | First array. Elements from subsequent arrays are added to it. |
| `...additionalArray` | Arrays | Subsequent arrays to be added to the previous one. |

### Return :id=push-return

`sourceArray` (array) - the original first array after adding elements from other arrays.

### Examples :id=push-examples

1. Add elements of the second array to the end of the first array.

```js
let firstArray = Source.getTracks(playlistArray); // assume there are 20 tracks
let secondeArray = Source.getSavedTracks(); // assume there are 40 tracks
Combiner.push(firstArray, secondeArray);
// now in firstArray there are 60 tracks
```

2. Add elements of two other arrays to the first array.

```js
let firstArray = Source.getTracks(playlistArray); // assume there are 25 tracks
let secondeArray = Source.getSavedTracks(); // assume there are 100 tracks
let thirdArray = Source.getPlaylistTracks(); // assume there are 20 tracks
Combiner.push(firstArray, secondeArray, thirdArray);
// now in firstArray there are 145 tracks
```
