---
layout:
title: "Day 2 - Spring Boot"
date: 2024-12-07 0:13:20 -0000
categories: [Personal]
tags: [java, spring_boot]
---

So... I really started my journey with Java as i said in my last post, and I'm really enjoying it! I mean... **every** technology that you learn is a new world that you discover and sometimes can be very hard, but it's normal! But, with **Java** i didn't have this feeling, maybe because i already have some experience with other languages, but i think that Java can be a good language to have in your toolbelt!

## Verbosity
One thing that everybody says about Java: `"Java is too verbose"`, and I can say that this is true! But, you need to understand why Java is so verbose! Java is a language that has a strong type system, and because of that, Java needs to know the type of everything that you are using, and this is the reason why Java is so verbose!
You might think something like: `"But Python are not so verbose! and Python has a strong type system!"`, and you are right! But Python is a language that has a dynamic type system, and because of that, Python can infer the type of the variables that you are using! But Java is a language that has a static type system, and because of that, Java needs to know the type of everything that you are using!

### Runtime errors vs Compile time errors
Since we are talking about verbosity and using a strong type system, I think that is important to talk about the difference between runtime errors and compile time errors!
In Python, for example, you can have a runtime error when you are trying to sum a string with a number, and this is because Python is a language that has a dynamic type system, and Python can't infer that you are trying to sum a string with a number! But in Java, you can't do this, because Java is a language that has a static type system, and Java knows that you are trying to sum a string with a number, and Java will give you a compile time error!

#### Note!
I know, I know... You can use the Type hinting in Python to avoid this kind of error, but this is not the point! The point is that Java is a language that has a strong type system, and because of that, Java can give you a compile time error when you are trying to do something that is not allowed!

## Spring Boot
Now... Let's talking about Spring Boot, finally!
So, today is my second day with Java, and after I learn the basics of Java, like variables, loops, classes, collections and etc, I decided to learn Spring Boot, and i mean... I just started to learn Spring Boot haha!
Then i can't say much about it, but i can say what I learned today! And is the directory structure of a Spring Boot project!

### Directory structure
When you create a new Spring Boot project, you will have a directory structure like this:

```
project-name
├── src
│   ├── main
│   │   ├── java    
│   │   │   └── com
│   │   │   │   └── example
│   │   │   │      └── projectname
│   │   │   │           ├── ProjectNameApplication.java
│   │   │   └── Resources
│   │   │       └── application.properties
|   |   └── test
|   |       └── java
|   |           └── com
|   |               └── example
|   |                   └── projectname
|   |                       └── ProjectNameApplicationTests.java
├── pom.xml
```
I'm not so good with ASCII art, but I think that you can understand the directory structure haha!

#### Src
The `src` directory is where your source code will be, and you will have two directories inside the `src` directory, the `main` directory and the `test` directory!

##### Main
The `main` directory is where your main code will be, and you will have two directories inside the `main` directory, the `java` directory and the `resources` directory!

###### Java
The `java` directory is where your Java code will be, and you will have a directory structure like this: `com.example.projectname`, and inside this directory, you will have your main class, that is the `ProjectNameApplication.java`!

###### Resources
The `resources` directory is where your resources will be, like static files, configuration files and etc, and you will have a file called `application.properties`!

##### Test
The `test` directory is where your tests will be, and you will have a directory structure like this: `com.example.projectname`, and inside this directory, you will have your test class, that is the `ProjectNameApplicationTests.java`!

#### Pom.xml
The `pom.xml` is a file that you will have all the dependencies of your project, and you can configure your project in this file!

## Conclusion
I'm do not think that this journey will be so difficult, after some years of experience in the field and multiple languages learned and frameworks used, I think that Java will be easy to learn, but I'm really excited to learn Spring Boot, and I will share my experiences here, so if you are interested in Java, stay tuned! And if you have any tips for me, please let me know! I will be very grateful!

**NOTE:** Until now... Elixir and Phoenix are still my favorite language and framework! But Java and Spring Boot are really good too! 
