# MSSpeechServiceXCFramework

## References
https://medium.com/strava-engineering/convert-a-universal-fat-framework-to-an-xcframework-39e33b7bd861


## Convert
download https://aka.ms/csspeech/iosbinary

run the following commands from the terminal
```
xcrun lipo -i MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech
mkdir -p iphoneos
mkdir -p iphonesimulator

cp -R MicrosoftCognitiveServicesSpeech.framework/ iphoneos/MicrosoftCognitiveServicesSpeech.framework
cp -R MicrosoftCognitiveServicesSpeech.framework/ iphonesimulator/MicrosoftCognitiveServicesSpeech.framework
xcrun lipo -i MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech


xcrun lipo -remove x86_64 ./iphoneos/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech -o ./iphoneos/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech
xcrun lipo -i iphoneos/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech

xcrun lipo -remove arm64 ./iphonesimulator/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech -o ./iphonesimulator/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech
xcrun lipo -i iphonesimulator/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech

xcodebuild -create-xcframework -framework iphoneos/MicrosoftCognitiveServicesSpeech.framework/ -framework iphonesimulator/MicrosoftCognitiveServicesSpeech.framework/ -output "MicrosoftCognitiveServicesSpeech.xcframework"

xcrun dwarfdump --uuid MicrosoftCognitiveServicesSpeech.framework.dSYM/Contents/Resources/DWARF/MicrosoftCognitiveServicesSpeech

xcrun dwarfdump --uuid MicrosoftCognitiveServicesSpeech.xcframework/ios-x86_64-simulator/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech
xcrun dwarfdump --uuid MicrosoftCognitiveServicesSpeech.xcframework/ios-arm64/MicrosoftCognitiveServicesSpeech.framework/MicrosoftCognitiveServicesSpeech
```

Zip the .xcframework folder and enjoy