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
2. Crear contenedor del proyecto HelloWorld
    1. Crear Dockerfile en el directorio del proyecto (HelloWorld)
    2. Crear imagen del proyecto:
     ```bash
        docker build -t helloworld .
     ```
    3. Correr la imagen para crear un contenedor con la imagen creada:
     ```bash
        docker run -d -p 7000:4000 helloworld
     ```
    4. Acceder al contenedor mediante el navegador:
        http://localhost:7000
3.	Realizar pruebas unitarias para el proyecto:
    1.	Instalar coverage de python3 dentro del contenedor del proyecto
    ```bash
      docker exec -it <nombre_contenedor> sh
      pip3 install coverage
    ```
    2.	Crear archivo test_app.py, para realizar las pruebas del proyecto (importando la librería unittest)
    3.	Realizar las pruebas unitarias del proyecto:
    ```bash
      coverage run test_app.py
    ```
    4.	Informar las estadísticas de las pruebas (mejorando el código hasta llegar al 100%):
    ```bash
      $ coverage report
      Name            Stmts    Miss    Cover
      ---------------------------------------
      src/app.py          6       0     100%
      src/test_app.py     7       0     100%
      ---------------------------------------
      TOTAL              13       0     100%
    ```
    5.	Guardar las estadísticas en un xml
    ```bash
       coverage xml
    ```
4. Realizar pruebas unitarias en SonarQube
    1.	Instalar SonarQube en un contenedor de Docker
    ```bash
        docker run --name sonarqube -p 9001:9000 sonarqube
    ```
    2.	Acceder a SonarQube desde el navegador:
        http://localhost:9001
    3.	Accedemos a la consola para instalar python3 y pip3
    ```bash
        sudo apt install -y python3 python3-pip
    ```
    4.	Instalamos y activar en el entorno virtual
    ```bash
        python3  -m  venv venv
        source venv/bin/activate
    ```
    5.	Instalamos pysonar-scanner
    ```bash
        pip3 install pysonar-scanner
    ```
    7.	A continuación, vamos a conectar el proyecto a SonarQube
    ```bash
        pysonar -Dsonar.host.url=http://localhost:9001 \
        -Dsonar.sources=src \
        -Dsonar.python.coverage.reportPaths=coverage.xml \
        -Dsonar.projectKey=HelloWorld \
        -Dsonar.token=sqp_42be3b0fa7dd669b1fd8e33a51fbc71fa2526fa2
   ```
    `-Dsonar.host.url`: Url y puerto donde se encuentra alojado SonarQube  
    `-Dsonar.source`: Directorio donde se encuentran los archivos del proyecto  
    `-Dsonar.python.coverage.reportPaths`: Nombre del archivo de coverage que hemos realizado anteriormente  
    `-Dsonar.proyectKey`: Nombre de referencia del proyecto que se creara en SonarQube para los tests unitarios  
    `-Dsonar.token`: Token que hemos creado en SonarQube para que `pysonar` pueda acceder al directorio y pueda realizar los tests
   
Una vez realizado, en SonarQube nos aparecerá los siguientes valores de las pruebas unitarias realizadas con PySonar:
