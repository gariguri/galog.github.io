---
layout: post
title: "인프런- 스프링 입문 '회고록1'"
categories:
  -Blog
tags:
  - [Blog, Github, Git] 
toc: true
toc_sticky: true
date: 2024-01-18
last_modified_at: 2024-01-18
---

## 인프런-스프링 입문 회고록1


* 스프링 웹 개발 기초
  1. 정적 컨텐츠
    - 정적 컨텐츠는 html내용을 그대로 전달하여 보여주기만 함.
    * 예시)
    ```html
      <!DOCTYPE HTML>
      <html>
      <head>
        <title>static content</title>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
      </head>
      <body>
      정적 컨텐츠입니다.
      </body>
      </html>
    ```
    <img src="" width="450px" height="300px" alt="정적컨텐츠"></img><br/>

  2. MVC와 템플릿 엔진
  - MVC: Model, View, Controller
  * 예시) 
    #### Controller
    ```java
    @Controller
    public class HelloController{
      @GetMapping("hello-mvc")
      public String helloMvc(@RequestParam("name") String name, Model, model){
        model.addAttribute("name", name);
        return "hello-template";
      }
    }
    ```
    ##### 반환한 hello template은 아래와 같다.
    #### View
    ```html
    resources/templates/hello-template.html
    <html xmlns:th="http://www.thymeleaf.org">
    <body>
    <p th:text='hello '+ ${name}">hello! empty</p>
    </body>
    </html>
    ```
    ##### Controller에서 반환한 View html코드가 화면에 보이게 됨.   
        
  3. API
  - @ResponseBody을 사용하면 veiwResolver를 사용하지 않음.
  - 대신에 HttpMessageConverter가 동작하여 HTTP의 Body에 문자 내용을 직접 반환함(html body tag를 말하는 것이 아님!!!)
  - 기본 문자 처리: StringHttpMessageConverter
  - 기본 객체 처리: MappingJackson2HttpMessageConverter
  - 예시)   
    #### ResponseBody 문자 반환

    ```java
    @Controller
    public class HelloController{
      @GetMapping("hello-string")
      @ResponseBody
      public String helloString(@RequestParam("name")String name){
        return "hello "+name;
      }
    }
    ```
    ##### 실행: http://localhost:8080/hello-string?name=spring   
    ##### 실행 결과: hello spring
    #### ResponseBody 객체 반환
    ```java
    @Controller
    public class HelloController{
      @GetMapping("hello-api")
      @ResponseBody
      public Hello helloApi(@RequestParam("name") String name){
        Hello hello=new Hello();
        hello.setName(name);
        return hello;
      }
      static class Hello{
        private String name;
        public String getName(){
          return name;
        }
        public void setName(String name){
          this.name=name;
        }
      }
    }
    ```
    ##### @ResponseBody를 사용하고 객체로 반환할때에는 객체가 json형태로 변환됨.




