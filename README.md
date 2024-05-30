# MapBox Load Map and get GPS access to move the marker on the map
Implementation of MapBox with GPS access and Marker-Map focus

Based on the links below:
https://www.youtube.com/watch?v=6MGUNBum2jo&list=PLmnIR323OXFiTurAisS6JfeGn3YeLL70m
and
https://docs.mapbox.com/android/maps/guides/migrate-to-v10/

1) MAPBOX INSTALLATION
https://docs.mapbox.com/android/maps/guides/install/

2) CREATE TOKEN ON MAPBOX FOR THIS APPLICATION
Give name and add (on secret scopes) DOWNLOADS:READ
a) secret token
b) public token

3) CONFIGURE ANDROID PERMISSIONS
<manifest ... >
  <!-- Always include this permission -->
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />

  <!-- Include only if your app benefits from precise location access. -->
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
</manifest>

4) Configure your secret token
To avoid exposing your secret token, add it as an environment variable:
Find or create a gradle.properties file in your Gradle user home folder. The folder is located at «USER_HOME»/.gradle. Once you have found or created the file, its path should be «USER_HOME»/.gradle/gradle.properties. More details about Gradle properties in the official Gradle documentation.
Add your secret token your gradle.properties file:
MAPBOX_DOWNLOADS_TOKEN=YOUR_SECRET_MAPBOX_ACCESS_TOKEN

5) Configure your public token
Go to res/values/ and create a new xml file called developer-config.xml
inside add the following:

<?xml version="1.0" encoding="utf-8"?>
<resources xmlns:tools="http://schemas.android.com/tools">
    <string name="mapbox_access_token" translatable="false" tools:ignore="UnusedResources">YOUR_PUBLIC_MAPBOX_ACCESS_TOKEN</string>
</resources>

6) Add the MapBox dependency
Mapbox provides the Maps SDK via Maven.
To add the Mapbox Maps SDK as a dependency, you will need to configure your build to download the Maps SDK from Mapbox maven repository directly. This requires a valid username and password (see section Configure credentials).
Open your project in Android Studio.
Declare the Mapbox remote repository.
Access to the Mapbox repository requires a valid username and password. In the previous section, you added the password to your gradle.properties file (see section Configure credentials). The username field should always be "mapbox".
By default, new Android Studio projects specify Maven repositories locations in the project's settings.gradle file:
Open your top level settings.gradle.kts file and add a new maven {...} definition inside the dependencyResolutionManagement.repositories:

...
google()
mavenCentral()
....
        // Mapbox Maven repository
        maven {
            url = uri("https://api.mapbox.com/downloads/v2/releases/maven")
            // Do not change the username below. It should always be "mapbox" (not your username).
            credentials.username = "mapbox"
            // Use the secret token stored in gradle.properties as the password
            credentials.password = providers.gradleProperty("MAPBOX_DOWNLOADS_TOKEN").get()
            authentication { basic(BasicAuthentication) }
        }

Open up your module-level (for example app/build.gradle.kts) Gradle configuration file and make sure that your project's minSdk is 21 or higher:

Add the Mapbox SDK for Android dependency in your module-level (for example app/build.gradle.kts) Gradle configuration file:
dependencies {
  ...
  implementation 'com.mapbox.maps:android:11.3.1'
  // If you're using compose also add the compose extension
  // implementation 'com.mapbox.extension:maps-compose:11.3.1'
  ...
}

7) Because you've edited your Gradle files, run Sync Project with Gradle Files in Android Studio.

