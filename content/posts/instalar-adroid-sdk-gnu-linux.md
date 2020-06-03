---
title: "Como Instalar o Android SDK no Gnu/Linux"
description: "como instalar android tools"
author:
  name: "Bruno Rozendo"
date: 2017-07-24
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




### 0. Prequisitos

 1. Ter o java 8 instalado


 2. Se estiver no __Ubuntu 64__
	```sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386 lib32z1 libbz2-1.0:i386```

 2. Se estiver no __Fedora 64__
	```sudo dnf install zlib.i686 ncurses-libs.i686 bzip2-libs.i686```	




### 1. Download do `tools`


Vá em [Android Studio > Downloads > sdk-tools-linux-XXXXXXX.zip](https://developer.android.com/studio/index.html#linux-tools)

Espere a página carregar , clique em "I have read and agree with the above terms and conditions" (aceitar) e  faça o download.


### 2. Estrutura 

Depois de baixado criar a seguinte estrutura de pastas (você pode fazer a sua.)


{{< terminal >}}{{< highlight bash >}}$ mkdir -p /opt/android/android-sdk-linux
{{< /highlight >}}
{{< /terminal >}}


 - __android__: aqui ficaram todas as cosisa relacionadas ao android (mas no momentos só tem uma pasta: android-sdk-linux)
 - __android-sdk-linux__: aqui irá ficar todos os recurso <span class="red">__***__</span>


<span class="red">__***__</span> Você não irá mecher nessa pasta diretamente, todo o conteúdo dela será criado/excluido/alterado pelas ferramentas do próximo passo.



Dentro da pasta `android-sdk-linux` descompacte o conteudo do zip baixado (`sdk-tools-linux-XXXXXXX.zip`), o resultado final será:

{{< terminal >}}{{< highlight bash >}}$ cd /opt/android/android-sdk-linux/tools
$ ls
android  bin  emulator  emulator-check  lib  mksdcard  monitor  NOTICE.txt  package.xml  proguard  source.properties  support

{{< /highlight >}}
{{< /terminal >}}


### 3. Variáveis de ambiente


Agora vamos adicionar as variaváveis de ambiente, se você reparar algumas pastas ainda não existem, mas calma elas irão ser criadas.

editar o `/etc/profile` ou o ` ~/.bashrc` e adicionar no final do aquivo

{{< terminal >}}{{< highlight bash >}}export ANDROID_HOME=/opt/android/android-sdk-linux
export ANDROID_SDK_ROOT=/opt/android/android-sdk-linux
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/tools/bin

{{< /highlight >}}
{{< /terminal >}}


__Depois disso reiniciar o computador__.



No terminal digite `android`


{{< terminal >}}{{< highlight bash >}}:~$ android
*************************************************************************
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools/bin/sdkmanager and tools/bin/avdmanager
*************************************************************************
Invalid or unsupported command ""

Supported commands are:
android list target
android list avd
android list device
android create avd
android move avd
android delete avd
android list sdk
android update sdk
{{< /highlight >}}
{{< /terminal >}}



Se a mensagem acima apareceu então fique feliz pois tudo está funcionando.


Agora vamos a ao passo final

### 4. Final: instalando as dependências 


No terminal digite 

{{< terminal >}}{{< highlight bash >}}sdkmanager "tools"
{{< /highlight >}}
{{< /terminal >}}


responda com `y` no final e espere(porque aqui vai demorar umpouco, ele vai baixar as dependências):

{{< terminal >}}{{< highlight bash >}}$ sdkmanager "tools"
Warning: File /home/bruno/.android/repositories.cfg could not be loaded.
License android-sdk-license:
---------------------------------------
Terms and Conditions

This is the Android Software Development Kit License Agreement

1. Introduction

1.1 The Android Software Development Kit (referred to in the License Agreement as the "SDK" and specifically including the Android system files, packaged APIs, and Google APIs add-ons) is licensed to you subject to the terms of the License Agreement. The License Agreement forms a legally binding contract between you and Google in relation to your use of the SDK.

...

14.7 The License Agreement, and your relationship with Google under the License Agreement, shall be governed by the laws of the State of California without regard to its conflict of laws provisions. You and Google agree to submit to the exclusive jurisdiction of the courts located within the county of Santa Clara, California to resolve any legal matter arising from the License Agreement. Notwithstanding this, you agree that Google shall still be allowed to apply for injunctive remedies (or an equivalent type of urgent legal relief) in any jurisdiction.


November 20, 2015
---------------------------------------
Accept? (y/N): y  
{{< /highlight >}}
{{< /terminal >}}


Se você for reparar a pasta `/opt/android/android-sdk-linux` tem mais contúdo agora.

Pronto!!!! agora sim está tudo instalado. agora é só aproveitar.
