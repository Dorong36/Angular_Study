## ✅ 컴포넌트 만들어보기
- src/app 디렉토리에 hello 폴더를 만들어보쟈
- 그리고 hello.component.ts를 만들어보쟈
```
import { Component } from "@angular/core";

@Component({
    selector : 'hello', // 통상 app용으로 만들어졌다고 알리기 위해 app-xxx로 쓰긴함
    templateUrl : './hello.component.html',
    styleUrls : ['./hello.component.scss']
})
export class HelloComponent {

}
```
- component가 사용되려면 angular는 기본적으로 module에 등록을 해야함
- 그냥 app.component.html에 넣어서 실행해보면 오류가 남
  - 브라우저는 hello 컴포넌트가 뭔지 모르는 상태
- 그래서 module에 등록하는 작업이 필요
- 이는 app.module.ts의 declarations에 추가해 등록
```
...
import { HelloComponent } from './hello/hello.component';

@NgModule({
  declarations: [
    AppComponent, 
    HelloComponent  // 🌟 요기 추가
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- 이정도 까지가 가장 기본적 개념!!
- 컴포넌트를 지정을 해서 하나하나 만들어서 사용하는 것
- 그 이후 개념으로 모듈이 있다~