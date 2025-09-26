# Fase 1: Contrucción de la aplicación
# Usamos una imagen base de Eclipse Temurin con JDK 21 y Maven
FROM eclipse-temurin:21-jdk AS build

# indicar el directorio de trabajo
WORKDIR /app

# Copiar los archivos de configuración y código fuente
COPY . .

# Dar permisos de ejecución al Maven Wrapper
RUN chmod +x ./mvnw

# Construir la aplicación usando Maven Wrapper
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