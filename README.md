# **Mi guía personalizada de configuración de Debian para Desktop**

## Configurar el repositorio

nano /etc/apt/sources.list

deb https://deb.debian.org/debian/ bookworm main contrib non-free-firmware non-free

deb https://deb.debian.org/debian/ bookworm-updates main contrib non-free-firmware non-free

deb https://deb.debian.org/debian/ bookworm-backports main contrib non-free-firmware non-free


## Instalar sudo

apt install sudo

### Adicionar usuario al grupo sudo

gpasswd sudo -a <usuario>


## Acualizar el sistema

apt update

apt upgrade

## Configurar Swap del sistema

_Como tenemos SSD no creamos particion de swap, usamos zram y creamos un fichero de swap de reserva

### Crear fichero de swap:

dd if=/dev/zero of=/var/swap bs=4096k count=1024
sync

chmod 600 /var/swap

### Formatear
mkswap /var/swap

### Activar la swap

swapon /var/swap

### Comprobar que esta funcionando
swapon -s

### Editar fstab para que cargue con el sistema

nano /etc/fstab

adicionar:

/var/swap  none        swap    sw     0     0


### zram:

apt install zram-tools

echo -e "ALGO=zstd\nPERCENT=40" | sudo tee -a /etc/default/zramswap

service zramswap reload

## Instalación de paquetes


### Instalacion de firmware

apt -y install firmware-linux intel-microcode

### Paquete necesario para el buscador de Synaptic

apt -y install apt-xapian-index

### Los archivos de cabecera del kernel:

apt -y install linux-headers-$(uname -r)

### Paquetes necesarios para compilar

apt -y install build-essential make automake cmake autoconf

### Instalador gráfico de paquetes .deb

apt -y install gdebi

### Isntalación de codecs multiumedia

apt -y install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-qt6 gstreamer1.0-pipewire gstreamer1.0-pulseaudio gstreamer1.0-vaapi gstreamer1.0-plugins-base-apps gstreamer1.0-plugins-bad-apps gstreamer1.0-libcamera gstreamer-qapt ffmpeg sox twolame vorbis-tools lame faad mencoder

### Reproductor de video

apt -y install vlc

### Reproductor de audio

apt -y install audacious

### Instalación de rsync

apt -y install rsync

### Configuración de flatpak

apt -y install flatpak

_Plugin para el gestor de software_

apt -y install gnome-software-plugin-flatpak

_Adicionar repositorio de flathub_

flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

 _Es necesaio reiniciar_

## Instalacion de fuentes

apt install fonts-freefont-ttf fonts-freefont-otf fonts-inconsolata fonts-droid-fallback xfonts-terminus fonts-droid-fallback ttf-bitstream-vera fonts-cantarell fonts-liberation fonts-oflb-asana-math fonts-mathjax

### Fuentes de microsoft, necesito VPN porque se descargan de  https://downloads.sourceforge.net

apt-get install ttf-mscorefonts-installer

### Actualizar la cache de fuentes

sudo fc-cache -fv

## Compresion de archivos

apt -y install rar unrar unace p7zip-full p7zip-rar lzip arj sharutils mpack lzma lzop unzip zip bzip2 lhasa cabextract lrzip rzip zpaq kgb xz-utils

## Dispositivos moviles

apt-get install mtp-tools ipheth-utils ideviceinstaller ifuse

## Gestionar usuarios y grupos

apt-get install gnome-system-tools

## Eliminar los juegos

apt remove gnome-games

apt autoremove

## Instalar python3 desde las fuentes

### Paquetes necesarios
apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libgdbm-compat-dev libgdbm-dev

tar xvf Python-3.13.0.tgz

cd Python-3.13.0/

./configure --enable-optimizations --with-ensurepip=install

make -j8

make altinstall

_Es importante que sea altinstall para no reemplazar la instalacion de python del sistema_


## Instalar oh-my-Bash

bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"


## Instalacion de golang

rm -rf /usr/local/go && tar -C /usr/local -xzf go1.23.2.linux-amd64.tar.gz

### Adicionar al PATH del sistema:

export PATH=$PATH:/usr/local/go/bin

## Instalacion de nodejs

## Desde el propio usuario, no es necesario root aqui

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash

nvm install 20

__Esto instalara la version 20 de nodejs__

## Instalar yarn

npm install --global yarn


## Crear llave SSH para los repositorios de git

ssh-keygen -t rsa -b 4096 -C "usuario@hostname" -f /home/<usuario>/.ssh/<nombre_llave> -N "frase privada"

ssh-add /home/<usuario>/.ssh/<nombre_llave_privada>

ssh-add -l  Para mostrar que la llave esta cargada

cat /home/<usuario>/.ssh/<nombre_llave>.pub

El contenido se __<nombre_llave>__ .pub es lo que se inserta el servidor de git, como GitHub

_Para comprobar con gitHub:_

ssh -T git@github.com

## Instalacion de algunas herramientas utiles

apt install meld timeshift gedit gedit-plugins gparted ghex git git-cola


## Limpieza y reiniciar

apt clean

apt autoremove

apt --configure -a

reboot
