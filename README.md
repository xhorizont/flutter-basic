# flutter-basic

Flutter basic tutorial

# Povezave

- https://code.visualstudio.com/ - VS Code
- https://developer.android.com/studio - Android Studio
- https://github.com/git-for-windows/git/releases/download/v2.40.1.windows.1/Git-2.40.1-64-bit.exe - Git
- https://docs.flutter.dev/ - Dokumentacija
- https://docs.flutter.dev/get-started/install - Namestitev
  - https://docs.flutter.dev/get-started/install/windows
  - https://docs.flutter.dev/get-started/install/macos
  - https://docs.flutter.dev/get-started/install/linux

# Preverjanje Flutter Doctor

```bash
PS C:\Users\johnny\Desktop\DEV\xhorizont\flutter-basic\sample-1> flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[√] Flutter (Channel stable, 3.10.0, on Microsoft Windows [Version 10.0.19042.1706], locale sl-SI)
[√] Windows Version (Installed version of Windows is version 10 or higher)
[!] Android toolchain - develop for Android devices (Android SDK version 33.0.2)
    X cmdline-tools component is missing
      Run `path/to/sdkmanager --install "cmdline-tools;latest"`
      See https://developer.android.com/studio/command-line for more details.
    X Android license status unknown.
      Run `flutter doctor --android-licenses` to accept the SDK licenses.
      See https://flutter.dev/docs/get-started/install/windows#android-setup for more details.
[√] Chrome - develop for the web
[!] Visual Studio - develop for Windows (Visual Studio Community 2022 17.5.5)
    X The current Visual Studio installation is incomplete. Please reinstall Visual Studio.
[√] Android Studio (version 2022.2)
[√] VS Code (version 1.78.2)
[√] Connected device (3 available)
[√] Network resources
```

# Ustvarimo prvo aplikacijo

```bash
flutter create --org com.myname my_app_name
```

# Preverimo delovanje v simulatorju

---

# Onemogočimo Debug Banner

```terminal
MaterialApp(
  debugShowCheckedModeBanner: false,

  home: Scaffold(
    appBar: AppBar(
      title: const Text('Home'),
    ),
  ),  
);
```
# PRAVA APLIKACIJA
# Dodamo dostop do interneta

/android/app/src/main/AndroidManifest. Xml


```javascript
<manifest xmlns:android...>
 
 <uses-permission android:name="android.permission.INTERNET" />
 <application ...
</manifest>
```

# Dodamo webview_flutter v pubspec.yaml

https://pub.dev/packages/flutter_inappwebview

```javascript
 $ flutter pub add flutter_inappwebview
```
ali v pubspec.yaml

```javascript
dependencies:
  flutter_inappwebview: ^5.7.2+3
```
Uporaba v kodi
```javascript
import 'package:flutter_inappwebview/flutter_inappwebview.dart';
```

# Pobrišemo vsebino main.dart in prilepimo

```javascript
import 'package:flutter/material.dart';
import 'mywebsite.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Test',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const MyHomePage(),
    );
  }
}

class MyHomePage extends StatelessWidget {
  const MyHomePage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Naša super aplikacija"),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.push(context,
                MaterialPageRoute(builder: (context) => const MyWebsite()));
          },
          child: const Text('Naprej'),
        ),
      ),
    );
  }
}

```
# V mapi /lib ustvarimo datoteko mywebsite.dart in prilepimo

```javascript
import 'package:flutter/material.dart';
import 'package:flutter_inappwebview/flutter_inappwebview.dart';

class MyWebsite extends StatefulWidget {
  const MyWebsite({Key? key}) : super(key: key);
  @override
  State<MyWebsite> createState() => _MyWebsiteState();
}

class _MyWebsiteState extends State<MyWebsite> {
  double _progress = 0;
  late InAppWebViewController inAppWebViewController;
  @override
  Widget build(BuildContext context) {
    return WillPopScope(
      onWillPop: () async {
        var isLastPage = await inAppWebViewController.canGoBack();
        if (isLastPage) {
          inAppWebViewController.goBack();
          return false;
        }
        return true;
      },
      child: SafeArea(
        child: Scaffold(
          body: Stack(
            children: [
              InAppWebView(
                initialUrlRequest:
                    URLRequest(url: Uri.parse("https://telekom.si/")),
                onWebViewCreated: (InAppWebViewController controller) {
                  inAppWebViewController = controller;
                },
                onProgressChanged:
                    (InAppWebViewController controller, int progress) {
                  setState(() {
                    _progress = progress / 100;
                  });
                },
              ),
              _progress < 1
                  ? LinearProgressIndicator(
                      value: _progress,
                    )
                  : const SizedBox()
            ],
          ),
        ),
      ),
    );
  }
}

```
# V android/app/build.gradle spremenimo

```javascript
minSdkVersion flutter.minSdkVersion
```
v 

```javascript
minSdkVersion 17
```

# Spremenimo ime pod ikono

android/app/src/main/AndroidManifest.xml

// staro
<application
     android:label="sampletwo"
     android:icon="@mipmap/ic_launcher">
// novo
<application
     android:label="Super App"
     android:icon="@mipmap/ic_launcher">