---
title: "Logs en tu aplicacion Spring-boot con LogBook"
date: 2021-03-26T22:02:54-04:00
author: "Renso Diaz"
draft: false
 
tags: ['Spring-boot', 'Log', 'LogBook']
 
noSummary: false
 
categories: ['Articulos']
 
resizeImages: false
---
 Logging peticiones y respuestas es fácil pero algunas veces se puede tornar un poco complicado y necesitamos entender y buscar cuales archivos y métodos están imprimiendo nuestra data. Para este tipo de problemas ya existe una herramienta que nos ayuda con este problema llamada logbook. [**Logbook**](https://github.com/zalando/logbook) es excelente porque nos ayuda a crear filtros que podemos usar para filtrar información importante que no queremos mostrar en los logs.

***Agregar dependencias en el archivo Pom.xml:***
```XML
<!-- LogBook dependency -->
<dependency>
    <groupId>org.zalando</groupId>
    <artifactId>logbook-spring-boot-starter</artifactId>
    <version>2.4.2</version>
</dependency>
```
***Agregar propiedades en el archivo de propiedades application.properties:*** 
aqui podemos configurar como logbook se va a comportar, el nivel de logging, cuales archivos excluir o cuáles filtros habilitar y muchos más.
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
**Nota:** Con el paso de arriba podemos correr nuestro servicio y enviar peticiones para poder ver los logs mostrandose en la terminal del mismo con información referente a las peticiones pero si quieres saber más sobre cómo configurar nuestros filtros sigue leyendo.

***Creando archivo de configuracion:***
```Java
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