# Utilise une image Maven avec Java 11 pour la phase de build, version allégée pour réduire la taille de l'image
FROM maven:3.6.3-jdk-11-slim AS build

# Crée un répertoire de travail pour stocker le code source et les fichiers de build
RUN mkdir -p /workspace

# Définit le répertoire de travail
WORKDIR /workspace

# Copie le fichier `pom.xml` dans le répertoire de travail pour gérer les dépendances Maven
COPY pom.xml /workspace

# Copie l'ensemble des fichiers source dans le répertoire de travail
COPY src /workspace/src

# Exécute Maven pour compiler et empaqueter l'application en un fichier JAR, en ignorant les tests pour accélérer la construction
RUN mvn -B -f pom.xml clean package -DskipTests

# Utilise une image OpenJDK Java 11 slim comme image de base pour l'exécution de l'application afin de réduire encore la taille
FROM openjdk:11-jdk-slim

# Copie le fichier JAR généré lors de la phase de build dans l'image finale pour l'exécuter
COPY --from=build /workspace/target/*.jar app.jar

# Ouvre le port 8080 pour permettre l'accès à l'application via ce port
EXPOSE 8080

# Définit le point d'entrée de l'image pour exécuter le fichier JAR
ENTRYPOINT ["java", "-jar", "app.jar"]
