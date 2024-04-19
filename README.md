# Herramientas_Big_Data
Trabajamos con herramientas de Big Data

# Practica Integradora

Durante esta practica la idea es imitar un ambiente de trabajo. Desde un área de innovación solicitan construir un MVP(Producto viable mínimo) de un ambiente de Big Data donde se deban cargar unos archivos CSV que anteriormente se utilizaban en un datawarehouse en MySQl, pero ahora en un entorno de Hadoop.

Desde la gerencia de Infraestructura no están muy convencidos de utilizar esta tecnología por lo que no se asigno presupuesto alguna para esta iniciativa, de forma tal que por el momento no es posible utilizar un Vendor(Azure, AWS, Google) para implementar dicho entorno. Es por esto, que todo el MVP se deberá implementar utilizando Docker de forma tal que se pueda hacer una demo al sector de infraestructura mostrando las ventajas de utilizar tecnologías de Big Data.

# Entorno Docker con Hadoop, Spark y Hive

Se pesenta un entorno Docker con Hadoop (HDFS) y la implementación de:
* Spark
* Hive
* HBase
* MongoDB
* Neo4J
* Zeppelin
* Kafka

Es importante mencionar que el entorno completo consume muchos recursos de su equipo, motivo por el cuál se proponen ejercicios pero con ambientes reducidos, en función de las herramientas utilizadas.

### Nota: Este proyecto se desarrollo en Windows generando una máquina virtual en VirtualBox con Ubuntu en la que se instaló Docker y se utilizó PuTTY para conectar la máquina virtual con la original con Windows.

Como primer paso fundamental, para implementar debemos clonar el repositorio en nuestra máquina virtual:

    git clone https://github.com/Rolizet/Herramientas_Big_Data.git

![](Imagenes/imagen1.png)

Ejecute `docker network inspect` en la red (por ejemplo, `docker-hadoop-spark-hive_default`) para encontrar la IP en la que se publican las interfaces de hadoop. Acceda a estas interfaces con las siguientes URL:

```
Namenode: http://<IP_Anfitrion>:9870/dfshealth.html#tab-overview
Datanode: http://<IP_Anfitrion>:9864/
Spark master: http://<IP_Anfitrion>:8080/
Spark worker: http://<IP_Anfitrion>:8081/	
HBase Master-Status: http://<IP_Anfitrion>:16010
HBase Zookeeper_Dump: http://<IP_Anfitrion>:16010/zk.jsp
HBase Region_Server: http://<IP_Anfitrion>:16030
Zeppelin: http://<IP_Anfitrion>:8888
Neo4j: http://<IP_Anfitrion>:7474
```

## 1) HDFS
### Ejecución de entorno

En primer lugar nos ubicamos en la carpeta del proyecto:

```
cd Proyecto_Integrador
```
Ejecutamos la version 1 del entorno docker-compose: 

```
sudo docker-compose -f docker-compose-v1.yml up -d
```

### Copia de los archivos ubicados en 'Datasets' al contenedor 'namenode'

Creamos el directorio 'Datasets' dentro del namenode y salimos del mismo:

Ubicarse en el contenedor "namenode"

```
sudo docker exec -it namenode bash
```

```
cd home
```

Crearemos el directorio Datasets con el comando mkdir

```
mkdir Datasets
```

```
exit
```

Ejecutamos el archivo 'Paso00.sh', que contiene los comandos para copiar los archivos al namenode. Primero le damos permiso de ejecución con chmod:

```
sudo chmod +x Paso00.sh
```

Ejecutar el archivo:

```
sudo ./Paso00.sh
```

Ingresamos al contenedor 'namenode':

```
sudo docker exec -it namenode bash
```

Nos ubicamos en el directorio 'home':

```
cd home
```

Creamos el directorio 'data':

```
hdfs dfs -mkdir -p /data
```

Pegamos los archivos:

```
hdfs dfs -put /home/Datasets/* /data
```

**También podemos ingresar a la interfaz de hadoop para verificar que los archivos esten en el HDFS, utilizando nuestro navegador, pegando la IP de nuestra máquina virtual y luego el puerto :9870.**

Ejemplo:

```
http://xxx.xxx.x.xxx:9870/
```

### Nota: Reemplazamos las xxx con nuestra IP

Luego nos dirigimos a *Utilities > Browse the file system:* si cumpliste con todos los pasos anteriores, deberias visualizar la carpeta data con los archivos .csv 
