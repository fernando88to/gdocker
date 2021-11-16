### Project Docker + GRAILS

1. grails version : 5.0.1

2. java version: 11

### First step => Create debug remote

Open o intellij idea and click 'Edit Configurations' after  add 'Remote JVM Debug'  finally copy the following code:

```
-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005
```


### Second step => Change file build.gradle 


```
bootRun {
    ignoreExitValue true
    jvmArgs(
        '-Dspring.output.ansi.enabled=always', 
        '-noverify', 
        '-XX:TieredStopAtLevel=1',
        '-Xmx1024m'        
    sourceResources sourceSets.main
    String springProfilesActive = 'spring.profiles.active'
    systemProperty springProfilesActive, System.getProperty(springProfilesActive)
}
```

to 


```
bootRun {
    ignoreExitValue true
    jvmArgs(
        '-Dspring.output.ansi.enabled=always', 
        '-noverify', 
        '-XX:TieredStopAtLevel=1',
        '-Xmx1024m',
        '-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005')
    sourceResources sourceSets.main
    String springProfilesActive = 'spring.profiles.active'
    systemProperty springProfilesActive, System.getProperty(springProfilesActive)
}
```

### Thid step => up compose

```
docker-compose up -d
```

### Fourth step => Start debug of the intellij idea

Click on the debug icon.


### Fifth step 

Open browser and go to http://localhost:8080/

