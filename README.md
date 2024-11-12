# **Mi guía personalizada de configuración de Debian para Desktop**

## Configurar el repositorio

```bash
nano /etc/apt/sources.list
```

deb https://deb.debian.org/debian/ bookworm main contrib non-free-firmware non-free

deb https://deb.debian.org/debian/ bookworm-updates main contrib non-free-firmware non-free

deb https://deb.debian.org/debian/ bookworm-backports main contrib non-free-firmware non-free

## Buscar los repositorios mas rapidos

```bash
apt install netselect-apt
```

### Buscar en Estados Unidos los 15 repositorios mas rapidos para amd64

```bash
netselect-apt -c us -t 15 -a amd64
```

## Instalar sudo

```bash
apt install sudo
```

### Adicionar usuario al grupo sudo

```bash
gpasswd sudo -a usuario


## Acualizar el sistema
```bash
apt update

apt upgrade
```

## Configurar Swap del sistema

_Como tenemos SSD no creamos particion de swap, usamos zram y creamos un fichero de swap de reserva_

### Crear fichero de swap:

```bash
dd if=/dev/zero of=/var/swap bs=4096k count=1024
sync
```

```bash
chmod 600 /var/swap
```

### Formatear

```bash
mkswap /var/swap
```

### Activar la swap

```bash
swapon /var/swap
```

### Comprobar que esta funcionando

```bash
swapon -s
```

### Editar fstab para que cargue con el sistema

```bash
nano /etc/fstab
```
adicionar:

/var/swap  none        swap    sw     0     0


### zram:

```bash
apt install zram-tools

echo -e "ALGO=zstd\nPERCENT=40" | sudo tee -a /etc/default/zramswap

service zramswap reload
```

## Instalación de paquetes


### Instalacion de firmware

```bash
apt -y install firmware-linux intel-microcode
```

### Paquete necesario para el buscador de Synaptic

```bash
apt -y install apt-xapian-index
```

### Los archivos de cabecera del kernel:

```bash
apt -y install linux-headers-$(uname -r)
```

### Paquetes necesarios para compilar

```bash
apt -y install build-essential make automake cmake autoconf
```

### Instalador gráfico de paquetes .deb

```bash
apt -y install gdebi
```

### Instalación de codecs multiumedia

```bash
apt -y install libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav gstreamer1.0-tools gstreamer1.0-x gstreamer1.0-alsa gstreamer1.0-gl gstreamer1.0-gtk3 gstreamer1.0-qt5 gstreamer1.0-qt6 gstreamer1.0-pipewire gstreamer1.0-pulseaudio gstreamer1.0-vaapi gstreamer1.0-plugins-base-apps gstreamer1.0-plugins-bad-apps gstreamer1.0-libcamera gstreamer-qapt ffmpeg sox twolame vorbis-tools lame faad mencoder
```

### Reproductor de video

```bash
apt -y install vlc
```

### Reproductor de audio

```bash
apt -y install audacious
```
museeks tiene un filtro global, algo que no he encontrado en audacious

Se pude descargar el .deb desde aqui
https://github.com/martpie/museeks/releases

### Instalación de rsync

```bash
apt -y install rsync
```
### Configuración de flatpak

```bash
apt -y install flatpak
```
_Plugin para el gestor de software_

```bash
apt -y install gnome-software-plugin-flatpak
```
_Adicionar repositorio de flathub_

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
 _Es necesaio reiniciar_

## Instalacion de fuentes

```bash
apt install fonts-freefont-ttf fonts-freefont-otf fonts-inconsolata fonts-droid-fallback xfonts-terminus fonts-droid-fallback ttf-bitstream-vera fonts-cantarell fonts-liberation fonts-oflb-asana-math fonts-mathjax
```

### Fuentes de microsoft, se descargan de  https://downloads.sourceforge.net

```bash
apt-get install ttf-mscorefonts-installer
```
### Actualizar la cache de fuentes

```bash
sudo fc-cache -fv
```
## Compresion de archivos

```bash
apt -y install rar unrar unace p7zip-full p7zip-rar lzip arj sharutils mpack lzma lzop unzip zip bzip2 lhasa cabextract lrzip rzip zpaq kgb xz-utils
```
## Dispositivos moviles

```bash
apt-get install mtp-tools ipheth-utils ideviceinstaller ifuse
```
## Gestionar usuarios y grupos

```bash
apt-get install gnome-system-tools
```
## Eliminar los juegos

```bash
apt remove gnome-games

apt autoremove
```
## Instalar python3 desde las fuentes

### Paquetes necesarios

```bash
apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libgdbm-compat-dev libgdbm-dev

tar xvf Python-3.13.0.tgz

cd Python-3.13.0/

./configure --enable-optimizations --with-ensurepip=install

make -j8

make altinstall
```
_Es importante que sea altinstall para no reemplazar la instalacion de python del sistema_


## Instalar oh-my-Bash

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh)"
```

## Instalacion de golang

```bash
rm -rf /usr/local/go && tar -C /usr/local -xzf go1.23.2.linux-amd64.tar.gz
```
### Adicionar al PATH del sistema:

```bash
export PATH=$PATH:/usr/local/go/bin
```
## Instalacion de nodejs

### Desde el propio usuario, no es necesario root aqui

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash

nvm install 20
```
__Esto instalara la version 20 de nodejs__

## Instalar yarn

```bash
npm install --global yarn
```
## Instalar pnpm

```bash
curl -fsSL https://get.pnpm.io/install.sh | sh -
```
## Crear llave SSH para los repositorios de git

```bash
ssh-keygen -t rsa -b 4096 -C "usuario@hostname" -f /home/<usuario>/.ssh/<nombre_llave> -N "frase privada"

ssh-add /home/<usuario>/.ssh/<nombre_llave_privada>

ssh-add -l  Para mostrar que la llave esta cargada

cat /home/<usuario>/.ssh/<nombre_llave>.pub
```

El contenido de __<nombre_llave>__ .pub es lo que se inserta el servidor de git, como GitHub

_Para comprobar con gitHub:_

```bash
ssh -T git@github.com
```
## Instalacion de algunas herramientas utiles

```bash
apt install meld timeshift gedit gedit-plugins gparted ghex git git-cola
```
### Para crear lanzador personalizado de aplicaciones

instalar arronax

https://www.florian-diesch.de/software/arronax/#

__En debian12 la version 0.8 dio problemas, manualmente se editaron los ficheros
de instalación segun el error.__

## Informacion de archivos multimedia

```bash
apt install mediainfo-gui/stable mediainfo/stable
```

## Virtualizacion

instalar qemu-kvm

```bash
sudo apt install qemu-kvm qemu-system qemu-utils python3 python3-pip libvirt-clients libvirt-daemon-system bridge-utils virtinst libvirt-daemon virt-manager
```

```bash
systemctl status libvirtd.service
```

```bash
sudo usermod -aG libvirt $USER
sudo usermod -aG libvirt-qemu $USER
sudo usermod -aG kvm $USER
sudo usermod -aG input $USER
sudo usermod -aG disk $USER
```
__El propietario de la carpeta que almacena las maquinas virtuales es libvirt-qemu__



## Limpieza y reiniciar

```bash

apt clean

apt autoremove

reboot
```

