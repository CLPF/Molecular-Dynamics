1. Abrir el archivo con `vi nombre_del_archivo.pdb`
2. Copia de un archivo y guardar con otro nombre
```Bash
#Abrir el archivo con 
vi nombre_del_archivo.pdb
#Dentro del archivo 
:w nuevo_nombre_del_archivo
```
3. Buscar lineas dentro del archivo, escribo: `numero_de_la_linea G`
4. Volver a la primer linea `gg`. Ir a la ultima linea `end`
5. Eliminar lineas (desde donde estoy parada) `cantidad_lineas_a_eliminar dd` o `cantidad_lineas_a_eliminar D` o `dd`(borro una linea a la vez)
6. Deshacer la última modificación `u`
7. Para buscar palabras utilizo `/palabra` y `n` para buscar la siguiente

6. Entrar al modo --INSERTAR-- con `i`