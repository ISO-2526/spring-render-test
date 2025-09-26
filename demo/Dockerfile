# Fase 1: Construcción de la aplicación
# Usamos una imagen base de Eclipse Temurin con JDK 21 y Maven
FROM eclipse-temurin:21-jdk AS build

# Instalar Maven
RUN apt-get update && apt-get install -y maven

# Indicar el directorio de trabajo
WORKDIR /app

# Copiar los archivos de configuración de Maven primero (para aprovechar cache de Docker)
COPY pom.xml .
COPY mvnw .
COPY mvnw.cmd .
COPY .mvn .mvn

# Descargar dependencias (esto se cachea si pom.xml no cambia)
RUN ./mvnw dependency:go-offline

# Copiar el código fuente
COPY src src

# Construir la aplicación
RUN ./mvnw clean package -DskipTests

# Fase 2: Ejecución de la aplicación
# Usamos una imagen base de Eclipse Temurin con JRE 21
FROM eclipse-temurin:21-jre

# Indicar el directorio de trabajo
WORKDIR /app

# Copiar el archivo JAR construido en la fase anterior
COPY --from=build /app/target/*.jar app.jar

# Exponer el puerto en el que la aplicación escuchará
EXPOSE 8080

# Comando para ejecutar la aplicación
ENTRYPOINT ["java", "-jar", "app.jar"]