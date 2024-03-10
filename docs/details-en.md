# Principle of Operation

goofy is a constructor with a set of functions in JavaScript. They are used to create an algorithm that automates various tasks. For example, finding favorite tracks that haven't been listened to for a long time.

The execution of the algorithm takes place on the side of the Google Apps Script platform. This means that tasks are performed according to a schedule without your participation.

> When installing or changing the version, a single request is sent to Google Forms. To have an idea of how many users actively use goofy.

## Differences from Smarter Playlists

The main difference lies in the way the algorithm is composed. In Smarter Playlists, it's a visual language, a diagram. In the case of goofy, a programming language is used.

Here's an example of the following algorithm in Smarter Playlists: take tracks from two playlists, perform random sorting, save the first 50 tracks to a new playlist.

![Creating a playlist in Smarter Playlists](/img/SmarterPlaylistsExample1.png)

Now the same thing using goofy
```js
let tracks = Source.getTracks([
  { name: 'Daily Mix 1', id: '123' },
  { name: 'Daily Mix 2', id: '456' },
]);

Order.shuffle(tracks);

Playlist.saveAsNew({
  name: 'Personal Daily Mix',
  tracks: Selector.sliceFirst(tracks, 50),
});
```

## Listening History

goofy automatically tracks listening history. When the limit (60 thousand) is reached, new listens continue to be saved at the expense of deleting the oldest ones. Tracking begins immediately after setup is complete. Listenings that occurred before setup will not appear in the list. However, there is a way to add them manually if you have last.fm.

> You can [add](https://support.last.fm/t/how-to-add-scrobbles-history-from-spotify-to-last-fm/40038) your listening history, which was before creating an account in last.fm.

Spotify API has a function for getting listening history. But *maximum* 50 latest tracks that were listened to for more than 30 seconds. Podcasts and tracks played in private mode do not appear in the history.

goofy's ability to track history appears due to the Apps Script platform and its access to Google Drive. Every 15 minutes, goofy contacts Spotify and updates the contents of the file on the disk with new data.

Theoretically, you can increase the limit of history to 100 thousand. But there is a question of performance within the limits of the Apps Script platform, availability of disk space, and overall feasibility of such an array. Since in most cases such a history is needed for removing tracks from newly created playlists. Tasks for compiling tops can be easily solved by other means with Last.fm.

Last.fm itself can track Spotify listens (connection in [profile](https://www.last.fm/settings/applications)). But all of the above does not allow you to intercept fast skipping of tracks. There are two solutions for this:

- The first one is described in detail on the [forum](https://github.com/Chimildic/goofy/discussions/53).
- The second one is to disable Last.fm tracking and use third-party programs for PCs and smartphones that allow you to set an interval of several seconds after which the track is saved in history. For example, [Pano Scrobbler](https://4pda.to/forum/index.php?showtopic=887068).

In practice, Spotify API works unstably. A track after 30 seconds may return with a delay or be lost altogether. This is a problem on the side of Spotify that cannot be solved in any way. However, together with Last.fm this becomes completely insignificant.

If you often listen to downloaded tracks without an internet connection, use third-party scrobblers as in the example above. They allow you to save listens locally and remove dependence on offline mode and Spotify failures. If offline listening is a rare scenario, it's enough to connect through your profile.

## Limitations

> Some details may be unclear at first acquaintance

The library adheres to the limitations of the platforms. Below is a description of specific indicators and their impact based on reference information provided by the platforms.

### Apps Script {docsify-ignore}
- Execution time (6 minutes / single execution)

  - Maximum total duration of *one* script run. As a rule, light templates complete in a matter of seconds. Approaching a minute or several is possible in the case of large volumes of input and/or output data.
  - For example, the function [getFollowedTracks](/reference-en/source?id=getfollowedtracks) for the user [spotify](https://open.spotify.com/user/spotify) and the argument `owned` on average takes 4 minutes to complete. At the same time receiving 1.4 thousand playlists and 102 thousand tracks. After removing duplicates, there are 78 thousand left.
  - If you call [rangeTracks](/reference-en/filter?id=rangetracks) for 78 thousand, the limit of 6 minutes will be exceeded. But by pre-rejecting unsuitable tracks in advance, for example using [rangeDateRel](/reference-en/filter?id=rangedaterel), [match](/reference-en/filter?id=match) and others, you can significantly and quickly reduce the number of tracks.

- Number of requests (20 thousand / day)

  - As a rule, one request to Spotify is getting 50 playlists or 50 tracks. In some cases, it's 100.
  - The example above received 1.4 thousand playlists and 102 thousand tracks in 1 735 requests.
  - It takes 110 requests and 25 seconds to get 11 thousand tracks from a playlist. About the same amount of time is needed to create a playlist with that number of tracks.
  - It will take 200 requests to get 10 thousand favorite tracks.
  - In general, it's hard to imagine a function that would require 20 thousand requests due to the limitation of 6 minutes for execution. For this reason, you can say that it's impossible to bypass all playlists of robot users with thousands of playlists. But personal profiles or medium-sized authors can be bypassed.

- Execution time triggers (90 minutes / day)

   - Maximum total duration of trigger execution. The only way to reach the limit is to call a function 15 times for 6 minutes in one day. It's hard to imagine a task that would require this and be justified.

- Number of triggers (20 / user / script)
  
   - Roughly speaking, these are 20 playlists that are created according to completely *different* schedules.
   - In practice, several functions can be called from one other function, which allows you to create N playlists with one trigger. More details [here](/best-practices-en?id=Trigger%20economy).
   - Additionally, you can create another copy of the library and also get a quota for 20 triggers.
    
     > If you needed to create another copy, you can reuse CLIENT\_ID and CLIENT\_SECRET values and not create a new application on the Spotify side.

The remaining limitations of Apps Script do not apply to the library. They are related to email, tables, and other services. Or they are unattainable due to the limitations of Web API Spotify. More details [here](https://developers.google.com/apps-script/guides/services/quotas).

### Web API Spotify {docsify-ignore}
- Local files are ignored. [API does not allow](https://developer.spotify.com/documentation/general/guides/local-files-spotify-playlists/) to add such tracks to new playlists and they practically do not carry data for filtering, sorting.
  
  > It is not currently possible to add local files to playlists using the Web API, but they can be reordered or removed.

- Number of tracks
  - Up to 11 thousand when adding to a playlist.
  - When receiving from one playlist also 11 thousand.
  - Favorite tracks up to 20 thousand.
  - When filtering, sorting, selecting the number is unlimited. But within the quota of Apps Script.
- Number of playlists
  - Theoretically up to 11 thousand, but there won't be enough quota from Apps Script to receive tracks from them. The real value is in the range of 2 thousand. It depends on the total number of tracks.
- Number of requests
  - There is no exact number. If there are too many requests with a large volume in a short period of time, errors 500, 503 and similar may occur. They pass after a pause.

### Google Disk {docsify-ignore}
- The size of one text file is limited to - 50 mb. To reduce the volume, you can use the function [Cache.compressTracks](/reference/cache?id=compresstracks). Experimentally, it was possible to create a file with 100 thousand **compressed** tracks and fit into 50 mb.
