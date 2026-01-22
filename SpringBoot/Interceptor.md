## Filter

- 클라이언트의 **요청**과 **서버의 자원** (Servlet/JSP) 사이에서 요청과 응답을 가로채 전/후 처리하는 객체
- 여러 개의 칠터를 체인 (chain) 형태로 연결하여 연쇄적으로 동작 → Filter Chain
    - annotation 사용하면 순서지정 X
    - `web.xml`처럼 하면 순서지정 가능 (작성순서에 따라)

### Filter 활용
- **인코딩 처리** : 요청/응답의 문자 인코딩 방식 통일 (Web은 주로 UTF-8)
- **인증/인가 확인** : 사용자가 인증을 받은 사람인지 확인, 사용자가 해당 요청을 수행할 권한이 있는지 확인
- **로깅** : 요청/응답에 대한 시간, 정보 기록 → 추적 가능
- **보안 확인** : 요청을 검사하여 악의적인 코드나 공격 차단

```
package com.ssafy.mvc.config;

import org.springframework.boot.web.servlet.FilterRegistrationBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import com.ssafy.mvc.filter.MyFilter;

//설정파일들의 모음집
@Configuration
public class FilterConfig {

    @Bean
    public FilterRegistrationBean<MyFilter> myfilter(){
        FilterRegistrationBean<MyFilter> registrationBean = new FilterRegistrationBean<>();

            registrationBean.setFilter(new MyFilter()); //어떤 필터를 등록할지
            registrationBean.addUrlPatterns("/hello"); //경로 작성
            registrationBean.setOrder(1); //순서 작성

            return registrationBean;
    }
}
```

---

## Interceptor
- HandlerInterceptor를 구현한 것
- 요청(request)을 처리하는 과정에서 **요청을 가로채서 처리**
- 접근 제어(Auth), 로그(Log) 등 비즈니스 로직과 구분되는 **반복적이고 부수적인 로직 처리**
    - `preHandle()`
    - `postHandle()`
    - `afterCompletion()`
    
![Interceptor 흐름](https://i.imgur.com/nB5vlBK.png)

### `preHandle`
```
@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
    throws Exception{
```
- Controller(핸들러) 실행 이전에 호출
- true를 반환하면 계속 진행
- false를 반환하면 요청 종료

### `postHandle`
```
@Override
public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception{
```
- Controller(핸들러) 실행 후 호출
- 정상 실행 후 **추가 기능 구현 시 사용**
- **Controller에서 예외 발생 시** 해당 메서드는 실행 X

### `afterCompletion`
```
@Override
public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex)
    throws Exception{
```
- 뷰가 클라이언트에게 **응답을 전송한 뒤** 실행
- Controller에서 예외 발생시, 네번째 파라미터로 전달 (기본은 null)
- 따라서 Controller에서 발생한 **예외 혹은 실행 시간** 같은 것들을 기록하는 등 후처리 시 주로 사용

### Interceptor 등록 (설정파일)
- addInterceptors 메서드를 통해서 등록할 수 있음
- 매핑할 URL을 지정할 수 있고, 제외할 URL 또한 지정할 수 있음
```
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    @Override
    public void addInterceptors(InterceptorRegistry registry){
        //인터셉터 등록
    }
}
```

### Interceptor 체이닝
- 여러 개의 인터셉터 동시에 등록
- 작성 순서에 따라 동작