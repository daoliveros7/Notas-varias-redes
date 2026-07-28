# Tips mantenimiento de repositorios Debian

A veces, los servidores de repositorios pueden desactualizarse o eliminar paquetes y dependencias con el paso del tiempo.

Lo descrito a continuación ayudará a que las fuentes siempre mantengan paquetes estables:

- En una consola bash: 
# Elimina los archivos de listas e índices descargados previamente
sudo rm -rf /var/lib/apt/lists/*

# Limpia los archivos parciales y la caché local
sudo apt clean

# Vuelve a descargar índices limpios y frescos desde el servidor
sudo apt update
