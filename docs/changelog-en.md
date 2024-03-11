# Changelog

The current version of the library is reflected in the `VERSION` constant in the `library` file.

To add your own functions or override existing ones, use [this instruction](https://github.com/Chimildic/goofy/discussions/18).

[Copy updated code](https://script.google.com/d/1DnC4H7yjqPV2unMZ_nmB-1bDSJT9wQUJ7Wq-ijF4Nc7Fl3qnbT0FkPSr/edit?usp=sharing).

## Version 1.8.3
- [12.09.22] Bug fixes for rare occurrences

## Version 1.8.2

- [#175](https://github.com/Chimildic/goofy/discussions/175) Error handling added when sending tracks to a playlist. The key indicating the position of track addition has been changed: instead of boolean `toEnd`, it is now a string `position`. By default, tracks are added to the end of the playlist.
- [#187](https://github.com/Chimildic/goofy/discussions/187) Re-reading file in case of unknown error with Google Drive
- Transliteration is applied when searching for the best match.
- Fixed an issue where tracks from `getSavedAlbumTracks` were not being written to cache.
- Fixed an issue where import was interrupted due to a zero search result from Spotify.
- Fixed: The first run of the Clerk task occurred at any time, ignoring the target value.

## Version 1.8.0

- Changed algorithm for finding the best match when importing to Spotify. This will reduce the number of "wrong" tracks when the desired one is not found in the Spotify database. The accuracy of comparison can be influenced by changing the `MIN_DICE_RATING` parameter in the _config_ file.
- Increased limits in various places.
- Country for `removeUnavailable` is taken from account data by default.
- Function `sendMusicRequest` has been removed.

## Version 1.7.0

- Add [Cheerio](https://github.com/Chimildic/goofy/discussions/91#discussioncomment-1931923) library.
- [#153](https://github.com/Chimildic/goofy/issues/153) Likes caching added. Set up [time zone](/tuning?id=Time-zone).
- [#159](https://github.com/Chimildic/goofy/issues/159) Added functions for parsing elements from last.fm tag pages: [getTracksByTag](/reference?id=gettracksbytag), [getArtistsByTag](/reference?id=getartistsbytag), [getAlbumsByTag](/reference?id=getalbumsbytag).
- (10.01.22) [#155](https://github.com/Chimildic/goofy/issues/155#issuecomment-1008871981) Changed algorithm for selecting releases due to a deficiency in artist names consisting of common words. The Cheerio library is required.

## Version 1.6.3
- Yandex module has been completely removed. Yandex stopped responding to requests. Many functions can be replaced by the [YaMuTools](https://github.com/Chimildic/YaMuTools) extension, but it does not have scheduled operation.
- [#155](https://github.com/Chimildic/goofy/issues/155) New function [Source.getReleasesByArtists](/reference-en/source?id=getreleasesbyartists). Collects artists' releases much faster than direct traversal used previously when combining different functions.
- [#162](https://github.com/Chimildic/goofy/issues/162) Language detection and filtering addon moved to main code [Filter.detectLanguage](/reference-en/filter?id=detectlanguage). When running for the first time from the editor, it will require permission to access Google Sheets.
- Most function documentation has been updated with a new writing style, and modules have been separated into separate files (old links to `/func` will no longer work).
- A link to reviews has been added to the site navigation.

## Version 1.6.2
- Reduced number of requests to Google Drive. Reading/writing operations for the same file within one script execution will take less time.
- Function `Cache.clear` has been removed. Use `Cache.write` to overwrite a file.

## Version 1.6.1
- Changed data structure on Disk to support multiple Spotify accounts on one Google account. When running the script for the first time, all files from the `Goofy Data` folder will be placed in a new folder with the user ID of the account being executed. Further runs will modify files from the corresponding user's folder.

If you have only one Spotify account, no action is required.

If you have already configured multiple Spotify accounts on one Google account, perform the following actions:
- Run any script for the first account
- Restore standard names to this account's files on Disk
- Move other accounts' files to the `Goofy Data` folder
- Repeat for each account
- Added ability to [create folders](/best-practices?id=File-path)

## Version 1.6.0
- Fixed errors in sending requests. The number of pauses between requests will increase, and the pause time will decrease (with a response status of 429).
- Adding tracks with [RecentTracks.appendTracks](/reference-en/recenttracks?id=appendtracks) no longer disrupts the algorithm for finding new listens in the history update trigger.
- [Cache.append](/reference-en/cache?id=append) no longer adds data to the original array (use [push](/reference-en/push?id=push)). Only adds to cache file. Returns total number of elements after addition.
- Added ability to disable console messages from library functions. Global setting is set in [config-file](/config). Temporary change via function `Admin.setLogLevelOnce`. Functions no longer have `isLogging` arguments.
- All functions from the `Lastfm` module use the value from the `config-file` if a username is not specified.

## Version 1.5.4
- Added attempt to re-read/write when an unknown error occurs with Google Drive service.
- Added functions: [Lastfm.rangeTags](/reference-en/lastfm?id=rangetags) and several Lastfm.getTop*ByTag.
- [addToQueue](/reference-en/player?id=addtoqueue) can add an array of tracks to the queue.
- [mineTracks](/reference-en/source?id=minetracks) has a parameter for skipping elements.
- [removeUnavailable](/reference-en/filter?id=removeunavailable) has an argument to disable log messages.

## Version 1.5.3
- New function [checkFavoriteTracks](/reference-en/library?id=checkfavoritetracks).
- Added parameter to [replaceWithSimilar](/reference-en/filter?id=replacewithsimilar) that allows deleting original artists from recommendations.

## Version 1.5.2
- New functions [getSavedAlbums](/reference-en/source?id=getsavedalbums), [followPlaylists](/reference-en/library?id=followPlaylists), [unfollowPlaylists](/reference-en/library?id=unfollowPlaylists).
- Function [getFollowedTracks](/reference-en/source?id=getfollowedtracks) received parameter `isFlat`.
- Removed functions `getTracks` and `getAlbums` from Yandex module. Since Yandex stopped responding to such requests from Apps Script.

## Version 1.5.1
- New module [Player](/reference-en/player). Need to [update access rights](/tuning?id=Update-access-rights).
- `getPlayback` moved to `Player`.
- Added function [Player.transferPlayback](/reference-en/player?id=transferplayback)
- `removeTracks` and `removeArtists` received mode for checking only the main artist of the track.
- Changed format of input parameters in [replaceWithSimilar](/reference-en/filter?id=replacewithsimilar).
- Fixed error with `getTop*`. Spotify does not allow the `locale` parameter in it. Write to the forum if you encounter an error `invalid request`.

## Version 1.5.0
- Consider all track artists in filter functions `remove*`, `match*`, `dedup*`.
- [getArtistsTracks](/reference-en/source?id=getartiststracks) and [getArtistsAlbums](/reference-en/source?id=getartistsalbums) received parameter `isFlat`, allowing to group the result by artist. Behavior remains unchanged by default, no need to change code.
- `rangeTracks` can filter by album type.
- `dedupTracks` has a new argument for controlling deviation in duration of tracks with identical names. More details [here](https://github.com/Chimildic/goofy/discussions/116).
- All get requests from `SpotifyRequest` contain the local parameter ([more info](https://github.com/Chimildic/goofy/discussions/79#discussioncomment-1019029)).

## Version 1.4.9
- Experiment with function [sendMusicRequest](/reference-en/search?id=sendmusicrequest)
- New parameter in [Lastfm.getCustomTop](/reference-en/lastfm?id=getcustomtop), new function [Lastfm.convertToSpotify](/reference-en/lastfm?id=converttospotify) and template with their use - [artists of one hit](/template?id=Исполнители-одного-хита).
- Function `getPlayingTrack` has been removed. Instead, added [getPlayback](/reference-en/player?id=getplayback). To use it, you need to [update access rights](/tuning?id=Update-access-rights).
- `doGet` adapted for opening `launch.html`, no longer need to replace function for [managing from phone](https://github.com/Chimildic/goofy/discussions/9)
- Modified function `replaceWithSimilar`. Should not create duplicates, requests recommendations with more data from the original track.
- Bugfix [Alisa #99](https://github.com/Chimildic/goofy/discussions/99#discussioncomment-973227)

## Version 1.4.8
- In [rangeTracks](/reference-en/filter?id=rangetracks), computed values `anger`, `happiness` and `sadness` have appeared. They are not available from Spotify, so they cannot be used when requesting recommendations.
- All match functions now check the main artist of the track in addition to album and title.
- Added parameters for minimum and maximum number of plays in [getCustomTop](/reference-en/lastfm?id=getcustomtop).
- Removed duplicate albums when requesting artists' albums. Bug Spotify, sometimes returns identical albums (for different countries).
- Added documentation for Search module
- Need to [update config file](https://github.com/Chimildic/goofy/blob/main/config.js):
  - Added locality parameter `RU` when requesting playlists. Due to [post](https://github.com/Chimildic/goofy/discussions/79#discussioncomment-814744): Cyrillic name was returned in Latin.
  - Limit of saved history to 20 thousand tracks moved to config, so as not to lose it when updating. There is an example of stable operation with 40 thousand.

## Version 1.4.7
- Experiment. When an unknown error occurs on Google's side during writing via `Cache.write`, a retry attempt will be made after a pause.
- [06.06.21]
  - [replaceWithSimilar](/reference-en/filter?id=replacewithsimilar) can take several arrays at once for replacement. If replacements are not found, the track is deleted.
- [20.05.21]
  - Experiment. Additional check when searching for the best match (string matching), [more info](https://github.com/Chimildic/goofy/discussions/64).
  - Order.sort can sort arrays of artists and albums.
  - New function [Playlist.removeTracks](/reference-en/playlist?id=removetracks).
  - [getTracks](/reference-en/source?id=gettracks) can select a limited number of tracks from playlists.

## Version 1.4.6
- New function _getPlayingTrack_. Requires [updating access rights](/tuning?id=Update-access-rights).
- When creating a playlist, you can specify a static cover through a direct link to it.

## Version 1.4.5
- Now [mineTracks](/reference-en/source?id=minetracks) can search for keywords in album names and track names themselves.
- Argument `playlistCount` in `mineTracks` **renamed** to `itemCount`.
- New function in Filter: [replaceWithSimilar](/reference-en/filter?id=replacewithsimilar).
- New function in Lastfm: [getSimilarArtists](/reference-en/lastfm?id=getsimilarartists).

## Version 1.4.4
- New filter [removeUnavailable](/reference-en/filter?id=removeunavailable).
- `Cache` can read/write files with `.txt` extension if explicitly specified in file name.
- Fixed logical error in `match` when selecting.

## Version 1.4.3
- Now [getCustomTop](/reference-en/lastfm?id=getcustomtop) can create a top by albums.
- When sorting by release date, tracks retain their original order within the album if they were initially in that order.
- Bugfixes

## Version 1.4.2
- Now [craftTracks](/reference-en/source?id=crafttracks) can accept static `seed_*` different from `key`.
- New function to Lastfm: [getCustomTop](/reference-en/lastfm?id=getcustomtop).
- New function to Selector: [pickYear](/reference-en/selector?id=pickyear).
- New function to Order: [separateYears](/reference-en/order?id=separateyears).
- Improvement for search. If the same element is present in the array multiple times (i.e., has the same keyword for searching), only one search request will be spent.
- Functions `Source`, related to playlists, add an object `origin` to each track, containing `name` and `id` of the source playlist.
- Attempt made to continue executing code after receiving exception `Exception: Address unavailable`, [more info](https://github.com/Chimildic/goofy/discussions/27).

## Version 1.4.1
- Accelerated execution time of function `craftTracks`.
- Found undocumented possibility in Spotify API. Function [getRecomTracks](/reference-en/source?id=getrecomtracks) supports key `popularity`. In connection with this, it **has been removed** from [craftTracks](/reference-en/source?id=crafttracks). Move it to the `query` parameter if you used it.
- Added ability to sort by album release date in `Order.sort`.
- Hidden functions that can be assigned a trigger from the list of functions: `displayAuthResult`, `updateRecentTracks`, `logProperties`.

## Version 1.4.0
- **Removed** function `Source.getRecentTracks`. Use `RecentTracks.get` or `Cache.read` for the desired history file.
- New functions to Source: [mineTracks](/reference-en/source?id=minetracks), [craftTracks](/reference-en/source?id=crafttracks).
- New function to RecentTracks: [appendTracks](/reference-en/recenttracks?id=appendtracks).
- Structure of `SpotifyRecentTracks` file updated to a regular array of tracks (like other history files). The update will happen automatically on the first trigger run. Until then, `Cache.read` will return the old structure.
- Added functions for saving and deleting albums from library in Library.

## Version 1.3.4
- New functions to Source: [getCategoryTracks](/reference-en/source?id=getcategorytracks), [getListCategory](/reference-en/source?id=getlistcategory).
- Appeared parameter [REQUESTS\_IN\_ROW](/config).
- When reading an empty file via Cache.read, an exception is thrown to prevent overwriting the file due to a bug on Google's side ([more info](https://github.com/Chimildic/goofy/discussions/26)).
- New function [Playlist.saveWithUpdate](/reference-en/playlist?id=savewithupdate).
- Functions match* can accept an array of artists. In the case of track arrays, comparison by track name and album (without artist). In the case of an array of artists, only their name is compared.
- Added templates from forum to documentation (Back in this day, Artist of the Day)

## Version 1.3.3
- Optimization of requests to Last.fm in accumulation mechanism. Search only for tracks that are new to listening history.
- New functions to Lastfm: [getSimilarTracks](/reference-en/lastfm?id=getsimilartracks), [getTopArtists](/reference-en/lastfm?id=gettopartists-1), [getTopAlbums](/reference-en/lastfm?id=gettopalbums).
- New function to Source: [getRelatedArtists](/reference-en/source?id=getrelatedartists), [getAlbumsTracks](/reference-en/source?id=getalbumstracks).
- New function to Yandex: _getAlbums_.
- Now [dedupArtists](/reference-en/filter?id=dedupartists) can delete duplicates from an array of artists.
- Now [removeArtists](/reference-en/filter?id=removeartists) can remove by an array of artists.
- Correction to group of methods match*.
- Special characters (,!@# etc.) are removed from string when searching and comparing.
- More informative messages in logs for listening history and search.

## Version 1.3.2
- Updated request sending mechanism. Many source functions have become faster due to asynchronous sending of N requests at once.
- Addition to mixin function. Now you can set the ratio for more than two arrays. More details in [mixinMulti](/reference-en/combiner?id=mixinmulti).
- New functions: [getTopArtits](/reference-en/lastfm?id=gettopartists), [getArtistsTopTracks](/reference-en/lastfm?id=getartiststoptracks).

## Version 1.3.1
- New functions for Cache module: [rename](/reference-en/cache?id=rename), [remove](/reference-en/cache?id=remove), [clear](/reference-en/cache?id=clear), [compressArtists](/reference-en/cache?id=compressArtists).
- Functions [getArtists](/reference-en/source?id=getartists), [getArtistsAlbums](/reference-en/source?id=getartistsalbums), [getAlbumTracks](/reference-en/source?id=getalbumtracks) became public.
- Function getTracksArtists **renamed** to getArtistsTracks.
- Repeated call of getSavedTracks in the same script sends new requests to Spotify, instead of returning previously received data. Use [sliceCopy](/reference-en/selector?id=slicecopy) to create a copy.
- Number of sent requests is now obtained via `CustomUrlFetchApp.getCountRequest`.
- Bugfix: spotify get with 404 interrupted script; lastfm with errors 500+ interrupted script.
- Bugfix: separateArtists did not split artists.
- Many small fixes.

## Version 1.3.0
- Updated installation instructions and video.
- Functions `removeTracks` and `removeArtists` have added argument `invert` (inversion).
- Error suppression from lastfm, so as not to interrupt script execution.
- Added anonymous tracking of library version distribution via Google Forms. Values of version and script ID are sent. To get an idea of the number of unique users.

## Version 1.2.0
- Added `parameters` for tracking history. Need to do [migration](https://4pda.ru/forum/index.php?act=findpost&pid=102495416&anchor=migrate_params).
- Increased limit of listening history from 10 to 20 thousand.
- Lastfm listening history tracks have added date of listening. Can be used with `rangeDateRel`.
- Mechanism for accumulating Lastfm listening history, if parameters are set. To get many and quickly instead of `Lastfm.getRecentTracks` with a small number of tracks due to limits.
- Get history with one function `RecentTracks.get`, regardless of `parameters`, including combined from two sources. In the combined, duplicates are removed, there is sorting from fresh to old listens.
