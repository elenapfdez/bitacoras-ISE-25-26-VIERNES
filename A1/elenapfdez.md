[elenapfdez](https://github.com/elenapfdez/bitacoras-ISE-25-26-VIERNES) 

<details>
  <summary><strong>P1-Lección 1 </strong></summary>

### Paso 1: Crear discos y particiones
Desde configuración/almacenamiento creamos otro disco de 10 GB.  
En cada disco hacemos una partición de 500 MB y otra con el tamaño restante.  
Luego configuramos dos RAID1:
- RAID1 #0 → Asociado a las particiones pequeñas (500 MB), montado en `/boot`.
- RAID1 #1 → Asociado a las particiones grandes.

<p align="center">
  <img width="600" src="https://github.com/user-attachments/assets/1e368ac0-542a-4226-8ccd-7440e536f1dd" alt="RAID1 sobre /boot y resto del sistema">
</p>

---

### Paso 2: Crear y cifrar volúmenes LVM

En nuestro RAID1 #1:
- Configuramos el gestor de volúmenes físicos (LVM).
- Creamos tres volúmenes: `swap`, `home` y `root`.
- Luego, ciframos cada volumen.

<p align="center">
  <img width="600" src="https://github.com/user-attachments/assets/f4ee05f0-ac7c-445d-9650-f3a30cb2333f" alt="Volúmenes físicos">
</p>

---

### Paso 3: Montaje de volúmenes

Montamos los volúmenes cifrados:
- `/home`
- Área de intercambio (`swap`)
- `/` raíz del sistema

<p align="center">
  <img width="600" src="https://github.com/user-attachments/assets/5a173595-b757-4219-932f-dc8dd37759c7" alt="Montaje de volúmenes">
</p>

---

### Resultado final

Así queda el esquema de particionado y montaje:

<p align="center">
  <img width="800" src="https://github.com/user-attachments/assets/08a73a8e-1fd4-4b00-917d-b8d2655b7da1" alt="Esquema final particionado">
</p>

</details>

<details>
  <summary><strong>P1-Lección 2 </strong></summary>

### Paso 1: Configuración usuarios
Tras iniciar la máquina configuramos el usuario y la contraseña y modificamos `/etc/sudoers` para dar todos los privilegios.
Desde la configuración crearemos un nuevo disco y crearemos una nueva partición en `sdb`, la reconoceremos como `sdb1`.
A continuación, creamos el nuevo LV `new_var` desde el VG almalinux, vemos que en nuestro caso es almalinux, para ello utilizaremos el siguiente comando: `lvcreate –n new_var –L 3G almalinux`
Estos pasos los veríamos reflejados de la siguiente manera:
  
<p align="center">
<img width="593" height="398" alt="Captura desde 2025-10-10 14-59-10" src="https://github.com/user-attachments/assets/caa128e6-9852-4cc3-8a89-a01bd0db8d7d" />
</p>

---

### Paso 2: Sistema de ficheros /var
Ahora pasaremos a montar el sistema de ficheros en `/var`, una vez creado, se formatea con el sistema de archivos ext4 y se monta temporalmente en `/new_var`. Se copia toda la información del directorio `/var` al nuevo volumen.

<p align="center">
<img width="871" height="437" alt="Captura desde 2025-10-10 15-17-07" src="https://github.com/user-attachments/assets/a6349575-9978-4afb-b5b7-85b31f27d29f" />
</p>

---

### Paso 3: Archivo fstab
Después, se edita el archivo `/etc/fstab` para indicar que el nuevo punto de montaje de `/var` debe ser el volumen `/dev/mapper/almalinux-new_var`.  Una vez actualizada la configuración, se comprueba con `lsblk` y se ejecuta `mount -a` para aplicar los cambios.

<p align="center">
<img width="1019" height="795" alt="Captura desde 2025-10-10 16-37-06" src="https://github.com/user-attachments/assets/203c9845-3552-436e-836d-e113e4ecacd7" />
</p>

Una vez copiado el contenido y configurado el nuevo `/var`, es necesario liberar el espacio del antiguo.
Para ello, se comenta temporalmente la línea del nuevo `/var` en `/etc/fstab` y se ejecuta `mount -a`, lo que permite acceder nuevamente al `/var` original.
Se mueve su contenido a otro directorio (/var_old) para conservarlo como respaldo.
Luego, se descomenta la línea en fstab para volver a montar el nuevo volumen lógico como `/var`.

<p align="center">
<img width="1020" height="796" alt="Captura desde 2025-10-10 16-46-31" src="https://github.com/user-attachments/assets/6db928a4-b569-42f9-adf2-86b9dcb0c374" />
</p>

---

### Resultado final
Finalmente quedaría así: 

<p align="center">
<img width="1023" height="304" alt="Captura desde 2025-10-10 16-48-09" src="https://github.com/user-attachments/assets/d75daaa1-f55c-41ee-b88e-53b76eb83a5a" />
</p>

</details>
