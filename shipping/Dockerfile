# Stage 1: Build using a dedicated Maven image
FROM maven:3.8.6-openjdk-8 AS build

WORKDIR /opt/shipping

# Copy only the pom first to cache dependencies (Speed boost!)
COPY pom.xml /opt/shipping/
RUN mvn dependency:resolve

# Copy source and build the JAR
COPY src /opt/shipping/src/
RUN mvn package -DskipTests

# Stage 2: Final lightweight Run image
FROM eclipse-temurin:8-jre

EXPOSE 8080
WORKDIR /opt/shipping

# Standard Robot Shop Env Variables
ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql

# Copy the built JAR from the first stage
# Note: The filename in target/ might be shipping-1.0.jar or shipping-1.0-SNAPSHOT.jar
# This wildcard (*) ensures it works regardless of the version string
COPY --from=build /opt/shipping/target/shipping-*.jar shipping.jar

# Memory limits are good for K8s stability
CMD [ "java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar" ]