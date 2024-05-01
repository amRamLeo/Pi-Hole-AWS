Instalación de Pi-Hole con DNS Recursivo en AWS de forma gratuita

Paso 1: Crear una instancia EC2

Para comenzar, es necesario disponer de una cuenta en AWS y acceder a ella. Una vez dentro de la consola de AWS, dirigirse al servicio EC2 y seleccionar "Lanzar instancias".

![Paso1](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/c3cb8ec1-c1d0-43a1-8ac2-b47f486e8eb1)


Asignamos un nombre descriptivo a la instancia y seleccionamos un sistema operativo compatible con la capa gratuita de AWS. En este caso, optaremos por Ubuntu 22.04, ya que, al momento de esta publicación, la versión 24.04 no es compatible con Pi-Hole.

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/8d2155d8-b09f-43ba-ba28-fa5c41eb49df)


![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/d2285bd6-82fa-4dd9-92a0-20c7f15881bd)


Dentro de la sección "Tipo de instancia", escogemos una opción que esté dentro de la capa gratuita de AWS. En este caso, seleccionamos la instancia t2.micro, la cual está disponible dentro de esta capa sin coste adicional.

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/2b8d60c2-b899-49b1-9bb1-4e910ddc4aed)


A continuación, generaremos un nuevo par de claves SSH para establecer una conexión segura con la instancia a través del protocolo SSH. Aunque en este caso específico no hagamos uso inmediato de estas claves, es una práctica recomendada, ya que asegura un acceso seguro y controlado a la instancia en el futuro.

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/e8625e99-ec20-4ef4-8e07-e5228bdb95ad)


En la sección "Configuración de red", seleccionamos "Editar" y dejamos los parámetros de VPC, subred y asignación de IP pública en sus valores predeterminados.

Luego, creamos un nuevo grupo de seguridad para definir las reglas de acceso al servidor DNS. Establecemos estas reglas basadas en direcciones IP y protocolos. En este escenario, utilizaremos cuatro reglas de seguridad, todas vinculadas a mi dirección IP, ya que solo accederemos al servidor desde mi ordenador.

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/202f3cf9-e5c6-4186-b8d4-468e3d695bb9)



Primera regla de seguridad: Permitir tráfico SSH solo desde mi dirección IP

Estableceremos la primera regla de seguridad para permitir el tráfico SSH únicamente desde mi dirección IP. De esta manera, se restringirá el acceso al servidor a través del protocolo SSH a mi dirección IP específica
![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/ef6d37f6-33fe-47d7-9848-a5a03675b954)



Segunda regla de seguridad: Permitir tráfico HTTP solo desde mi dirección IP

En esta segunda regla, se permitirá el tráfico HTTP únicamente desde mi dirección IP. Esto garantizará que el acceso al servidor a través del protocolo HTTP esté restringido exclusivamente a mi dirección IP.
![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/8328b6dd-a8c7-405d-a744-8119520126e2)




Tercera regla de seguridad: Permitir tráfico DNS (UDP) solo desde mi dirección IP

En esta tercera regla, se permitirá el tráfico DNS, que generalmente se realiza a través del protocolo UDP, únicamente desde mi dirección IP. Esto asegura que el acceso al servidor a través del protocolo DNS esté restringido exclusivamente a mi dirección IP.
![regla3](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/90af46a7-a07b-4acf-9645-8accf0c175fe)



Cuarta regla de seguridad: Permitir tráfico DNS (UDP) solo desde mi dirección IP

En esta cuarta regla, se permitirá el tráfico DNS, que generalmente se realiza a través del protocolo UDP, únicamente desde mi dirección IP. Esto garantiza que el acceso al servidor a través del protocolo DNS esté restringido exclusivamente a mi dirección IP.
![regla4](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/d5ae91b1-e209-4049-b959-c29c916114fb)


Una vez configuradas todas las opciones, hacemos clic en 'Lanzar instancia' y nuestra instancia estará lista y funcionando en AWS.


PASO 02: Instalar Pi-hole

Ahora que hemos creado la máquina virtual, nos dirigimos al panel de instancias, seleccionamos la instancia creada y hacemos clic en "Conectar".

![paso02}](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/bf4b63f6-a985-47bd-b2da-116d31c45661)
![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/119e07ba-7f3f-4984-b924-c2e78487f643)



NOTA: Consideraciones adicionales sobre el acceso

Es importante tener en cuenta que al restringir las conexiones únicamente a tu dirección IP, también es necesario modificar el grupo de seguridad para permitir conexiones desde el servicio EC2 Instances dentro de la región donde te encuentres. Esto garantizará que la instancia pueda recibir conexiones no solo desde tu dirección IP, sino también desde otros recursos dentro de la infraestructura de AWS en esa región específica
![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/1299974f-ea60-4088-9191-274e30c90f59)


Ahora procederemos a actualizar la máquina virtual utilizando los siguientes comandos. Después de ejecutarlos, reiniciaremos la instancia para que el kernel se actualice:

sudo apt update
sudo apt upgrade -y


Una vez que la máquina virtual haya sido actualizada, procedemos a instalar Pi-hole utilizando el siguiente comando:

sudo curl -sSL https://install.pi-hole.net | bash


Durante la instalación, se te presentarán varias opciones. Asegúrate de aceptar todas las que aparezcan.

En cierto punto de la instalación, se te solicitará seleccionar un servidor DNS. Puedes elegir cualquier opción disponible, ya que en el paso 03 del proceso de configuración, modificaremos esta configuración para convertir esta instalación básica de Pi-hole en un servidor DNS recursivo.

Al finalizar la instalación, se mostrará una ventana con la dirección IP privada de la máquina virtual y la contraseña. Sin embargo, para acceder a la interfaz de administración, necesitaremos la dirección IP pública, la cual se encuentra en la parte inferior de la pestaña.


![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/f3bc89f2-ed3f-4302-b945-008bfc623b92)


Dado que la contraseña generada por defecto puede ser poco segura, es recomendable cambiarla utilizando el siguiente comando:

pihole -a -p "contraseña"

Una vez modificada la contraseña, podemos acceder a nuestro Pi-hole utilizando la dirección IP pública.

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/3879d446-0d29-4d85-bdb5-d001c23d041e)

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/61aa0d0e-ade0-458c-a800-81aca2133197)



PASO 03: Configurar DNS Recursivo

Para convertir el servidor DNS en un servidor DNS recursivo, vamos a utilizar Unbound. Primero, instalaremos Unbound con el siguiente comando:

sudo apt install unbound


Una vez instalado Unbound, necesitamos un archivo de configuración. Unbound nos proporciona un archivo de configuración estándar que puede ser copiado desde su página web: https://docs.pi-hole.net/guides/dns/unbound/

Primero tenemos que crear el archivo, copiar el codigo que se encuentra en la pagina y guardar el contenido en el archivo creado.
sudo nano /etc/unbound/unbound.conf.d/pi-hole.conf

Una vez creado el archivo de configuración, reiniciamos el servicio utilizando el siguiente comando:

sudo service unbound restart



![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/68128eb8-6dd1-49e6-9dfb-788ca18b1084)


Para garantizar un entorno profesional y seguro, es crucial configurar adecuadamente la fuente de DNS desde la interfaz de administrador de Pi-hole.

1. Desmarcar las opciones de Google para IPv4.

2. A continuación, añadir la dirección loopback de la máquina virtual, junto con el puerto designado para las consultas DNS. Por ejemplo: 127.0.0.1#5335.

Esta configuración garantizará que el servidor Pi-hole utilice adecuadamente la fuente de DNS especificada, asegurando un funcionamiento óptimo y seguro del sistema.

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/2cd35a99-0add-4fca-849b-0358e24520da)


Para finalizar, asegúrate de guardar los cambios haciendo clic en el botón correspondiente ubicado en la parte inferior de la pestaña.
![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/1bc4a7b2-6b10-4bad-9398-0683cd2d1c98)


PASO 04: ¿Cómo utilizar el servidor DNS?

Pi-hole está configurado por defecto para permitir conexiones a nivel local. Sin embargo, al haberlo instalado en la nube, es necesario permitir conexiones desde cualquier dirección IP. Aunque, cabe destacar que el grupo de seguridad configurado debería permitir únicamente consultas desde nuestra dirección IP.

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/5ae35159-8be5-4002-b059-04dd7382fc39)


Una vez hayamos modificado los ajustes según sea necesario, guardamos los cambios en la parte inferior de la pestaña.

Posteriormente, debemos configurar nuestro ordenador cliente para realizar consultas al servidor DNS que hemos creado.

Para ello, accedemos a "Panel de control\Redes e Internet\Centro de redes y recursos compartidos". Luego, hacemos clic en nuestra red y seleccionamos "Propiedades".

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/9ecb4707-6a2b-4a2b-a2d4-f766b92a0388)


Hacemos doble clic en "Habilitar Protocolo de Internet versión 4 (TCP/IPv4)".
Introducimos la dirección IP pública de nuestro servidor DNS en los campos correspondientes.
Guardamos los cambios.
Con estos pasos, la configuración del servidor DNS debería estar completa y funcional.


![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/3c3fc079-6ff4-47d8-864d-9bafc2035808)





