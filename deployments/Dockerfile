# Use an Eclipse Temurin JDK image (formerly AdoptOpenJDK)
FROM eclipse-temurin:11-jdk-alpine

# Expose the port the application runs on
EXPOSE 8080

# Add a non-root user to run the application
RUN addgroup -S spring && adduser -S spring -G spring

# Set the working directory to the root of the copied source code
WORKDIR /app

# Copy the entire source code into the Docker image
COPY . .

# Make the Maven wrapper script executable
RUN chmod +x mvnw

# Build the project and create the .war file
RUN ./mvnw clean package

# Change ownership of the target directory to the spring user
RUN chown -R spring:spring /app/target

# Switch to the non-root user
USER spring:spring

# Command to run the application
ENTRYPOINT ["java","-jar","/app/target/SpringbootV1-0.0.1-SNAPSHOT.war"]
