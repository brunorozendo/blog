---
title: "Como Instalar o Android SDK no Gnu/Linux"
description: "como instalar android tools"
author:
  name: "Bruno Rozendo"
date: 2022-02-15
draft: false
tags:
- "android"
- "gnu/linux"
- "android sdk"
- "android na raça"
comments: true
imageCredit: "<a href='http://www.freepik.com'>Designed by pikisuperstar / Freepik</a>"
image: "/images/posts/android-sdk.svg"
---


{{< table "table table-striped" >}}
| rev.  | data |
|---------|--------|
| 1 revisão      | 24/07/2017    |
| 2 revisão      | 18/07/2020    |
| 3 revisão      | 03/02/2021    |
| 4 revisão      | 15/02/2022    |
{{</ table >}}


### Prequisitos

 1. Ter o java 17 instalado
 2. Intalar dependências\
    2.1. __Ubuntu 64__:
    {{< terminal >}}{{< highlight bash >}}$ sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1 libbz2-1.0:i386{{< /highlight >}}{{< /terminal >}}
    2.2. __Fedora 64__:
    {{< terminal >}}{{< highlight bash >}}$ sudo dnf install zlib.i686 ncurses-libs.i686 bzip2-libs.i686{{< /highlight >}}{{< /terminal >}}


### 1. Download do `command-tools`


Vá em [Android Studio > Downloads > Command line tools only > commandlinetools-linux-XXXXXXXXXXX.zip](https://developer.android.com/studio#command-tools)

Espere a página carregar, clique em "I have read and agree with the above terms and conditions" (aceitar) e  faça o download.


### 2. Estrutura

Depois de baixado criar a seguinte estrutura de pastas (você pode fazer a sua.)


{{< terminal >}}{{< highlight bash >}}$ mkdir -p /opt/android/android-sdk-linux
{{< /highlight >}}
{{< /terminal >}}


 - __android__: aqui ficaram todas as coisas relacionadas ao android (mas no momento só tem uma pasta: android-sdk-linux)
 - __android-sdk-linux__: aqui irá ficaram todos os recurso **

** Você não irá mecher nessa pasta diretamente, todo o conteúdo dela será criado/excluido/alterado pelas ferramentas do próximo passo.



Dentro da pasta `android-sdk-linux` descompacte o conteudo do zip baixado (`commandlinetools-linux-XXXXXXXXXXX.zip`), o resultado final será, você terá de renomar a pasta:

{{< terminal >}}{{< highlight bash >}}$ cd /opt/android/android-sdk-linux/cmdline-tools/lasted
$ ls
bin  lib  NOTICE.txt  source.properties
{{< /highlight >}}
{{< /terminal >}}


### 3. Variáveis de ambiente


Agora vamos adicionar as variaváveis de ambiente, se você reparar algumas pastas ainda não existem, mas calma elas irão ser criadas.

editar o `/etc/profile` ou o ` ~/.profile` e adicionar no final do aquivo

{{< terminal >}}{{< highlight bash >}}
export ANDROID_HOME=/opt/android/android-sdk-linux
export ANDROID_SDK_ROOT=$ANDROID_HOME
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/cmdline-tools/lasted/bin
export PATH=$PATH:$ANDROID_HOME/emulator
{{< /highlight >}}
{{< /terminal >}}


__Depois disso reiniciar o computador__.



No terminal digite `sdkmanager`


{{< terminal >}}{{< highlight bash >}}:~$ sdkmanager
[=======================================] 100% Computing updates...
{{< /highlight >}}
{{< /terminal >}}



Se a mensagem acima apareceu então fique feliz pois tudo está funcionando.


### 4. Final: checando o instalador

Agora vamos instalar algum pacote qualquer para testar se tudo está funcionando.

Responda com `y` no final e espere(vai demorar um pouco, as dependências serão baixadas), No terminal digite:

{{< terminal >}}{{< highlight bash >}}$ sdkmanager emulator
License android-sdk-license:            ] 10% Computing updates...              
---------------------------------------
Terms and Conditions

This is the Android Software Development Kit License Agreement

1. Introduction

1.1 The Android Software Development Kit (referred to in the License Agreement as the "SDK" and specifically including the Android system files, packaged APIs, and Google APIs add-ons) is licensed to you subject to the terms of the License Agreement. The License Agreement forms a legally binding contract between you and Google in relation to your use of the SDK.

...

14.6 The rights granted in the License Agreement may not be assigned or transferred by either you or Google without the prior written approval of the other party. Neither you nor Google shall be permitted to delegate their responsibilities or obligations under the License Agreement without the prior written approval of the other party.

14.7 The License Agreement, and your relationship with Google under the License Agreement, shall be governed by the laws of the State of California without regard to its conflict of laws provisions. You and Google agree to submit to the exclusive jurisdiction of the courts located within the county of Santa Clara, California to resolve any legal matter arising from the License Agreement. Notwithstanding this, you agree that Google shall still be allowed to apply for injunctive remedies (or an equivalent type of urgent legal relief) in any jurisdiction.


January 16, 2019
---------------------------------------
Accept? (y/N):


{{< /highlight >}}
{{< /terminal >}}


Se você for inspecionar a pasta `/opt/android/android-sdk-linux` tem mais conteúdo agora.

Pronto! Agora sim, está tudo instalado, só aproveitar. 

[Como sugestão que tal criar um emulator agora!](https://brunorozendo.com/post/criar-avd-gnu-linux.html)
