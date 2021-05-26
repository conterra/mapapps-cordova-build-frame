# map.apps Cordova builds

This documentation explains how to build native hybrid apps from map.apps using [Apache Cordova](https://cordova.apache.org/).

To create a native hybrid app based on a map.apps app using Apache Cordova, the following steps are required:

1. Setup Cordova build environment
2. Export map.apps app
3. Execute Cordova build

## Setup Cordova build environment

To setup the Cordava build environment, refer to the [Apache Cordova documentation](https://cordova.apache.org/docs/en/latest/guide/cli/index.html).

[Node.js](https://nodejs.org/en/) is required to run Cordova.
Depending on the app's target platform there might be additional requirements.

For example, building apps for iOS requires a build system set up on macOS.
Also, every platform needs its own SDK, e.g. to build apps for Android, [Android Studio](https://developer.android.com/studio/index.html) has to be installed.

To ensure that map.apps apps can be used, the following minimal versions have to be used:

* Cordova CLI: 10.0.0
* [Android platform](https://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html): 9.0.0
* [iOS platform](https://cordova.apache.org/docs/en/latest/guide/platforms/ios/index.html): 6.1.0
* [Windows platform](https://cordova.apache.org/docs/en/latest/guide/platforms/windows/index.html): 7.0.1

### Bootstrap Cordova setup

Install Cordova
```
$ npm install -g cordova
```

Create project:
```
$ cordova create mapapps de.conterra.mapapps map.apps
```

Add required platform:
```
$ cordova platform add <ios, android, windows>
```

Check requirements:
```
$ cordova requirements
```

### Install Cordova plugins

map.apps requires the following [Cordova plugins](https://cordova.apache.org/docs/en/latest/guide/hybrid/plugins/index.html) to be installed into the project:
[file](https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-file/index.html), [geolocation](https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-geolocation/index.html), [whitelist](https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-whitelist/index.html).

Add the required plugins:
```
$ cordova plugin add cordova-plugin-file
$ cordova plugin add cordova-plugin-geolocation
$ cordova plugin add cordova-plugin-whitelist
```

For these plugins, some configuration is needed inside the `config.xml` file.

Configuration for *cordova-plugin-file*:
```xml
<access origin="*" />
```

Configuration for *cordova-plugin-whitelist*:
```xml
<allow-intent href="http://*/*" />
<allow-intent href="https://*/*" />
<allow-intent href="tel:*" />
<allow-intent href="sms:*" />
<allow-intent href="mailto:*" />
<allow-intent href="geo:*" />
```

## Export map.apps app

An app that should be converted into a native hybrid app has to be able to be executed without the map.apps bundle registry.
Thus, all resources of an app (JavaScript, CSS, HTML, images, ...) have to be provided in a certain folder structure.

Execute **Export for Native App** on an app in map.apps Manager to download it as a ZIP file containing all the required resources.

Extract the ZIP file into the `www` folder of the Cordova project, overwriting any existing files.

### Remove pre-compressed files

A native exported app contains pre-compressed `.gz` files.
Cordova does not need them and even lead to an unsuccessful build.
You can safely delete all these files in the `www` directory:

On a Unix-based system:
```
$ find www/ -name \*.gz -exec rm {} \;
```

On a Windows system:
```
$ for /R www/ %A in (*.gz) do ( del /f /q "%A" )
```

## Execute Cordova build

Run the build:
```
$ cordova build
    # or:
$ cordova build <ios, android, windows>
```

If the project needs to be cleaned, run:
```
$ cordova clean
    # or:
$ cordoa clean <ios, android, windows>
```

To run and test the project on a *plugged-in device* run:
```
$ cordova run <ios, android, windows>
```

To run and test the project on a *emulator* run:
```
$ cordova emulate <ios, android, windows>
```
