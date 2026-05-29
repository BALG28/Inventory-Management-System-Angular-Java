
# Traemos un Linux miniatura con Maven y Java 21
FROM maven:3.9.6-eclipse-temurin-21-alpine AS build

# Creamos la carpeta de trabajo dentro del contenedor
WORKDIR /app

# Copiamos el archivo pom.xml de tu compu a la carpeta actual (.) del contenedor
COPY pom.xml .

# Le decimos a Maven que lea el pom.xml y descargue todo de internet
RUN mvn dependency:go-offline -B

# Copiamos toda tu carpeta 'src' hacia adentro de la máquina virtual
COPY src ./src

# Le ordenamos a Maven que compile el código y lo empaquete en un archivo .jar
# Usamos -DskipTests para que no ejecute pruebas unitarias y sea más rápido
RUN mvn clean package -DskipTests


# ==========================================
# ETAPA 2: Ejecución (Producción)
# ==========================================
# Pedimos una máquina limpia que SOLO tenga Java 21 (mucho más ligera)
FROM eclipse-temurin:21-jre-alpine


# Creamos la carpeta de trabajo en esta nueva máquina
WORKDIR /app

# Copiamos el archivo .jar desde la máquina 'build' hacia esta máquina
COPY --from=build /app/target/*.jar app.jar

# Le decimos a Docker (y a otros programadores) que esta app se comunica por el puerto 8080
EXPOSE 8080

# El comando de vida: Le decimos a la máquina limpia que ejecute nuestro archivo .jar
ENTRYPOINT ["java", "-jar", "app.jar"]