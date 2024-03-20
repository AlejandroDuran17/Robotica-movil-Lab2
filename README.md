# Robotica-movil-Lab2
Juan Sebastian Daleman

Juan David Chica Garcia

Luis Alejandro Duran Espitia

Santiago Olaya

## Conexión Lego EV3 con ROS
Para poder conectar el lego EV3 con ROS primero se necesita tener una memoria SD de minimo 2 GB de alamacenamiento y una antena USB wifi para el robot EV3. Para elegir una SD compatible y un adaptador wifi se recomienda leer las siguientes paginas:
* [Seleccion de SD](https://github.com/ev3dev/ev3dev/wiki/Selecting-a-microSD-card)
* [Antenas wifi compatibles leJos](https://lejosnews.wordpress.com/2015/02/03/comparing-wifi-adapters/).
* [Antena wifi compatibles con ev3dev](https://github.com/ev3dev/ev3dev/wiki/USB-Wi-Fi-Dongles)

  Para acceder a la programación del robot EV3 por una API diferente a la de lego usamos un booteo de una distribución de Linux Debian desarrollada para el robot conocido como [ev3dev](https://www.ev3dev.org/) este fue desarrollado para el uso de diferentes lenguajes de programación con el robot ev3 como python, micropython, java, C++, C y etc. (Para conocer todos los lenguajes disponibles ver [lenguajes de programación](https://www.ev3dev.org/docs/programming-languages/)).

  **Nota:** Acabe aclarar que este es un booteo por una unidad de almacenamiento diferente por lo cual no se afecta o modifica el firmware original que posee el bloque ev3.
  
  Para crear la SD booteable se siguieron los pasos de la pagina de ev3dev [SD booteable](https://www.ev3dev.org/docs/getting-started/). Una vez con la antena colocada en el robot y la SD se prende este y se espera que se inicialice el sistema.

### Conexión al PC via wifi
Para esta conexión se puede hacer por dos vias la primera es configurar manualmente la red wifi a la cual se conectara o configurandola por medio del pc por conexión [bluethoot](https://www.ev3dev.org/docs/tutorials/connecting-to-the-internet-via-bluetooth/) o [USB](https://www.ev3dev.org/docs/tutorials/connecting-to-the-internet-via-usb/). Una vez configurada nos podremos conectar al robot atravez de esta en el PC usando una conexión [SSH](https://www.ev3dev.org/docs/tutorials/connecting-to-ev3dev-with-ssh/).Para esto lanzaremos una terminal y mandaremos el siguente comando

```
ssh robot@<Dirección IP del robot>
```
**Nota:** La dirrección IP asignada al robot se puede ver en la parte superior a la izquierda del robot y el password es "maker".

#### Pruebas de motores
Si desea probar el funcionamiento de motores por el terminal puede conectar los motores en los puestos A y D del robot y corra el siguente comando el cual movera las dos ruedas con una velocidad de 50 grados/s sin frenado

```
python3 -c "from ev3dev2.motor import LargeMotor, OUTPUT_A, OUTPUT_D; LargeMotor(OUTPUT_A).on_for_seconds(speed=50, seconds=2); LargeMotor(OUTPUT_D).on_for_seconds(speed=50, seconds=2)"
```
* prueba con frenadao
```
python3 -c "from ev3dev2.motor import MoveTank, OUTPUT_A, OUTPUT_D, SpeedPercent, MoveTank; tank_drive = MoveTank(OUTPUT_A, OUTPUT_D); tank_drive.on_for_seconds(left_speed=50, right_speed=50, seconds=5, brake=True)"
```

* Giro del robot
```
python3 -c "from ev3dev2.motor import MoveTank, OUTPUT_A, OUTPUT_D, SpeedPercent, MoveTank; tank_drive = MoveTank(OUTPUT_A, OUTPUT_D); tank_drive.on_for_seconds(left_speed=50, right_speed=45, seconds=5, brake=True)"
```
* frenado suave
```
python3 -c "from ev3dev2.motor import MoveTank, OUTPUT_A, OUTPUT_D, SpeedPercent, MoveTank; tank_drive = MoveTank(OUTPUT_A, OUTPUT_D); tank_drive.on_for_seconds(left_speed=50, right_speed=45, seconds=5); tank_drive.off(brake=True)"

```
### Preparación de entorno e intalacion de librerias y paquetes

#### Paquete de control de ev3 con ros

Lo primero sera la intalación de paquetes que permitan el manejo del Ev3 con ROS en la terminal correremos los siguentes comandos para instalar el paquete 'ev3_ros'. en distro en nuestro caso ponemos la noetic
```
sudo apt-get update
sudo apt-get install ros-<distro>-ev3-ros

```

##### Confirmar vesion de ros instalada

Si es necesario verificar que distribución se tiene actualmentee se puede hacer con los siguentes comandos

```
rosversion -d
```
 tambien puede verificar la versión con el comando

```
rosversion -v
```

#### Creacion del workspace

Para esto crearemos un directrotio que sera nuestro workspace y tendra nuestros archivos de intalación

```
cd ~
mkdir catkin_ws
cd catkin_ws
mkdir src
catkin build
cd src
catkin_create_pkg ev3dev_ros
cd ev3dev_ros
```

#### Intalacion de libreria de python

Para la instalacion de la libreria en python descargaremos los archivos necesarios y haremos la instalcion con los siguientes comandos

```
git clone https://github.com/ev3dev/ev3dev-lang-python.git
cd ev3dev-lang-python
sudo python3 setup.py install
cd ..
```

#### Intalacion de libreria C++

Para la instalacion de la libreria en C++ descargaremos los archivos necesarios y haremos la instalcion con los siguientes comandos

```
git clone https://github.com/ddemidov/ev3dev-lang-cpp.git
cd ev3dev-lang-cpp
mkdir build
cd build
cmake ..
sudo make install
```



## Otros links de interes
* [Conexion de Lego Ev3 por medio de una raspberry pi](https://github.com/aws-samples/aws-builders-fair-projects/blob/master/reinvent-2019/lego-ev3-raspberry-pi-robot/README.MD) 
* [ROS desde el Lego Ev3](https://github.com/moriarty/ros-ev3) 
* [Manejo de motores del Ev3 con Python](https://www.youtube.com/watch?v=j0-ATIe6pqg) 
* [Uso de sensor lidar con Ev3](https://www.youtube.com/watch?v=JX0zeYa-faM) 
* [Programacion y conexion SSH desde Visual Studio Code con Ev3](https://www.youtube.com/watch?v=uNSIOvqzAnY) 
* [Instalacion de ROS distribucion JADE en el Ev3](https://github.com/osmado/ev3dev_ros_distribution) 
* [ROS blog sobre lego Ev3](http://wiki.ros.org/Robots/EV3)
* [Conexión Ev3 y arduino](https://www.dexterindustries.com/howto/connecting-ev3-arduino/)
* [Manejo con ROS de Ev3 con C++](https://www.youtube.com/watch?v=iRQqEKYDRI4)
* [Github manejo con ROS de Ev3 con C++](https://github.com/srmanikandasriram/ev3-ros?tab=readme-ov-file)
* [Libreria de Python de ev3dev](https://github.com/ev3dev/ev3dev-lang-python)
* [Libreria de C++ de ev3dev](https://github.com/ddemidov/ev3dev-lang-cpp)

