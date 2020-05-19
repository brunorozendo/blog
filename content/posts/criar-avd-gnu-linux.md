---
title: "Criar AVD (Android) por linha de comando no Gnu/Linux"
description: "criação de uma maquina virtual android (AVD) no linux Ubuntu"
author:
  name: "Bruno Rozendo"
date: 2017-07-26
draft: false
tags:
- "android"
- "gnu/linux"
- "avd"
- "android na raça"
comments: true
imageCredit: "<a href='http://www.freepik.com'>Designed by ijeab / Freepik</a>"
image: "/images/posts/criar-avd-gnu-linux/criar-avd.jpg"
---




#### 0. Prequisitos

 1. Ter o [android sdk instalado](/post/instalar-adroid-sdk-gnu-linux.html)


#### 1. Criar avd


Uma vez que que já tenha instalado o sdk tudo fica bem mais fácil.

Supondo que queremos rodar uma versão do `Android 5.0 (Lollipop)` abra o terminal e digite e espere baixar:


{{< terminal >}}{{< highlight bash >}}$ sdkmanager "platforms;android-22"
done
$ sdkmanager "system-images;android-22;default;x86_64"
done
{{< /highlight >}}{{< /terminal >}}

>Caso queira ver todas as opções possiveis digite no terminal


{{< terminal >}}{{< highlight bash >}}$ sdkmanager --list --verbose
{{< /highlight >}}{{< /terminal >}}

Agora vamos criar o própriamente o `avd`. Ponha no terminal:



{{< terminal >}}{{< highlight bash >}}$ avdmanager create avd\
 -n bruno\
 -k "system-images;android-22;default;x86_64"\
 --device "Nexus 5"\
 --sdcard 100M
{{< /highlight >}}{{< /terminal >}}

>Caso queira ver todas as opções device possiveis digite no terminal

{{< terminal >}}{{< highlight bash >}}$ avdmanager list device
{{< /highlight >}}{{< /terminal >}}

#### 2. Skins

No Android Studio existem algumas [skins](https://developer.android.com/studio/run/managing-avds.html#skins) disponíveis.

Como esse tutorial é na raça (tudo na mão), não teriamos esses disponiveis, mas calma, para tudo na vida tem jeito.


{{< terminal >}}{{< highlight bash >}}$ cd  $ANDROID_HOME
$ git clone https://github.com/brunorozendo/android-skins.git skins
{{< /highlight >}}{{< /terminal >}}

Pronto agora essas skins estão de fácil acesso.


#### 3. Excutar o AVD com o EMULATOR



{{< terminal >}}{{< highlight bash >}}sudo $ANDROID_HOME/tools/emulator\
 -avd bruno\
 -skindir "$ANDROID_HOME/skins"\
 -skin "nexus_5"\
 -memory 4096\
 -accel on\
 -gpu on
{{< /highlight >}}{{< /terminal >}}

e _voilá_ temos um adroid rodando.

{{< image src="/images/posts/criar-avd-gnu-linux/avd.png"  >}}

