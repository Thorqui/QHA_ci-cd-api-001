> #### Aitor Quilez Herrero      `AN_Actividad02*`

## API Spring Boot con GitHub Actions y AWS Elastic Beanstalk

**Solución de la Actividad 2** - Explicación Técnica del Flujo CI/CD

---

### 1. Funcinamiento del Pipeline

El proyecto utiliza GitHub Actions como motor de automatización, definido mediante archivos YAML en .github/workflows/.
Se han implementado dos flujos diferenciados:

* ``CI (Integración Continua):`` Se dispara con cada push o pull_request. Se encarga de clonar el código en un
  contenedor Ubuntu, configurar el JDK 17 de Amazon Corretto y ejecutar los tests unitarios mediante el Maven Wrapper (
  ./mvnw test). Su objetivo es garantizar que ningún código con errores de compilación o lógica sea aceptado.

* ``CD (Despliegue Continuo):`` Una vez que el código es válido, este pipeline empaqueta la aplicación en un archivo JAR
  ejecutable, genera un paquete de despliegue comprimido (ZIP) junto con un archivo Procfile y lo envía a AWS Elastic
  Beanstalk utilizando las credenciales de AWS Academy almacenadas de forma segura en los GitHub Secrets.

---

### 2. Descripción del Flujo CI/CD

El flujo sigue una arquitectura de "entrega garantizada":

1. ``Desarrollo Local:``

El código se escribe y valida en IntelliJ. Se utiliza el Maven Wrapper para asegurar la paridad de versiones de Maven
entre el local y la nube.

2. ``(Trigger):``

Al realizar un git push origin main, GitHub detecta los cambios y activa los workflows.

3. ``Construcción y Artefacto:``

El servidor de GitHub genera el archivo binario (.jar) y lo renombra a app.jar para que coincida con la instrucción del
Procfile.

4. ``Despliegue:``

Se utiliza el servicio de AWS Elastic Beanstalk (plataforma Java SE) que recibe el artefacto y actualiza la instancia
EC2 sin intervención manual, exponiendo la API en una URL pública. e informe final Excel

---

### 3. Decisiones Tomadas y Resolución de Problemas

* ``Adaptación a Windows:`` Al desarrollar en Windows y desplegar en Linux (AWS), se tomó la decisión de utilizar
  comandos de PowerShell para la creación de artefactos locales (Compress-Archive) y de gestionar los permisos de
  ejecución de archivos mediante Git (chmod +x mvnw) para evitar el error de permisos 126 en el pipeline.

* ``Seguridad y Credenciales:`` Se decidió utilizar GitHub Secrets para manejar el AWS_SESSION_TOKEN, una medida
  obligatoria al trabajar con entornos de AWS Academy/Learner Lab, asegurando que las claves no queden expuestas en el
  código fuente.

* ``Compatibilidad de Puerto:`` Se configuró server.port=${PORT:8080} en las propiedades de Spring Boot. Esta decisión
  técnica permite que la aplicación funcione en el puerto 8080 localmente pero se adapte dinámicamente al puerto que AWS
  asigne en producción.

---

### 4. Checklist de Entrega:

``URL GitHub:`` https://github.com/Thorqui/QHA_ci-cd-api-001.git

<figure align="center">
<img src="images/Fase%204/Pipeline.png" alt="Pipeline" width="500"/>
<img src="images/Fase%204/GitHubPush.png" alt="GitHub push" width="500"/>
  <figcaption>Captura del pipeline en estado Success en GitHub Actions</figcaption>
</figure>

<figure align="center">
<img src="images/Fase%205/ElasticBeanstalk.png" alt="Elastic Beanstalk" width="500"/>
<img src="images/Fase%205/URL_Elastic.png" alt="URL Elastic" width="500"/>
  <figcaption>URL pública del entorno de Elastic Beanstalk donde se muestre</figcaption>
</figure>

## Capturas totales realizadas del proceso por fases:

### Fase 1: Desarrollo en Local y Entorno

Preparación del entorno de construcción reproducible y validación de la API en local.

<figure align="center">
  <img src="images/Fase%201/ProjectCreation.png" alt="Project" width="500"/>
  <figcaption>Creación y estructura inicial del proyecto base</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%201/JavaVersion17.png" alt="Java" width="500"/>
  <img src="images/Fase%201/OpenJDK17.png" alt="OpenJDK" width="500"/>
  <figcaption>Verificación de versiones: Java 17, Maven y OpenJDK 17</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%201/SpringBootLocal.png" alt="SpringBoot" width="500"/>
  <img src="images/Fase%201/SpringBootRun.png" alt="SpringBoot" width="500"/>
  <figcaption>Ejecución y validación de la aplicación Spring Boot en el entorno local</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%201/BuildSuccess.png" alt="Build Success" width="500"/>
  <figcaption>Resultado satisfactorio del empaquetado (Build Success)</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%201/VariablesEntorno.png" alt="Variables entorno" width="500"/>
  <img src="images/Fase%201/VariablesEntornoConfig.png" alt="Variables entorno" width="500"/>
  <figcaption>Resolución de problemas en Windows: Configuración de variables de entorno y JAVA_HOME</figcaption>
</figure>

---

## Fase 2: Control de Versiones (Git)

Inicialización del repositorio y gestión de estados de los archivos.

<figure align="center">
  <img src="images/Fase%202/GitInit.png" alt="Git Init" width="500"/>
  <figcaption>Inicialización del repositorio Git local</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%202/GitAdd.png" alt="Git add" width="500"/>
  <figcaption>Preparación de archivos (Staging) para el primer commit</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%202/BuildingGit.png" alt="Building Git" width="500"/>
  <figcaption>Seguimiento de la estructura de archivos en el control de versiones</figcaption>
</figure>

---

## Fase 3: Conexión con GitHub

Configuración del repositorio remoto y sincronización de código.

<figure align="center">
  <img src="images/Fase%203/GitHubProject.png" alt="GitHub Project" width="500"/>
  <figcaption>Creación del repositorio vacío en la plataforma GitHub</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%203/GitGitHub.png" alt="Git GitHub" width="500"/>
  <figcaption>Vinculación del repositorio local con el origen remoto</figcaption>
</figure>

---

## Fase 4: Integración Continua (CI) y Correcciones

Automatización de pruebas y resolución de incompatibilidades Windows-Linux.

<figure align="center">
  <img src="images/Fase%204/GitHubPush.png" alt="GitHub push" width="500"/>
  <figcaption>Subida de código mediante Personal Access Token</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%204/PermisosNVNW.png" alt="Permisos mvnw" width="500"/>
  <figcaption>Corrección de permisos de ejecución para mvnw mediante Git update-index</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%204/CorreccionGitHub.png" alt="Corrección en GitHub" width="500"/>
  <figcaption>Ajuste de flujos de trabajo tras detectar fallos en el pipeline</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%204/Pipeline.png" alt="Pipeline" width="500"/>
  <figcaption>Pipeline de GitHub Actions en estado Success</figcaption>
</figure>

---

## Fase 5: Despliegue Continuo (CD) y AWS

Puesta en producción de la API en AWS Elastic Beanstalk.

<figure align="center">
  <img src="images/Fase%205/DeployMKDIR.png" alt="Deploy MKDIR" width="500"/>
  <img src="images/Fase%205/ConfigDeploy.png" alt="Configuración Deploy" width="500"/>
  <figcaption>Preparación de la carpeta de despliegue y configuración del Procfile</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%205/ElasticBeanstalk.png" alt="Elastic Beanstalk" width="500"/>
  <figcaption>Entorno de AWS Elastic Beanstalk configurado y operativo</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%205/ApiAWS.png" alt="Api AWS" width="500"/>
  <img src="images/Fase%205/URL_Elastic.png" alt="URL Elastic" width="500"/>
  <figcaption>Verificación de la API accesible públicamente mediante URL de AWS</figcaption>
</figure>

<figure align="center">
  <img src="images/Fase%205/PrevioCambio.png" alt="Previo a cambio" width="500"/>
  <img src="images/Fase%205/ActualizAutomatica.png" alt="Actualización automática" width="500"/>
  <img src="images/Fase%205/ActualizacionFase05.png" alt="Actualización Fase 5" width="500"/>
  <figcaption>Demostración del ciclo CD: Actualización automática del servicio tras un push en el repositorio</figcaption>
</figure>


