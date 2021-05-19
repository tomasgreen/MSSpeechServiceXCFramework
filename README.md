# MSSpeechServiceXCFramework

## Convert
download https://aka.ms/csspeech/iosbinary

run the following commands from the terminal
```
mkdir -p iphoneos
mkdir -p iphonesimulator

cp -R MicrosoftCognitiveServicesSpeech.framework/ iphoneos/MicrosoftCognitiveServicesSpeech.framework
cp -R MicrosoftCognitiveServicesSpeech.framework/ iphonesimulator/MicrosoftCognitiveServicesSpeech.framework

xcrun lipo -remove x86_64 ./iphoneos/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech -o ./iphoneos/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech
xcrun lipo -remove arm64 ./iphonesimulator/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech -o ./iphonesimulator/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech

xcodebuild -create-xcframework -framework iphoneos/MicrosoftCognitiveServicesSpeech.framework/ -framework iphonesimulator/MicrosoftCognitiveServicesSpeech.framework/ -output "MicrosoftCognitiveServicesSpeech.xcframework"

xcrun dwarfdump --uuid MicrosoftCognitiveServicesSpeech.framework.dSYM/Contents/Resources/DWARF/MicrosoftCognitiveServicesSpeech

xcrun dwarfdump --uuid MicrosoftCognitiveServicesSpeech.xcframework/ios-x86_64-simulator/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech

xcrun dwarfdump --uuid MicrosoftCognitiveServicesSpeech.xcframework/ios-arm64/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech
```

Zip the .xcframework folder and upload it to a webserver.

## SPM
Create a package that you can use as a dependency in other packages (or your app).

```swift
import PackageDescription

let package = Package(
    name: "MicrosoftCognitiveServicesSpeech",
    platforms: [.macOS(.v10_15), .iOS(.v13), .tvOS(.v13)],
    products: [
        .library(
            name: "MicrosoftCognitiveServicesSpeech",
            targets: ["MicrosoftCognitiveServicesSpeech"])
    ],
    targets: [
        .binaryTarget(
            name: "MicrosoftCognitiveServicesSpeech",
            url: "https://www.example.com/MicrosoftCognitiveServicesSpeech.xcframework.zip",
            checksum: "SHA 265 zip checksum"
        )
    ]
)
```

## OBSERVE
Using bitcode when uploading to app store won't work anymore, Not sure why

## References
- https://medium.com/strava-engineering/convert-a-universal-fat-framework-to-an-xcframework-39e33b7bd861
- https://developer.apple.com/documentation/swift_packages/distributing_binary_frameworks_as_swift_packages
- https://stackoverflow.com/questions/44060642/invalid-bundle-app-store-rejection
- https://developer.apple.com/forums/thread/662247