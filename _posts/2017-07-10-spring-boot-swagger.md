---
layout: post
title:  "Swagger Spring boot two steps"
date:   2017-07-10 15:39:46 -0500
categories: java springboot
---

##SPRING BOOT SWAGGER 2 GRADLE

1) Add dependencies on build.gradle

{% highlight gradle linenos %}
    compile 'io.springfox:springfox-swagger2:2.6.1'
    runtime 'io.springfox:springfox-swagger-ui:2.6.1' 
{% endhighlight %}
  
2) Add configuration file for swagger with a new Docket that contains the path of the api and the path of the url.

{% highlight java linenos %}

import com.google.common.base.Predicate;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

import static com.google.common.base.Predicates.*;

import static springfox.documentation.builders.PathSelectors.regex;

/**
 * Created by User on 7/10/17.
 */
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket dataApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()                 .apis(RequestHandlerSelectors.any())
                .paths(paths())
                .build().apiInfo(apiInfo());

    }


    //Here is an example where we select any api that matches one of these paths
    private Predicate<String> paths() {
        return or(
                regex("/data.*"),
                regex("/test.*"));
    }


    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("Test Api")
                .description("DESCRIPTION")
                .version("VERSION")
                .termsOfServiceUrl("http://terms-of-services.url")
                .license("LICENSE")
                .licenseUrl("http://url-to-license.com")
                .build();
    }
}


{% endhighlight %}



  
  
  

