# O que é DOCKER
    Docker não é um sistema de virtualização tradicional baseado en máquinas virtuais.

    Docker é engine de administração de containers, é um serviço que vai administrar os contener a ser criado por ele.

Docker se baseia em uma tecnologia chamda **LXC** *(Linux Container)* . É tecnologia Open Source, desenvolvida em **GO**, utiliza uma virtualização baseada em *software de sietma operacional* (SO). O *Host* e *Container* compartilham o **Kernel**.

# Diferença para uma VM

As VM revolucionaram o mercado de tecnologia, uma grande quantidade de software desenvolvidos por conta das VM's a computação de nuvens por baixo usa as VM, pois tem a facilidade de dimencionar máquinas de acordo com seu uso, então por que não usar as VM pois elas já tem um padrão excelente aceito no mercado. A questão da VM's agente conseque ter a maior parte dos beneficios que leas tem, o fato de ter um isolamento do sistema, o fato de ter de elasticidade de aumentar ou diminuir recursos para uma determinada VM, a capacidade de dimensionar as diversas vantagem presente nas VM's nos encontramos nos containers. Container levam vantagem e a quantidade de recursos consumidos ao ser executado, ou seja, é possivel startar um container em menus de um segfundo, muito mais rápido que as VM, o consumo de memória tambem é muito menor que um SO de uma VM. Resumindo em um container você tem todos recursos de um VM, mas com consumo de recurso muito menor.

![imagem01](img/img01.png)

# Containers
Container é o nome dado para a segregação de processos no mesmo kernel, de forma que o processo
seja isolado o máximo possível de todo o resto do ambiente.

Em termos práticos são File Systems, criados a partir de uma "imagem" e que podem possuir
também algumas características próprias.

![imagem02](img/img02.png)

# Imagens Docker
Uma imagem Docker é a materialização de um modelo de um sistema de arquivos, modelo este
produzido através de um processo chamado build.

Esta imagem é representada por um ou mais arquivos e pode ser armazenada em um repositório.

<center><strong>Docker</strong> <em>File Systems</em></center>

O Docker utiliza file systems especiais para otimizar o uso, transferência e armazenamento
das imagens, containers e volumes.

O principal é o AUFS, que armazena os dados em camadas sobrepostas, e somente a camada
mais recente é gravável.

 * [https://pt.wikipedia.org/wiki/Aufs](https://pt.wikipedia.org/wiki/Aufs)

 * [https://docs.docker.com/engine/userguide/storagedriver/aufs-driver/](https://docs.docker.com/engine/userguide/storagedriver/aufs-driver/)

![imagem03](img/img03.png)

# Imagem vs Container

Para explicar melhort a difeneça entre imagem e container temos ter em mente o paradigma orientado a objetos, este tem o conceito chamado ***Classe*** que a parti dela você pode criar multi ***Objetos*** usando a classe como modelo. Na mesma forma nos temos a **imagem Docker** sendo usada como modelo para criar os **Container**. Então a **Imagem** é o modelo e os **Container** é a realização dessa imagem.

    O **Container** é o processo esta na memória do computador sendo executado e a **Imagem** é justamente o modelo que sistema de arquivo de somente leitura, que é usado no momento do **Container** vai iniciar que ele monta o sistema de arquivo que é baseado em *camadas* para que ele possa startar  deo processo de aplicações, blibliotecas etc..

# Arquitetura Docker

![imagem04](img/img04.png)

*Na imagem você pode reparar que o Docker esta dividido em duas partes*

Tem o **Docker Daemon** que também é chamado de **Docker Server** ou **Docker Engine**

Tem as parte do **Client** tem parte `CLI>_` é a forma padrão que é usado para acessar os serviços gerenciado pelo **Daemon**

Por esxemplo você passa para **Daemon** que quer criar uma *imagem* e o *Daemon* vai no *Registry* que pode ser no **GITHUB* ou **Imagem no sistema**. No **DockerHub** ele já tem essas imagens prontas, ele faz o download dessas imagens para seu computador local (Host) e apartir dessas imagens locais é que o container é estanciados, são criados para serem executados na máquina local (Host).

O **Docker** possui uma ferramenta grafica para quem não quiser trabalhar com linhas de comando é **KITEMATIC**