
# Generating app
On Quarkus
$ mvn archetype:generate \
    -DarchetypeGroupId=org.kie.kogito \
    -DarchetypeArtifactId=kogito-quarkus-archetype \
    -DgroupId=com.zamaflow.bpm -DartifactId=coffee-shop-app \
    -DarchetypeVersion=0.17.0 \
    -Dversion=1.0-SNAPSHOT

On Spring Boot
$ mvn archetype:generate \
    -DarchetypeGroupId=org.kie.kogito \
    -DarchetypeArtifactId=kogito-springboot-archetype \
    -DgroupId=com.zamaflow.bpm -DartifactId=coffee-shop-app \
    -DarchetypeVersion=0.17.0 \
    -Dversion=1.0-SNAPSHOT


# org.kie.kogito.kogito-quarkus-archetype - 0.17.0 #

# Running

- Compile and Run

    ```
     mvn clean package quarkus:dev
    ```

- Native Image (requires JAVA_HOME to point to a valid GraalVM)

    ```
    mvn clean package -Pnative
    ```
  
  native executable (and runnable jar) generated in `target/`

# Test your application

Generated application comes with sample test process that allows you to verify if the application is working as expected. Simply execute following command to try it out

```sh
curl -d '{}' -H "Content-Type: application/json" -X POST http://localhost:8080/greetings
```

Once successfully invoked you should see "Hello World" in the console of the running application.

# Developing

Add your business assets resources (process definition, rules, decisions) into src/main/resources.

Add your java classes (data model, utilities, services) into src/main/java.

Then just build the project and run.


# Swagger documentation

The exposed service [OpenAPI specification](https://swagger.io/docs/specification) is generated at 
[/docs/openapi.json](http://localhost:8080/docs/openapi.json).

You can visualize and interact with the generated specification using the embbeded [Swagger UI](http://localhost:8080/swagger-ui) or importing the generated specification file on [Swagger Editor](https://editor.swagger.io).

In addition client application can be easily generated from the swagger definition to interact with this service.

# Creating DMN Model: traffic-violation.

1. Add file: Traffic-Violation.dmn.
2. !Model Decision-Requirement Diagram-DRD(/traffic-violation.png).
3. Define data model: tDriver(Age, Name, State, City, Points), tViolation(Code,Date, Type(speed,parking,driving under the influence), Speed Limit, Actual Speed) and tFine(Amount, Points).
4. Add decision tables/model.
5. Write test scenario: traffic-violation-test.scesim and run 'sh mvn clean test'
6. Test api using curl or postman or something similar.

Request
curl -X POST -H 'Content-Type: application/json' http://localhost:8080/Traffic-Violation --data '
{
  "Driver" : {
    "Name" : "John Doe",
    "Points" : 14,
    "City" : "Sao Paulo"
  },
  "Violation" : {
    "Type" : "speed",
    "Speed Limit" : 100,
    "Actual Speed" : 120
  }
}'

Result
{"Violation":{"Type":"speed","Speed Limit":100,"Actual Speed":120},"Driver":{"Points":14,"City":"Sao Paulo","Name":"John Doe"},"Fine":{"Ponts":3,"Amount":500},"Should the driver be suspended?":"No"}

Request 2
curl -X POST -H 'Content-Type: application/json' http://localhost:8080/Traffic-Violation --data '
{
  "Driver" : {
    "Name" : "John Doe",
    "Points" : 14,
    "City" : "Sao Paulo"
  },
  "Violation" : {
    "Type" : "speed",
    "Speed Limit" : 100,
    "Actual Speed" : 140
  }
}'

Response 2
{"Violation":{"Type":"speed","Speed Limit":100,"Actual Speed":140},"Driver":{"Points":14,"City":"Sao Paulo","Name":"John Doe"},"Fine":{"Points":7,"Amount":1000},"Should the driver be suspended?":"Yes"}