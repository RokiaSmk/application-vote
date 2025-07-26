FROM jenkins/jenkins:lts
# Installer Docker
USER root
RUN apt-get update && \
    apt-get install -y apt-transport-https ca-certificates curl gnupg2 lsb-release software-properties-common && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install -y docker-ce docker-ce-cli containerd.io && \
    apt-get clean

# Installer Docker Compose
RUN curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
chmod +x /usr/local/bin/docker-compose && \
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
# Vérifier l'installation de Docker et Docker Compose
RUN docker --version && docker-compose --version
# Ajouter L'utilisateur Jenkins au groupe Docker
RUN usermod -aG docker jenkins
# Installer les plugins Jenkins nécessaires
RUN jenkins-plugin-cli --plugins "role-strategy blueocean:1.25.3 docker-workflow:1.28"
# Reventr à l'utilisateur Jenkins
USER jenkins