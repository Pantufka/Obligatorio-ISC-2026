# Obligatorio Implementación de Soluciones Cloud

Se presenta el diseño de la mejora para la actual infraestructura cumpliendo los objetivos brindados en la consigna para la materia Implementación de Soluciones Cloud.

## Contenido

En el directorio raíz se encuentran todos los archivos Terraform además de la carpeta con los módulos y la carpeta que contiene todos los archivos de la aplicación, necesarios para poder cumplir con el despliegue de la infraestructura. Para tener una idea también de la infraestructura que se va a desplegar, hay un archivo .png que muestra el diagrama completo de la misma.

## Requisitos generales

Para poder clonar y correr el repositorio en el ordenador correctamente, primero es necesario instalar varios paquetes, siendo Git el primero de ellos. Su instalación se hace con el comando:

```bash
$ sudo dnf install -y git
```
Una vez instalado, se puede verificar su versión con el comando:

```bash
$ git --version
```
El segundo paquete que se necesita instalar es Terraform con el comando:

```bash
$ sudo dnf install -y terraform
```
Como con el paquete anterior, se puede verificar su versión con el comando:

```bash
$ terraform -version
```
Para instalar el último paquete, que es AWS, necesitamos primero verificar que unzip esté instalado con el comando:

```bash
$ unzip --version
```
En caso de que el resultado sea unzip: command not found, eso significa que unzip no está instalado. Se puede instalar con el comando:

```bash
$ sudo dnf install -y unzip
```
Ahora si, verificamos que esté correctamente instalado con el comando:

```bash
$ unzip --version
```
Una vez confirmado lo de unzip, se necesita instalar AWS con el comando: 

```bash
$ curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o awscliv2.zip
```
Luego se descomprime el paquete con el comando:

```bash
$ unzip awscliv2.zip
```
Una vez descomprimido, se ejecuta el insalador con el comando:

```bash
$ sudo ./aws/install
```
Como con el resto de paquetes, verificamos con el comando:

```bash
$ aws --version
```
Con AWS ya instalado, se puede configurar las keys de AWS en ~/.aws/credentials para poder acceder sin problemas. De otra forma puede configurarlo utilizando:

```bash
$ aws configure
```
Ahora si, luego de tener los tres paquetes instalados, se puede clonar el repositorio con el comando:

```bash
$ git clone https://github.com/Pantufka/Chewbacca.git
```
Y , para acceder al repositorio recientemente clonado, se utiliza el comando:

```bash
$ cd Chewbacca
```

## Modo de uso

Para poder empezar con el deployment, primero se debe eliminar el archivo terraform.tfvars.example, ya que el archivo simplemente simula al archivo real que debería de existir en un deployment de producción. Esto se hace con el comando

```bash
$ git rm terraform.tfvars.example
```
Una vez eliminado, se necesita crear el archivo original que va a contener todas las variables necesarias para poder hacer el deployment correctamente. Eso se hace con el comando:

```bash
$ vim terraform.tfvars
```
Una vez dentro del archivo, es necesario copiar la siguiente información:

aws_region = "us-east-1"

vpc_cidr = "10.0.0.0/16"

availability_zones = [
  "us-east-1a",
  "us-east-1b"
]

public_subnets = [
  "10.0.1.0/24",
  "10.0.2.0/24"
]

app_subnets = [
  "10.0.11.0/24",
  "10.0.12.0/24"
]

db_subnets = [
  "10.0.21.0/24",
  "10.0.22.0/24"
]

instance_type = "t2.micro"

db_instance_class = "db.t3.micro"

db_username = "admin"

db_password = "CambiarPorUnaPasswordSegura123!"

En este caso y al ser una entrega educativa, se brinda el contenido que va dentro del archivo mencionado ya que no hay ninguna información sensible para ninguna de las partes.

Una vez finalizado el proceso anterior, se debe inicializar el proyecto de Terraform con el comando:

```bash
$ terraform init
```
Para verificar que toda la infraestructura propuesta está correcta, se corre el comando:

```bash
$ terraform validate
```
El resultado del comando anterior debería ser "Success! The configuration is valid.". Una vez verificada la infraestructura, se calcula qué cambios realizará Terraform con el comando:

```bash
$ terraform plan
```
Por último, Terraform creará los recursos necesarios para que la infraestructura quede igual a la definida en el código con el comando:

```bash
$ terraform apply
```
Es un proceso que va a llevar unos cuantos minutos debido a la cantidad de recursos que se están creando, siendo la que más demore la base de datos. Terminado el proceso, se recibirá un mensaje que va a decir "Resources: 29 added, 0 changed, 0 destroyed."

Habiendo finalizado el deployment, se puede ingresar al Load Balancer creado para obtener el DNS name por el que se va a acceder a la aplicación yendo a AWS > EC2 > Load Balancer.

## License

[MIT](https://choosealicense.com/licenses/mit/)
