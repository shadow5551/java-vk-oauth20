# API for interacting with vk.com #

Moved to https://github.com/alexeygrigorev/java-vk-oauth20

## Interacting ##
Interacting with vk.com consists of the following steps:

  1. Register an application and obtain appId and secret code
  1. Show for a user authorization pop-up
  1. Handle access code sent from vk.com
  1. Get access token
  1. Invoke api methods

Additional details are at http://vk.com/developers.php

## java-vk-oauth20 ##
This library wraps http-interaction with vk.com using HttpClient

## Quick start ##

Maven dependency

```
<dependencies>
    <dependency>
        <groupId>ru.0agr</groupId>
        <artifactId>vkontakte-http-api</artifactId>
        <version>0.2</version>
    </dependency>
</dependencies>

<repositories>
    <repository>
        <id>java-vk-oauth20.googlecode.com</id>
        <url>http://java-vk-oauth20.googlecode.com/svn/m2/releases</url>
    </repository>
</repositories>
```


Spring configuration and Spring MVC contoller
```
<bean name="vkApi" class="com.googlecode.vkapi.HttpVkApi">
    <constructor-arg index="0" value="${vk.app.id}" />
    <constructor-arg index="1" value="${vk.app.key}" />
    <constructor-arg index="2" value="${vk.app.response_uri}" />
</bean>
```

```
@Controller
public class VkController {
    private final VkApi vkApi;

    @Autowired
    public VkController(VkApi vkApi) {
        this.vkApi = vkApi;
    }

    @RequestMapping(value = "/vk/auth", method = RequestMethod.GET)
    public String auth() {
        return "redirect:" + vkApi.getAuthUri();
    }

    @RequestMapping(value = "/vk/process", method = RequestMethod.GET)
    public String process(@RequestParam(value = "code", required = false) String code) throws VkException {
        if (code != null) {
            OAuthToken token = vkApi.authUser(code);
            ...
        }
        // ...
    }

    @ExceptionHandler(VkTokenExpiredException.class)
    public ModelAndView handleVkTokenExpiredException(Principal principal, VkTokenExpiredException ex) {
        // handle as appropriately
    }
}
```

## Download ##
If, for any reasons, you don't use maven, and don't want to build artifacts yourself, here is [the jar for downloading](http://java-vk-oauth20.googlecode.com/svn/m2/releases/ru/0agr/vkontakte-http-api/0.2/vkontakte-http-api-0.2.jar)

## About ##
Anyone is free to contribure - ask me for permissions at _alexey.s.grigoriev(a)gmail(.)com_

This is released under [WTFPL](http://sam.zoy.org/wtfpl/COPYING), which means you can do anything you want with this code.