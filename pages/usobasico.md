# Introdução ao Docker Client

## 1 - Hello World: Docker Funcional

No termial testar a instalação Docker
```console
> docker
```

```console
Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default
                           "C:\\Users\\jnasciso\\.docker")
  -c, --context string     Name of the context to use to connect to the
                           daemon (overrides DOCKER_HOST env var and
                           default context set with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level
                           ("debug"|"info"|"warn"|"error"|"fatal")
                           (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default
                           "C:\\Users\\jnasciso\\.docker\\ca.pem")
      --tlscert string     Path to TLS certificate file (default
                           "C:\\Users\\jnasciso\\.docker\\cert.pem")
      --tlskey string      Path to TLS key file (default
                           "C:\\Users\\jnasciso\\.docker\\key.pem")
      --tlsverify          Use TLS and verify the remote

      ...
```

## Crie container default

```console
> docker container run hello-world

> Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## 2 - Comando Run

A partir da versão 1.13 ouve mudança sensível aos comandos que você usa ao terminal do docker, houve uma reorganização dos comandos, va,os trabalhar com a versão novas.

O comando `run` é a porta de entrada para o *mundo docker* ele agrupa uma série de comandos.

Ele faz:
1. Docker image pull -> baixar uma *image* do *registry* para sua máquina local
2. Docker container create -> é a criação do container.
3. Docker container start -> inicialização do container.
4. Docker container exec -> é a execução do container em modo interativo.

## 3 - Ferramentas Diferentes

Existe dosi modos básicos que pode usar dentro do docker para executar container.

1. Modo DAIMON que é modo que executa em processo em background, podemos dizer qué o modo primário.

2. Modo INTERATIVO serve muito para fazer esperimento, para entrar no container, fazer configuração ou alguns testes.

Alguns comando para verificar a versão do bash na maquina local:

1. 
```console
> bash --version

GNU bash, version 4.4.23(1)-release (x86_64-pc-msys)
Copyright (C) 2016 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

This is free software; you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```

Baixar imagem do **Debian** e despois executar o mesmo comnado local `bash --version`

```console
> docker container run debian bash --version

GNU bash, version 5.0.3(1)-release (x86_64-pc-linux-gnu)
Copyright (C) 2019 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>

This is free software; you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
```
> *Esse comando vai fazer o download da imagem `docker image pull`ele vai fazer a criação do container `docker container create`, ele vai fazer a inicialização do container `docker container start`, e vai executar esse comando em modo interativo vai aparecer a execução do comando `bash --version` direto no console, como visulizadoi na imagem acima, e tambem executa o `docker container exec`*

Comando para verificar quais os container que estão sendo executados no momento:
```console
> docker container ps

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

Comando mostra os container executados independente do status:
```console
> docker container ps -a

CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
6cb4754d1bb9        debian                      "bash --version"         15 minutes ago      Exited (0) 15 minutes ago
                     practical_cannon
295a62e7c14c        hello-world                 "/hello"                 48 minutes ago      Exited (0) 48 minutes ago
                     hardcore_euler
23a09c3e8b67        jenkins/jenkins             "/sbin/tini -- /usr/…"   3 days ago          Exited (255) 9 hours ago    0.0.0.0:8080->8080/tcp, 50000/tcp   dia03_jenkins_1
196c225d0c1e        hello-world                 "/hello"                 6 days ago          Exited (0) 6 days ago
                     suspicious_kalam
0e6472c2c477        hello-world                 "/hello"                 6 days ago          Exited (0) 6 days ago
                     recursing_elbakyan
58cec9f9f670        nginx                       "nginx -g 'daemon of…"   2 months ago        Exited (255) 7 weeks ago    0.0.0.0:8080->80/tcp                ex-daemon-basic
a9347ec2a9e4        nginx                       "nginx -g 'daemon of…"   2 months ago        Exited (0) 2 months ago
                     competent_elbakyan
```

```console
> docker container run --rm debian bash --version
```
*Deois executar o comando ele automaticamente exclui o container, não aparece `ps -a`*

> O comando `--rm` remove um container

## 4- Run cria sempre novo containers
O método `run` sempre cria um novo container toda vez que é chamado.

### Vamos para exemplo pratico:

1. rode o comando 
```console
> docker container run -it debian bash
```
> [!IMPORTANT]
> *Esse comando você entra no terminal do container, onte `i` é o modo interativo e o `t` é chamda para o terminal.*

2. Agora você tem acesso ao terminal do container, crie um arquivo com comando touch
```console
root@35133706f312:/# touch curso-docker.txt
```
3. Agora execute o comando `ls`para verificar se de fato o arquivo foi criado dentro do container
```console
root@35133706f312:/# ls curso-docker.txt
curso-docker.txt
root@35133706f312:/# 
```
4. Agora saia do container com comando `exit`
```console
root@35133706f312:/# exit
```
5. Crie novamente o container e verifique se de fato o arquivo não está no novo container

```console
> docker container run -it debian bash
root@35133706f312:/# ls curso-docker.txt
ls: cannot access 'curso-docker.txt': No such file or directory
root@35133706f312:/# 
```
> [!NOTE]
> De fato o arquivo não está, e comprova que o comando `run`cria sempre novo container.
