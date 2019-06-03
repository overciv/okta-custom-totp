# okta-custom-totp
This project is an example of using Okta APIs to create a custom TOTP factor on smartphone. It include a registration flow without any QR code or shared secret to type.

## Pre-requisites

### Basic Setup

Install Cordova
```bash
npm install -g cordova
```

Configure xcode
```bash
xcode-select --install
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
```

## Running this Example

### Build the app

To build this application, you first need to clone this repo:

```bash
git clone git@github.com:overciv/okta-custom-totp.git
cd okta-custom-totp
```

And also you need to add the plateform you wnats in the cordova configuration (ios or android, however I neved tested android build yet):
```bash
cordova platform add ios
```

The build the app:
```bash
cordova build ios
```

**Important** : if you get the following error:
```bash
Cannot find module '../../src/plugman/platforms/ios'
```
You need to modify/patch a file because one of the cordova plugins may not be compatible.
To fix it
 - modify "plugins/cordova-universal-links-plugin/hooks/lib/ios/xcodePreferences.js"
 - go to line 135
 - search/replace the above function:
```javascript
 function loadProjectFile() {
   var platform_ios;
   var projectFile;

   try {
     // try pre-5.0 cordova structure
     platform_ios = context.requireCordovaModule('cordova-lib/src/plugman/platforms')['ios'];
     projectFile = platform_ios.parseProjectFile(iosPlatformPath());
   } catch (e) {
     // let's try cordova 5.0 structure
     platform_ios = context.requireCordovaModule('cordova-lib/src/plugman/platforms/ios');
     projectFile = platform_ios.parseProjectFile(iosPlatformPath());
   }

   return projectFile;
```
- Replace it by the following updated function:

```javascript
 function loadProjectFile() {
   var platform_ios;
   var projectFile;
   try {
     // try pre-5.0 cordova structure
     platform_ios = context.requireCordovaModule('cordova-lib/src/plugman/platforms')['ios'];
     projectFile = platform_ios.parseProjectFile(iosPlatformPath());
   } catch (e) {
     try {
       // let's try cordova 5.0 structure
       platform_ios = context.requireCordovaModule('cordova-lib/src/plugman/platforms/ios');
       projectFile = platform_ios.parse(iosPlatformPath());
     } catch (e) {
       // try cordova 7.0 structure
       var iosPlatformApi = require(path.join(iosPlatformPath(), '/cordova/Api'));
       var projectFileApi = require(path.join(iosPlatformPath(), '/cordova/lib/projectFile.js'));
       var locations = (new iosPlatformApi()).locations;
       projectFile = projectFileApi.parse(locations);
     }
   }
   return projectFile;
 }
 ```
 
 Now you can build the app:
 ```bash
 cordova build ios
 ```
 
### Build the app
```bash
cordova run ios
```
cd okta-custom-totp
