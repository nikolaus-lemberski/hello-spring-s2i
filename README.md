# Hello Spring with s2i

Simple Spring Boot app to demo OpenShift source-to-image.

## Endpoints:

* `/`  -> says hello
* `/actuator/health/readiness`
* `/actuator/health/liveness`

## Build with s2i

```bash
oc new-app openshift/java:openjdk-17-ubi8~https://github.com/nikolaus-lemberski/hello-spring-s2i
```

## Make public route

```bash
oc create route edge --insecure-policy='Redirect'  --service=hello-spring-s2i
oc get route
```

## Add healthchecks

```bash
oc set probe deployment hello-spring-s2i --readiness --get-url=http://:8080/actuator/health/readiness --initial-delay-seconds=10 --period=5
oc set probe deployment hello-spring-s2i --liveness --get-url=http://:8080/actuator/health/liveness --initial-delay-seconds=10 --period=5
```
