---
layout: post
title:  "Como criar um novo usuário e habilitar para o grupo sudo no CentOS 8"
date:   2022-01-14 14:00:00 -0300
categories: Linux Servdirores
tags: Linux Servdirores
---
O comando **sudo** fornece um mecanismo para a concessão de privilégios de administrador (normalmente, apenas disponível para o usuário **root**) para usuários normais. Este guia mostrará como criar um novo usuário com acesso **sudo no CentOS 8**, sem precisar modificar o arquivo **/etc/sudoers** do seu servidor. Se quiser configurar o sudo para um usuário existente do CentOS, pule para passo 3


## Passo 1 — Fazendo login em seu servidor
Acesse seu servidor via SSH como usuário **root**:


``` shell
$ ssh root@your_server_ip_address
```


Use seu endereço IP ou nome do host do seu servidor no lugar de **your_server_ip_address** acima.


## Passo 2 — Adicionando um novo usuário ao sistema

Use o comando adduser para adicionar um novo usuário ao seu sistema:


``` shell
# adduser jonas
```

O **jonas** deve ser substituído pelo nome de usuário que você gostaria de criar.

Use o comando **passwd** para atualizar a nova senha do usuário:



``` shell
# passwd jonas
```

Lembre-se de substituir o **jonas** pelo usuário que você acabou de criar. Você será solicitado a colocar uma nova senha duas vezes:

``` shell
Output

Changing password for user jonas.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.

```

## Passo 3 — Adicionando o usuário ao grupo wheel
Use o comando usermod para adicionar o usuário ao grupo **wheel**:


``` shell
# usermod -aG wheel jonas

```

Novamente, o **jonas** deve ser substituído pelo nome de usuário a que você gostaria de conceder os privilégios sudo. Por padrão, no CentOS, todos os membros do grupo wheel têm acesso sudo completo.

## Passo 4 — Testando o acesso sudo
Para testar que as novas permissões sudo estejam em funcionamento, utilize primeiro o comando **su** para trocar do usuário root para a nova conta de usuário:


```shell
# su - jonas
```

Como o novo usuário, verifique se consegue usar o **sudo**, colocando sudo antes do comando que deseja executar com privilégios de superusuário:

```shell
$ sudo command_to_run
``` 

Por exemplo, você pode listar o conteúdo do diretório **/root**, que é normalmente acessível apenas para o usuário root:

``` shell
$ sudo ls -la /root
``` 

 
A primeira vez que usar o **sudo** em uma sessão, será solicitada a senha da conta do usuário. Digite a senha para continuar:

``` shell
Output
$ [sudo] password for jonas:
``` 

<br>

> Nota: isso não está pedindo pela senha root! Digite a senha do usuário habilitado para sudo, não a senha root.

<br>

Se o usuário estiver no grupo correto e você digitar a senha corretamente, o comando que emitiu com sudo será executado com privilégios **root**.

## Conclusão
Neste tutorial de início rápido, criamos uma nova conta de usuário e a adicionamos ao grupo wheel para habilitar o acesso sudo. Para obter informações mais detalhadas sobre como configurar um servidor CentOS 8, leia nosso tutorial Configuração inicial de servidor com o CentOS 8.

[Link original](https://www.digitalocean.com/community/tutorials/how-to-create-a-new-sudo-enabled-user-on-centos-8-quickstart-pt)