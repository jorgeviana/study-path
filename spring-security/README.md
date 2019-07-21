# spring security

https://stackoverflow.com/questions/25633477/security-configuration-with-spring-boot?rq=1

Per the docs you have disabled the spring boot autoconfig in the first example by using @EnableWebSecurity, so you would have to explicitly ignore all the static resources manually. In the second example you simply provide a WebSecurityConfigurer which is additive on top of the default autoconfig.

answered Sep 3 '14 at 6:13

Dave Syer



---------------------------
@Bean
    WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/**")
                        .allowedOrigins("*")
                        .allowedMethods("HEAD", "GET", "PUT", "POST", "DELETE", "PATCH");
            }
        };
    }

    
http
                .csrf().disable()
                .cors().and()
---------------------------


----------------------
https://shout.setfive.com/2015/11/02/spring-boot-authentication-with-custom-http-header/
https://gist.github.com/adatta02/1103f7a81a5e6c1931bf9a09b6131d59
----------------------


https://stackoverflow.com/questions/22767205/spring-security-exclude-url-patterns-in-security-annotation-configurartion

@Override
    public void configure(WebSecurity web) {
        web.ignoring().antMatchers("/");
    }

----------------------
public ModelAndView someRequestHandler(Principal principal) {
   User activeUser = (User) ((Authentication) principal).getPrincipal();
   ...
}
----------------------

Spring Guides Security

The WebSecurityConfig class is annotated with @EnableWebSecurity to enable Spring Security’s web security support and provide the Spring MVC integration. It also extends WebSecurityConfigurerAdapter and overrides a couple of its methods to set some specifics of the web security configuration.

@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/", "/home").permitAll()
                .anyRequest().authenticated()
                .and()
            .formLogin()
                .loginPage("/login")
                .permitAll()
                .and()
            .logout()
                .permitAll();
    }

    @Bean
    @Override
    public UserDetailsService userDetailsService() {
        UserDetails user =
             User.withDefaultPasswordEncoder()
                .username("user")
                .password("password")
                .roles("USER")
                .build();

        return new InMemoryUserDetailsManager(user);
    }
}
The WebSecurityConfig class is annotated with @EnableWebSecurity to enable Spring Security’s web security support and provide the Spring MVC integration. It also extends WebSecurityConfigurerAdapter and overrides a couple of its methods to set some specifics of the web security configuration.

The configure(HttpSecurity) method defines which URL paths should be secured and which should not. Specifically, the "/" and "/home" paths are configured to not require any authentication. All other paths must be authenticated.

When a user successfully logs in, they will be redirected to the previously requested page that required authentication. There is a custom "/login" page specified by loginPage(), and everyone is allowed to view it.

As for the userDetailsService() method, it sets up an in-memory user store with a single user. That user is given a username of "user", a password of "password", and a role of "USER".