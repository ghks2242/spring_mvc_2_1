# 타임리프 스프링 통합
### 수강 mvc2 23강

### 스프링 통합으로 추가되는 기술
* 스프링의 SpringEL문법 통합
* ```${@myBean.doSomething()} 처럼 스프링 빈 호출 지원```
* 편리한 폼 관리를 위한 추가 속성
  * th:object (기능강화, 폼 커맨드 객체 선택)
  * th:filed, th:errors, th:errorclass
* 폼 컴포넌트 기능
  * checkbox, radio button, List 등을 편리하게 사용할 수 있는 기능 지원
* 스프링의 메시지, 국제화 기능 편리한 통합
* 스프링의 검증, 오류처리 통합
* 스프링의 변환 서비스통합(ConversionService)

~~~ 
타임리프 관련 설정을 변경하고싶으면 다음을 참고해서 application.properties 에 추가하면된다 
~~~
https://docs.spring.io/spring-boot/appendix/application-properties/index.html#appendix.application-properties.templating


### th:object
~~~ 
th:object 를 사용하면 
하위 태그에 th:filed 사용시 *{} 별표를 붙이면 th:object에 종속되어있다는걸 표현하여 사용할수있다.
ex) th:object="${item}"
    th:filed="*{itemName}"
    
사용안할시 
ex) th:filed="${item.itemName}"

th:filed 는 id 와 name 을 value 자동으로 만들어준다
하지만 인텔리제이에서 라벨 for 랑 같이없으면 에러를 표출해서 남겨둘떄도있다
~~~

### http 로그
~~~
  html 전송을 로그로 찍고싶으면
  logging.level.org.apache.coyote.http11=debug
  를 하면 헤더와 바디 등등 디버그 네트워크에 있는 정보를 로그로도 볼수있다.
~~~

## 체크박스
기본 html 로 체크박스를 구현하면 체크박스를 선택하면 값이 넘어가지만 선택하지않으면 변수자체가 넘어가지않는다

- (체크박스는 선택시 기본적으로 on 이라는 값이 넘어가게되는데 boolean으로 값을 받으면 이것을 스프링에서는 타입컨버터가 true로 변경해준다)
~~~java
    <div>판매 여부</div>
    <div>
        <div class="form-check">
            <input class="form-check-input" type="checkbox" id="open" name="open">
            <label class="form-check-label" for="open">판매 오픈</label>
        </div>
    </div>
~~~
하지만 스프링 mvc 에서 트릭같이 사용할수있는 기능이있다.
~~~java
    <div>판매 여부</div>
    <div>
        <div class="form-check">
            <input class="form-check-input" type="checkbox" id="open" name="open">
            <input type="hidden" name="_open" value="on">
            <label class="form-check-label" for="open">판매 오픈</label>
        </div>
    </div>
~~~
이렇게 앞에 _넣은 히든필드를 생성하게되면 _open 이것만있으면 체크가 안되어있는걸로 알고 false 를 넘겨준다

타임리프를 사용하여 th:filed 를 사용하게되면 hidden 부분을 직접만들지않아도 자동으로 만들어준다.
심지어 값이 true 이면 체크까지 해준다 

---
### @ModelAttribute 의 특별한 사용법
~~~ 
등록폼, 상세화면, 수정 폼에서 모두 서울, 부산, 제주라는 체크박스를 반복해서 보여주여야한다. 이렇게하려면 각각의 컨트롤러에서
model.addAttribute(...) 를 사용하여 체크박스를 구성하는 데이터를 반복해서 넣어주어야한다.
@ModelAttribute 는 이렇게 컨트롤러에 있는 별도의 메서드에 적용할수있다.
이렇게하면 해당 컨트롤러를 요청할때 regions 에서 반환한 값이 자동으로 model 에 담기게된다
물론 이렇게 사용하지않고 각각 컨트롤러 메서드에서 모델에 직접 데이터를 담아서 처리해도된다.
~~~
---
### each 로 체크박스시 
~~~java
<!--multi checkbox -->
<div>
    <div>등록 지역</div>
    <div th:each="region : ${regions}" class="form-check form-check-inline">
        <input type="checkbox" th:field="*{regions}" th:value="${region.key}" class="form-check-input">
        <label class="form-check-label" th:for="${#ids.prev('regions')}" th:text="${region.value}"></label>
    </div>
</div>
~~~
여기서 th:field="*{regions}" 이건 위에 each의 변수를 사용하는게 아니라 form 에 있는 object (``` th:field="*{regions}" ```) 를 사용한다
th:field 는 실제로 데이터가 넘어가는 변수를 연결시키기때문이다 
아니면 직접 id name value 를 생성해도 되지만 checked 로직도 직접구현해주어야한다.

``` model 을 로그찍고싶으면 System.out.println(model.asMap()); 을 사용하면된다```