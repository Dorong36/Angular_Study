# 🅰️ angular란
- TypeScript를 기반으로 개발된 개발 플랫폼
- 동시에 확장가능한 컴포넌트 구조로 웹 애플리케이션을 만드는 프레임 워크
- 라우팅, 폼 관리 등 필요한 라이브러리를 통합
- 애플리케이션 개발, 빌드, 테스트, 수정에 필요한 개발자 도구 제공

<br>

# 🧐  기초 개념
## ✅ 컴포넌트 (Component)
- App을 구성하는 기본단위
- 🌟 컴포넌트의 구성은
  - @Component() 데코레이터가 붙는
  - TypeScript 클래스
  - HTML 탬플릿
  - 스타일
- @Component() 데코레이터는 Angular에 필요한 정보를 지정하는 역할
  - 컴포넌트가 템플릿에 사용될 CSS 셀렉터 지정
    - 템플릿에서 셀렉터에 해당되는 HTML 엘리먼트마다 컴포넌트 인스턴스 생성
  - Angular가 컴포넌트 내용으로 렌더링할 HTML 템플릿을 지정
  - 템플릿 HTML 엘리먼트의 모습 지정해야 한다면 필요한 CSS 스타일 지정
- 컴포넌트 구성하기
  ```
  import { Component } from '@angular/core';

  @Component({
      selector: 'hello-world',
      template: `
          <h2>Hello World</h2>
          <p>This is my first component!</p>
      `
  })
  export class HelloWorldComponent {
      // 여기에는 컴포넌트의 동작을 정의하는 코드가 들어갑니다.
  }
  ```
- 템플릿에서 컴포넌트 사용
  ```
  <hello-world></hello-world>
  ```
- Angular가 컴포넌트 렌더링 한 후 DOM
  ``` 
  <hello-world>
      <h2>Hello World</h2>
      <p>This is my first component!</p>
  </hello-world>
  ```
- Angular 컴포넌트는 강력하게 캡슐화되어 있지만 애플리케이션 구조에 맞게 직관적으로 구성 
- 컴포넌트를 사용하면 애플리케이션에 유닛 테스트를 적용하기 쉽고, 코드의 가독성도 높일 수 있음

<br>

## ✅ 템플릿 (Template)
- 컴포넌트는 이 컴포넌트가 어떻게 렌더링될지 정의하기 위해 HTML 템플릿이 존재
- 🌟 템플릿은 인라인으로 정의하거나 별도 파일로 작성해서 불러올 수 있음

- 템플릿은 HTML 문법을 기본으로 작성
- 컴포넌트에 있는 값을 동적으로 반영하도록 구성
- 🌟 그래서!! 컴포넌트의 상태가 변경되면 Angular가 자동으로 렌더링된 DOM을 갱신

### 🏃‍♂️ 동적 렌더링 예시들
### 🔸 문자열 바인딩
```
// 렌더링 컴포넌트의 템플릿
// 🌟 이중 중괄호 ( {{, }} )는 템플릿에 문자열을 바인딩하는 문법 (interpolation)
<p>{{ message }}</p>

// 컴포넌트 클래스에서 문자열 전달
import { Component } from '@angular/core';

@Component ({
  selector: 'hello-world-interpolation',
  templateUrl: './hello-world-interpolation.component.html'
})
export class HelloWorldInterpolationComponent {
    message = 'Hello, World!';
}

// 애플리케이션이 로드하면 보이는 화면은
<p>Hello, World!</p>
```

### 🔸 HTML 엘리먼트의 property나 attibute 값 할당(property binding)
```
// 🌟 대괄호 ( [, ] )는 컴포넌트 클래스에 있는 값을 property나 attribute로 바인딩하는 문법
<p
  [id]="sayHelloId"
  [style.color]="fontColor">
  You can set my color in the component!
</p>
```

### 🔸 이벤트리스너 추가
```
// 🌟 감지하려는 이벤트 이름을 소괄호 ( (, ) )로 감싸줌
<button
  type="button"
  [disabled]="canClick"
  (click)="sayMessage()">
  Trigger alert message
</button>

// 이벤트에 따라 실행될 메서드 in 컴포넌트 클래스
sayMessage() {
  alert(this.message);
}
```

### 🔸 Angular 템플릿에서 사용예
```
// ts 파일
import { Component } from '@angular/core';

@Component ({
  selector: 'hello-world-bindings',
  templateUrl: './hello-world-bindings.component.html'
})
export class HelloWorldBindingsComponent {
  fontColor = 'blue';
  sayHelloId = 1;
  canClick = false;
  message = 'Hello, World';

  sayMessage() {
    alert(this.message);
  }
}

// html 파일
<button
  type="button"
  [disabled]="canClick"
  (click)="sayMessage()">
  Trigger alert message
</button>

<p
  [id]="sayHelloId"
  [style.color]="fontColor">
  You can set my color in the component!
</p>

<p>My color is {{ fontColor }}</p>
```

### 🔸 디렉티브 (Directives)
- 템플릿에 추가 기능 구현하기
- DOM 구조를 동적으로 변경
- 가장 많이 사용되는 디렉티브는 ngIf, ngFor
```
// ngIf 예시
// ts 파일
import { Component } from '@angular/core';

@Component({
  selector: 'hello-world-ngif',
  templateUrl: './hello-world-ngif.component.html'
})
export class HelloWorldNgIfComponent {
  message = "I'm read only!";
  canEdit = false;

  onEditClick() {
    this.canEdit = !this.canEdit;
    if (this.canEdit) {
      this.message = 'You can edit me!';
    } else {
      this.message = "I'm read only!";
    }
  }
}

// html 파일
<h2>Hello World: ngIf!</h2>

<button type="button" (click)="onEditClick()">Make text editable!</button>

<div *ngIf="canEdit; else noEdit">
    <p>You can edit the following paragraph.</p>
</div>

<ng-template #noEdit>
    <p>The following paragraph is read only. Try clicking the button!</p>
</ng-template>

<p [contentEditable]="canEdit">{{ message }}</p>
```

- Angular는 선언적인 템플릿 문법을 사용
- 화면에 표시되는 단위로 애플리케이션 로직을 깔끔하게 분리할 수 있음
- 템플릿에는 표준 HTML 문법을 활용하기 때문에 구성, 관리, 수정이 용이

<br>

## ✅ 의존성 주입 (Dependency injection, DI)
- Angular는 TypeScript 클래스를 활용하는 의존성 주입 시스템을 제공
- 컴포넌트에 필요한 객체의 인스턴스를 어떻게 생성하는지 직접 신경쓸 필요가 없음 
- 인스턴스 생성은 Angular가 알아서 처리
- 이런 패턴 덕분에 애플리케이션 코드를 좀 더 유연하게 작성, 테스트 가능 
- Angular 애플리케이션을 개발할 때 의존성 주입 시스템을 반드시 알아야 하는 것은 아니자만 
- 의존성 주입 시스템을 활용했을 때 얻는 장점이 무엇인지 알아보는 것을 적극 권장
```
// logger.service.ts
// 인자로 받은 숫자 콘솔 출력 함수 정의
import { Injectable } from '@angular/core';

@Injectable({providedIn: 'root'})
export class Logger {
  writeCount(count: number) {
    console.warn(count);
  }
}

// hello-world-di.component.ts
// angular 컴포넌트
// 버튼 클릭시 Logger 클래스 writeCount 함수 실행
// 어떻게? => 클래스 생성자에 logger 코드 추가해서 Logger 서비스가 의존성 객체로 주입되도록 요청
import { Component } from '@angular/core';
import { Logger } from '../logger.service';

@Component({
  selector: 'hello-world-di',
  templateUrl: './hello-world-di.component.html'
})
export class HelloWorldDependencyInjectionComponent  {
  count = 0;

  constructor(private logger: Logger) { }

  onLogMe() {
    this.logger.writeCount(this.count);
    this.count++;
  }
}
```