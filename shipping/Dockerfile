#
# Build
#
FROM debian:10 AS build

RUN apt-get update && apt-get -y install maven

WORKDIR /opt/shipping

COPY pom.xml /opt/shipping/
RUN mvn dependency:resolve
COPY src /opt/shipping/src/
RUN mvn package

#
# Run
#
FROM openjdk:8-jdk

EXPOSE 8080

WORKDIR /opt/shipping

ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql

RUN apt-get update && apt-get -y install curl

COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar
RUN wget -P /opt/shipping/ https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/download/v1.20.0/opentelemetry-javaagent.jar

CMD [ "java", "-Xmn256m", "-Xmx768m", "-javaagent:opentelemetry-javaagent.jar","-Dotel.metrics.exporter=none","-Dotel.exporter.otlp.traces.endpoint=http://otelcollector:4317","-jar", "shipping.jar" ]

