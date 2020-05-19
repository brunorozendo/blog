---
title: "archlinux wifi"
description: "Configuração inicial par  ter wifi no arch linux."
author:
  name: "Bruno Rozendo"
date: 2015-04-15
tags:
- archlinux
- wifi
- gnu/linux
- networkmanager
comments: true
imageCredit: "<a href='http://www.freepik.com'>Designed by Freepik</a>"
image: "/images/posts/archlinux-wifi.svg"
---


O método apresentado aqui server para Desktop Envirement tais como Unity, XFCE, LXDE,... 

Nós vamos usa o NetworkManager. Este que é usando em quase todas as distros linux. 

Vamos tomar como ponto de partida o momento em que se instala  o Arch Linux  e que se tem uma conexão com a internet.

Caso esse não seja o seu caso digite no terminal :

{{< terminal >}}{{< highlight bash >}}root@archlinux ~# ping -c4 google.com
{{< /highlight >}}
{{< /terminal >}}

Caso a sua saida tenha sido algo parecido com:

{{< terminal >}}{{< highlight bash >}}--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 4618ms
{{< /highlight >}}
{{< /terminal >}}

Tudo Ok. Se sua maquina estiver na wifi

{{< terminal >}}{{< highlight bash >}}sudo wifi-menu 
{{< /highlight >}}
{{< /terminal >}}

Vamos ao que interessa:


A partir desse momento você deve ter uma conexão. 

{{< terminal >}}{{< highlight bash >}}sudo pacman -S networkmanager network-manager-applet
sudo pacman -S wireless_tools dbus
sudo pacman -S gnome-keyring
sudo systemctl enable NetworkManager.service
sudo systemctl enable wpa_supplicant.service
{{< /highlight >}}
{{< /terminal >}}


Pronto agora basta reiniciar.
