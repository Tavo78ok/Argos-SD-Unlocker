## ArgOS SD Unlocker
<p align="center">
  <img src="https://img.shields.io/badge/version-1.2.0-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/platform-Linux-orange?style=for-the-badge&logo=linux" />
  <img src="https://img.shields.io/badge/GTK-4.0-green?style=for-the-badge" />
  <img src="https://img.shields.io/badge/libadwaita-1.x-purple?style=for-the-badge" />
  <img src="https://img.shields.io/badge/license-GPL--3.0-red?style=for-the-badge" />
</p>
<p align="center">
  Herramienta GTK4/libadwaita para desbloquear, reparar, formatear y limpiar tarjetas SD en Linux.<br/>
  Parte del ecosistema <strong>ArgOS Platinum Edition</strong>.
</p>

Capturas de pantalla




# Características:

- DesbloquearFuerza🔍 Verificar bloqueoDetecta si la tarjeta está en modo solo lectura (blockdev --getro)🔓

- DesbloquearFuerza modo lectura/escritura (blockdev --setrw)💾

- FormatearFAT32, NTFS o EXT4 con etiqueta ARGOS_SD🛠️ RepararEjecuta fsck -y desmontando previamente el dispositivo🧬 Recuperar superbloqueRecuperación avanzada de particiones ext2/3/4 con superbloque corrupto via e2fsck -b🧹

- LimpiarBorrado rápido (zeros) o seguro (zeros + aleatorio, 2 pasadas)
Otras características

# Detección automática de dispositivos removibles via lsblk
Autenticación gráfica mediante polkit / pkexec (sin necesidad de terminal)
Operaciones en hilo secundario — la UI nunca se congela
Notificaciones tipo toast y spinner de progreso
Confirmación obligatoria antes de operaciones destructivas
Registro de actividad en tiempo real


Requisitos
Sistema

Linux con GNOME o entorno compatible con GTK4/libadwaita
polkit instalado y activo (policykit-1 o polkitd)

Dependencias (instaladas automáticamente con el .deb)
python3 >= 3.8
python3-gi
gir1.2-gtk-4.0
gir1.2-adw-1
util-linux       # blockdev, lsblk
e2fsprogs        # fsck, e2fsck, mke2fs
dosfstools       # mkfs.vfat
ntfs-3g          # mkfs.ntfs
policykit-1 | polkitd

Instalación
Desde el .deb (recomendado)
bash# Descargar la última release
wget https://github.com/Tavo78ok/argos-sd-unlocker/releases/latest/download/argos-sd-unlocker_1.2.0_all.deb

# Instalar
sudo dpkg -i argos-sd-unlocker_1.2.0_all.deb

# Si faltan dependencias
sudo apt install -f
Desde el código fuente
bashgit clone https://github.com/Tavo78ok/argos-sd-unlocker.git
cd argos-sd-unlocker

# Instalar dependencias manualmente
sudo apt install python3-gi gir1.2-gtk-4.0 gir1.2-adw-1 \
    util-linux e2fsprogs dosfstools ntfs-3g policykit-1

# Instalar la policy de polkit
sudo cp usr/share/polkit-1/actions/io.openargos.SDUnlocker.policy \
    /usr/share/polkit-1/actions/

# Instalar el helper privilegiado
sudo cp usr/share/argos-sd-unlocker/sd_unlocker_helper \
    /usr/share/argos-sd-unlocker/
sudo chmod +x /usr/share/argos-sd-unlocker/sd_unlocker_helper

# Ejecutar
python3 usr/share/argos-sd-unlocker/sd_unlocker.py

Uso

Conecta tu tarjeta SD
Abre ArgOS SD Unlocker desde el menú de aplicaciones
Haz clic en Actualizar dispositivos — aparecerá tu tarjeta en el selector
Elige la acción deseada — se pedirá tu contraseña de administrador via el diálogo gráfico del sistema

Recuperación de superbloque corrupto
Si fsck reporta Bad magic number in super-block, usa Recuperar superbloque:

Selecciona el dispositivo ext2/ext3/ext4 afectado
Haz clic en Recuperar superbloque → Recuperar
La app busca automáticamente los bloques de respaldo con mke2fs -n
Prueba cada candidato con e2fsck -b <bloque> hasta encontrar uno válido
Si tiene éxito, haz backup de tus datos antes de reformatear


⚠️ Esta función solo aplica a sistemas de archivos ext2/ext3/ext4. Para FAT32/NTFS usa Reparar o Formatear.

```
Arquitectura
argos-sd-unlocker/
├── usr/
│   ├── bin/
│   │   └── argos-sd-unlocker          # Lanzador
│   └── share/
│       ├── argos-sd-unlocker/
│       │   ├── sd_unlocker.py         # Aplicación GTK4/libadwaita
│       │   └── sd_unlocker_helper     # Helper bash (ejecutado via pkexec)
│       ├── applications/
│       │   └── io.openargos.SDUnlocker.desktop
│       ├── icons/hicolor/scalable/apps/
│       │   └── io.openargos.SDUnlocker.svg
│       └── polkit-1/actions/
│           └── io.openargos.SDUnlocker.policy
¿Por qué un helper separado?
GTK4 no puede llamar a sudo desde una app gráfica sin terminal. El helper bash concentra todos los comandos privilegiados (blockdev, mkfs.*, fsck, dd) y se ejecuta via pkexec, que muestra el diálogo gráfico de autenticación del sistema.

Construir el .deb
bashgit clone https://github.com/Tavo78ok/argos-sd-unlocker.git
cd argos-sd-unlocker
dpkg-deb --build build argos-sd-unlocker_1.2.0_all.deb

Parte del ecosistema ArgOS
ArgOS SD Unlocker es parte de ArgOS Platinum Edition, una distribución Linux basada en Debian con un conjunto de herramientas nativas desarrolladas con GTK4/libadwaita.
Otras apps del ecosistema:

Melodia — Reproductor de música con EQ y letras sincronizadas
ArgOS Tag Studio — Editor de etiquetas MP3/Opus/FLAC
FFmpeg Studio — Conversor multimedia GTK4
RsgainGui — Frontend para normalización de volumen


Licencia
GPL-3.0 © Andrés (Tavo78ok)
