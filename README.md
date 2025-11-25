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
     `-fsSl`:
     `gpg`: 
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
3. Crear contenedor del proyecto HelloWorld
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
4.	Realizar pruebas unitarias para el proyecto:
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
5. Realizar pruebas unitarias en SonarQube
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
    6.	A continuación, vamos a conectar el proyecto a SonarQube
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
![Prueba SonarQube 1](Sonarqube1.png)

## Practica Python con Jenkins y SonarQube
#### Subir proyecto a repositorio de GitHub
1.	Instalamos y configuracion git en WSL:
    ```bash
      sudo apt install git -y
    ```
    1.	Configurar el nombre de usuario y correo del repositorio
    ```bash
      git config --global user.name "SemaJalon"
      git config --global user.email "j.jalon.guerrero@iescristobaldemonroy.es"
    ```
    2.	Crear repositorio en Github
        1.	Ir a GitHub.
        2.	Crear nuevo “New”
        3.	Ponerle un nombre al proyecto y elegir que sea público o privado

    3.	Subir proyecto al repositorio creado en Github
        1.	Ir al directorio local del proyecto
        ```bash
          cd helloworld
        ```
        2.	Inicializar Git:
        ```bash
          git init
        ```
        3.	Agregar los archivos:
        ```bash
          git add .
        ```
        4.	Realizar el primer commit
        ```bash
          git commit -m “subida de proyecto”
        ```
        5.	Conectar el repositorio local con el de GitHub
        ```bash
            git remote add origin https://github.com/SemaJalon/HelloWorld.git
        ```
        6.	Nos pediran el nombre de usuario y un token para confirmar la subida
          1.	Obtener token:
              Ire a GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → Generate new token.  
              Elegir Expiration según prefiramos (yo he puesto 30 days).  
              Seleccionar los scopes:  
                  repo (para repositorios completos)  
              Hacer clic en Generate token y nos generara el token necesario  
          2.	Guardar el token en el pc local para no tener que volver a insertarlo:
        ```bash
            git config ––global credential.helper store
        ```
        7.	Subir los archivos al repositorio de GitHub:
        ```bash
            git Branch -M main
            git push -u origin main
        ```
#### Análisis de SonarQube con un pipeline de Jenkins
1. En el directorio raíz del proyecto de GitHub creamos un archivo Jenkinsfile para configurar el pipeline y los comandos necesarios para realizar las pruebas mediante pysonar. (En caso de necesitar el archivo Jenkinsfile, póngase en contacto con el autor del documento).
2. Conectar Jenkins con el repositorio de GitHub:
    1. Instalar plugin de GitHub:
        1. Administrar Jenkins -> Plugins -> Available plugins -> Buscar Git
    2. Creamos una nueva tarea y seleccionamos “Pipeline” e indicamos un nombre para la tarea:
       ![Prueba Jenkins 1](Jenkins1.png)
    3. En la opción de Pipeline seleccionamos Git, introducimos la url del repositorio de GitHub y seleccionamos Add en la parte de Credentials para añadir las credenciales de GitHub  
       ![Prueba Jenkins 2](Jenkins2.png)  
       ![Prueba Jenkins 3](Jenkins3.png)
3.	Conectar Jenkins con SonarQube
    1.	Instalamos el plugin de SonarQube Scanner en Jenkins:
        1.	Administrar Jenkins -> Plugins -> Availables plugins -> Buscamos SonarQube e instalamos SonarQube Scanner
    2.	Configurar plugin SonarQube:
        1.	Obtenemos un token de SonarQube:
            1.	Administration  System  Users  Apartado de Administrator creamos el token para Jenkins. 
            Copiamos el token que nos proporciona:
      ![Token SonarQube](TokenSonarQube.png)


