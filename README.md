# Transformación Digital y a Arquitectura Empresarial.
##   Patrones Arquitecturales

### Tutorial de cómo construir
*  Desplegar un sitio estático usando S3
*   Desplegar un formulario dinámico usando EC2
*   Enlazar el formulario a una base de datos relacional o no-relacional, 
*   Configurar un VPC para gestionar la seguridad y los permisos de acceso a sus servidores. 

### Requisitos 
*   Java 1.8
*   Maven 3.6.0
*   SSH Y SFTP
*   Cuenta en AWS o AWS-EDUCATE

### Video

[Click Aqui](https://www.youtube.com/watch?v=-OzxeoqP9KA&feature=youtu.be)

### Diagrama
![](Diagram)

### Turorial

##### Configuración de VPC - Security group


#### Configuracion de inbound Rules Security group


#### Creación RDS con su configuracíon


#### Creación EC2 con su configuracíon



#### Test del funcionamiento de la base datos

#### API-REST
*   Descagar proyecto y descargar sus dependencias y compilar
    ```console
    arep@arep:~$ git https://github.com/CrkJohn/appSpringBootAWS.git
    arep@arep:~$ cd appSpringBootAWS 
    arep@arep:~/appSpringBootAWS$ mvn package
    ```
* Configuración del archivo **applicacion.properties**
    ```console
    arep@arep:~/appSpringBootAWS$ nano src/main/resources/application.properties
    ```
* Escribir información tomada de la base datos
    
    *   spring.datasource.url=jdbc:mysql://**hostDB**:3306/**NombreDB**?useSSL=false
    * spring.datasource.username=**admin**
    * spring.datasource.password=**Clave de la db, la que se puso cuando se creo la base datos**
    * spring.datasource.driverClassName=com.mysql.jdbc.Driver
*   Ejecutar la API
    ```console
    arep@arep:~/appSpringBootAWS$ mvn spring-boot:run
    ```
* Consultar metodo GET hecho en la API-REST
Este metodo HTTP esta en el siguiente directorio **/src/main/java/edu/escuelaing/arem/controller/**
```java
    @RequestMapping(value="/say",method = RequestMethod.GET)
	public ResponseEntity<?> listAllUsers(){
	    try {
	        return new ResponseEntity<>(greeting.findAll(),HttpStatus.ACCEPTED);
	    } catch (Exception ex) {
	        return new ResponseEntity<>("mal",HttpStatus.NOT_FOUND);
	    }
    }
```




#### Desplegar API-REST en EC2

```console
arep@arep:~$ 
```


#### Test EC2

#### Configuración de S3


### Autor

John David Ibañez - [CrkJohn](https://github.com/CrkJohn)

Escuela colombina de ingenieria Julio Garavito. 





