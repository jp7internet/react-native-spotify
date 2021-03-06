
# react-native-spotify

React Native module that exposes Spotify's iOS and Android SDKs to Javascript

## Getting started

`$ yarn add react-native-spotify https://github.com/jp7internet/react-native-spotify.git`

## Installation
`$ react-native link react-native-spotify`

### For Android

On android/app/build.gradle make sure it's **implementation** and not **compile**:

```
dependencies {
  implementation project(':react-native-spotify')
  ...
```

Also on android/app/build.gradle activate support for Java 8:

```
android {
    ...
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
...
```

On android/settings.gradle add:

```
include ':spotify-app-remote'
project(':spotify-app-remote').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-spotify/android/spotify-app-remote')
include ':spotify-auth'
project(':spotify-auth').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-spotify/android/spotify-auth')
```


### For iOS
Add to AppDelegate.m

```
#import <RNSpotifyBridge.h>

  - (BOOL)application:(UIApplication *)app
              openURL:(NSURL *)url
              options:(NSDictionary<UIApplicationOpenURLOptionsKey, id> *)options
  {
    return [RNSpotifyBridge application:app openURL:url options:options];
  }

  - (void)applicationDidBecomeActive:(UIApplication *)application
  {
    return [RNSpotifyBridge applicationDidBecomeActive:application];
  }

  - (void)applicationWillResignActive:(UIApplication *)application
  {
    return [RNSpotifyBridge applicationWillResignActive:application];
  }
  
  - (void)applicationWillTerminate:(UIApplication *)application
  {
    return [RNSpotifyBridge applicationWillTerminate:application];
  }
```

Add to Info.plist

```
<key>CFBundleURLTypes</key>
<array>
	<dict>
		<key>CFBundleURLName</key>
		<string>{BUNDLE_ID}</string>
		<key>CFBundleURLSchemes</key>
		<array>
			<string>{RedirectURI}</string>
		</array>
	</dict>
</array>
<key>LSApplicationQueriesSchemes</key>
<array>
	<string>spotify</string>
</array>
```

Add to the Project Configs

- General -> Linked Frameworks and Libraries
	- Click the plus sign (+), then click "Add Other..."
	- add ../node_modules/react-native-spotify/ios/Frameworks/SpotifyiOS.framework
- Build Settings
	- Framework Search Path
		- add: $(SRCROOT)/../node_modules/react-native-spotify/ios/Frameworks - recursive
	- Header Search Path
		- add $(SRCROOT)/../node_modules/react-native-spotify/ios - recursive

## Usage
```javascript
import Spotify from 'react-native-spotify';

Spotify.initialize({configs})
/*
 Readies Spotify
 Returns: Promise that resolves once Spotify is initialized or is rejected if there's an error
 params:
 configs: {
	clientID: String - required,
	redirectURI: String - required,
	tokenSwapURL: String - required,
	tokenRefreshURL: String - required,
	playURI: String - required,
 }
*/

Spotify.connect()
/*
 Connects to Spotify
 Returns: Promise that resolves to accessToken once a connection is stablished or is rejected if there's an error
*/

Spotify.disconnect()
/*
 Disconnects from Spotify
 Returns: void
*/

Spotify.setPlayState(play)
/*
 Play or pause a song
 Returns: Void
 params:
  play: Bool
*/

Spotify.nextSong()
/*
 Skips to the next song
 Returns: Void
*/

Spotify.previousSong()
/*
 Skips to the previous song
 Returns: Void
*/

Spotify.playURI(spotifyURI)
/*
 Plays from Spotify URI
 Returns: Void
 params:
  spotifyURI: String
*/

Spotify.updatePlayerState()
/*
 Updates Spotify player state
 Returns: Void
*/

Spotify.isInitializedAsync()
/*
 Checks if Spotify is initialized
 Returns: Promise that resolves to true if Spotify is initialized or false if it's not
*/

Spotify.isLoggedInAsync()
/*
 Checks if Spotify is connected
 Returns: Promise that resolves to true if Spotify is initialized or false if it's not
*/

Spotify.subscribe(callback)
/*
 Calls callback on Spotify state change event.
 Returns: void
 Callback params: {
  trackInfo: {
	 name: String - current song name,
	 album: String - current song album,
	 albumURI: String - current song album URI,
	 artist: String - current song artist,
	 artistURI: String - current song artist URI,
	 coverArt: String - current song cover art,
	},
	paused: Bool - is player currently paused?,
	next: Bool - can player skip to next song?,
	previous: Bool - can player skip to previous song?,
	accessToken: String - User's acess token
 }
*/

RNSpotify.unsubscribe()
/*
 Removes all Spotify state change event listeners.
*/


RNSpotify.webApiGet(endpoint, params)
/*
 Makes call to Spotify Web Api endpoint
 Returns: Promise that resolve or reject acording to endpoint results
 Params:
	endpoint: String - The requested Spotify Web Api endpoint
	params: Object - The params to be passed to the request
*/

RNSpotify.openInstallUrl(packageName)
/*
 Opens Spotify install page on the App/Play Store, and tracks the request.
 Returns: void
 Params:
 	packageName: String - The package name for tracking
*/
```
