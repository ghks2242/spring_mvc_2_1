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