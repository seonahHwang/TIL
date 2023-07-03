# MVC 패턴

애플리케이션을 세가지 역할로 구분한 개발 방법론

<aside>
💡 Controller 조작 → Model을 통해 데이터 가져옴

→ 데이터 바탕으로 View를 통해 시각적 표현을 제어하여 사용자에게 전달

</aside>

이 패턴을 사용하면 사용자 인터페이스로부터 비즈니스 로직을 분리하여

애플리케이션의 시작적 요소나 그 이면에서 실행되는 비즈니스 로직을 서로 영향없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있게된다 

### **✔️ 위의 개념을 WEB에 적용 시!**

1. **사용자가 웹사이트에 접속 (Users)**
2. **Controller는 사용자가 요청한 웹페이지를 서비스하기 위해서 모델을 호출 (Manipulates)**
3. **Model은 데이터베이스나 파일과 같은 데이터 소스를 제어한 후 그 결과를 Return**
4. **Controller는 Model이 리턴한 결과를 View에 반영 (Updates)**
5. **데이터가 반영된 View는 사용자에게 보여짐 (Sees)**

# MVC 패턴 방식

- 모델 1 방식 : JSP에서 출력과 로직을 전부 처리
- 모델 2 방식 : JSP에서 출력만 처리

## Model 1

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4fbe99a9-7070-4626-baa9-231f17303ee1/Untitled.png)

Controller영역에 View 영역을 같이 구현하는 방식

## Model 2

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bed4621a-a893-4ebb-9255-9171b98c2c9a/Untitled.png)

웹브라우저 사용자의 요청을 서블릿이 받고 

서블릿은ㄴ 해당 요청으로 view를 보여줄 것인지 Model로 보낼 것인지를 판단하여 전송

HTML 소스와 JAVA 소스를 분리해놧기 때문에 모델 1 방식에 비해 확장시키기도 쉽고 유지보수도 쉬움

### Model 1 VS Model 2

|  | Model 1 | Model 2 |
| --- | --- | --- |
|  장점 | 빠르고 쉽게 개발 가능  | 디자이너와 개발자의 분업이 가능하며
유지보수 및 확장이 쉬움 |
| 단점 | JSP 파일이 너무 비대해지며 
Controller와 View가 혼재하므로 향후 유지보수에 어려움 | 설계가 어려우며 개발 난이도가 높음 |

### MVC 패턴을 사용해야 하는 이유

- 비즈니스 로직과 UI 로직을 분리하여 유지보수를 독립적으로 수행가능
- Model과 View가 다른 컴포넌트들에 종속되지 않아 애플리케이션의 확장성, 유연성에 유리함
- 중복 코딩의 문제점 제거

## 📗 **MVC 패턴 요약**

**Model - 백그라운드에서 동작하는 비즈니스 로직(데이터) 처리**

**View - 정보를 화면으로 보여주는 역할.**

**Controller - 사용자의 입력 처리와 흐름 제어 담당. 화면과 Model과 View를 연결시켜주는 역할**
