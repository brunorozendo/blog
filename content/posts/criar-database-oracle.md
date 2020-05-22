---
title: "Criar Database Oracle 11g"
description: "criação de database no oravle 11g xe"
author:
  name: "Bruno Rozendo"
date: 2016-04-18
draft: false
tags:
- "oracle"
- "gnu/linux"
- "database"
comments: true
imageCredit: "<a href='http://www.freepik.com'>Designed by fullvector / Freepik</a>"
image: "/images/posts/database-oracle.jpg"
---


Por curiosidade instalei um banco oracle numa maquina ubuntu e para minha surpresa criar uma database e bem mais complexo.

Exemplo válidos em mysql e postgre, mas que não funciona no oracle.

{{< highlight sql >}}
create database teste;
{{< /highlight >}}

Eis aqui a mágica, precisamos criar um  `TABLESPACE`, `DATAFILE`, `USER` e dar as permissões (`GRANT`).

O produto final:

{{< terminal >}}{{< highlight bash >}}bruno@ubuntu:~$: sqlplus / as sysdba

SQL*Plus: Release 11.2.0.2.0 Production on Qui Jul 27 11:39:36 2017

Copyright (c) 1982, 2011, Oracle.  All rights reserved.

SQL> connect / as sysdba
Conectado a uma instância inativa.
SQL> CREATE TABLESPACE <database> DATAFILE '/u01/app/oracle/oradata/XE/<database>.dbf' SIZE 100m AUTOEXTEND ON NEXT 100m EXTENT MANAGEMENT LOCAL AUTOALLOCATE;
Tablespace criado.
SQL> CREATE USER <database> IDENTIFIED BY <database> DEFAULT TABLESPACE <database> TEMPORARY TABLESPACE temp;
Usuário criado.
SQL> grant all privileges to <database> identified by <database>;
Concessão bem-sucedida.
SQL> 
{{< /highlight  >}}
{{< /terminal >}}

Feito! agora basta conecatar com sua ferramenta de preferência.

*Obs: A senha é igual ao nome do usuário, fiz isso por simples comodidade.

