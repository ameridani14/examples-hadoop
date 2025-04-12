# examples-hadoop
### **Inicio del proceso**
1. Abrimos una terminal en **docker-hadoop** y ejecutamos el siguiente comando para cargar los 5 contenedores necesarios:
```bash
docker-compose up -d
```
2. Verificamos que los contenedores estén en ejecución con:
```bash
docker ps
```
3. Para entrar al **nodo maestro (namenode)**, usamos:
```bash
docker exec -it namenode bash
```

---

### **Ejercicio de contar palabras**
1. Descargamos el archivo **`hadoop-mapreduce-examples-2.7.1-sources.jar`** desde:
🔗 [https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-examples/2.7.1/](https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-examples/2.7.1/)
Lo guardamos en una carpeta llamada **`hadoop`**.

2. Descargamos un archivo de **texto (.txt)**, por ejemplo, **`ejemplo2_wc.txt`**, y lo movemos a la misma carpeta **`hadoop`**.

3. Copiamos los archivos al contenedor:
```bash
docker cp hadoop-mapreduce-examples-2.7.1-sources.jar namenode:/tmp
docker cp ejemplo2_wc.txt namenode:/tmp
```

4. Entramos al contenedor:
```bash
docker exec -it namenode bash
```

5. Creamos un directorio en **HDFS** para los archivos de entrada:
```bash
hdfs dfs -mkdir /user/root/input_contador
```

6. Movemos el archivo de texto al **HDFS**:
```bash
cd /tmp
hdfs dfs -put ejemplo2_wc.txt /user/root/input_contador
```

7. Ejecutamos el **MapReduce** para contar palabras:
```bash
hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.WordCount input_contador output_contador
```

8. Vemos los resultados:
```bash
hdfs dfs -cat /user/root/output_contador/*
```

9. Verificamos los archivos generados:
```bash
hdfs dfs -ls /user/root/output_contador
```

10. Exportamos los resultados a la carpeta local:
```bash
hdfs dfs -cat /user/root/output_contador/part-r-00000 > /tmp/ejemplo2_wc.txt
exit
```

11. Copiamos el resultado a nuestra máquina:
```bash
docker cp namenode:/tmp/ejemplo2_wc.txt .
```

---

### **Ejercicio de ordenar números (de menor a mayor)**
1. Descargamos el mismo archivo **`.jar`** y un archivo de temperaturas (**`Temperature.txt`**).
2. Copiamos los archivos al contenedor:
```bash
docker cp hadoop-mapreduce-examples-2.7.1-sources.jar namenode:/tmp
docker cp Temperature.txt namenode:/tmp
```

3. Entramos al contenedor y creamos un directorio para los datos:
```bash
docker exec -it namenode bash
hdfs dfs -mkdir /user/root/input_temperaturas
```

4. Movemos el archivo al **HDFS**:
```bash
cd /tmp
hdfs dfs -put Temperature.txt /user/root/input_temperaturas
```

5. Ejecutamos **MapReduce** para ordenar los datos:
```bash
hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.SecondarySort input_temperaturas output_temperaturas
```

6. Vemos los resultados:
```bash
hdfs dfs -cat /user/root/output_temperaturas/*
hdfs dfs -ls /user/root/output_temperaturas
```

7. Exportamos los resultados (similar al ejercicio anterior, pero con el nombre **`temperaturas_ordenadas.txt`**).

---

### **Resolución de Sudoku**
1. Descargamos el archivo **`puzzle1.dta`** desde:
🔗 [https://drive.google.com/file/d/1m6uSyzNCQV1617fmhQyW59sMQ2DYef2E/view](https://drive.google.com/file/d/1m6uSyzNCQV1617fmhQyW59sMQ2DYef2E/view)
Lo guardamos en la carpeta **`hadoop`**.

2. Copiamos el archivo al contenedor:
```bash
docker cp puzzle1.dta namenode:/tmp
```

3. Entramos al contenedor y ejecutamos el algoritmo:
```bash
cd /tmp
hadoop jar hadoop-examples-0.20.205.0.jar sudoku puzzle1.dta
```

4. Guardamos la solución en un archivo:
```bash
hadoop jar hadoop-examples-0.20.205.0.jar sudoku puzzle1.dta > solucion_puzzle1.txt
exit
```

5. Revisamos la carpeta para ver el resultado.

---

### **Notas finales**
- Todos los comandos se ejecutan paso a paso.
- Si hay errores, se puede modificar el nombre de la carpeta de entrada (ej: **`input_temperaturas1`**).
- Los resultados se guardan en archivos de texto para su revisión.
