# Documentación Contenedores y Orquestadores
## Proyecto Python en Docker y SonarQube
#### Elementos utilizados
- **Plataforma de desarrollo:** WSL (Windows Subsystem for Linux)
  - Activar característica de WSL
  - Descargar consola de Ubuntu 22.04
- **Lenguaje / Frameworks:** Python 3.12, Flask.
- **Dependencias:** coverage, pysonar-scanner.
- **Entorno Virtual:** venv de python3
- **Servicios:** SonarQube (Docker), puerto 9001.
- **Archivos importantes:** app.py, test_app.py, coverage.xml, requirements.txt.
- **Imágenes de Docker:** SonarQube, Ubuntu 22.04
- **Contenedores de Docker:** SonarQube y HelloWorld (Proyecto de Python corriendo en Ubuntu 22.04).
- **Configuraciones:** variables de entorno, Dockerfile
#### Comandos
1. Intalar Docker
   1. Actualizar los paquetes:
   ```bash
       sudo apt update
   ```
   
   2. Instalar curl (comando para obtener el codigo fuente de una web):
   ```bash
       sudo apt install curl
   ```
   3. Añadir GPG y repositorios de Docker:
   ```bash
       curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
       echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```
   4. Actualizar los paquetes:
   ```bash
       sudo apt update
   ```
   5. Instalar Docker Cli ():
   ```bash
        sudo apt install docker-ce docker-ce-cli containerd.io
   ```
   6. Instalar Docker Compose:
   ```bash
      sudo apt install docker-compose-plugin
   ```
   7. Ejecutar demon de Docker():
   ```bash
      sudo dockerd &
   ```
   8. Inicio automatico de Docker():
   ```bash
      sudo systemctl enable docker
      sudo systemctl start docker
   ```
   9. Añadir usuario al grupo de Docker:
   ```bash
      sudo usermod -aG docker $USER
   ```
