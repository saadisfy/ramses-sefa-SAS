# Docker Compose file
    - need to proper dependency, don't let everythign depend on everything because it will make the compose up time to high 


# RAMSES Implementations

## PLAN
    - PlanApplication.java uses library that only works for arm64. meaning if the Docker image with tag "amd64" is used, it will not work 
        - **Error**: Caused by: java.lang.UnsatisfiedLinkError: /app/libjniortools.so: /app/libjniortools.so: cannot open shared object file: No such file or directory (Possible cause: can't load AARCH64 .so on a AMD 64 platform)
        - **CurrentSolution**: use arm64 and emulate


# Actuator 

## Instance Manager 
    - the application property has to specify docker host. but it is hardcoded for arm64