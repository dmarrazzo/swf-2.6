# Serverless Workflow Sample Project

## Greeting

Launch the process:

curl -X POST -H 'Content-Type:application/json' -H 'Accept:application/json' -d '{ "workflowdata": { "name": "donato"} }' http://localhost:8080/greeting


## Correlation

Enable persistence:

1. Dependencies:

```xml
<dependency>
    <groupId>org.kie.kogito</groupId>
    <artifactId>kogito-addons-quarkus-persistence-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-jdbc-postgresql</artifactId>
</dependency>
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-agroal</artifactId>
</dependency>
```

2. Properties:

```
#Persistence configuration
kogito.persistence.type=jdbc

#run create tables scripts
kogito.persistence.auto.ddl=true
kogito.persistence.proto.marshaller=true

quarkus.datasource.db-kind=postgresql
```

Start the process:

```sh
curl -X POST -H "Content-Type: application/json" -H "ce-specversion: 1.0" -H "ce-source: greetEvent" -H "ce-type: startEventType" -H "ce-id: f0643c68-609c-48aa-a820-5df423fa4fe0" -d '{ "name":"Donato" }' http://localhost:8080
```

```sh
curl -X POST -H "Content-Type: application/json" -H "ce-specversion: 1.0" -H "ce-source: greetEvent" -H "ce-type: continueEventType" -H "ce-id: f0643c68-609c-48aa-a820-5df423fa4fe0" -d '{ "name":"Donato" }' http://localhost:8080
```