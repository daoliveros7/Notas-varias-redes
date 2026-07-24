# COMANDOS PARA INCLUIR SOPORTE DE IMAGENES IOU 32-BITS EN GNS3 VM #

# 1. Habilitar la arquitectura de 32 bits y actualizar los repositorios
sudo dpkg --add-architecture i386
sudo apt-get update

# 2. Instalar el paquete de dependencias IOU nativo de GNS3
sudo apt-get install -y gns3-iou

# 3. Descargar e instalar las librerías antiguas de OpenSSL necesarias para IOU (Librería de 32 bits)
wget http://ubuntu.com

sudo dpkg -i libssl1.1_1.1.1f-1ubuntu2.24_i386.deb

# 4. Crear el enlace simbólico para corregir el error libcrypto.so.4
sudo ln -s /usr/lib/i386-linux-gnu/libcrypto.so.1.1 /usr/lib/libcrypto.so.4

# 5. Reiniciar la GNS3 VM para aplicar todos los cambios de dependencias
sudo reboot

