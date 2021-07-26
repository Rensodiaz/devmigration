---
title: "Logging in your Spring-boot application with LogBook"
date: 2021-03-26T22:02:54-04:00
author: "Renso Diaz"
draft: false
 
tags: ['Spring-boot', 'Log', 'LogBook']
 
noSummary: false
 
categories: ['Articles']
 
resizeImages: false
---
Logging requests and responses is easy but sometimes it turns into a complicated mess because we need to change and figure out where things are being printed or which files are doing it. So a great solution for that is [**Logbook**](https://github.com/zalando/logbook), because it allows you to modify Beans with your own filters and intercept the requests and responses in one single place.

***Add dependencies to the Pom.xml file:***
```XML
<!-- LogBook dependency -->
<dependency>
    <groupId>org.zalando</groupId>
    <artifactId>logbook-spring-boot-starter</artifactId>
    <version>2.4.2</version>
</dependency>
```
***Add properties to the application.properties file:*** 

here we can set how logbook is going to behave, like what level of logging, which file to exclude or if we want to enable filters and many more:

```
### Logbook Configurations ###
logging.level.ROOT=INFO
logbook.filter.enabled=true
logbook.exclude=/actuator/**, /swagger-ui**, /api-docs**
logbook.format.style=json
logbook.secure-filter.enabled=true
logging.level.org.zalando.logbook=TRACE
### This property is not part of logbook but to have a cleaner support when masking 
fields we will use for our configuration ###
application.maskFields=my,field,to,mask
```

**Note:** With the above steps you can run you service endpoints and see logs coming in and out but if you want to configure logbook to your needs keep reading.

***Creating configuration file:***
```java
import com.macys.enclave.rest.tracing.LogbookMasking;
import org.json.JSONException;
import org.json.JSONObject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.zalando.logbook.BodyFilter;
import static org.zalando.logbook.BodyFilter.merge;
import static org.zalando.logbook.BodyFilters.defaultValue;

@Configuration
public class LogBookConfig extends LogbookMasking {
    private static final Logger LOGGER = LoggerFactory.getLogger(LogBookConfig.class);

    @Value("${application.maskFields}")
    private String[] propertiesToMask;

    @Bean
    public BodyFilter bodyFilter() {
        BodyFilter bodyFilter = merge(defaultValue(), maskFieldsAndObjects());
        return bodyFilter;
    }

    private BodyFilter maskFieldsAndObjects() {
        return (contentType, body) -> {
            if (body != null && body.length() > 0) {
                try {
                    JSONObject json = new JSONObject(body);
                    maskJsonObject(json, propertiesToMask);
                    return ("\n" + json + "\n");
                } catch (JSONException e) {
                    if (LOGGER.isErrorEnabled())
                        LOGGER.error("error parsing body as json to pretty-print it", e);
                }
            }
            return (body);
        };
    }
}
```