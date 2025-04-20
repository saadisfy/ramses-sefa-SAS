# Docker Compose file
    - need to proper dependency, don't let everythign depend on everything because it will make the compose up time to high 


# RAMSES Implementations

## PLAN
    - PlanApplication.java uses library that only works for arm64. meaning if the Docker image with tag "amd64" is used, it will not work 
        - **Error**: Caused by: java.lang.UnsatisfiedLinkError: /app/libjniortools.so: /app/libjniortools.so: cannot open shared object file: No such file or directory (Possible cause: can't load AARCH64 .so on a AMD 64 platform)
        - **CurrentSolution**: use arm64 and emulate



### TODO: Decouple Analyse and Plan Services

#### Current Issue
- Analyse and Plan services are tightly coupled through adaptation options
- Both services need to know about the same set of adaptation options
- Changes to adaptation options require modifications in both services

#### Proposed Solution
1. **Analyse Service**
   - Should only detect and describe problems
   - Should not know about specific adaptation options
   - example output format:
     ```json
     {
       "serviceId": "SERVICE_ID",
       "problem": "QOS_VIOLATION",
       "metric": "METRIC_NAME",
       "currentValue": VALUE,
       "threshold": THRESHOLD
     }
     ```

2. **Plan Service**
   - Should receive problem descriptions
   - Should contain all knowledge about possible adaptations
   - Should make final decisions about which adaptations to apply

#### Benefits
- Single responsibility for each service
- Easier to add new adaptation options
- Better separation of concerns
- More maintainable and flexible system

#### Implementation Steps
1. Modify Analyse service to output problem descriptions
2. Update Knowledge service to store problem descriptions
3. Refactor Plan service to handle problem descriptions
4. Update communication between services

# Actuator 

## Instance Manager 
    - the application property has to specify docker host. but it is hardcoded for arm64