---
title: "Criar AVD (Simulador Android) por linha de comando no Gnu/Linux"
description: "criação de uma maquina virtual android (AVD/Simulador) no linux"
author:
  name: "Bruno Rozendo"
date: 2022-02-16
draft: false
tags:
- "android"
- "gnu/linux"
- "avd"
- "android na raça"
comments: true
imageCredit: "<a href='http://www.freepik.com'>Designed by ijeab / Freepik</a>"
image: "/images/posts/criar-avd-gnu-linux/banner.jpg"
---

{{< table "table table-striped" >}}
| rev.  | data |
|---------|--------|
| 1 revisão      | 26/07/2017    |
| 2 revisão      | 16/02/2022    |
{{</ table >}}


#### Prequisitos

Ter o [android sdk instalado](/post/instalar-adroid-sdk-gnu-linux.html)


#### 1. Criar avd


Uma vez que que já tenha instalado o sdk tudo fica bem mais fácil.

Supondo que queremos rodar uma versão do `Android 5.0 (Lollipop)` abra o terminal e vamos instalar os seguites pacotes:

 - `platforms;android-22`
 - `system-images;android-22;default;x86_64`
 - `emulator`
 - `platform-tools`

No terminal

{{< terminal >}}{{< highlight bash >}}$ sdkmanager "platforms;android-22"
[=======================================] 100% Computing updates...
$ sdkmanager "system-images;android-22;default;x86_64"
[=======================================] 100% Computing updates...
$ sdkmanager "emulator"
[=======================================] 100% Unzipping... emulator/qemu/linux-
$ sdkmanager "platform-tools"
[=======================================] 100% Unzipping... platform-tools/systr
{{< /highlight >}}{{< /terminal >}}


** Caso queira ver todas as opções possiveis digite no terminal


{{< terminal >}}{{< highlight bash >}}$ sdkmanager --list --verbose
{{< /highlight >}}{{< /terminal >}}

Agora vamos criar o própriamente o `avd`. Ponha no terminal:



{{< terminal >}}{{< highlight bash >}}$ avdmanager create avd\
 -n bruno\
 -k "system-images;android-22;default;x86_64"\
 --device "Nexus 5"\
 --sdcard 100M
{{< /highlight >}}{{< /terminal >}}

** Caso queira ver todas as opções device possiveis digite no terminal

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



{{< terminal >}}{{< highlight bash >}} emulator\
 -avd bruno\
 -skindir "$ANDROID_HOME/skins"\
 -skin "nexus_5"\
 -memory 4096\
 -accel on\
 -gpu on
{{< /highlight >}}{{< /terminal >}}

e _voilá_ temos um adroid rodando.

{{< image src="/images/posts/criar-avd-gnu-linux/avd.png"  >}}

Clique no **X** para fechar o simulador. 

Agora vamos deixa esse emulador usavel!

 - Unable to connect to adb daemon on port: 5037

   Ao executar o comando acima o log deve ter sido algo parecido

   {{< terminal >}}{{< highlight bash >}} $ emulator\
   -avd bruno\
   -skindir "$ANDROID_HOME/skins"\
   -skin "nexus_5"\
   -memory 4096\
   -accel on\
   -gpu on
   INFO    | Android emulator version 31.2.8.0 (build_id 8143646) (CL:N/A)
   WARNING | encryption is off
   Fontconfig warning: "/usr/share/fontconfig/conf.avail/05-reset-dirs-sample.conf", line 6: unknown element "reset-dirs"
   INFO    | configAndStartRenderer: setting vsync to 60 hz
   WARNING | cannot add library /opt/android/sdk-android-linux/emulator/qemu/linux-x86_64/lib64/vulkan/libvulkan.so: failed
   INFO    | added library /opt/android/sdk-android-linux/emulator/lib64/vulkan/libvulkan.so
   host doesn't support requested feature: CPUID.80000001H:ECX.abm [bit 5]
   host doesn't support requested feature: CPUID.80000001H:ECX.abm [bit 5]
   host doesn't support requested feature: CPUID.80000001H:ECX.abm [bit 5]
   host doesn't support requested feature: CPUID.80000001H:ECX.abm [bit 5]
   pc_memory_init: above 4g size: 40000000
   INFO    | Started GRPC server at 127.0.0.1:8554, security: Local
   INFO    | Advertising in: /run/user/1000/avd/running/pid_6979.ini
   ERROR   | Unable to connect to adb daemon on port: 5037
   INFO    | Your emulator is out of date, please update by launching Android Studio:
   - Start Android Studio
     - Select menu "Tools > Android > SDK Manager"
     - Click "SDK Tools" tab
     - Check "Android Emulator" checkbox
     - Click "OK"

   INFO    | Shutting down gRPC endpoint

   {{< /highlight >}}{{< /terminal >}}

   O que nos importa no momento é a seguinte linha 

   ```ERROR   | Unable to connect to adb daemon on port: 5037```

   então para resolver isso

   {{< terminal >}}{{< highlight bash >}} $ adb start-server {{< /highlight >}}{{< /terminal >}}

   Ao executar novamente o simulador essa mensagem deve ter desaparecido:

   {{< terminal >}}{{< highlight bash >}} $ emulator -avd bruno -skindir "$ANDROID_HOME/skins" -skin "nexus_5" -memory 4096 -accel on -gpu on
   INFO    | Android emulator version 31.2.8.0 (build_id 8143646) (CL:N/A)
   WARNING | encryption is off
   Fontconfig warning: "/usr/share/fontconfig/conf.avail/05-reset-dirs-sample.conf", line 6: unknown element "reset-dirs"
   INFO    | configAndStartRenderer: setting vsync to 60 hz
   WARNING | cannot add library /opt/android/sdk-android-linux/emulator/qemu/linux-x86_64/lib64/vulkan/libvulkan.so: failed
   INFO    | added library /opt/android/sdk-android-linux/emulator/lib64/vulkan/libvulkan.so
   host doesn't support requested feature: CPUID.80000001H:ECX.abm [bit 5]
   host doesn't support requested feature: CPUID.80000001H:ECX.abm [bit 5]
   host doesn't support requested feature: CPUID.80000001H:ECX.abm [bit 5]
   host doesn't support requested feature: CPUID.80000001H:ECX.abm [bit 5]
   pc_memory_init: above 4g size: 40000000
   INFO    | Started GRPC server at 127.0.0.1:8554, security: Local
   INFO    | Advertising in: /run/user/1000/avd/running/pid_7658.ini
   INFO    | Shutting down gRPC endpoint
   {{< /highlight >}}{{< /terminal >}}


 - Caso tenho feito alguns teste você deve ter percebido que o teclado não está fucionando vamos resolver isso.

   Feche novamente o simulador.
   
   Dentro da pasta ```~/.android/avd``` deve existir o emulador ( caso esteja usando a variável ```ANDROID_AVD_HOME``` então procure dentro dela) e vamos editar o arquivio
   
   ```bruno.avd/config.ini```
   
   Procure pela linha ```hw.keyboard=no``` e substitua por ```hw.keyboard=yes```

   ** [Ctrl+C/Ctrl+V não é possivel com o teclado, mas há outras maneiras](https://stackoverflow.com/questions/3391160/paste-text-on-android-emulator).

 - É possivel já deixar a `skin` configurada, para isso, busque pela linha `skin.path=_no_skin` e substitua, no caso ficou assim

   ```skin.path=/opt/android/sdk-android-linux/skins/nexus_5```

 - GPU: igual a skin, bsque pela linha `hw.gpu.enabled=no` e substitua por ```hw.gpu.enabled=true```
 

Agora o novo comando para iniciar o simulador

{{< terminal >}}{{< highlight bash >}} $emulator -avd bruno  -memory 4096 -accel on{{< /highlight >}}{{< /terminal >}}

{{< image src="/images/posts/criar-avd-gnu-linux/final.png"  >}}