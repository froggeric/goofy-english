# Search

Search methods. Used by other functions. Only needed for individual solutions.

### [find\*](#find)

Returns an array with search results by keyword for types: playlist, track, album, artist.

Arguments
- (array) `keywords` - list of keywords, only strings. There will be a separate array in the results for each one.
- (number) `requestCount` - number of requests for **one** keyword. Up to 50 objects per request, if they exist. Maximum 40 requests. Default is 1.

Example 1 - Find 100 playlists by the word `rain`
> The best way will be the function [mineTracks](/reference-en/source?id=minetracks). Direct use of the Search module is needed for solutions that are not implemented by default. For example, when [importing tracks from FM radio](https://github.com/Chimildic/goofy/discussions/35).
> 
> 

```js
let keywords = ['rain'];
let playlists = Search.findPlaylists(keywords, 2);
```
### [getNoFound](#getnofound)

Returns an array with keywords and search type for which no results were found during the current script execution.

Arguments: none

```js
let noFound = Search.getNoFound();
// structure: { type: '', keyword: '', item: {} }
```
### [multisearch\*](#multisearch)

Returns the best match by keyword for a track/artist/album

Arguments
- (array) `items` - elements to iterate over
- (function) `parseNameMethod` - callback, called for each element. Must return a string that is the keyword for search.

Example 1 - When the array of elements has simple text
```js
let keywords = ['skillet', 'skydive'];
let artists = Search.multisearchArtists(keywords, (i) => i);
```
Example 2 - When the array of elements has complex structure
```js
let tracks = Search.multisearchTracks(items, (item) => {
    return `${item.artist} ${item.title}`.formatName();
});
```
