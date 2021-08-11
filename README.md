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

xcodebuild OTHER_CFLAGS="-fembed-bitcode" -create-xcframework -framework iphoneos/MicrosoftCognitiveServicesSpeech.framework/ -debug-symbols /Users/tomasgreen/Downloads/MSSpeech2/MicrosoftCognitiveServicesSpeech.framework.dSYM -framework iphonesimulator/MicrosoftCognitiveServicesSpeech.framework/ -debug-symbols /Users/tomasgreen/Downloads/MSSpeech2/MicrosoftCognitiveServicesSpeech.framework.dSYM -output "MicrosoftCognitiveServicesSpeech.xcframework" 

xcodebuild BITCODE_GENERATION_MODE=bitcode -create-xcframework -framework iphoneos/MicrosoftCognitiveServicesSpeech.framework/ -debug-symbols /Users/tomasgreen/Downloads/MSSpeech2/MicrosoftCognitiveServicesSpeech.framework.dSYM -framework iphonesimulator/MicrosoftCognitiveServicesSpeech.framework/ -debug-symbols /Users/tomasgreen/Downloads/MSSpeech2/MicrosoftCognitiveServicesSpeech.framework.dSYM -output "MicrosoftCognitiveServicesSpeech.xcframework" 


xcodebuild create-xcframework fembed-bitcode

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

fastlane create_xcframework(frameworks: ["MicrosoftCognitiveServicesSpeech.framework"], output: "MicrosoftCognitiveServicesSpeech.xcframework")

fastlane run create_xcframework frameworks:"MicrosoftCognitiveServicesSpeech.framework" output:"MicrosoftCognitiveServicesSpeech.xcframework"


fastlane run create_xcframework frameworks:"iphonesimulator/MicrosoftCognitiveServicesSpeech.framework" output:"MicrosoftCognitiveServicesSpeech.xcframework"


## OBSERVE
Using bitcode when uploading to app store won't work anymore, Not sure why

## References
- https://medium.com/strava-engineering/convert-a-universal-fat-framework-to-an-xcframework-39e33b7bd861
- https://developer.apple.com/documentation/swift_packages/distributing_binary_frameworks_as_swift_packages
- https://stackoverflow.com/questions/44060642/invalid-bundle-app-store-rejection
- https://developer.apple.com/forums/thread/662247
- https://forums.swift.org/t/what-are-the-limitations-to-what-can-be-bundled-in-a-binary-target/38877
- https://github.com/bielikb/xcframeworks
- https://stackoverflow.com/questions/41322186/is-there-any-dsym-file-for-framework-besides-app
- https://developer.apple.com/forums/thread/655768