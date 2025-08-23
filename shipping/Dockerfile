# FROM maven
# WORKDIR /opt/server
# COPY pom.xml .
# COPY src .
# RUN mvn clean package
# RUN mv /opt/server/target/shipping-1.0.jar /opt/server/shipping.jar
# ENV CART_ENDPOINT="cart:8080" \
#     DB_HOST="mysql"
# CMD ["java","-jar","shipping.jar"]


# FROM almalinux:9
# WORKDIR /opt/server
# COPY pom.xml .
# COPY src /opt/server/src
# CMD ["sleep", "1000"]


# FROM maven
# WORKDIR /opt/server
# COPY pom.xml .
# COPY src /opt/server/src
# RUN useradd --system --home /opt/server --shell /sbin/nologin --comment "roboshop system user" roboshop
# RUN mvn clean package
# RUN mv /opt/server/target/shipping-1.0.jar /opt/server/shipping.jar
# ENV CART_ENDPOINT="cart:8080" \
    # DB_HOST="mysql"
# CMD ["java","-jar","shipping.jar"]


FROM maven:3.9.4-eclipse-temurin-17

WORKDIR /opt/server

# Copy Maven project files
COPY pom.xml .
COPY src /opt/server/src

# Build the JAR with embedded manifest
RUN mvn clean package

# Rename for simplicity
RUN mv target/shipping-1.0.jar shipping.jar

# Set environment variables
ENV CART_ENDPOINT="cart:8080" \
    DB_HOST="mysql"

# Run the JAR
CMD ["java", "-jar", "shipping.jar"]