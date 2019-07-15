# expo-run-emulator-and-phone

### Expo Emulator 실행방법 & 스마트폰에서 직접 질행하기 \(OSX\)

## 아이폰

Xcode 설치하고 공식문서 따라하면 됨

## 안드로이드

### 1. 경로 추가

공식문서 그대로 따라하면 되는데 `~/.bash_profile`에 아래 경로를 추가해줘야 함  
SDK Path는 Android Studio 설정에 나와있는 SDK 경로 적으면 됨

```text
touch .bash_profile
open -e .bash_profile
source ~/.bash_profile

# adb 버전 체크
adb version
# Setting PATH for Android Studio Emulator
export ANDROID_SDK="/Users/mypath/Library/Android/sdk"
export PATH="/Users/mypath/Library/Android/sdk/platform-tools:${PATH}"
```

### 2. adb 버전 체크

```text
# 제대로 되어있는지 확인
adb version

# 아래와 같이 뜨면 제대로 된 것임
Android Debug Bridge version 1.0.41
Version 29.0.0-5611747
Installed as /Users/doctorkitchen/Library/Android/sdk/platform-tools/adb
```

### 안드로이드 에뮬레이터 실행

1. Tools-Android-&gt;AVD Manager 클릭
2. create virtual device 클릭해 가상머신 생성

![&#xC774;&#xBBF8;&#xC9C0;](https://i.imgur.com/fqmi0yS.png) 만약 Tools에서 AVD Manager가 보이지 않는 경우, 안드로이드 스튜디오 우측 상단의 위 버튼을 클릭하면 됨.

### 안드로이드 실행시 주의사항

에뮬레이터 실행시 꼭 안드로이드 스튜디오에서 에뮬레이터를 켜놓고 **Run on Android device/emulator** 버튼을 눌러야 함. 그러지 않으면 에러 뿜어냄.

## 스마트폰에서 직접 실행

귀찮게 iOS용 Expo 애플리케이션은 QR코드를 찍을 수가 없음. 따라서 Connection을 LAN으로 설정해두고 `exp://192...`를 사파리에 입력, Expo 애플리케이션을 실행하면 된다. 크롬은 구글 검색으로 이동하니 꼭 사파리에서 입력해야 한다.

## refs

* [https://docs.expo.io/versions/v33.0.0/introduction/installation/](https://docs.expo.io/versions/v33.0.0/introduction/installation/)
* [https://docs.expo.io/versions/v33.0.0/workflow/ios-simulator/](https://docs.expo.io/versions/v33.0.0/workflow/ios-simulator/)

