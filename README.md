# INSTALACION SERVIDORES EN UBUNTU
---

## Usuarios
1.  usuario: manup --> contraseña: manup
2. usuario: usuarioftp -->contraseña: usuario
3. usuario: usuariossh --> contraseña: usuario

---------
## IP del Servidor

192.168.1.205

---------

## Instalación de SSH

1. Para simplificar, ejecuta `sudo bash` para obtener privilegios de administrador y evitar repetir `sudo`.
2. Actualiza los paquetes con `apt update`.
3. Instala el servidor SSH usando `apt install openssh-server`. Debería mostrar algo similar a:
   ![alt](<imagenes/Captura de pantalla 2026-03-18 214659.png>)

4. Verifica el estado con `systemctl status ssh` para confirmar que funciona correctamente.
   ![alt](<imagenes/Captura de pantalla 2026-03-18 220449.png>)
   - Activa SSH al inicio con: `systemctl enable ssh`

5. Permite SSH en el firewall con `ufw allow ssh` (si está activo).
   ![alt](<imagenes/Captura de pantalla 2026-03-18 220509.png>)

## Instalación Servidor Apache

1. Instala Apache con `apt install apache2`.
   ![alt](<imagenes/Captura de pantalla 2026-03-18 220837.png>)

2. Comprueba su estado con `systemctl status apache2`.
   ![alt](<imagenes/Captura de pantalla 2026-03-18 220958.png>)

3. Abre el puerto de Apache en el firewall: `ufw allow in "Apache"`.
   ![alt](<imagenes/Captura de pantalla 2026-03-18 221146.png>)

## Instalación Servidor FTP

1. El servicio FTP más seguro es `vsftpd`, por eso lo elegimos.
2. Instálalo con `apt install vsftpd`.
   ![alt](<imagenes/Captura de pantalla 2026-03-18 221248.png>)

3. Verifica su estado con `systemctl status vsftpd`.
   ![alt](<imagenes/Captura de pantalla 2026-03-18 221318.png>)

4. Habilita los puertos FTP en el firewall: `ufw allow 20/tcp` y `ufw allow 21/tcp`.
   - Puerto 20: transferencia de datos
   - Puerto 21: control de conexión
   ![alt](<imagenes/Captura de pantalla 2026-03-18 221355.png>)

---

## Usuario en Servidor FTP

1. Crea el usuario con `adduser (nombre)`, establece contraseña y presiona Enter para el resto.
   ![alt](<imagenes/Captura de pantalla 2026-03-18 221438.png>)

2. Edita la configuración: `nano /etc/vsftpd.conf`

3. Activa `chroot_local_user` quitando el `#` inicial.

4. Activa `write_enable=YES` de la misma forma.

5. Añade al final: `allow_writeable_chroot=YES`, guarda con Ctrl+O.
   ![alt](<imagenes/Captura de pantalla 2026-03-18 222040.png>)

6. Reinicia el servicio: `systemctl restart vsftpd`.

------

## Usuario SSH

1. Crea el usuario: `adduser (nombre)`
   ![alt](<imagenes/Captura de pantalla 2026-03-18 222140.png>)

2. Añádelo al grupo sudo: `usermod -aG sudo (nombre)`

## Cambiar a Adaptador Puente

1. Apaga la VM: `poweroff`
2. En configuración de VM → Red → Selecciona "Adaptador puente"
   ![alt](<imagenes/Captura de pantalla 2026-03-18 222325.png>)
3. Inicia la máquina.

--------

## Cómo Transferir Archivos

1. Instala FileZilla en el portátil (máquina anfitriona)
2. Obtén la IP de la VM: `ip addr`
3. Conecta en FileZilla (IP, usuario, contraseña, puerto) → "Conexión rápida"
   ![alt](<imagenes/Captura de pantalla 2026-03-18 223133.png>)

4. Si hay error de permisos: `sudo chown -R usuarioftp:usuarioftp /home/usuarioftp`

-------

## Pasar de FTP a Apache

1. Copia el proyecto a Apache: `sudo cp -R /home/usuarioftp/nombre_proyecto/* /var/www/html/`
2. La web estará disponible usando la IP del servidor.
