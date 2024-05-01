¿Como instalar Pi-Hole en AWS sin costo?

Lo primero seria crearse una cuenta en AWS y loguearse. Una vez dentro de nuestra cuenta, el siguiente paso seria entrar al panel EC2 y click en lanzar instancias.

![Paso1](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/c3cb8ec1-c1d0-43a1-8ac2-b47f486e8eb1)

Le ponemos un nombre a la instancia y seleccionamos un SO para nuestra maquina virtual que sea apto para la capa gratuita. En mi caso utilizare Ubuntu 22.04, ya que en el momento de esta publicación la versión 24.04 no es compatible con pi-hole.

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/8d2155d8-b09f-43ba-ba28-fa5c41eb49df)

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/d2285bd6-82fa-4dd9-92a0-20c7f15881bd)

En tipo de instancia seleccionamos una que sea apto para la capa gratuita, en este caso seleccionaremos el t2.micro.

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/2b8d60c2-b899-49b1-9bb1-4e910ddc4aed)

Luego crearemos un nuevo par de claves para poder conectarnos de manera segura a la instancia a través del protocolo SSH. Aunque para este caso especifico no utilizemos estas claves, crearlas es parte de las buenas practicas ya que garantiza un acceso seguro y controlado a la instancia.

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/e8625e99-ec20-4ef4-8e07-e5228bdb95ad)

En configuración de red le damos a editar y dejamos por defecto los parametros de VPC, subred y asignación de IP Publica automatica. 

Creamos un nuevo grupo de seguridad para configurar las reglas del grupo de seguridad para ver quienes(direccion IP) y de que manera(protocolo) van a poder acceder a nuestro servidor DNS. En este caso vamos a utilizar cuatro reglas de seguridad. Como solo voy a utilizarlo desde mi ordenador, todas las reglas van a estar vinculadas a mi dirección IP.

![image](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/202f3cf9-e5c6-4186-b8d4-468e3d695bb9)

Primera regla: Permitir trafico SSH solo desde mi dirección IP.
![aws2](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/7609c1d2-e6ee-4530-8a85-00038d39de58)

Segunda regla: Permitir trafico HTTP solo desde mi dirección IP.
![aws2](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/b259866b-e699-48ff-8ef1-fc454f4ca917)

Tercera regla: Permitir trafico DNS(TCP) solo desde mi dirección IP.
![regla3](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/90af46a7-a07b-4acf-9645-8accf0c175fe)

Cuarta regla: Permitir trafico DNS(UDP) solo desde mi dirección IP.
![regla4](https://github.com/amRamLeo/Pi-Hole-AWS/assets/87347460/d5ae91b1-e209-4049-b959-c29c916114fb)


