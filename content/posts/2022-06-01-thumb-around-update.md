---
title: "React native upgrade 0.66.2 -> 0.68.2"
path: post3
slug: getting-started-with-gridsome-and-bleda
description: "detox로 E2E 테스트"
date: 2022-06-01 17:52:00
author: hyeon namkung
tags:
    - 썸어라운드 - 운동 기록
    - 개인 프로젝트
    - React native
    - Test
cover: ""
fullscreen: true
---

## 📱**썸어라운드 - 운동 기록**
미리 운동을 구성하고 구성된 운동을 불러와 운동을 기록하는 앱

- [IOS](https://apps.apple.com/us/app/%EC%8D%B8%EC%96%B4%EB%9D%BC%EC%9A%B4%EB%93%9C-%EC%9A%B4%EB%8F%99-%EA%B8%B0%EB%A1%9D/id1538255500)

### 🛠 기술 스택

```markdown
- React Native
  - Redux-Saga
- Spring boot
  - JPA
  - Querydsl
- AWS
  - Elastic Beanstalk
  - Mysql
```

### 👨🏻‍💻 React native version 업그레이드
최근 주말에 여유가 생겨 1년전에 개발했던 **썸어라운드 - 운동 기록**를 다시 업데이트하기로 마음을 먹었다.

2020년 여름쯤에 처음 개발을 시작했고 2022년 4월 이후로는 업데이트를 하지 못했다. 그동안 React native 버전이 많이 올라가 우선 최신 버전으로 올리고 시작하는게 정신 건강에 좋을것 같아 업데이트를 진행했다.
회사 업무는 서버 개발만 하다보니 실행하는 방법이 기억나지 않아 기록해둔 README를 보고 다시 처음부터 시작했다.

#### 업그레이드 순서
[React native 업그레이드 공식 가이드](https://reactnative.dev/docs/upgrading)

**1. React native cli**
```bash
npx react-native upgrade
```
**2. Conflict 발생한 부분 확인**

**3. React Native upgrade helper로 Conflict 해결**

- 충돌이 발생한 파일을 [helper](https://react-native-community.github.io/upgrade-helper/)에서 찾아 하나씩 변경한다.

**4. 빌드**

- 2~4번을 반복한다.


### 🤦‍♂️ 라이브러리 업그레이드
React native 버전을 업그레이드 하다보니 기존에 사용하던 라이브러리에도 영향이 발생해 사용하고 있던 라이브러리도 모두 업데이트하면서 점점 업데이트할 수 있을까라는 생각이 들었다....  
라이브러리 버전을 최신 버전으로 올리고 빌드하고 문제가 발생하는 부분을 수정하는데 이렇게 힘든줄 몰랐다.

### 업그레이드 완료
2주 약4일 동안 업그레이드를 진행해 React native 0.68.2 최신 버전으로 올렸고, 서버 코드도 리팩토링을 진행했다. 
업그레이드 하면서 E2E 테스트 코드에 중요성을 다시 상기 시켜주는 좋은 기회였고 테스트 코드가 없었더라면 아마 업그레이드 시도 조차 하지 못했을 것이다.

