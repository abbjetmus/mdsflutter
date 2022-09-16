# mdsflutter

Flutter plugin for MDS (Movesense Device Service) that is used for communicating with Movesense devices.

### Additional steps for using the plugin

#### iOS

Install Movesense iOS library using CocoaPods like this in `ios/Podfile`:
  ```
  target 'Runner' do
    # Add :linkage => :static
    use_frameworks! :linkage => :static
    use_modular_headers!

    flutter_install_all_ios_pods File.dirname(File.realpath(__FILE__))

    # Add this line
    pod 'Movesense', :git => 'https://bitbucket.org/movesense/movesense-mobile-lib.git'
  end
  ```

#### Android

1. Download 'mdslib-x.x.x-release.aar' from movesense-mobile-lib repository and put it somewhere under 'android' folder of your app. Preferably create a new folder named 'android/libs' and put it there.

2. In 'build.gradle' of your android project, add the following lines (assuming .aar file is in android/libs):
```
allprojects {
    repositories {
        ...
        flatDir{
            dirs "$rootDir/libs"
        }
    }
}
```
## Usage

```dart
import 'package:mdsflutter/Mds.dart';

// Scan for new devices
Mds.startScan((name, address) {
    // Handle new scanned device
});

// Stop scanning
Mds.stopScan();

// Connect to a Movesense device
Mds.connect(address,
            (serial) { /* onConnected */ },
            () { /* onDisconnected */ },
            () { /* onConnectionError */ }
    );

// Disconnect from a device
Mds.disconnect(address);

// Make a GET, PUT, POST, DEL request
Mds.get(Mds.createRequestUri(serial, resourceUri),
      contract,
      (data, statusCode) { /* onSuccess */ },
      (error, statusCode) { /* onError */ }
    );

// Make a subscription request
int subscriptionId = Mds.subscribe(Mds.createSubscriptionUri(serial, resourceUri),
      contract,
      (data, statusCode) { /* onSuccess */ },
      (error, statusCode) { /* onError */ },
      (data) { /* onNotification */ },
      (error, statusCode) { /* onSubscriptionError */ }
    );

// Unsubscribe from a subscription
Mds.unsubscribe(subscriptionId);
```
For more information and detailed documentation, check Mds.dart file. Example
application further demonstrates the usage off the plugin.
