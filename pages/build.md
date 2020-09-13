# Primeiro **Build**

* Vamos criar um Descritor, para isso vamos acessar um editor de sua preferência, *exemplo:* *VS Code*, *Note Pad++* *etc..*

* Crie diretorio onde vai guardar o arquivo e crie o arquivo `Dockerfile`, *exemplo:* `/primeiro-build/Dockerfile`.

> ⚠️ O arquivo tem ter esse nome `Dockerfile` começa com 'D' maiusculo o 'f' minusculo e não tem instenção.


* FROM &rarr; é basicamento onde você quer basear sua image a partir de outra image.

* RUN &rarr; executa comandos.
* \> &rarr; direciona para um diretorio do arquivo

```text
# conteudo arquivo Dockerfile
FROM nginx:latest
RUN echo '<h1> Olá Mundo</h1>' > /usr/share/nginx/html/index.html
```

* Dentro do diretorio onde criou o arquivo descritor abra o terminal.

```bash
$ primeiro-build> docker image build -t ex-simple-build .
```
* `-t`  &rarr; comando para dar o nome (tag) a imagem.

* **`.`**  &rarr; indica o arquivo esta no mesmo diretorio do terminal.

* Execute o comando: 
```bash
$ primeiro-build> docker image build -t ex-simple-build .
Sending build context to Docker daemon  2.048kB
Step 1/2 : FROM nginx:latest
latest: Pulling from library/nginx
d121f8d1c412: Pull complete
ebd81fc8c071: Pull complete
655316c160af: Pull complete
d15953c0e0f8: Pull complete
2ee525c5c3cc: Pull complete
Digest: sha256:9a1f8ed9e2273e8b3bbcd2e200024adac624c2e5c9b1d420988809f5c0c41a5e
Status: Downloaded newer image for nginx:latest
 ---> 7e4d58f0e5f3
Step 2/2 : RUN echo '<h1> Olá Mundo</h1>' > /usr/share/nginx/html/index.html
 ---> Running in 73e405ba671d
Removing intermediate container 73e405ba671d
 ---> 2218581f0fa9
Successfully built 2218581f0fa9
Successfully tagged ex-simple-build:latest
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.

$ primeiro-build>
```

* Verificar se realmente realmente a imagem foi criada:

```bash
$ primeiro-build> docker image ls
ex-simple-build     latest              2218581f0fa9        4 minutes ago       133MB
nginx               latest              7e4d58f0e5f3        3 days ago          133MB

```

* Agora executar o container a partir dessa imagem:

```bash
$ primeiro-build> docker container run -p 80:80  ex-simple-build

/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
```

> O comando `-p 80:80` por default o servidor do nginx funcina na porta 80, então esse comando transporta tudo esta rodando no container nessa porta para ser utilizadas na porta 80 externo.

* Agora é só verificar no browser

![ima09](img/img09.PNG)



