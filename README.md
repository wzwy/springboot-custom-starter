# springboot-custom-starter


package com.wz.springboot;

public class Hello {

    private String msg;

    public String sayHello() {
        return "hello " + msg;
    }

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }
}


package com.wz.springboot;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration //为带有@ConfigurationProperties注解的Bean提供有效的支持。
// 这个注解可以提供一种方便的方式来将带有@ConfigurationProperties注解的类注入为Spring容器的Bean。
@EnableConfigurationProperties(HelloProperties.class)//开启属性注入,通过@autowired注入
@ConditionalOnClass(Hello.class)//判断这个类是否在classpath中存在，如果存在，才会实例化一个Bean
//当设置hello=enabled的情况下,值为true生效@Configuration注解，false失效@Configuration注解，
//配置文件不写就默认是matchIfMissing的值，要是在配置文件配置false则失效@Configuration注解，如果配置文件写只能写true，否则报错
//因为设置啦matchIfMissing=true，原因如果没有设置则默认为true,即条件符合
@ConditionalOnProperty(prefix="hello", value="enabled", matchIfMissing = true)
//当设置hello=enabled的情况下,并且值为true才生效@Configuration注解，并且抛出异常
//@ConditionalOnProperty(prefix="hello", value="enabled", matchIfMissing = false)
public class HelloAutoConfiguration {

    @Autowired
    private HelloProperties helloProperties;
    @Bean
    @ConditionalOnMissingBean(Hello.class)
    //容器中如果没有Hello这个类,那么自动配置这个Hello
    public Hello hello() {
        Hello hello = new Hello();
        hello.setMsg(helloProperties.getMsg());
        return hello;
    }

}


package com.wz.springboot;

import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties(prefix = "hello") //获取属性值
public class HelloProperties {
    private static final String MSG = "world";
    private String msg = MSG;

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }
}


META-INF-spring.factories
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.wz.springboot.HelloAutoConfiguration


<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

	<!--springboot最终打的jar包-->
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.wz</groupId>
	<artifactId>springboot-custom-starter</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>springboot-custom-starter</name>
	<description>Spring Boot</description>

	<properties>
		<!--jar版本-->
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-autoconfigure</artifactId>
			<version>2.0.4.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-configuration-processor</artifactId>
			<version>2.0.4.RELEASE</version>
			<optional>true</optional>
		</dependency>

	</dependencies>

</project>

{
    "opr":"listusr",
    "data":{
        "start":0,
        "limit":200,
        "filter":{
            "filterType":"all",
            "id":100,
            "gid":-1,
            "type":888
        },
        "search":"",
        "sort":"onlineTime",
        "direction":"ASC",
        "searchValue":"",
        "is_deposit":false,
        "is_central_area":false
    }
}
