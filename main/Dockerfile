# ===============================
# Stage 1: Build WAR using Maven
# ===============================
FROM maven:3.9-eclipse-temurin-8 AS build

WORKDIR /app

# Copy pom.xml
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy web application source
COPY webapp ./webapp

# Build WAR
RUN mvn clean package -DskipTests


# ===============================
# Stage 2: Run WAR on Tomcat
# ===============================
FROM tomcat:9.0-jdk8

# Remove default apps
RUN rm -rf /usr/local/tomcat/webapps/*

# Copy WAR to Tomcat ROOT
COPY --from=build /app/target/*.war /usr/local/tomcat/webapps/ROOT.war

EXPOSE 8080

CMD ["catalina.sh", "run"]
