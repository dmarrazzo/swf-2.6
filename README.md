# Serverless Workflow Sample Project

## Greeting

Launch the process:

```sh
curl -X POST -H 'Content-Type:application/json' -H 'Accept:application/json' -d '{ "workflowdata": { "name": "Donato"} }' http://localhost:8080/greeting
```

### Foreach

```sh
curl -X POST -H 'Content-Type:application/json' -H 'Accept:application/json' -d '{ "workflowdata": { "names": ["Donato", "Rachid"]}}' http://localhost:8080/greeting_foreach
```

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

### GraphQL

```graphql
{
  ProcessInstances {
    id
    processId
    processName
    businessKey
    parentProcessInstanceId
    roles
    variables
    state
    start
    lastUpdate
    end
    addons
    endpoint
    addons
    serviceUrl
    nodes {
      id
      nodeId
      name
      enter
      exit
      type
      definitionId
      __typename
    }
    __typename
  }
}
```

#### GraphQL Problem with correlation

```sh
curl -X POST -H "Content-Type: application/json" -H "ce-specversion: 1.0" -H "ce-source: coolSystem" -H "ce-type: cbStartEventType" -H "ce-id: f0643c68-609c-48aa-a820-5df423fa4f01" -d '{ "name":"Donato" }' http://localhost:8080

curl 'http://localhost:8180/graphql' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Origin: http://localhost:8180' \
  -H 'Referer: http://localhost:8180/graphiql/' \
  -H 'Sec-Fetch-Dest: empty' \
  -H 'Sec-Fetch-Mode: cors' \
  -H 'Sec-Fetch-Site: same-origin' \
  --data-raw '{"query":"{\n  ProcessInstances {\n    id\n    processId\n    processName\n    businessKey\n    parentProcessInstanceId\n    roles\n    variables\n    state\n    start\n    lastUpdate\n    end\n    addons\n    endpoint\n    addons\n    serviceUrl\n    nodes {\n      id\n      nodeId\n      name\n      enter\n      exit\n      type\n      definitionId\n      __typename\n    }\n    __typename\n  }\n}\n","variables":null}' \
  --compressed

curl -X POST -H "Content-Type: application/json" -H "ce-specversion: 1.0" -H "ce-source: coolSystem" -H "ce-type: cbContinueEventType" -H "ce-id: f0643c68-609c-48aa-a820-5df423fa4f02" -d '{ "name":"Donato" }' http://localhost:8080

curl 'http://localhost:8180/graphql' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Origin: http://localhost:8180' \
  -H 'Referer: http://localhost:8180/graphiql/' \
  -H 'Sec-Fetch-Dest: empty' \
  -H 'Sec-Fetch-Mode: cors' \
  -H 'Sec-Fetch-Site: same-origin' \
  --data-raw '{"query":"{\n  ProcessInstances {\n    id\n    processId\n    processName\n    businessKey\n    parentProcessInstanceId\n    roles\n    variables\n    state\n    start\n    lastUpdate\n    end\n    addons\n    endpoint\n    addons\n    serviceUrl\n    nodes {\n      id\n      nodeId\n      name\n      enter\n      exit\n      type\n      definitionId\n      __typename\n    }\n    __typename\n  }\n}\n","variables":null}' \
  --compressed

```