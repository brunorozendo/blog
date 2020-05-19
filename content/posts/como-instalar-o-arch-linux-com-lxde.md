---
title: "Como instalar o ArchLinux com LXDE (sem UEFFI)"
description: "instalação completa do arch linux sem usar o UEFFI"
author:
  name: "Bruno Rozendo"
date: 2018-06-28
tags:
- archlinux
- lxde
- gnu/linux
comments: true
imageCredit: "<a href='https://github.com/brunorozendo/linux-pictures'>Linux Pictures</a>"
image: "/images/posts/como-instalar-o-arch-linux-com-lxde/banner-archlinux.png" 
math: true
---


| Rv.              | Data        |
| -----------------| ------------|
| Primeira revisão |  01/06/2015 | 
| Segunda revisão  |  28/06/2018 |





__Obs__: A instalação a seguir  e feita numa maquina real (não é maquina virtual) por causa disso a parte relacionada em conexão com a internet pode divergir de outros tutoriais.


Esse tutorial vai mostrar como instalar o Arch Linux com o gerenciador de janelas [LXDE](https://lxde.org/). No momento em que se escreve esse post
a versão é __Current Release: 2018.06.01__.

__Atenção:__ a parte da instalação do Arch Linux é válida idependente da interface grafica a ser instalada.
O que isso quer dizer é: Você pode fazer esse tutorial até a parte em que instala o Arch Linux, trocar de tutorial e instalar outra interface
como [XFCE](https://xfce.org/), [Mate](https://mate-desktop.org/pt/), [Unity](https://launchpad.net/unity) ou [Gnome](https://www.gnome.org/).



E vamos lá. Os passos a seguir serão:

	1. Requisitos básicos
	2. Instalar
	 2.1 Conexão com internet
	  2.1.1 Conexão com internet wifi
	 2.2 Particionar e formatar o HD
	3. Instalar o Arch Linux no HD
	 3.1 Mudar mirror (opcional)
	 3.2 Instalar pacotes
	4. Instalar outros programas
	5. Instalar o LXDE




### 1. Requisitos básicos

#### <span class="red">Ter acesso a internet </span>(cabo ou wifi).

Comece baixando [Arch Linux](https://www.archlinux.org/), e instale ele dentro do seu pendrive (ou cd).

Se estiver usando um outro sistemas linux e for usar um pendrive é possivel fazer através do terminal com o seguinte comando.

{{< terminal >}}{{< highlight bash >}}sudo dd if=archlinux-2015.02.01-dual.iso of=/dev/sdb bs=4M && sync
{{< /highlight >}}
{{< /terminal >}}


Lembrando que __/dev/sdb__ deve ser a referência para o usb.

Agora se estiver usando Windows pode usar o programa [Universal USB Installer](http://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/).




### 2. Instalar

Reinicer seu computardor com a midia inserida. a tela que deve aparecer é


{{< image src="/images/posts/como-instalar-o-arch-linux-com-lxde/archlinux.jpg"  >}}

Dependendo da maquina a opção de x64 pode não aparecer, indicando que a maquina não tem suporte.

Escolha sua opção entre x86 (32 bits) ou x64 (64 bits) e de  um enter.

Depois de disso deve aparecer :

{{< terminal >}}{{< highlight bash >}}root@archlinux ~#{{< /highlight >}}{{< /terminal >}}


#### 2.1 Conexão com  internet

Digite

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# ping -c4 google.com{{< /highlight >}}{{< /terminal >}}

Caso a sua saida tenha sido algo parecido com:

{{< terminal >}}{{< highlight bash >}}--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 4618ms
{{< /highlight >}}{{< /terminal >}}

Não será necessário fazer mais nada, pois conexão já está feita (muito provalvelmente sua internet deve estar vindo via cabo).



#### <span class="red">Pular para o 2.2</span>.

#### 2.1.1 Conexão com  internet wifi

Caso você esteja usando wifi (como num notebook ) e sua saida tenha sido:

{{< terminal >}}{{< highlight bash >}}ping: unknown host google.com{{< /highlight >}}{{< /terminal >}}

Digite

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# wifi-menu{{< /highlight >}}{{< /terminal >}}


Então selecione a sua rede wifi e de enter


> __Atenção__: na hora no nome do profile, alguns teste que fiz quando se tem algum espaço entre as palavras dava um erro mais na frente dizendo que não encontrava o profile.
> Dica: salve o nome do profile sem espaços Ex:
> {{< highlight bash >}}nome sugerido (e potencialmente problematico):
wlp4s0-NET BRUNO
nome alternativo (e seguro)
wlp4s0NETBRUNO 
{{< /highlight >}}



<!--
Escolha a sua rede e ponha sua senha. E por fim digite as seguintes linhas:

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# systemclt start dhcpcd
root@archlinux ~# systemclt status dhcpcd
root@archlinux ~# ping -c4 google.com{{< /highlight >}}{{< /terminal >}}

-->
Agora sua saida deve ser como:

{{< terminal >}}{{< highlight bash >}}--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 4618ms
{{< /highlight >}}{{< /terminal >}}

Agora que a conexão está pronta, vamos para o proximo passo.


#### 2.2 Particionar e formatar o HD

Aqui estou assumindo que você usara todo o seu HD para o Arch Linux. Tanto no tutorial oficial do Arch, como outros artigos, é feito uma partição para  boot, root e home. Particularmente eu não fasso isso. Eu prefiro uma única partição, é o que eu acho mais fácil.

> __Atenção__: se o seu HD nunca tiver sido usado, pode aparecer uma tela perguntando o tipo, caso isso aconteça escolha a opção "DOS".

Digite:

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# fdisk -l

Disco /dev/sda: 298,1 GiB, 320072933376 bytes, 625142448 setores
Unidades: setor de 1 * 512 = 512 bytes
Tamanho de setor (lógico/físico): 512 bytes / 512 bytes
Tamanho E/S (mínimo/ótimo): 512 bytes / 512 bytes
Tipo de rótulo do disco: dos
Identificador do disco: 0x0008393b

Device     Boot Start       End   Sectors   Size Id Type
/dev/sda1  *     2048 625142447 625140400 298,1G 83 Linux 
{{< /highlight >}}{{< /terminal >}}

No meu caso __/dev/sda__ é o meus hd (descrobri isso baseado no tamanho 298,1 GiB).

Agora vamos particiona-lo,

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# cfdisk /dev/sda 
{{< /highlight >}}
{{< /terminal >}}


 1. Delete todas as partições existentes.
 2. Sobre o "Free Sapce" escolha a opção "new" (na parte insferior vai aparecer o tamnhao total do HD ) e de um enter
 3. Sobre a recem criada partição (e única disponivel) escolha a opção "Bootable"  de um enter
 4. Vá em "Write" e digite 'yes'
 5. então vá em "Quit"

 Agora vamos formatar o o HD

No passo anterior no a nova partição ficou com novo nome na coluna 'Device',  vamos usar esse nome nos próximos comandos.

{{< terminal >}}{{< highlight bash >}}
root@archlinux ~# mkfs -t ext4 /dev/sda1 
{{< /highlight >}}
{{< /terminal >}}

### 3. Instalar o Arch Linux no HD

No passo anterior no a nova partição ficou com novo nome na coluna 'Device',  vamos usar esse nome nno próximo comando.

A primeira coisa a fazer e mountar a o HD, 

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# mount /dev/sda1 /mnt
{{< /highlight >}}
{{< /terminal >}}


#### 3.1 Mudar mirror (opcional)


Antes de começar a instalar vamos mudar o mirror list para buscar os arquivos em servidores do Brasil.

Isso server para diminuir o tempo de download. Não é obrigatório  mas para mim fez bastante diferença.

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# nano /etc/pacman.d/mirrorlist 
{{< /highlight >}}
{{< /terminal >}}


 1. `Crtl+W`
 2. digite `Brazil`
 3. Na linha de baixo onde está: 'Server = http://pet.inf.ufsc.br/...'  aperte `Ctrl+K` (isso vai recortar a linha)
 3. aperte `Ctrl+U` (isso vai colar a linha de volta no mesmo lugar)
 4. Vá para o inicio do arquivo
 5. Em uma linha que não tena nada aperte `Ctrl+U`
 5. `Ctrl+x` para sair e salvar


Exemplo de como o aqruivo deve ficar no final

{{< terminal >}}{{< highlight bash >}}##
## Arch Linux repository mirrorlist
## Sorted by mirror score from mirror status page
## Generated on 2018-06-01
##

Server = http://br.mirror.archelinux-br.org/$repo/os/$arch

## Germany
Server = http://mirror.gnomus.de/$repo/os/$arch
## France
...
## United States
Server = http://www.gtlib.gatech.edu/pub/archlinux/$repo/os/$arch
## Brazil
Server = http://br.mirror.archelinux-br.org/$repo/os/$arch
## United States
Server = http://mirrors.cat.pdx.edu/archlinux/$repo/os/$arch
...
{{< /highlight >}}{{< /terminal >}}

### 3.2 Instalar 


Pronto, agora vamos realmente instalar o Arch Linux

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# pacstrap /mnt base base-devel
{{< /highlight >}}
{{< /terminal >}}

Agora espere (pode demorar...)

Agora que terminou de baixar e instalar a base vamos gerar o fstab

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# genfstab -p /mnt >> /mnt/etc/fstab
{{< /highlight >}}
{{< /terminal >}}

Vamos entrar no sistema recém instalado

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# arch-chroot /mnt
[root@archiso /]#
{{< /highlight >}}
{{< /terminal >}}

Vamos configurar a senha do root

{{< terminal >}}{{< highlight bash >}}[root@archiso /]# passwd
{{< /highlight >}}
{{< /terminal >}}


Vamos configurar o layout do teclado (isso é temporario, no proximo reboot o layout volta para  o americano)

{{< terminal >}}{{< highlight bash >}}[root@archiso /]# loadkeys br-abnt2
{{< /highlight >}}
{{< /terminal >}}


Para deixa o teclado em pt-br de uma vez por todas vai ser necessário um pouco mais de trabalho. E vamos lá:

{{< terminal >}}{{< highlight bash >}}[root@archiso /]# mkdir -p /etc/X11/xorg.conf.d/
[root@archiso /]# nano  /etc/X11/xorg.conf.d/10-evdev.conf
{{< /highlight >}}
{{< /terminal >}}


E digite o conteúdo dentro do arquivo 10-evdev.conf

{{< terminal >}}{{< highlight bash >}}Section "InputClass"
  Identifier "evdev keyboard catchall"
  MatchIsKeyboard "on"
  MatchDevicePath "/dev/input/event*"
  Option "XkbLayout" "br"
  Option "XkbVariant" "abnt2"
EndSection
{{< /highlight >}}
{{< /terminal >}}

>  <span class="red">__Atenção__</span>: Tenha certeza que o arquivo está igual ao do exemplo. Em testes percebi que caso esse arquivo fosse escrito errado o sistema não ligava, me forçando a reiniciar a maquina com se fosse instalr e montar a partição e editar o arquivo.


Agora vamos configurar  a lingua do sistema para pt-br

{{< terminal >}}{{< highlight bash >}}[root@archiso /]# nano /etc/locale.gen
{{< /highlight >}}
{{< /terminal >}}

 1. Crtl+W
 2. digite `pt_BR.UTF-8`
 3. Retirar o comentario "#" no inicio da linha
 7. Ctrl+x para sair e salvar


O arquivo final deve ficar assim:

{{< terminal >}}{{< highlight bash >}}...
en_US.UTF-8
...
#ps_AF UTF-8
pt_BR.UTF-8 UTF-8
#pt_BR ISO-8859-1
{{< /highlight >}}
{{< /terminal >}}


 Com o arquivo pronto, vamos inpor ao sistema a nova lingua

{{< terminal >}}{{< highlight bash >}}[root@archiso /]# locale-gen
Generating locales...
pt_BR.UTF-8... done
Generation complete.
[root@archiso /]# echo LANG=pt_BR.UTF-8 > /etc/locale.conf
[root@archiso /]# export LANG=pt_BR.UTF-8
{{< /highlight >}}
{{< /terminal >}}

Agora vamos configurar a hora ( no meu caso é litorial -3)


{{< terminal >}}{{< highlight bash >}}[root@archiso /]# ls /usr/share/zoneinfo/Brazil/
Acre  DeNoronha  East  West
[root@archiso /]# rm /etc/localtime
[root@archiso /]# ln -s /usr/share/zoneinfo/Brazil/East /etc/localtime
{{< /highlight >}}
{{< /terminal >}}
Lebrando que: Acre = -5h, West = -4h, East = -3h, DeNoronha = -2h.

Vamos instalar o o gerenciador de boot


{{< terminal >}}{{< highlight bash >}}[root@archiso /]# pacman -S grub
[root@archiso /]# grub-install /dev/sda
[root@archiso /]# mkinitcpio -p linux
[root@archiso /]# grub-mkconfig -o /boot/grub/grub.cfg
{{< /highlight >}}
{{< /terminal >}}

Se você estiver usando wifi é necessário instalar os seguintes pacotes:

{{< terminal >}}{{< highlight bash >}}[root@archiso /]# pacman -S dialog wpa_supplicant
{{< /highlight >}}
{{< /terminal >}}


Terminamos com o HD agora e hora de reiniciar, e <span class="red">__não esquece de remover a midia__</span>.


{{< terminal >}}{{< highlight bash >}}[root@archiso /]# exit
root@archlinux ~# umount -a
root@archlinux ~# reboot
{{< /highlight >}}
{{< /terminal >}}

### 4. Instalar outros programas



Vamos começar a instalar os drives de video e audio .

O para descobri o drive de video veja

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# lspci | grep VGA
{{< /highlight >}}
{{< /terminal >}}

a saida do meu foi

{{< terminal >}}{{< highlight bash >}}00:02.0 VGA compatible controller: Intel Corporation Core Processor Integrated Graphics Controller (rev 02)
{{< /highlight >}}
{{< /terminal >}}

Nesse caso o pacote para o drive da intel


{{< terminal >}}{{< highlight bash >}}root@archlinux ~# pacman -S xf86-video-intel
{{< /highlight >}}
{{< /terminal >}}


Agora vamos aos outros drives (audio, video, touchpad)

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# pacman -S alsa-utils alsa-firmware
root@archlinux ~# pacman -S xorg-server xorg-xinit xorg-apps
root@archlinux ~# pacman -S xorg-twm xorg-xclock xterm
root@archlinux ~# pacman -S ttf-dejavu
root@archlinux ~# pacman -S xf86-input-synaptics
root@archlinux ~# pacman -S wicd wicd-gtk
root@archlinux ~# systemctl enable wicd.service
{{< /highlight >}}
{{< /terminal >}}

Agora vamos testar se os drives estão devidamente instalados e funcionando

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# startx
{{< /highlight >}}
{{< /terminal >}}

A tela abaixo deve aparecer

{{< image src="/images/posts/como-instalar-o-arch-linux-com-lxde/startx.png"  >}}


Para sair digite "exit" em todas as janelas

Agora vamos instalar os prgramas basicos:

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# pacman -S firefox
root@archlinux ~# pacman -S firefox-i18n-pt-br
root@archlinux ~# pacman -S flashplugin
root@archlinux ~# pacman -S jdk8-openjdk
{{< /highlight >}}
{{< /terminal >}}


Falta muito pouco agora...


Até esse momento somente usamos o usuario root, então vamos criar o nosso;

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# useradd -m bruno
root@archlinux ~# passwd bruno
root@archlinux ~# nano /etc/sudoers
##
## User privilege specification
##
root ALL=(ALL) ALL
bruno ALL=(ALL) ALL

root@archlinux ~# pacman -S xdg-user-dirs


{{< /highlight >}}
{{< /terminal >}}

Salve o arquivo sudoers.


### 5. Instalar o LXDE

O fim

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# pacman -S lxde
root@archlinux ~# systemctl enable lxdm
{{< /highlight >}}
{{< /terminal >}}

Reicie e pronto tudo funcionado



## referências

[Wiki Arch Linux](https://wiki.archlinux.org/index.php/Installation_Guide_%28Portugu%C3%AAs%29)


[Linux Veda](http://www.linuxveda.com/2014/06/07/arch-linux-tutorial/)

[Marcelo Fox (video)](https://www.youtube.com/watch?v=7sGSroOz4y0)

[Marcelo Fox (artigo)](http://marcelfox.com/blog/instalando-o-archlinux-de-uma-vez-por-todas/)
