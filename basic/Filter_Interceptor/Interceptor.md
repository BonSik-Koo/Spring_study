๐__Interceptor__
=======================
![image](https://user-images.githubusercontent.com/96917871/158424196-d67438e7-0de6-45cf-b75a-f873f3803ef0.png)

- ์คํ๋ง ์ธํฐ์ํฐ๋ ์คํ๋ง MVC๊ฐ ์ ๊ณตํ๋ ๊ธฐ์ ์ด๋ค.
- ์คํ๋ง ์ธํฐ์ํฐ๋ ๋์คํจ์ฒ ์๋ธ๋ฆฟ๊ณผ ์ปจํธ๋กค๋ฌ ์ฌ์ด์์ ์ปจํธ๋กค๋ฌ ํธ์ถ ์ง์ ์ ํธ์ถ ๋๋ค. -> ์๋ธ๋ฆฟ ํํฐ๋ ๋์คํจ์ธ  ์๋ธ๋ฆฟ ํธ์ถ ์ 
- ์คํ๋ง ์ธํฐ์ํฐ์๋ URL ํจํด์ ์ ์ฉํ  ์ ์๋๋ฐ, ์๋ธ๋ฆฟ URL ํจํด๊ณผ๋ ๋ค๋ฅด๊ณ , ๋งค์ฐ ์ ๋ฐํ๊ฒ ์ค์ ํ  ์ ์๋ค.(ํฌํจํ  URL, ์ ์ธํ  URL๋ฑ)
- ์๋ธ๋ฆฟ ํํธ์ ๋ง์น๊ฐ์ง๋ก "์ฒด์ธ"์ ๊ธฐ๋ฅ๋ ๊ฐ์ง๊ณ  ์๋ค.
- ์๋ธ๋ฆฟ ํํฐ์ ๋ค๋ฅด๊ฒ ์คํ๋ง ์ธํฐ์ํฐ๋ ์ปจํธ๋กค๋ฌ ํธ์ถ์ (preHandle, ๋ ์ ํํ๋ ํธ๋ค๋ฌ ์ด๋ํฐ ํธ์ถ์ ), ํธ์ถํ(postHandel, ์ปจํธ๋กค๋ฌ ํธ์ถํ ModelAndView๋ฅผ ๋์คํจ์ฒ ์๋ธ๋ฆฟ์ ๋ฐํ ํ), ์์ฒญ ์๋ฃ ์ดํ(afterCompletion, ๋ทฐ ํํ๋ฆฟ ๋ ๋๋ง์ ๋ง์น ํ) ์ ๊ฐ์ด ๋จ๊ณ์ ์ผ๋ก ์ ์ธ๋ถํ ๋์ด ์๋ค.        
-> ์๋ธ๋ฆฟ ํํฐ์ ๊ฒฝ์ฐ ๋จ์ํ request , response ๋ง ์ ๊ณตํ์ง๋ง, ์ธํฐ์ํฐ๋ ์ด๋ค ์ปจํธ๋กค๋ฌ(handler)๊ฐ ํธ์ถ๋๋์ง ํธ์ถ ์ ๋ณด๋ ๋ฐ์ ์ ์๋ค. ๊ทธ๋ฆฌ๊ณ  ์ด๋ค modelAndView ๊ฐ ๋ฐํ๋๋์ง ์๋ต ์ ๋ณด๋ ๋ฐ์ ์ ์๋ค.

__์คํ๋ง ์ธํฐ์ํฐ ํธ์ถ ํ๋ฆ(์ ์์ ์ธ ํ๋ฆ, ์์ธ ์ํฉ ํ๋ฆ)__
=============================

__<์ ์์ ์ธ ํ๋ฆ>__
------------------------------
![image](https://user-images.githubusercontent.com/96917871/158425115-b92f0484-3b91-48f9-bb1c-6a7bd2b514f1.png)

- ์คํ๋ง ์ธํฐ์ํฐ์๋ 3๊ฐ์ง ๊ธฐ๋ฅ(๋ฉ์๋) ์๋ค. -> preHandle, postHandle, afterCompletion
- preHandle : ์ปจํธ๋กค๋ฌ ํธ์ถ ์ ์ ํธ์ถ๋๋ค. (๋ ์ ํํ๋ ํธ๋ค๋ฌ ์ด๋ํฐ ํธ์ถ ์ ์ ํธ์ถ๋๋ค.)     
-> preHandle ์ ์๋ต๊ฐ์ด true ์ด๋ฉด ๋ค์์ผ๋ก ์งํํ๊ณ , false ์ด๋ฉด ๋๋ ์งํํ์ง ์๋๋ค. false ์ธ ๊ฒฝ์ฐ ๋๋จธ์ง ์ธํฐ์ํฐ๋ ๋ฌผ๋ก ์ด๊ณ , ํธ๋ค๋ฌ ์ด๋ํฐ๋ ํธ์ถ๋์ง ์๋๋ค. ๊ทธ๋ฆผ์์ 1๋ฒ์์ ๋์ด ๋๋ฒ๋ฆฐ๋ค.
- postHandle : ์ปจํธ๋กค๋ฌ ํธ์ถ ํ์ ํธ์ถ๋๋ค. (๋ ์ ํํ๋ ํธ๋ค๋ฌ ์ด๋ํฐ ํธ์ถ ํ์ ํธ์ถ๋๋ค.)
- afterCompletion : ๋ทฐ๊ฐ ๋ ๋๋ง ๋ ์ดํ์ ํธ์ถ๋๋ค.

__<์์ธ ์ํฉ ํ๋ฆ>
----------------------------------------------
![image](https://user-images.githubusercontent.com/96917871/158425719-7d84a26b-86c8-4ba8-98cb-607b6b0a99eb.png)

- preHandle : ์ปจํธ๋กค๋ฌ ํธ์ถ ์ ์ ํธ์ถ๋๋ค. -> ์์ธ ์ํฉ์ด ์ปจํธ๋กค๋ฌ์์ ๋ฐ์ํ๊ธฐ ๋๋ฌธ์ ์๊ด์์ด ํธ์ถ
- postHandle : ์ปจํธ๋กค๋ฌ์์ ์์ธ๊ฐ ๋ฐ์ํ๋ฉด postHandle ์ ํธ์ถ๋์ง ์๋๋ค.!!!!!!
- afterCompletion : afterCompletion ์ ํญ์ ํธ์ถ๋๋ค.      
-> ํด๋น ๋ฉ์๋์์ ์์ธ(Exception)์ ๋ฐ๋ ํ๋ผ๋ฏธํฐ๊ฐ ์๋๋ฐ ์ปจํธ๋กค๋ฌ์์ ์ด๋ค ์์ธ๊ฐ ๋ฐ์ํ๋์ง ํ์ธํ  ์ ์๋ค.      
-->์์ธ ๋ฐ์์ postHadle๋ ํธ์ถ๋์ง ์์ง๋ง "afterCompletion"์ ํญ์ ํธ์ถ ๋๊ธฐ ๋๋ฌธ์ ์์ธ์ ๋ฌด๊ดํ๊ฒ ๊ณตํต ์ฒ๋ฆฌ๋ฅผ ํ๋ ค๋ฉด ํด๋น ๋ฉ์๋๋ฅผ ์ฌ์ฉํ๋ฉด ๋๋ค.


__์คํ๋ง ์ธํฐ์ํฐ ์ฌ์ฉ1 - Log ์ถ ๋ ฅ__
=================================
```
/** ์คํ๋ง ์ธํฐ์ํธ ์ฌ์ฉ - Log ๋จ๊ธฐ๋ ๊ธฐ๋ฅ์ ํํฐ **/
@Slf4j
public class LogInterceptor implements HandlerInterceptor {

    public static final String LOG_ID = "logId";

    /** ์ปจํธ๋กค๋ฌ ํธ์ถ์  (์ ํํ ํธ๋ค๋ฌ ์ด๋ํฐ ํธ์ถ์ ) **/
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String requestURI = request.getRequestURI();
        String uuid = UUID.randomUUID().toString();
        request.setAttribute(LOG_ID, uuid);

        log.info("REQUEST [{}] [{}] [{}]", uuid, requestURI, handler);
        return true; //false ์งํx
    }

    /** ํธ๋ค๋ฌ(์ปจํธ๋กค๋ฌ)ํธ์ถํ  **/
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        log.info("postHandel [{}]" ,modelAndView);
    }

    /** ๋ทฐ ๋ ๋๋ง ๋ง์น ํ  **/
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        String requestURI = request.getRequestURI();
        Object logId = request.getAttribute(LOG_ID);

        log.info("REQUEST [{}] [{}]", logId, requestURI);
        if(ex!=null) {
            log.error("afterCompletion error!!", ex);
        }
    }
}
```
- ์คํ๋ง ์ธํฐ์ํฐ๋ฅผ ์ฌ์ฉํ๋ ค๋ฉด "HandlerInterceptor"์ ์ธํฐํ์ด์ค๋ฅผ ๊ตฌํํด์ผ๋๋ค. -> ํ์ํ ๋ฉ์๋๋ฅผ ์ค๋ฒ๋ผ์ด๋ฉ ํ์ฌ ์ฌ์ฉํ๋ฉด ๋๋ค.

- request.setAttribute(LOG_ID, uuid)   
-> ์๋ธ๋ฆฟ ํํฐ์ ๊ฒฝ์ฐ ์ง์ญ๋ณ์๋ก ํด๊ฒฐ์ด ๊ฐ๋ฅํ์ง๋ง, ์คํ๋ง ์ธํฐ์ํฐ๋ ํธ์ถ ์์ ์ด ์์ ํ ๋ถ๋ฆฌ๋์ด ์๋ค. ๋ฐ๋ผ์ preHandle ์์ ์ง์ ํ ๊ฐ์ postHandle , afterCompletion ์์ ํจ๊ป ์ฌ์ฉํ๋ ค๋ฉด ์ด๋๊ฐ์ ๋ด์๋์ด์ผ ํ๋ค. LogInterceptor ๋ ์ฑ๊ธํค ์ฒ๋ผ ์ฌ์ฉ๋๊ธฐ ๋๋ฌธ์ ๋งด๋ฒ๋ณ์๋ฅผ ์ฌ์ฉํ๋ฉด ์ํํ๋ค. ๋ฐ๋ผ์ request ์ ๋ด์๋์๋ค. ์ด ๊ฐ์ afterCompletion ์์ request.getAttribute(LOG_ID) ๋ก ์ฐพ์์ ์ฌ์ฉํ๋ค.

- return true -> true ๋ฉด ์ ์ ํธ์ถ์ด๋ค. ๋ค์ ์ธํฐ์ํฐ๋ ์ปจํธ๋กค๋ฌ๊ฐ ํธ์ถ๋๋ค.

- "preHandle"๋ฉ์๋์์ "Handler"ํ๋ผ๋ฏธํฐ๋ฅผ ๋ฐ์์ฌ ์ ์๋ค.     
-> ํธ๋ค๋ฌ ๋ชฉ๋ก์กฐํ(ํธ๋ค๋ฌ ๋งคํ- @controller,@RequestMapping ์ ํตํ ์ปจํธ๋กค๋ฌ ํ์)๋ฅผ ํตํด ์ปจํธ๋กค๋ฌ๋ฅผ ์ฐพ๊ฒ ๋๊ณ  ํด๋น ํธ๋ค๋ฌ ์ด๋ํฐ๋ฅผ ํธ์ถํ๊ธฐ์ ์ ์คํ๋ง ์ธํฐ์ํฐ๊ฐ ํธ์ถ๋๊ธฐ ๋๋ฌธ์ ์ปจํธ๋กค๋ฌ ๊ฐ์ฒด๋ฅผ ๋ฐ์์ฌ ์ ์๋ค.  

-------------------------------------------
```
 @Configuration
public class WebConfig implements WebMvcConfigurer {
 @Override
 public void addInterceptors(InterceptorRegistry registry) {
 registry.addInterceptor(new LogInterceptor())
 .order(1)
 .addPathPatterns("/**")
 .excludePathPatterns("/css/**", "/*.ico", "/error");
 }
 //...
}
```
- ์ธํฐ์ํฐ๋ฅผ ๋ฑ๋กํ๋ ์ฝ๋
- WebMvcConfigurer ๊ฐ ์ ๊ณตํ๋ addInterceptors() ๋ฅผ ์ฌ์ฉํด์ ์ธํฐ์ํฐ๋ฅผ ๋ฑ๋กํ  ์ ์๋ค. -> ์คํ๋ง์ด ์ ๊ณตํ๋ ๋ฐฉ์. 
- registry.addInterceptor(new LogInterceptor()) : ์ธํฐ์ํฐ๋ฅผ ๋ฑ๋กํ๋ค.
- order(1) : ์ธํฐ์ํฐ์ ํธ์ถ ์์๋ฅผ ์ง์ ํ๋ค. ๋ฎ์ ์๋ก ๋จผ์  ํธ์ถ๋๋ค.
- addPathPatterns("/**") : ์ธํฐ์ํฐ๋ฅผ ์ ์ฉํ  URL ํจํด์ ์ง์ ํ๋ค. -> ์๋ธ๋ฆฟ ํํฐ์ URL ํจํด๊ณผ  ๋ค๋ฅด๋ค!.
- excludePathPatterns("/css/**", "/*.ico", "/error") : ์ธํฐ์ํฐ์์ ์ ์ธํ  ํจํด์ ์ง์ ํ๋ค.     
-> ํํฐ์ ๋น๊ตํด๋ณด๋ฉด ์ธํฐ์ํฐ๋ addPathPatterns , excludePathPatterns ๋ก ๋งค์ฐ ์ ๋ฐํ๊ฒ URL ํจํด์ ์ง์ ํ  ์ ์๋ค.



__์คํ๋ง ์ธํฐ์ํฐ ์ฌ์ฉ2 - ๋ก๊ทธ์ธ ํ์ธ ๊ธฐ๋ฅ__
=================================
```
@Slf4j
public class LoginCheckInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        String requestURI = request.getRequestURI();
        log.info("์ธ์ฆ ์ฒดํฌ ์ธํฐ์ํฐ ์คํ {}", requestURI);

        HttpSession session = request.getSession();
        if(session==null || session.getAttribute(SessionConst.LOGIN_MEMBER)==null) {
            log.info("๋ฏธ์ธ์ฆ ์ฌ์ฉ์ ์์ฒญ");
            response.sendRedirect("/login?redirectURL=" + requestURI);
            return false;
        }

        return true;
    }
}
```
- HandlerInterceptor ์ธํฐํ์ด์ค๋ฅผ ๊ตฌํํ ์ธํฐ์ํฐ ๊ตฌํ์ฒด. -> ํ์ํ ๋ฉ์๋๋ฅผ ์ค๋ฒ๋ผ์ด๋ฉํ์ฌ ์ฌ์ฉํ๋ฉด ๋๋ค.
- retrun์ ๊ฐ์ ๋ฐ๋ผ ๋ค์ ์ธํฐ์ํฐ, ์ปจํธ๋กค๋ฌ ํธ์ถ(๋์ ํํ ํธ๋ค๋ฌ ์ด๋ํฐ ํธ์ถ)์ ํ๊ฒ ๋๋ค.
- ์๋ธ๋ฆฟ ํํฐ์ ๊ฐ์ ๊ฒฝ์ฐ์๋ ํด๋น ๋ก์ง ๋ด์ ์ ์ฉํ์ง ์์ "URL"์ ํ๋ณํ๋ ๊ธฐ๋ฅ์ด ์์์ง๋ง ์คํ๋ง ์ธํฐ์ํฐ ๊ฐ์ ๊ฒฝ์ฐ์๋ ์ธํฐ์ํฐ๋ฅผ ๋ฑ๋กํ๋ ๋ถ๋ถ์์ "์ ์ธํ  URL"์ ์ง์ ํ  ์ ์๊ธฐ ๋๋ฌธ์ ์ฌ๊ธฐ์ ํ์ง ์๋๋ค.    
-> ์๋ธ๋ฆฟ ํํฐ๋ณด๋ค ํธํ๊ณ  ์ข์์ง๋ค.

-------------------------------------
```
public class WebConfig implements WebMvcConfigurer {

    /** ์คํ๋ง ์ธํฐ์ํฐ ์ฌ์ฉ - ์ธํฐํ์ด์ค์ ๋ฉ์๋ ๊ตฌํํ์ฌ ์ฌ์ฉ(์๋์ผ๋ก ์คํ๋ง ์ธํฐ์ํฐ๋ก ๋ฑ๋ก) - Log ๊ธฐ๋ฅ์ ์ธํฐ์ํฐ , ๋ก๊ทธ์ธ ํ์ธ ๊ธฐ๋ฅ์ ์ธํฐ์ํฐ **/
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LogInterceptor())
                .order(1)
                .addPathPatterns("/**") //๋ชจ๋  ๊ฒฝ๋ก!!!!!
                .excludePathPatterns("/css/**", "/*.ico", "/error");

        registry.addInterceptor(new LoginCheckInterceptor())
                .order(2)
                .addPathPatterns("/**")
                .excludePathPatterns("/", "/member/add", "/login", "/logout", "/css/**","/*.ico", "/error");
    }

}
```
- ์ธํฐ์ํฐ๋ฅผ ์คํ๋ง ์ธํฐ์ํฐ๋ก ๋ฑ๋กํ๋ ๋ถ๋ถ ์ฝ๋ -> "WebMvcConfigurer" ์ธํฐํ์ด์ค๋ฅผ ๊ตฌํํ๊ฒ ๋๋ฉด ์๋์ผ๋ก ์คํ๋ง ์ธํฐ์ํฐ๋ก ๋ฑ๋ก๋๋ค.
- ์ธํฐ์ํฐ ํ์ง์์ ํน์  URL๋ค์ ๋ฃ์ด ์ธํฐ์ํฐ๋ก ๋ฑ๋กํ  ์ ์๋ค.






