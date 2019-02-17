<a href="https://github.com/Mrzhangxiaoduo/react-native-speech-recognizer/blob/master/README-CN.md">中文文档(Usage in Chinese)</a>
# react-native-speech-recognizer
A speech-recongintion library for React Native(Support Chinese and English),Chinese is dependent on <a href="http://yuyin.baidu.com/docs/asr/186">baidu API</a>.
# install
```
npm install react-native-speech-recognizer --save
```
# Linking
### Automatic  
```
react-native link react-native-speech-recognizer
```
### Manual  
  Make alterations to the following files in your project:
#### Android

1. Add following lines to `android/settings.gradle`
```
...
include ':react-native-speech-recognizer'
project(':react-native-speech-recognizer').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-speech-recognizer/android')
...
```
2. Add the compile line to dependencies in `android/app/build.gradle`
```
...
dependencies {
    ...
    compile project(':react-native-speech-recognizer')
    ...
}
```
3. Add import and link the package in `android/app/src/.../MainApplication.java`
```
import port com.zhangxiaoduo.rnsr.SpeechRecognitionPackage;  // <--- add import

public class MainApplication extends Application implements ReactApplication {
    // ...
    @Override
    protected List<ReactPackage> getPackages() {
        return Arrays.<ReactPackage>asList(
            new MainReactPackage(),
            // ...
            new SpeechRecognitionPackage()                      // <--- add package
        );
    }
```
#### IOS
1. In the XCode's "Project navigator", right click on your project's Libraries folder ➜ `Add Files to <...>`
2. Go to `node_modules` ➜ `react-native-speech-recognizer` ➜ `ios` ➜ select `SpeechRecognition.xcodeproj`
3. Add `SpeechRecognition.a` to `Build Phases -> Link Binary With Libraries`
# Permission
### Android
1. Add permission in `android/app/src/main/AndroidManifest.xml`
```
<uses-permission android:name="android.permission.RECORD_AUDIO"/>
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
```
2. Since Android M(6.0), user need to grant permission at runtime.  
Add following lines to `android/app/src/.../MainActivity.java`
```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    checkAndRequestPermissions();
}
private void checkAndRequestPermissions() {
      int sdkVersion = Build.VERSION.SDK_INT;
      if (sdkVersion >= Build.VERSION_CODES.M) {
          if (ActivityCompat.checkSelfPermission(this, Manifest.permission.RECORD_AUDIO)
                  != PackageManager.PERMISSION_GRANTED
                  || ActivityCompat.checkSelfPermission(this, Manifest.permission.INTERNET)
                  != PackageManager.PERMISSION_GRANTED
                  || ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_NETWORK_STATE)
                  != PackageManager.PERMISSION_GRANTED
                  || ActivityCompat.checkSelfPermission(this, Manifest.permission.READ_PHONE_STATE)
                  != PackageManager.PERMISSION_GRANTED) {
              ActivityCompat.requestPermissions(this,
                      new String[]{Manifest.permission.RECORD_AUDIO,
                              Manifest.permission.INTERNET,
                              Manifest.permission.ACCESS_NETWORK_STATE,
                              Manifest.permission.READ_PHONE_STATE
                      },
                      1);
          }
      }
  }
```
3. Add your `APP_ID`,`API_KEY`,`SECRET_KEY` in `android/app/src/main/AndroidManifest.xml`  
Click <a href="http://yuyin.baidu.com/">here</a> to get your own `APP_ID`,`API_KEY`,`SECRET_KEY`
```
<application
    ...
    <meta-data
        android:name="com.baidu.speech.APP_ID"
        android:value="10337431" />
    <meta-data
        android:name="com.baidu.speech.API_KEY"
        android:value="Ptivienw5Qx75uRnKalq7Eq2" />
   <meta-data
       android:name="com.baidu.speech.SECRET_KEY"
       android:value="bd5c689721bfa3530330c677e87303a6" />
    ...
</application>
```
### IOS
Add permission to your `Info.plist`
```
<dict>
  ...
  <key>NSSpeechRecognitionUsageDescription</key>
  <string>Your usage description here</string>
  <key>NSMicrophoneUsageDescription</key>
  <string>Your usage description here</string>
  ...
</dict>
```
# Usage
```
import SpeechRecognizer from 'react-native-speech-recognizer';

...

constructor(props) {
    super(props);
    this.state = {
        result: '';
    };
    SpeechRecognizer.init( result => this.setState({result}));
}

...

componentWillUnmount() {
    SpeechRecognizer.end();
}

...

render() {
//SpeechRecognizer.start('zh'); 'zh' or 'eng',default:'zh'
...
    <TouchableOpacity 
        onPressIn={()=> SpeechRecognizer.start('zh')} 
        onPressOut={()=> SpeechRecognizer.finish()}>
        ...
    </TouchableOpacity>
    <Text>{this.state.result}</Text>
...
}
```
