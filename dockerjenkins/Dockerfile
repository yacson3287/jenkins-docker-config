# Usa una imagen base específica para la reproducibilidad.
# lts-jdk17 es la versión LTS actual recomendada.
FROM jenkins/jenkins:lts-jdk21

# Cambia al usuario root para instalar paquetes.
USER root

# 1. Instala dependencias básicas y prepara el entorno para los repositorios.
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release && \
    mkdir -p /usr/share/keyrings

# 2. Instala el CLI de Docker (MÉTODO MODERNO Y SEGURO)
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
    $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install -y --no-install-recommends docker-ce-cli docker-ce

# 3. Instala Docker Compose v2 (que ahora es un plugin de Docker)
RUN apt-get install -y --no-install-recommends docker-compose-plugin

# 4. Añade al usuario jenkins al grupo de docker para que pueda usar el Docker socket.
#    Esto es necesario si montas el socket del host en el contenedor.
RUN usermod -aG docker jenkins

# 5. Limpia la caché de apt para reducir el tamaño de la imagen final.
RUN rm -rf /var/lib/apt/lists/*

# Vuelve al usuario jenkins por defecto para una mayor seguridad en la ejecución.
USER jenkins
