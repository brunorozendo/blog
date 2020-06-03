---
title: "Instalando Gradle No Gnu/Linux"
description: "como como instalar o gradle no gnu/linux"
author:
  name: "Bruno Rozendo"
date: 2017-07-28
draft: false
tags:
- "gnu/linux"
- "android"
- "gradle"
- "java"
- "android na raça"
comments: true
imageCredit: "<a href='http://www.freepik.com'>Designed by upklyak / Freepik</a>"
image: "/images/posts/gradle-tool.jpg"
---



Gradle é uma poderosa ferramenta para o gerenciamento de build e dependências em java(e capaz de se extender para outras linguagens)


#### 0. Prerequisitos

Ter o java 8 instalado

#### 1. Dowload do gradle

Vá em https://gradle.org/releases/ e baixe a versão mais recente.

Depois disso escolha uma pasta do no seu computador para descompactar, no meu caso será `/opt/gradle`

{{< terminal >}}{{< highlight bash >}}bruno@ubuntu:~$ ls /opt/gradle/
gradle-4.0.2-all.zip gradle-4.0.2 
bruno@ubuntu:~$
{{< /highlight >}}
{{< /terminal >}}


Agora basta adicionar no `$PATH` so sistema opercional. 

Dois lugares possiveis são `/etc/profile` ou `~/.bashrc`, basta adiconar no final de um desse arquivos



{{< terminal >}}{{< highlight bash >}}bruno@ubuntu:~$ sudo nano /etc/profile

export GRADLE_HOME=/opt/gradle/gradle-4.0.2
export PATH=$PATH:$GRADLE_HOME/bin
 
bruno@ubuntu:~$
{{< /highlight >}}
{{< /terminal >}}

__Basta reiniciar a maquina e pronto.__

Para testar ponha no terminal:

{{< terminal >}}{{< highlight bash >}}bruno@ubuntu:~$ gradle --help

USAGE: gradle [option...] [task...]

-?, -h, --help          Shows this help message.
-a, --no-rebuild        Do not rebuild project dependencies.
-b, --build-file        Specify the build file.
--build-cache           Enables the Gradle build cache. Gradle will try to reuse outputs from previous builds. [incubating]
-c, --settings-file     Specify the settings file.
...
{{< /highlight >}}
{{< /terminal >}}
