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
Antes de iniciar toda la configuración nuestros tres servicios necesitamos estar logueados en aws para poder buscar los servicios.

En la pagina principal *home*, buscamos en la parte central *buscar servicios*, ahí vamos a buscar los tres servicios que necesitamos (RDS- S3- EC2).
#### Configuración de VPC - Security group

Antes crear nuestro servicio RDS necesitamos configurar vpc- Security group, Security group le permite tener salida a la base datos por el puerto que le digitamos, en este caso es el puerto 3306(puerto por defecto mysql).


 * Primero necesitamos buscar el security group para ello buscamos en buscar servicios *VPC*.
 
![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/vpc.png)

Al encontrar VPC en la parte izquierda de la pagina, buscamos la sub-sección  *Security>Security Groups*, Darle click.

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/left.png)

Después de darle click, se listara la lista de *Security groups* existentes y la opción de crear un nuevo *security group*. Se da click en *create security group*

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/listandcreate.png)

Le digitamos el nombre que le deseamos poner en *security group* y la *VPC* que vamos a asociar a ella. 

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/createSecurity.png)

#### Configuracion de inbound Rules Security group

Para que nuestra *VPC-Security Group* nos permita conectarnos a la base datos externamente tenemos configurar las reglas.

Nos dirigimos a lista *security rules* y seleccionamos  la cual queremos crear la regla para que tengamos acceso al db, en la parte inferior después del listado de *security groups*, buscamos la pestaña inbound rules.

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/editRules.png)

Seleccionamos *add rule* y creamos una regla MySQL como se muestra a continuación 

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/rules.png)

#### Creación RDS con su configuracíon

Buscar en servicios RDS

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/rdsbuscador.png)

En la parte central de la vista de RDS,buscamos una opcion de *create database*, como se muestra en la siguiente foto.

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/createDB.png)

Cuando ya estamos creando la base datos es importante escoge MySQL, ya que los puertos de salida del *security group* esta para este gestionador de db.

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/mysql.png)

Ahora es importante escoger *free tier* ya que es gratuito y es para desarrollo y test

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/templates.png)

En *credentials Settings* configuramos la información básica que necesitamos para poder conectarnos más adelante a la base de datos

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/password.png)

Si solo se tiene configurado el puerto de salida de la base datos por medio del *security group* no es suficiente para poder establecer conexion, la solucion de este problema es confirmar el *publicly accessible*

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/accebilty.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/vpconfig.png)


Nombre de la base datos

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/name.png)

### Verificacíon que la base datos fue creada exitosamente.

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

En la pagina principal  buscamos el servicio  EC2.

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/ec2.png)

En la parte central de la vista de EC2,buscamos una opcion de *launch instance*, como se muestra en la siguiente foto.
	
![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/createec2.png)

Buscamos la maquina *Amazon linux AMI* y que tenga en la descripcion *java* ya que existe otra que no contiene java lo cual si la escogemos no podemos hacer el test del servicio dinamico más adelante

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/ubuntu.png)

En la configuración es muy imporante configurarle las reglas ya que debemos crear una regla que salga por el puerto 8080 ya que por ese puerto vamos a subir nuestro servicio dinamico.

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/configuracion.png)


#### Test del funcionamiento de la EC2

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/conexion.png)

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/connect.png)


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

En la pagina principal  buscamos el servicio  S3 y en la pagina S3 creamos un buckect

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/createbucket.png)

La tercera configuracion se desactiva el bloque para la consula publica del recurso estatico.

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/confibucket.png)

Cuando tenemos creado el bucket, se selecionado el creado y buscamos el boton cargar, se carga un documento, fotos o paginas,

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/cargar.png)

Antes de consultar el archivo selecionamos el archivo  y en *acciones* buscamos la opcion *hacer publico*

![](https://github.com/CrkJohn/EC2-S3-RDS/blob/master/img/public.png)



### Autor

John David Ibañez - [CrkJohn](https://github.com/CrkJohn)

Escuela colombina de ingenieria Julio Garavito. 
