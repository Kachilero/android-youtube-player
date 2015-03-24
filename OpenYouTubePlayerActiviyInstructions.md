# OpenYouTubeActivity Usage Instructions #

There is decent support in Android for media, including video.  If you're looking for an Activity that does the following, then the OpenYouTubePlayerActivity may be right for you:

  * Stream media.  You don't want to include the video as a resource in your project, for obvious reasons.  Furthermore, you want the flexibility to be able to potentially make changes to the video after deployment of the application.
  * Support low- and high-bandwidth scenarios.  You don't want the display of the video to require WiFi or 3G connectivity.  If your users don't have 3G locally, it can be a huge pain to try and stream HD video.
  * Embedded workflow.  You don't want to lose control of your user when displaying a video.  For example, registering an Intent that fires up the YouTube application can leave the user in the YouTube app.  This is an effort to support YouTube's viral features.  You want the user to return immediately to your application after the video completes.
  * Support for video updates.  If you discovered after deployment that your fly was unzipped in the video and wanted to update content, you need to be able to upload a zipped-up version.
  * No backend server work.  You don't want to write a new server backend to do any of this.

If you answered yes to these questions, then YouTube is a likely candidate for hosting your videos, and the OpenYouTubePlayerActivity could very well be the mechanism that you're looking for to render those videos in your app.

Figuring out how to interact with YouTube, retrieve tokens properly, calculate download URLs and other details is a difficult task.  It took plenty of time, and fiddling, and playing, and hair pulling.  What's more, the solution is a moving target as Google is constantly changing the YouTube infrastructure to support new features and to upgrade security.

The Android activity in this project is meant to be included in your Android project.  Simply jar up the classes and add the jar to your project in Eclipse (or whatever development environment that you're using).  The activity itself is easily invoked using a simple Intent, and can load a video from YouTube identified in one of the following two ways:

  * Video Id.  By including a URL in the Intent data of the form ytv://videoid, one can play a specific video.
  * Playlist Id.  By including a URL in the Intent data of the form ytpl://playlistid, one can play the latest video added to a YouTube playlist.  This is great for adding new video content that you want your users to see later on.  Simply add a new video to the YouTube playlist, and the next time that the user invokes the activity pointing at that playlist, they will see the new video.

OpenYouTubePlayerActivity takes care of doing all of the tricky YouTube token negotiation.  It also uses a simple algorithm for detecting bandwidth available at the client, and adjusts the quality of the downloaded video appropriately.

Invoking the activity is easy.  Simply use a code snippet similar to the following:

```
Intent lVideoIntent = new Intent(null, Uri.parse("ytpl://"+YOUTUBE_PLAYLIST_ID), this, OpenYouTubePlayerActivity.class);
startActivity(lVideoIntent);
```

There is other coniguration that you can do with respect to the messages that get displayed to the user, but those are well documented in the Javadoc.

Finally, make sure that you add the following permissions to your manifest:

```
uses-permission android:name="android.permission.INTERNET" 
uses-permission android:name="android.permission.ACCESS_WIFI_STATE" 
```

There have been some cool observations by other smart people on how to improve the usage of this Activity.
  * See [this blog post](http://it-ride.blogspot.com/2010/04/android-youtube-intent.html) for how to use the stock YouTube player if available, but fall back to this Activity if it's not.
  * Scott noticed that you need to add the following to your manifest entry for the Activity:

```
    android:configChanges="orientation" 
```