๐__์๋ธ๋ฆฟ ํํฐ__
==========================
> ์ ํ๋ฆฌ์ผ์ด์ ์ฌ๋ฌ ๋ก์ง์์ ์น๊ณผ ๊ด๋ จ๋ ์ฌ๋ฌ ๊ณตํต ๊ด์ฌ์ฌํญ์ ํด๊ฒฐํ ๋ ์ฌ์ฉ๋๋ ๊ธฐ๋ฅ์ด๋ค.    
> ๊ณตํต ๊ด์ฌ์ฌํญ์ด๋ผ AOP๋ก ์ฒ๋ฆฌํ ์ ์์ง๋ง HTTP์ URL ๊ด๋ จ๋ ๊ณตํต๊ด์ฌ์ฌํญ์ ์ฒ๋ฆฌํ  ๋๋ ์๋ธ๋ฆฟ ํํฐ, ์คํ๋ง ์ธํฐ์ํฐ๋ฅผ ์ฌ์ฉํ๋ ๊ฒ์ด ์ข๋ค.      

![image](https://user-images.githubusercontent.com/96917871/158068206-03b1c55b-65c4-4b6a-9c9d-22a9d17b0755.png)      
- ํํฐ๋ ์๋ธ๋ฆฟ์ด ์ง์ํ๋ ์๋ฌธ์ฅ๊ณผ ๊ฐ์ ์ญํ ์ ํ๋ค.
- ์ฌ๊ธฐ์ ์๋ธ๋ฆฟ์ ์คํ๋ง์ ๋์คํจ์ฒ ์๋ธ๋ฆฟ์ด๊ณ  WAS(ํฐ์บฃ)์์ ํํฐ๋ฅผ ๊ฑฐ์ณ ํต๊ณผ๋ ๊ฒฝ์ฐ์๋ง ์๋ธ๋ฆฟ์ ํธ์ถํ๊ณ  ์ปจํธ๋กค๋ฌ๋ฅผ ํธ์ถํ  ์ ์๋ค.

- ํํฐ๋ ์ฒด์ธ์ผ๋ก ๊ตฌ์ฑ๋๋๋ฐ, ์ฌ๋ฌ ํํฐ๋ฅผ ๋ฃ์์ ์๋ค. ์ฌ์ฉ์๊ฐ ์ ์ํ ์ฐ์ ์์ ๋๋ก๋ ํํฐ๋ฅผ ์ ์ฉํ  ์ ์๋ค.
- ์๋ธ๋ฆฟ ํํฐ๋ "doFilter()"ํ๋๋ง ์ฌ์ฉํ์ฌ ์๋ธ๋ฆฟ์ด ์คํ๋๊ธฐ์ ์ ํํฐ๊ฐ ์ ์ฉ๋๊ธฐ ๋๋ฌธ์ ์ปจํธ๋กค๋ฌ ํธ์ถ์ ํ, ์๋ฃ(View reader)์ ๊ฐ์ด ๋จ๊ณ์ ์ผ๋ก ์ธ๋ถํ ์ํค์ง ๋ชปํ๊ณ  doFilter๋ฉ์๋ ๋ด์์ try,finally๋ฅผ ํตํด ์๋ธ๋ฆฟ ํธ์ถ์ , ์ปจํธ๋กค๋ฌ ํธ์ถ ํ ๋ทฐ ๋ ๋๋ง ํ   
-> ์ด๋ ๊ฒ ๋๊ฐ๋ก๋ง ๋ถํ ํ์ง ๋ชปํ๋ค.      
--> ๋ค์ ๋์ค๋ ์ธํฐ์ํฐ๋ ์ปจํธ๋กค๋ฌ ํธ์ถ ์  ํ, ์๋ฃ ๋จ๊ณ๋ก ์ธ๋ถํ ๋์ด ์๋ค.    

__์๋ธ๋ฆฟ ํํฐ ์ฌ์ฉ1 -> log ๋จ๊ธฐ๊ธฐ__
============================
```
@Bean
 public FilterRegistrationBean logFilter() {
 FilterRegistrationBean<Filter> filterRegistrationBean = new FilterRegistrationBean<>();
 filterRegistrationBean.setFilter(new LogFilter());
 filterRegistrationBean.setOrder(1);
 filterRegistrationBean.addUrlPatterns("/*");
 return filterRegistrationBean;
 }
```
- ์คํ๋ง ๋ถํธ๋ฅผ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ ๋น์ผ๋ก "FilterRegistrationBean" ๊ฐ์ฒด๋ฅผ ์์ฑํ์ฌ ๋น์ผ๋ก ๋ฑ๋กํ๋ฉด ๋๋ค.
-> ์คํ๋ง๋ถํธ๊ฐ ์ฌ๋ผ์ฌ๋ WAS(์๋ธ๋ฆฟ ์ปจํ์ด๋)๋ ๊ฐ์ด ์ฌ๋ ค ์๋ธ๋ฆฟ์ ์ฌ์ฉํ ์ ์๊ฒ ๋๊ณ  ์๋ธ๋ฆฟ ์ปจํ์ด๋๊ฐ ์ฌ๋ผ์ค๋ฉด์ ํด๋น "์๋ธ๋ฆฟ ํํฐ"๊ฐ ๋น์ผ๋ก ๋ฑ๋ก๋์ด ์์ผ๋ฉด ์๋ธ๋ฆฟ ์ปจํ์ด๋๊ฐ ํํฐ๋ฅผ ์ฑ๊ธํค ๊ฐ์ฒด๋ก ์์ฑํ๊ณ  ๊ด๋ฆฌํ๋ค.         
--> http ์์ฒญ์ WAS๋ฅผ ํตํด ์๋ธ๋ฆฟ ์ปจํ์ด๋์ ํํฐ๋ฅผ ๊ฑฐ์น๊ณ  ์๋ธ๋ฆฟ(๋์คํจ์ฒ ์๋ธ๋ฆฟ)์ผ๋ก ์ด๋ํ๊ฒ ๋๋ค.     

- setFilter(new LogFilter()) : ๋ฑ๋กํ  ํํฐ๋ฅผ ์ง์ ํ๋ค. -> ์๋ธ๋ฆฟ์์ ์ ๊ณตํ๋ "Filter" ์ธํฐํ์ด์ค๋ฅผ ๊ตฌํํ ๊ตฌํ์ฒด(LogFilter)    
- setOrder(1) : ํํฐ๋ ์ฒด์ธ์ผ๋ก ๋์ํ๋ค. ๋ฐ๋ผ์ ์์๊ฐ ํ์ํ๋ค. ๋ฎ์ ์๋ก ๋จผ์  ๋์ํ๋ค.
- addUrlPatterns("/*") : ํํฐ๋ฅผ ์ ์ฉํ  URL ํจํด์ ์ง์ ํ๋ค. ํ๋ฒ์ ์ฌ๋ฌ ํจํด์ ์ง์ ํ  ์ ์๋ค. -> ์ฌ๊ธฐ์  ๋ชจ๋  URL๋ฅผ ๋์์ผ๋ก

---------------------------------------------------------
```
public class LogFilter implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("log filter init");
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        /**
         * ServletRequest ๋ HttpServletReqeust ์ ๋ถ๋ชจ์ด๊ธฐ ๋๋ฌธ์ ๋ค์ด ์บ์คํ ํ์ฌ ์ฌ์ฉ
         * ServletRequest request ๋ HTTP ์์ฒญ์ด ์๋ ๊ฒฝ์ฐ๊น์ง ๊ณ ๋ คํด์ ๋ง๋  ์ธํฐํ์ด์ค์ด๋ค.
         */
        HttpServletRequest httpRequest = (HttpServletRequest) request;

        String requestURI = httpRequest.getRequestURI();
        String uuid = UUID.randomUUID().toString();

        try {
            log.info("REQUEST [{}] [{}]", uuid, requestURI);
            chain.doFilter(request, response); //๋ค์ ํํฐ๊ฐ ์์ผ๋ฉด ๋ค์ ํํฐ ์ ์ฉ, ์์ผ๋ฉด ์๋ธ๋ฆฟ ํธ์ถ !!!!!!!๊ฐ์ฅ ์ค์ํ ๋ถ๋ถ

        }catch(Exception e) {
            throw e;
        }finally {
            log.info("REQUEST [{}] [{}]",uuid,requestURI);
        }
    }

    @Override
    public void destroy() {
        log.info("log filter destroy");
    }
}
```
- ์๋ธ๋ฆฟ์์ ์ ๊ณตํ๋ "Filter" ์ธํฐํ์ด์ค๋ฅผ ๊ตฌํํ ๊ตฌํ์ฒด(LogFilter)
- ํํฐ ์ธํฐํ์ด์ค(Filter)๋ฅผ ๊ตฌํํ๊ณ  ๋ฑ๋กํ๋ฉด ์๋ธ๋ฆฟ ์ปจํ์ด๋๊ฐ ํํฐ๋ฅผ ์ฑ๊ธํค ๊ฐ์ฒด๋ก ์์ฑํ๊ณ , ๊ด๋ฆฌํ๋ค.
- init(): ํํฐ ์ด๊ธฐํ ๋ฉ์๋, ์๋ธ๋ฆฟ ์ปจํ์ด๋๊ฐ ์์ฑ๋  ๋ ํธ์ถ๋๋ค.
- doFilter(): ๊ณ ๊ฐ์ ์์ฒญ์ด ์ฌ ๋ ๋ง๋ค ํด๋น ๋ฉ์๋๊ฐ ํธ์ถ๋๋ค. ํํฐ์ ๋ก์ง์ ๊ตฌํํ๋ฉด ๋๋ค.
- destroy(): ํํฐ ์ข๋ฃ ๋ฉ์๋, ์๋ธ๋ฆฟ ์ปจํ์ด๋๊ฐ ์ข๋ฃ๋  ๋ ํธ์ถ๋๋ค
- ์ต๊ทผ์๋ "Filter" ์ธํฐํ์ด์ค์ ํญ๋ชฉ์ ๋ชจ๋ ๊ตฌํ ํ์ง ์์๋ ๋๊ณ  "doFilter()"์ ๋ฉ์๋๋ง ๊ตฌํํ๋ฉด ๋๋ค.


__์๋ธ๋ฆฟ ํํฐ ์ฌ์ฉ2 -> login ํ์ธ__
================================
```
/** ์๋ธ๋ฆฟ ํํฐ ์ฌ์ฉ - ๋ก๊ทธ์ธ ํ์ธ ๊ธฐ๋ฅ์ ํํฐ **/
@Slf4j
public class LoginCheckFilter implements Filter {

    //๊ฒ์ฌ๋ฅผ ํ์ง์์ URL!
    private static final String[] whitelist = {"/", "/members/add", "/login", "logout", "/css/*"};

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {

        HttpServletRequest httpRequest = (HttpServletRequest) request;
        String requestURI = httpRequest.getRequestURI();

        HttpServletResponse httpResponse = (HttpServletResponse) response;

        try {
            log.info("์ธ์ฆ ์ฒดํฌ ํํฐ ์์ {}", requestURI);

            if(isLoginCheckPath(requestURI)) {
                log.info("์ธ์ฆ ์ฒดํฌ ๋ก์ง ์คํ {}",requestURI);

                HttpSession session = httpRequest.getSession();
                //๋ก๊ทธ์ธ ํ์ง ์์ ์ฌ์ฉ์
                if(session==null || session.getAttribute(LOGIN_MEMBER)==null) {
                    log.info("๋ฏธ์ธ์ฆ ์ฌ์ฉ์ ์์ฒญ {}",requestURI);

                    /**
                     * ๋ก๊ทธ์ธ์ผ๋ก redirect -> response "redirect"์ ๊ธฐ๋ฅ์ ๋ฉ์๋ ์ฌ์ฉ
                     * ์ถ๊ฐ๋ก ์ฌ์ฉ์๊ฐ ๋ค์ด์จ URL์ ์ฟผ๋ฆฌํ๋ผ๋ฏธํฐ๋ก ๋๊ธฐ๋ฏ๋ก์จ ์ฌ์ฉ์๊ฐ login ํ ํ ์ฒ์ ๋ค์ด์จ URL๋ก ๋ณด๋ด์ฃผ๊ธฐ ์ํด์ -> ํ๋ผ๋ฏธํฐ๋ก ๋๊น
                     */
                    httpResponse.sendRedirect("/login?redirectURL=" + requestURI);

                    return; //์ฌ๊ธฐ๊ฐ ์ค์, ๋ฏธ์ธ์ฆ ์ฌ์ฉ์๋ ๋ค์์ผ๋ก ์งํํ์ง ์๊ณ  ๋!!! -> ์๋ธ๋ฆฟ๋ ํธ์ถ ํ์ง ์๋๋ค.!!
                }
            }

            chain.doFilter(request, response);
        }catch(Exception e) {
            throw e;
        }finally {
            log.info("์ธ์ฆ ์ฒดํฌ ํํฐ ์ข๋ฃ {}", requestURI);
        }
    }

    /**
     * ํ์ดํธ ๋ฆฌ์คํธ์ ๊ฒฝ์ฐ ์ธ์ฆ ์ฒดํฌX
     */
    private boolean isLoginCheckPath(String requestURI) {
        return !PatternMatchUtils.simpleMatch(whitelist, requestURI); //๊ฒ์ฌํด์ผํ  URI์ผ ๊ฒฝ์ฐ true
    }

}
```
- Http ์์ฒญ์ ์ฒ๋ฆฌํ๊ธฐ ์ํด ์๋ธ๋ฆฟ(๋์คํจ์ฒ ์๋ธ๋ฆฟ)์ด ํธ์ถ๋๊ธฐ์  ์๋ธ๋ฆฟ ์ปจํ์ด๋์ "์๋ธ๋ฆฟ ํํฐ" (๋ก๊ทธ์ธ ํ์ธ ๊ธฐ๋ฅ)๋ฅผ ์ฌ์ฉํ๋ค.
- ๋ก๊ทธ์ธ ํ์ง ์์ ์ด์ฉ์(session์ ํด๋น ๊ฐ์ด ์๋)๋ "return"์ ํตํด ์๋ธ๋ฆฟ์ ํธ์ถํ์ง ์๊ณ  "redirect"๋ฅผ response์ ๋ด์ผ๋ฏ๋ก ์น ๋ธ๋ผ์ฐ์ ๊ฐ redirect์์ฒญ์ ํ๋ค.!!!
- ํด๋น URL๋ฅผ ์ ์ฉ ์ฌ๋ถ๋ฅผ ์๋ ์ฝ๋์ ๊ฐ์ด ์๋ธ๋ฆฟ ์ปจํ์ด๋์ ํํฐ(FiterRegistraionBean)๋ก ๋ฃ์๋ URL๋ฅผ ์ง์ ํด์ค ์ ์์ง๋ง ๊ทธ๋ ๊ฒ ๋๋ฉด ์๋น์ค๊ฐ ํ์ฅ๋์์ ๋ ์๋ ๋ถ๋ถ์ ์ฝ๋๊น์ง ๋ณ๊ฒฝํด์ผ๋๊ธฐ ๋๋ฌธ์ ํํฐ๋ด์์ URL๋ฅผ ํ๋ณํ๋ ์์์ ํด์ค๋ค.    
-> ๋ํ ์๋ธ๋ฆฟ ํํฐ๋ ํด๋น ํํฐ๋ฅผ ๋ฑ๋กํ  ๋ ํน์  URL์ ๋ฃ์์ ์์ง๋ง ์ ์ธํ ์ ์๋ ๊ธฐ๋ฅ์ ์๋ค.    
--> ์คํ๋ง ์ธํฐ์ํฐ๋ ์ธํฐ์ํฐ๋ฅผ ๋ฑ๋กํ  ๋ ์ ์ธํ  URL๋ฅผ ์ง์ ํ ์ ์๋ค.

-------------------------------------

```
/** ์๋ธ๋ฆฟ ํํฐ ์ฌ์ฉ - ๋ก๊ทธ์ธ ํ์ธ ๊ธฐ๋ฅ์ ํํฐ ๋ฑ๋ก**/
    @Bean
    public FilterRegistrationBean loginCheckFilter() {
        FilterRegistrationBean<Filter> filterRegistrationBean = new FilterRegistrationBean<>();

        filterRegistrationBean.setFilter(new LoginCheckFilter());
        filterRegistrationBean.setOrder(2);
        filterRegistrationBean.addUrlPatterns("/*");

        return filterRegistrationBean;
    }
```
- ํํฐ ๋ฑ๋ก ์ฝ๋
- ์๋ธ๋ฆฟ ํํฐ๋ ํด๋น ํํฐ๋ฅผ ๋ฑ๋กํ  ๋ ํน์  URL์ ๋ฃ์์ ์์ง๋ง ์ ์ธํ ์ ์๋ ๊ธฐ๋ฅ์ ์๋ค.        
--> ์คํ๋ง ์ธํฐ์ํฐ๋ ์ธํฐ์ํฐ๋ฅผ ๋ฑ๋กํ  ๋ ์ ์ธํ  URL๋ฅผ ์ง์ ํ ์ ์๋ค.











