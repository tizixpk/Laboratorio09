### Pirez Tiziano 4to 4ta
# Laboratorio 09 : Gestión de procesos

## 1. ¿Qué es el PID?
El PID (Process Identifier) es un número único asignado a cada proceso en ejecución en un sistema operativo. Este identificador permite al sistema y a los administradores de sistemas gestionar y controlar procesos específicos.

## 2. ¿Diferencia entre la opción –a y la opción –x de la orden ps?
- La opción `-a` muestra todos los procesos que tienen un terminal asociado, excepto los procesos de sesión y de control.
- La opción `-x` muestra todos los procesos, incluso aquellos que no tienen un terminal asociado (procesos daemon).

## 3. Investiga sobre el numero NICE de un proceso ¿Qué valores puede tomar el parámetro NICE?
¿Qué usuarios pueden cambiar este parámetro?
El número NICE es un valor que determina la prioridad de un proceso en un sistema Unix o Linux. Los valores que puede tomar varían de -20 a 19:
- Valores más bajos (-20) indican una mayor prioridad.
- Valores más altos (19) indican una menor prioridad.
  
Solo el usuario root puede asignar valores negativos (mayor prioridad) a los procesos. Los usuarios no privilegiados pueden ajustar la prioridad de sus procesos dentro del rango de 0 a 19.

## 4.  Listar todos los archivos que contengan la cadena “ .jpg” dentro de la estructura de directorios del sistema. Usar la orden ls y grep.
```bash
ls -R / | grep ".jpg"
```

## 5. Ejecutar otra vez la orden anterior, pero está vez con la prioridad más baja posible.
nice -n 19 ls -R / | grep ".jpg"

## 6. Abrir otra terminal, y mientras la orden anterior se ejecuta, mediante la orden ps listar todos los procesos asociados al terminal del usuario actual y obtener el PID del proceso ls. Con la orden renice otorgar la máxima prioridad a la orden ls. Después con la orden kill terminar el proceso de la manera “más correcta” posible.

a. Abrir una terminal y ejecutar:
    ```bash
    ps -u $USER
    ```
b. Identificar el PID del proceso `ls` en la lista.

c. Cambiar su prioridad:
    ```bash
    renice -n -20 -p [PID]
    ```
d. Terminar el proceso correctamente:
    ```bash
    kill -SIGTERM [PID]
    ```

## 7. Visualizar mediante el editor nano el archivo que contiene la información de los usuarios del
sistema. Ejecutar el programa nano con la máxima prioridad posible.
```bash
sudo nice -n -20 nano /etc/passwd
```

## 8.  En otro terminal, obtener el PID del editor nano y el PID de su proceso padre mediante la orden pstree.
```bash
pstree -p | grep nano
```

## 9. Mediante la orden top establecer una prioridad normal al editor nano y después terminar el proceso del editor.

a. Con la orden `top`, buscar el PID del `nano` y ajustarlo:
    ```bash
    renice -n 0 -p [PID]
    ```
b. Terminar el proceso desde `top` o con:
    ```bash
    kill -SIGTERM [PID]
    ```

## 10. Ejecutar el navegador WEB firefox desde el terminar en segundo plano. Indicar el número de trabajo y su PID.
```bash
firefox &
```
- El número de trabajo y su PID se muestran en la terminal al ejecutarlo.

## 11. Listar todos los archivos terminados en .gif del sistema de directorios del sistema y almacenarlo en el archivo todoslosgif. Ejecutar esta orden en segundo plano.
```bash
find / -name "*.gif" > todoslosgif &
```

## 12. Mediante la orden jobs listar todos los trabajos en segundo plano del terminal. Procesos en 1º plano
```bash
jobs
```

## 13.Mirar los procesos existentes en el sistema y lanzar un proceso que dure 600 segundos en 1º plano (p.e. sleep 600)
```bash
sleep 600
```

## 14. Mata el proceso ¿Qué observamos? ¿Aparecen los mismos procesos que al principio?
a. Identificar el PID:
    ```bash
    ps -u $USER
    ```
b. Terminar el proceso:
    ```bash
    kill [PID]
    ```
Observamos que el proceso `sleep` ya no está activo y el sistema vuelve al estado inicial.

## 15. Repetimos el Ej. 13 y el 14, pero ahora sólo queremos detener el proceso (no cancelarlo). ¿Qué observamos? ¿Aparecen los mismos procesos que al principio? Ej.
a. Detener el proceso:
    ```bash
    kill -SIGSTOP [PID]
    ```
b. Observamos que el proceso está detenido, pero no cancelado, y puede ser continuado.

## 16. ¿Cómo podemos hacer para que continúe el proceso en 1º plano? ¿Se puede hacer con la orden kill?
```bash
kill -SIGCONT [PID]
```

## 17. Indica dos maneras para que un proceso detenido, continúe en 2º plano.
a. Con `bg`:
    ```bash
    bg %[job_number]
    ```
b. Con `kill`:
    ```bash
    kill -SIGCONT [PID]
    ```

## 18. Lanzar un proceso en 2º plano y obtener su PID. ¿Cuál es su número de trabajo y nº deproceso?
```bash
sleep 600 &
jobs -l
```

## 19. Si un proceso lanzado en 2º plano termina su ejecución, ¿seguirá apareciendo en la salida de jobs? Razona tu respuesta

Un proceso en segundo plano que ha terminado no aparecerá en la salida de `jobs` porque `jobs` solo muestra trabajos activos.

## 20. Detener y volver a recontinuar en 2º plano un proceso lanzado en 1º plano
a. Detener el proceso:
    ```bash
    kill -SIGSTOP [PID]
    ```
b. Continuar en segundo plano:
    ```bash
    bg %[job_number]
    ```
