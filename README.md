# wb2-debian

## Instalación base

Descargue una imagen de debian 10 minimal e instalala una VM con un hdd de 10gb en tu ordenador configurando la red en NAT y con las siguientes particiones de sistema:
- 5gb /
- 2gb /home
- resto /var
- sin swap

## Ampliar disco

Una vez instalado, apaga la máquina virtual y amplía el disco a 20gb en el software de virtualización. A continuacion enciende la maquina virtual y amplia la partición var con:
```
fdisk /dev/sda

Welcome to fdisk (util-linux 2.33.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help): d
Partition number (1-3, default 3): 3

Partition 3 has been deleted.

Command (m for help): n
Partition type
   p   primary (2 primary, 0 extended, 2 free)
   e   extended (container for logical partitions)
Select (default p): p
Partition number (3,4, default 3): 3
First sector (13670400-41943039, default 13670400): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (13670400-41943039, default 41943039): 

Created a new partition 3 of type 'Linux' and of size 13.5 GiB.
Partition #3 contains a ext4 signature.

Do you want to remove the signature? [Y]es/[N]o: n

Command (m for help): w

The partition table has been altered.
Syncing disks.
```
Ahora hacemos resize al sistema de ficheros:
```
resize2fs /dev/sda3
```

## Instalar tools sistema virtualización
 
virtualbox
1. En el menu de virtualbox:
devices -> insert guest additions cd image
 
2. En la maquina virtual:
```
apt update
apt install build-essential dkms linux-headers-$(uname -r)
mount /dev/cdrom /mnt
sh /mnt/VBoxLinuxAdditions.run --nox11
reboot
```

## Configura el accesso con llave ssh
```
mkdir ~/.ssh/
vi ~/.ssh/authorized_keys
```
Copiamos el contenido de cat *~/.ssh/id_rsa.pub*
 
Si no tenemos clave creada, antes hacer:

`ssh-keygen -t rsa -b 4096`

Comprueba que puedes acceder por ssh.

> Pregunta 1 : Arregla el problema y comprueba de nuevo que puedes subir archivos (no se puede subir ficheros a ftp2)
	
  `chown sftp2 /home/sftp2/upload`
  
> Pregunta 2 : mirar como poner filtro de ip en el puerto 22 para que solo acepte conexiones de nuestra IP.
	
  En rules de shorewall metemos esta regla
```
  ACCEPT          net:95.127.164.224      $FW             tcp     22
```
> Pregunta 3 : instalar gentoo stage 3

Seguimos este tutorial:
https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage/es

