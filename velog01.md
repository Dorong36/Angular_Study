## ✅ angular cli 설치
- 터미널> npm install -g @angular/cli
- ng new 파일이름

<br>

## ✅ 주요 구조 살펴보기
### 🔸 src/
  - index.html => SPA에서 사용자가 받아보는 파일
  - app-root 태그는 angular가 만들어준것
  - main.ts는 사용자가 index.html을 받았을 때 가장 먼저 실행되는 파일 (실질적으로는 js파일이 실행)
### 🔸 src/app/
  #### 🔹 app.module.ts
    - @NgModule => decorator(장식자)
    - AppModule을 NgModule로 장식을 해주겠다 => 쉽게 말해 이 클래스를 모듈로 만들어 주겠다
    - NgModule은 angular/core에서 import한 것으로 angular에서 제공하는 api
    - module은 독립가능한 기능의 상자 => 모듈 안에는 다른 모듈, 컴포넌트, 서비스로직 등이 담길 수 있음
    - NgModule안 내용
      - declation : 선언값
      - imports : 다른 모듈
      - providers : 서비스 로직
      - bootstrap : 실행할 컴포넌트 
    - 모듈은 뒷 강의에서 더 자세히 다룸
    - 일단은 main.ts에서 AppModule을 실행을 했고, AppModule에서 AppComponent를 부팅을 했다 정도

  #### 🔹 app.component.ts
    - decorator가 동일하게 쓰임
    - 클래스를 컴포넌트로 만들어주는 angular의 api
    - 세 가지 설정값
      - selector : 컴포넌트의 태그네임 => 🌟 index.html에 있던 그 app-root 태그!!!
      - templateUrl : 어떤 html 파일을 가지고 있는가
      - styleUrls : 어떤 css 파일을 가지고 있는가
  
### 🔸 살짝 정리해보면
- index.html을 받아서
- main.ts가 실행이 되고
- main.ts는 app.module.ts를 실행
- app.module.ts는 가장 처음이 되는 루트태그 app-root인 app.component.ts를 실행
