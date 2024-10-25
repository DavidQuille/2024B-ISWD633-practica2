# Variables de Entorno
### ¿Qué son las variables de entorno
Las variables de entorno son valores que configuran el entorno de ejecución del sistema, proporcionando información como rutas (PATH), directorios de usuario (HOME), y preferencias que los programas usan para funcionar correctamente.

### Para crear un contenedor con variables de entorno?

```
docker run -d --name <nombre contenedor> -e <nombre variable1>=<valor1> -e <nombre variable2>=<valor2>
```

### Crear un contenedor a partir de la imagen de nginx:alpine con las siguientes variables de entorno: username y role. Para la variable de entorno rol asignar el valor admin.

```
docker run -d --name srv-nginx -e username=david -e role=admin nginx:alpine 

docker exec -it srv-nginx sh

/ # echo $username

/ # echo $role

```


# CAPTURA CON LA COMPROBACIÓN DE LA CREACIÓN DE LAS VARIABLES DE ENTORNO DEL CONTENEDOR ANTERIOR

![Imagen](img/da.PNG)

### Crear un contenedor con mysql:8 , mapear todos los puertos
```
docker run -d --name mysql-cont --publish published=3306,target=3306 mysql:8
```

### ¿El contenedor se está ejecutando?
No, solamente se creo.

### Identificar el problema
Me presentan los siguientes problemas:

    You need to specify one of the following as an environment variable:
    - MYSQL_ROOT_PASSWORD
    - MYSQL_ALLOW_EMPTY_PASSWORD
    - MYSQL_RANDOM_ROOT_PASSWORD

Que me dice que es necesario especificar estas variables de entorno.
### Eliminar el contenedor creado con mysql:8 
```
docker rm mysql-cont
```

### Para crear un contenedor con variables de entorno especificadas
- Portabilidad: Las aplicaciones se vuelven más portátiles y pueden ser desplegadas en diferentes entornos (desarrollo, pruebas, producción) simplemente cambiando el archivo de variables de entorno.
- Centralización: Todas las configuraciones importantes se centralizan en un solo lugar, lo que facilita la gestión y auditoría de las configuraciones.
- Consistencia: Asegura que todos los miembros del equipo de desarrollo o los entornos de despliegue utilicen las mismas configuraciones.
- Evitar Exposición en el Código: Mantener variables sensibles como contraseñas, claves API, y tokens fuera del código fuente reduce el riesgo de exposición accidental a través del control de versiones.
- Control de Acceso: Los archivos de variables de entorno pueden ser gestionados con permisos específicos, limitando quién puede ver o modificar la configuración sensible.

Previo a esto es necesario crear el archivo y colocar las variables en un archivo, **.env** se ha convertido en una convención estándar, pero también es posible usar cualquier extensión como **.txt**.
```
docker run -d --name <nombre contenedor> --env-file=<nombreArchivo>.<extensión> <nombre imagen>
```
**Considerar**
Es necesario especificar la ruta absoluta del archivo si este se encuentra en una ubicación diferente a la que estás ejecutando el comando docker run.

### Crear un contenedor con mysql:8 , mapear todos los puertos y configurar las variables de entorno mediante un archivo
```
docker run -d --name mysql-cont --env-file=./mysql_config_var.txt mysql:8
```
![Imagen](img/sq.PNG)

# CAPTURA CON LA COMPROBACIÓN DE LA CREACIÓN DE LAS VARIABLES DE ENTORNO DEL CONTENEDOR ANTERIOR 
![Imagen](img/pass.PNG)
### ¿Qué bases de datos existen en el contenedor creado?
```
docker exec -it mysql-cont mysql -u root -p
mysql> show databases;
```

![Imagen](img/db.PNG)
