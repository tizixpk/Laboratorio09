# Laboratorio 09 : Gestión de procesos
### Pirez Tiziano 4to 4ta

## 1. ¿Qué es el PID?
El PID (Process Identifier) es un número único asignado a cada proceso en ejecución en un sistema operativo. Este identificador permite al sistema y a los administradores de sistemas gestionar y controlar procesos específicos.

## 2. Diferencia entre la opción `-a` y la opción `-x` de la orden `ps`
- La opción `-a` muestra todos los procesos que tienen un terminal asociado, excepto los procesos de sesión y de control.
- La opción `-x` muestra todos los procesos, incluso aquellos que no tienen un terminal asociado (procesos daemon).

## 3. Investiga sobre el número NICE de un proceso
El número NICE es un valor que determina la prioridad de un proceso en un sistema Unix o Linux. Los valores que puede tomar varían de -20 a 19:
- Valores más bajos (-20) indican una mayor prioridad.
- Valores más altos (19) indican una menor prioridad.
  
Solo el usuario root puede asignar valores negativos (mayor prioridad) a los procesos. Los usuarios no privilegiados pueden ajustar la prioridad de sus procesos dentro del rango de 0 a 19.

## 4. Listar todos los archivos que contengan la cadena “.jpg” dentro de la estructura de directorios del sistema
```bash
ls -R / | grep ".jpg"
```

## 5. Ejecutar la orden anterior con la prioridad más baja posible
```bash
nice -n 19 ls -R / | grep ".jpg"
```

## 6. Obtener el PID del proceso `ls` y cambiar su prioridad
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

## 7. Visualizar el archivo de usuarios del sistema con máxima prioridad
```bash
sudo nice -n -20 nano /etc/passwd
```

## 8. Obtener el PID del editor `nano` y su proceso padre
```bash
pstree -p | grep nano
```

## 9. Establecer prioridad normal al editor `nano` y terminar el proceso
a. Con la orden `top`, buscar el PID del `nano` y ajustarlo:
    ```bash
    renice -n 0 -p [PID]
    ```
b. Terminar el proceso desde `top` o con:
    ```bash
    kill -SIGTERM [PID]
    ```

## 10. Ejecutar Firefox en segundo plano
```bash
firefox &
```
- El número de trabajo y su PID se muestran en la terminal al ejecutarlo.

## 11. Listar archivos `.gif` y almacenarlo en `todoslosgif`
```bash
find / -name "*.gif" > todoslosgif &
```

## 12. Listar trabajos en segundo plano
```bash
jobs
```


## 13. Lanzar un proceso de 600 segundos en 1º plano
```bash
sleep 600
```

## 14. Matar el proceso
a. Identificar el PID:
    ```bash
    ps -u $USER
    ```
b. Terminar el proceso:
    ```bash
    kill [PID]
    ```
Observamos que el proceso `sleep` ya no está activo y el sistema vuelve al estado inicial.

## 15. Detener el proceso sin cancelarlo
a. Detener el proceso:
    ```bash
    kill -SIGSTOP [PID]
    ```
b. Observamos que el proceso está detenido, pero no cancelado, y puede ser continuado.

## 16. Continuar el proceso en 1º plano
```bash
kill -SIGCONT [PID]
```

## 17. Continuar un proceso detenido en 2º plano
a. Con `bg`:
    ```bash
    bg %[job_number]
    ```
b. Con `kill`:
    ```bash
    kill -SIGCONT [PID]
    ```

## 18. Lanzar un proceso en 2º plano y obtener su PID
```bash
sleep 600 &
jobs -l
```

## 19. Proceso en 2º plano al finalizar
Un proceso en segundo plano que ha terminado no aparecerá en la salida de `jobs` porque `jobs` solo muestra trabajos activos.

## 20. Detener y continuar en 2º plano un proceso lanzado en 1º plano
a. Detener el proceso:
    ```bash
    kill -SIGSTOP [PID]
    ```
b. Continuar en segundo plano:
    ```bash
    bg %[job_number]
    ```
