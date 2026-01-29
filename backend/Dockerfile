# Etapa de Desarrollo
FROM eclipse-temurin:21-jdk-alpine
WORKDIR /app

# Instalamos Maven
RUN apk add --no-cache maven
COPY pom.xml .
# RUN mvn dependency:go-offline -B # Descomentar cuando exista el pom.xml

# Copiamos código fuente
COPY src ./src

# Comando por defecto para arrancar
CMD ["mvn", "spring-boot:run"]