# Introdução ao 🐳Docker Client

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
1. Docker image pull &rarr; baixar uma *image* do *registry* para sua máquina local
2. Docker container create &rarr; é a criação do container.
3. Docker container start &rarr; inicialização do container.
4. Docker container exec &rarr; é a execução do container em modo interativo.

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

## 4- Run cria sempre novos containers
O método `run` sempre cria um novo container toda vez que é chamado.

### Vamos para exemplo pratico:

1. rode o comando 
```console
> docker container run -it debian bash
```
> *Esse comando você entra no terminal do container, onde o* `i` *é o modo interativo e o* `t` *é chamda para o terminal.*

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
> De fato o arquivo não está, e comprova que o comando `run`sempre cria novo container.

## 5 - Nomeando containers
Vamos começar agora aprender como nomer container, criar estratégia de reusar o mesmo container mais de uma vez.
```console
> docker container run --name mydeb -it debian bash
```
>*O nome dado ao container é* `mydeb` 

Tente criar novamente o container, repita o mesmo comando anterior
```console
> docker container run --name mydeb -it debian bash
docker: Error response from daemon: Conflict. The container name "/mydeb" is already in use by container "7e18875847aa2dcc2b4b1b9e758575ce4131a54ff7b5dd239019f41b8d359785". You have to remove (or rename) that container to be able to reuse that name.
See 'docker run --help'
```
>Veja que gera um erro e reforçando que devemos nomear os container com  nomes unicos.

## 6 - Reutilizar containers
Primeiro list todos os container
```console
> docker container ls -a
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS                      PORTS           NAMES
7e18875847aa        debian                      "bash"                   2 months ago        Exited (0) 2 months ago                   mydeb
81867d9cce2f        debian                      "bash"                   2 months ago        Exited (2) 2 months ago                   reverent_spence
5f4078c29cbd        debian                      "bash"                   2 months ago        Exited (0) 2 months ago                   reverent_wozniak
0feaf4a52590        debian                      "bash --version"         2 months ago        Exited (0) 2 months ago                   distracted_wilbur
f4efc372d677        debian                      "bash --version"         2 months ago        Exited (0) 2 months ago                   competent_blackburn
179e8f966fe3        hello-world                 "/hello"                 2 months ago        Exited (0) 2 months ago                   stupefied_newton
164bb861f22e        kafka-docker-master_kafka   "start-kafka.sh"         2 months ago        Exited (143) 2 months ago                 -docker-master_kafka_1
2e47368c1c15        wurstmeister/zookeeper      "/bin/sh -c '/usr/sb…"   2 months ago        Exited (137) 2 months ago                -docker-master_zookeeper_1
```

Agora para inicializar um container uso o comando
a &rarr; attachment (anexo)
i &rarr; modo interativo
```console
> docker container start -ai mydeb
root@7e18875847aa:/#
```

Agora repita os passos anterios para criar o arquivo, feche e entre novamente no container reutilizando.
```console
root@7e18875847aa:/# ls curso-docker.txt
curso-docker.txt
root@7e18875847aa:/#
```
Passos importante para nomear container que serão reutilizados:
1. Dê nome relevantes, que faça sentido ao que o com a função do container.
2. Use o comando `start` e nome para inicializar o container.

## 7 - Mapear porta de container
Vamos instalar um container com servidor nginx e mapea-lo para a porta 8080 para podemos acessa-lo externamente apartir dessa porta.
1. digite o comando
> -p &rarr; porta
>
> 8080 &rarr; é a porta onde você vai acessar na sua máquina host ou outro meio externo.
>
> 80 &rarr; é a porta padrão do nginx

```console
> docker container run -p 8080:80 nginx
```
2. Verifique no browser se esta funcionando o servidor na porta 8080
![imagem06](https://github.com/jairosousa/Curso-de-Docker/blob/master/pages/img/img06.PNG)

## 8  - Mapear diretório para o container
Vamos nos mapaer volumes, o mapeamento mais simples é o volume de uma pasta do computador host para uma pasta no container, nesse exemplo vamos continuar usando a imagem do nginx, vamos ter criar uma extrutura de arquivo no seu desktop.
```console
desktop\ mkdir curso-docker
desktop\ cd curso-docker
desktop\curso-docker\ mkdir ex-volume
desktop\ curso-docker\cd ex-volume
desktop\ curso-docker\ex-volume\
```

✏️ Abra o editor de texto de sua preferência e crie arquivo html.

```console
desktop\curso-docker\ex-volume\ docker container run -p 8080:80 -v ${pwd}/html:/usr/share/nginx/html nginx
```
* **-v** &rarr; indica vamos mapear um volume para o container.
* **${pwd}** &rarr; ele pega o diretorio atual.
* **/html** &rarr; pasta onde esta o arquivo html.
* **:** &rarr; começa mapeamento da pasta do container.
* **/src/share/nginx/html** &rarr; este é o diretorio padrão do servidor no nginx.

:+1: **OBSERVAÇÃO**

No windowas use endereço completo do diretorio o comando ${pwd} não funciona.

desktop\curso-docker\ex-volume\ docker container run -p 8080:80 -v c:/curso-docker/ex-volume/html:/usr/share/nginx/html nginx

Verifique se no brawser esta carregando na porta 8080 😉

![imagem07](https://github.com/jairosousa/Curso-de-Docker/blob/master/pages/img/img07.PNG)

## 9 - Rodar o servidor web em background

Vamos rodar o servidor em modo backgound ou seja usar no modo DAEMON.

🚩 Este é modo principal do Docker, é o mais utilizado.

```console
desktop\curso-docker\ex-volume\ docker container run -d --name ex-daemon-basic -p 8080:80 -v ${pwd}/html:/usr/share/nginx/html nginx
da771ece1efa4231bab66c39952ce4602265d32cbb28fcd1e290ec7f0c44b615
desktop\curso-docker\ex-volume\
```

* **-d &rarr;** indica em rodar no modo DAEMON

⚠️ repare que ele passa o ID do container e libera o consol, mas o container esta em funcionamento.

🧐 Para verifica basta usar o comando
```console
desktop\curso-docker\ex-volume\ docker container ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
da771ece1efa        nginx               "nginx -g 'daemon of…"   5 minutes ago       Up 5 minutes        0.0.0.0:8080->80/tcp   ex-daemon-basic
```

✋ Para parar o container
```console
desktop\curso-docker\ex-volume\ docker container stop ex-daemon-basic
ex-daemon-basic
desktop\curso-docker\ex-volume\
```
✏️ Voçê pode passar o nome do container ou o ID.

## 10 - Gerênciar o container em background

Existe 3 comando básico, que ja foram falados anteriormente, que você pode usar para gerenciar seus contauners.

### ▶️ **Start**
```console
> docker container start ex-daemon-basic
ex-daemon-basic

> docker container ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
da771ece1efa        nginx               "nginx -g 'daemon of…"   43 minutes ago      Up 3 minutes        0.0.0.0:8080->80/tcp   ex-daemon-basic
```
👁️ em seguida você verifica se de fato o container esta funcionando.

### 🔁 **Restart**

```console
docker container restart ex-daemon-basic
ex-daemon-basic
```

### ⏹️ **Stop**

```console
docker container stop ex-daemon-basic
ex-daemon-basic

> docker container ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

```

## 11 - Manipulação de containers em modo Daemon

### **ls &rarr;** Lista dos containers ativos.
```console
> docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

### **list &rarr;** Lista dos containers ativo.
```console
> docker container list
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

### **ps &rarr;** Lista dos containers ativo.
```console
> docker container ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

### **ls -a &rarr;** Lista dos containers ja criados e em todos status possíveis.
```console
> docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

da771ece1efa        nginx                       "nginx -g 'daemon of…"   58 minutes ago      Exited (0) 9 minutes ago                                        ex-daemon-basic
0b8ee8d66896        nginx                       "nginx -g 'daemon of…"   2 hours ago         Exited (0) 58 minutes ago                                       flamboyant_brattain
541189717ad3        nginx                       "nginx -g 'daemon of…"   2 hours ago         Created                                                         quizzical_hopper
9664f32c8ef4        nginx                       "nginx -g 'daemon of…"   2 hours ago         Created                                                         zen_shamir b2007a1d0113        nginx                       "nginx -g 'daemon of…"   2 hours ago         Created        busy_black
```

⚠️ O mesmo comando **`-a`** serve para os demais comandos de lista **`list` e `ps`**

⚠️ Os comando **`ls`, `list` e `ps`** são **Alias** (pseudônimo), apelidos que o docker dá para mesma funcionalidades.

### **logs &rarr;** Virifica os ***`Logs`*** do sistema do container.

```console
> docker container logs ex-daemon-basic
172.17.0.1 - - [01/Jan/2020:20:49:25 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36" "-"
172.17.0.1 - - [01/Jan/2020:21:29:16 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36" "-"
172.17.0.1 - - [01/Jan/2020:21:56:19 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36" "-"
> 
```

### **inspect &rarr;** Inspeciona e mostra em formato ***`json`***  varias caracteristicas do container, que tipo de imagem que ele se baseia, o diretorio de log, status etc..

```console
>  docker container inspect ex-daemon-basic
[
    {
        "Id": "da771ece1efa4231bab66c39952ce4602265d32cbb28fcd1e290ec7f0c44b615",
        "Created": "2020-01-01T20:49:14.0742488Z",
        "Path": "nginx",
        "Args": [
            "-g",
            "daemon off;"
        ],

        ---
]

```

### **exec &rarr;** Executa um comando dentro do container

```console
> docker container exec ex-daemon-basic uname -or
4.9.184-linuxkit GNU/Linux
```

✏️ Este comando mostra que tipo de sistema esta rodando no container.

## Nova sintaxe do Docker Client

### Listar
```console
> docker container ls
> docker image ls
> docker volume ls
```

### Excluir
```console
> docker container rm <ID ou nome container>
> docker image rm <ID ou nome imagem>
> docker volume rm <ID ou nome volume>
```