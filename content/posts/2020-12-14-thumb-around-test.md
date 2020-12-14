---
title: "Detox로 React native app E2E 테스트"
path: post2
slug: getting-started-with-gridsome-and-bleda
description: "detox로 E2E 테스트"
date: 2020-12-14 17:52:00
author: hyeon namkung
tags:
    - 썸어라운드
    - 개인 프로젝트
cover: ""
fullscreen: true
---

## 📱**썸어라운드 - 운동 기록**
미리 운동을 구성하고 구성된 운동을 불러와 운동을 기록하는 앱

- [IOS](https://apps.apple.com/us/app/%EC%8D%B8%EC%96%B4%EB%9D%BC%EC%9A%B4%EB%93%9C-%EC%9A%B4%EB%8F%99-%EA%B8%B0%EB%A1%9D/id1538255500)
- Android: 미출시
<div class="bg-orange-100 border-l-4 border-orange-500 text-orange-900 leading-normal p-4 md:mx-6 mb-6" role="alert">
 Android용 폰이 없어 실제 사용하고 테스트 해보지 못해 아직 출시 하지 않았습니다.
</div>

## 🛠 기술 스택

```markdown
- React Native
  - Redux-Saga
- Spring boot
  - JPA
  - Querydsl
- AWS
  - Elastick Beanstalk
  - Mysql
```

## 👨🏻‍💻 테스트
회사에서는 Spring boot로 서버를 개발하고 Junit5로 테스트해 익숙하지만, 앱은 익숙하지 않고 컴포넌트 하나 하나 테스트하기에는 시간이 오래 걸리 것 같아 **[detox](https://github.com/wix/Detox)**를 사용해 **E2E 테스트** 로 진행하기로 했습니다.

**[detox](https://github.com/wix/Detox)**
- 설정
```json
{
  "testRunner": "jest",
  "runnerConfig": "e2e/config.json",
  "configurations": {
    "ios": {
      "binaryPath": "ios/build/Build/Products/Debug-iphonesimulator/thumb-around.app",
      "build": "ENVFILE=.env.test xcodebuild -workspace ios/firstapp.xcworkspace -configuration Debug -scheme thumb-around -sdk iphonesimulator -derivedDataPath ios/build",
      "type": "ios.simulator",
      "device": {
        "type": "iPhone 11 Pro"
      }
    }
  }
}
```

<div class="bg-green-100 border-l-4 border-green-500 text-green-900 leading-normal p-3 md:mx-6 mb-6" role="alert">
  .env.test 설정 파일은 <strong>react-native-config</strong> 라이브러리를 사용해 빌드 타임에 사용
</div>

- 환경 변수 .env.test
```ini
API_URL=http://localhost:5000
MODE=test
VERSION=0.0.7
```

- 테스트 코드
```javascript
describe('테스트', () => {
  it('나만의 운동을 추가할 때 이름 넣지 않음', async () => {
    await element(by.text('구성')).tap();
    await element(by.id('excerise-management')).tap();
    await element(by.id('add-exercise-list')).tap();
    await element(by.text('확인')).tap();
    await element(by.text('확인')).tap();
    await waitFor(element(by.text('운동 이름을 입력해주세요.')))
  })
})

```
<div class="bg-green-100 border-l-4 border-green-500 text-green-900 leading-normal p-3 md:mx-6 mb-6" role="alert">
  앱을 사용하는 것 처럼 유저 시나리오를 작성해 테스트 코드를 작성
  <li> 구성 버튼을 누른다. </li>
  <li> 운동 관리 버튼을 누른다. </li>
  <li> 운동 추가 버튼을 누른다. </li>
  <li> 확인 버튼을 누른다. </li>
  <li> 운동 이름을 입력해주세요. 텍스트가 나온다. </li>
</div>


- 👏 실행 화면
![iMac rear photo by Georgie Cobbs on Unsplash](/images/posts/detox-test-1.gif)


#### detox로 테스트가 힘든 부분
- 소셜 로그인
- 타이머
- UI 라이브러리 (Picker, Date modal...)

<div class="bg-green-100 border-l-4 border-green-500 text-green-900 leading-normal p-3 md:mx-6 mb-6" role="alert">
  <p>컴포넌트에 testID값을 설정 하거나 화면에 보이는 텍스트를 찾아 터치해 테스트를 진행하게 되는데
  동일한 텍스트가 여러개 있거나, 텍스트가 살짝 가려지는 경우는 테스트 불가</p>
  <p>직접 만든 컴포넌트가 아닐경우 테스트가 어려움</p>
</div>


