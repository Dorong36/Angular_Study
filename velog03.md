## ✅ Angular 사용해보기 - 상품 목록 생성하기
### 🔸 *ngfor
```
<div *ngFor="let product of products"></div>
```
- 이런식으로 넣어주면 products 배열에 요소들이 product에 담겨
- 그 수만큼 div가 반복

### 🔸 문자열 바인딩
```
<div *ngFor="let product of products">{{ product.name }}</div>
```
- 이렇게 {{}}를 사용해 문자열 바인딩을 할 수 있고,
- 받아온 product의 정보를 사용할 수 있음

### 🔸 프로퍼티 바인딩
```
<div *ngFor="let product of products">
  <h3>
    <a [title]="product.name + ' details'">
      {{ product.name }}
    </a>
  </h3>
</div>
```
- 위처럼 []로 프로퍼티 바인딩을 할 수 있음
- 또, ""안에서 변수가 그냥 쓰이기 때문에 ''로 문자열을 합쳐줄 수 있음

### 🔸 *ngIf
```
<div *ngFor="let product of products">
  <h3>
    <a [title]="product.name + ' details'">
      {{ product.name }}
    </a>
  </h3>
  <p *ngIf="product.description">
    Description: {{ product.description }}
  </p>
</div>
```
- *ngIf는 if 문법을 따라감
- 위의 경우 product.description이 있다면 p 태그를 표시, 없으면 제외

### 🔸 이벤트 바인딩
```
<div *ngFor="let product of products">
  <h3>
    <a [title]="product.name + ' details'">
      {{ product.name }}
    </a>
  </h3>
  <p *ngIf="product.description">
    Description: {{ product.description }}
  </p>
  <button type="button" (click)="share()">
    Share
  </button>
</div>
```
- ()로 이벤트 바인딩하기
- 파일명.component.ts에서 정의해 두었던 share()메서드를 클릭이벤트에 연결

<br>
<br>

## ✅ Angular 사용해보기 - 자식 컴포넌트로 데이터 전달하기
- ProductListComponent 상품 데이터 활용 가격이 700이상이면 세일 알림 버튼 넣어주기
- ProductListComponent 데이터를 받는 자식 컴포넌트 ProductAlertComponent 만들자
- 컴포넌트 생성
  - 터미널> ng g c "파일명"

### 🔸 Input 심볼 로드
- 해당 컴포넌트가 데이터를 받으려면 먼저 @angular/core 패키지에서 Input 심볼을 로드
```
import { Component, Input } from '@angular/core';
import { Product } from '../products';
```

### 🔸 @Input() 데코레이터
- ProductAlertComponent 클래스에 product 프로퍼티를 선언하고 @Input() 데코레이터를 지정
- 🌟 @Input() 데코레이터를 사용하면, 해당 값은 부모 컴포넌트 ProductListComponent에서 전달된다는 의미
```
export class ProductAlertsComponent {

  @Input() product: Product | undefined;

}
```

### 🔸 파일 기능 구현
```
// product-alert.component.html
<p *ngIf="product && product.price > 700">
  <button type="button">Notify Me</button>
</p>
```
- +) ❗️angular CLI로 컴포넌트를 생성하면 자동으로 AppModule에 추가됨

### 🔸 상속 기능 구현하기
- ProductListComponent의 자식으로 ProductListComponent를 추가
- product-list.component.html 파일에 <app-product-alerts> 엘리먼트를 추가
- 이 때 현재 상품을 프로퍼티바인딩으로 전달
```
...
<button type="button" (click)="share()">
  Share
</button>

<app-product-alerts
  [product]="product">
</app-product-alerts>
```

<br>
<br>

## ✅ Angular 사용해보기 - 부모 컴포넌트로 데이터 전달하기
- Notify Me 버튼이 제대로 동작하려면 자식 컴포넌트에서 부모 컴포넌트로 알림을 보내기 위해 데이터를 전달할 수 있어야 함
- 그러면 ProductAlertsComponent는 사용자가 Notify Me 버튼을 클릭했을 때 부모 컴포넌트로 이벤트를 보내야 함

### 🔸 Output, EventEmitter 심볼 로드
- product-alerts.component.ts 파일에 @angular/core 패키지가 제공하는 Output 심볼과 EventEmitter 심볼을 로드
```
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { Product } from '../products';
```

### 🔸 데코레이터 지정, 인스턴스 할당
```
export class ProductAlertsComponent {
  @Input() product: Product | undefined;
  @Output() notify = new EventEmitter();
}
```
- 컴포넌트 클래스에 notify 프로퍼티를 추가하고,
- 이 프로퍼티에 @Output() 데코레이터를 지정하고, 
- EventEmitter() 인스턴스를 할당
- 프로퍼티에 @Output() 데코레이터를 지정하면 해당 컴포넌트가 해당 프로퍼티를 통해 이벤트를 보낼 수 있음

### 🔸 이벤트 바인딩
- product-alerts.component.html 파일에 있는 Notify Me 버튼에 이벤트 바인딩을 연결
- (click) 이벤트가 발생하면 notify.emit() 메서드를 실행
```
<p *ngIf="product && product.price > 700">
  <button type="button" (click)="notify.emit()">Notify Me</button>
</p>
```

### 🔸 부모 컴포넌트에 메서드 추가
- 자식 컴포넌트에서 발생한 이벤트는 자식 컴포넌트 ProductAlertsComponent가 아니라 부모 컴포넌트 ProductListComponent에서 처리해야함
- product-list.component.ts 파일에서 onNotify() 메서드를 추가
```
export class ProductListComponent {

  products = [...products];

  share() {
    window.alert('The product has been shared!');
  }

  onNotify() {
    window.alert('You will be notified when the product goes on sale');
  }
}
```

### 🔸 메서드 바인딩
- ProductAlertsComponent가 보낸 데이터를 ProductListComponent가 받을 수 있도록 수정
- product-list.component.html 파일에서 <app-product-alerts>와 onNotify() 메서드를 바인딩
```
<button type="button" (click)="share()">
  Share
</button>

<app-product-alerts
  [product]="product" 
  (notify)="onNotify()">
</app-product-alerts>
```