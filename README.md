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
![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/diagram.png)

### Turorial

#### Configuración de VPC - Security group

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/vpc.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/left.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/listandcreate.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/createSecurity.png)

#### Configuracion de inbound Rules Security group

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/editRules.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/rules.png)


#### Creación RDS con su configuracíon

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/rdsbuscador.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/createDB.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/mysql.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/templates.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/password.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/accebilty.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/vpconfig.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/name.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/dbcreated.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/endPoint.png)


#### Test del funcionamiento de la base datos

Ahora con dbeaver nos conectaremos a nuestra RSD, **IMPORTANTE RECORDAR LA CLAVE QUE PUSO Y EL NOMBRE DE LA BASE DATOS, PASO ANTERIOR SE VE EN QUE PARTE SE PUSO**

```SQL
    CREATE TABLE prueba(
        sayHello varchar(10)
    )
    INSERT INTO prueba VALUES ("it is works")
```




#### Creación EC2 con su configuracíon




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

* Cómo consultarlo después de tener el servicio arriba, [Click aqui](http://localhost:8080/say)



#### Desplegar API-REST en EC2

Para desplegar el  API-Rest primeros tenemos que eliminar el Java7 e installar Java8, el siguiente ejemplo es como se debería realizar.

```console
arep@arep:~$ ssh -i "YOUR_PUBLIC_KEY" ec2-user@YOUR_PUBLIC_DNS
[ec2-user@ip-172-31-43-107~]$ sudo su 
[root@ip-172-31-43-107 ec2-user]$ yum remove java-1.7.0-openjdk -y
[root@ip-172-31-43-107 ec2-user]$ yum install java-1.8.0
[root@ip-172-31-43-107 ec2-user] exit
[ec2-user@ip-172-31-43-107~]$exit 
arep@arep:~$
```

Ahora se debe desconecta la conexión ssh, después de haber desconectado se debe enviar la API-REST al EC2, para ello necesitamos estar parados en la raíz del proyecto


```console
arep@arep:~/appSpringBootAWS$ mvn clean intall package
arep@arep:~/appSpringBootAWS$ sftp -i "YOUR_PUBLIC_KEY" ec2-user@YOUR_PUBLIC_DNS
sftp> put target/aws-1.5.1.RELEASE.jar
sftp>exit
```
Se espera que termine subir el archivo .jar y nos desconectamos sftp

#### Test EC2
```console
arep@arep:~$ ssh -i "YOUR_PUBLIC_KEY" ec2-user@YOUR_PUBLIC_DNS
[ec2-user@ip-172-31-43-107~]$ ls 
aws-1.5.1.RELEASE.jar
[ec2-user@ip-172-31-43-107~]$ java -jar aws-1.5.1.RELEASE.jar
```

Ahora pare verificar que nuestro servicio quedo bien configurado, ponemos la siguiente url
```
**YOUR_PUBLIC_DNS**:8080/say
```
Ejemplo 
```
http://ec2-54-146-42-59.compute-1.amazonaws.com:8080/say
```
La respuesta de url tiene que ser igual de la API cuando se probo localmente


#### Configuración de S3


### Autor

John David Ibañez - [CrkJohn](https://github.com/CrkJohn)

Escuela colombina de ingenieria Julio Garavito. 





