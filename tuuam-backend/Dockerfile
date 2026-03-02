# ==========================================
# STAGE 1: Build (Builder Image)
# ==========================================
FROM eclipse-temurin:21-jdk-alpine AS builder

# Arbeitsverzeichnis im Container setzen
WORKDIR /app

# Gradle-Wrapper und Konfigurationsdateien kopieren (für Layer-Caching)
COPY gradlew .
COPY gradle gradle
COPY build.gradle.kts .
COPY settings.gradle.kts .

# Ausführungsrechte für den Gradle-Wrapper setzen
RUN chmod +x gradlew

# Source-Code kopieren
COPY src src

# Applikation bauen (Tests überspringen, da die DB beim Build-Prozess nicht läuft)
RUN ./gradlew bootJar -x test --no-daemon

# ==========================================
# STAGE 2: Runtime (Production Image)
# ==========================================
FROM eclipse-temurin:21-jre-alpine

# Arbeitsverzeichnis setzen
WORKDIR /app

# Security Best Practice: Eigenen Non-Root User anlegen
RUN addgroup -S tuuamgroup && adduser -S tuuamuser -G tuuamgroup
USER tuuamuser:tuuamgroup

# Das kompilierte JAR aus der Build-Stage kopieren
COPY --from=builder /app/build/libs/*-SNAPSHOT.jar app.jar

# Port freigeben
EXPOSE 8080

# Applikation starten
ENTRYPOINT ["java", "-jar", "app.jar"]