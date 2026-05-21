# --- Etapa 1: Construcción (Build) ---
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /app

# Copiar el archivo de configuración y descargar dependencias
COPY pom.xml .
RUN mvn dependency:go-offline

# Copiar el código fuente y compilar el archivo .jar (omitiendo los tests)
COPY src ./src
RUN mvn clean package -DskipTests

# --- Etapa 2: Ejecución (Runtime Ligero) ---
FROM eclipse-temurin:17-jre-alpine

# Buenas Prácticas: Crear un usuario sin privilegios (No-Root)
RUN addgroup -S spring && adduser -S spring -G spring
USER spring

WORKDIR /app
# Copiamos el archivo .jar compilado en la etapa anterior
COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]