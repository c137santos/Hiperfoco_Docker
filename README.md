# Repositório para Descomplicar o Docker

![alt text](./imgs/santosindocker.jpeg)

Livro base é o [descomplicandoDocker](https://livro.descomplicandodocker.com.br/)

Conteiner não é Docker. Porém o Docker foi aquele populou essa forma de isolamento de processos. Por exemplo, o avó do Docker é o Shroot

## O que é conteiner?

[Ref de capítulo](https://livro.descomplicandodocker.com.br/chapters/chapter_01.html) 

Diferente de uma VM, não existe uma grande camada de abstração onde se isola todo uma parte do processamento do computador. O conteiner tem muito mais relação com **isolamento de processo**. Isolando também networking

De onde surgiu? A Dor de Gerenciar Diversas Máquinas.

Imagine um cenário em que você precisa gerenciar vários sistemas operacionais em diferentes máquinas físicas. Cada sistema exige atenção individual, desde a instalação e configuração até a manutenção e segurança. Essa multiplicidade de máquinas gera um problema de gestão:

Então a virtualização (ainda não o conteiner) foi a Solução para Isolamento e Gestão Eficaz. 

Isolamento: Cada VM é um ambiente isolado e seguro, sem interferência de outras VMs.
Gerenciamento Simplificado: Uma única interface centralizada facilita a gestão de todas as VMs.
Economia de Recursos: Otimiza o uso de hardware, energia e espaço físico.


**Docker: Evolução da Virtualização com Containers Leves:**

O Docker representa a próxima geração da virtualização, com foco em containers. O container é uma unidade leve que encapsula tudo o que é necessário para executar um aplicativo: código, bibliotecas, configurações e dependências. As principais vantagens do Docker são:

Portabilidade: Os containers podem ser facilmente transferidos entre diferentes ambientes, como desenvolvimento, teste e produção.
Eficiência: Otimizam o uso de recursos, pois compartilham o kernel do sistema operacional host.
Escalabilidade: Facilitam a replicação e o dimensionamento de aplicações.


**Compreendendo Imagens, Containers e Dockerfiles:**

![alt text](./imgs/image.png)

* Imagem: É um modelo pré-configurado que define o ambiente do container. Imagine-a como a planta de uma casa, que define a estrutura e os elementos básicos.
* Container: É a instância real da imagem, como a casa construída a partir da planta. Vários containers podem ser criados a partir da mesma imagem.
* Dockerfile: É um arquivo que define as instruções passo a passo para construir a imagem. É como o manual de construção da casa, que detalha os materiais e as etapas necessárias.


**Exemplo Prático: Banco de Dados em um Container:**

Imagine que você precisa executar um banco de dados MySQL. Com o Docker, você pode:

1. Obter a imagem: Acessar o Docker Hub, um repositório com diversas imagens prontas para uso, e baixar a imagem oficial do MySQL.
2. Criar o container: Executar o comando docker run para criar um container a partir da imagem do MySQL.
3. Conectar-se ao banco de dados: Acessar o banco de dados através de um cliente MySQL, como o MySQL Workbench.
Isso é simples e escalável. 

**Conclusão**:

A virtualização e o Docker são ferramentas poderosas que simplificam a gestão de ambientes de TI. A virtualização oferece isolamento e gerenciamento eficaz de diferentes sistemas operacionais, enquanto o Docker leva a virtualização a um nível mais granular com containers leves e portáteis. A combinação dessas tecnologias contribui para a eficiência, escalabilidade e segurança de aplicações em diversos cenários.

## o que uma Imagem?

![alt text](./imgs/ogrossaocomocebolas.jpeg)


RESUMO: 
1. As imagens tem camadas;

2. Apenas a última camada do docker pode ser escrita;

3. Caso queira escrever em qualquer outra camada, essa será a cópia e não original;

Imagens são feita em camadas. Apenas a última camada do docker é read and writter, o resto é only read. Ou seja, tudo que está abaixo da útima camada (voltada a escrita), será copiado e reescrito em caso de necessidade de alteração. O docker copia o arquivo, traz para última camada e escreve na cópia. 
Então todas as camadas abaixo da read and writter são identicas, mas a camada de cima é aquela que tem informações diferentes, como logs.

**Exemplo prático**: caso você crie dois 'runs', um para realizar o apt-get install do requirements, e outro para limpar o cache, o da limpeza do cache não funcionará, porque apenas a última camada é read and writter, impossível de alterar camada anterior que é apenas only read.

Escrever até mesmo um simples "a" em arquivos grandes pode causar certa degradação da performance, já que precisa ser copiado completamente para a 1ª camada.A partir disso surgiram novas soluções a fim de minimar o problema da cópia.


"A camada base (ready-only), a última camada da lista (de cima para baixo), é a camada de inicialização, é onde fica o kernel e o setor de inicialização bootfs, que é bastante similar ao sistema de inicialização raiz / do Linux/Unix. Nessa parte é onde temos toda a criação dos cgroups para fazer todo o controle de processos, namespace para blindar o container." [Álvares, Bernard, 2020. "As camadas que compõe uma docker image](https://medium.com/@bernard.luz/as-camadas-que-comp%C3%B5e-uma-docker-image-f77cfa7d04ce)



Lembre-se, uma imagem dá luz a vários conteiner, ou seja, é a mesma imagem e a mesma informação, tanto que, 10 imagens docker com de 50GB ocupará apenas 50GB ao invés de meio Terabyte. Apenas é adicionado as informações que a última camada de read and writter, como os logs. 

## Namespace, cgroups e netfilter.

![alt text](./imgs/netfilter.jpeg)

**Namespace**:
Cada conteiner tem sua árvore de processos que não interfere na execução do outro. Além do PID namespace e Netnamespace, há outros namespaces, como o Mount namespace para isolar pontos de montagem, o UTS namespace para isolamento de hostname. 

"PID namespace: como o nome sugere, é este namespace que faz o isolamento dos processos permitindo que os containers tenham seus próprios PID (como um aliás do PID original);
Net namespace: este é o namespace responsável por permitir que cada container possua sua própria interface de rede e portas;
MNT namespace: permite que cada container tenha seu sistema de arquivos;
IPC namespace: garante o isolamento dos sistemas de arquivos criados pelo MNT namespace;
User namespace: garante o isolamento dos usuários em cada container;
UTS namespace: responsável pelo isolamento de nome de domínios, hostname entre outros.

Os containers possuem suas próprias árvores de processos, interface de rede, portas, ip, sistema de arquivos, usuários, hostname entre outros. Tudo isso é provido pelos namespaces que promovem um isolamento destas características de forma que nenhum container pode/consegue interferir em outro. Desta forma, na minha opinião está é a melhor forma de entender melhor os containers: são grupos de processos isolados através dos namespaces de forma que o host conhece todos os processos de todos os containers, mas os containers enxergam/conhece apenas os seus processos. Isso faz com que os containers sejam isolados uns dos outros sem que se interfiram." [Andrade, Flávio, 2018. "Entendendo os containers do Docker"](https://medium.com/@flaviochess/entendendo-os-containers-do-docker-a4a481007885)

**Cgroups**:

Control Groups, são responsáveis por limitar, controlar recursos e por medir, contabilizar e isolar recursos de um grupo de processos. Eles são utilizados para limitar não apenas CPU e memória, mas também outros recursos como I/O de disco, largura de banda de rede, entre outros.

**Netfilter**:

Framework no kernel do Linux para manipulação de pacotes de rede e filtragem. No contexto de containers, o Docker utiliza o Netfilter para realizar a tradução de endereços (NAT) e permitir a comunicação entre o container e a rede externa. É importante destacar que o Netfilter não está diretamente relacionado à comunicação entre containers dentro do mesmo host. É mais comum referir-se ao serviço que gerencia as regras do Netfilter como iptables.


## Instalando docker

[Siga a documentação, é mais seguro!](https://docs.docker.com/engine/install/ubuntu/)
Detalhes no livro [descomplicando docker](https://livro.descomplicandodocker.com.br/chapters/chapter_03.html)
Tive problemas em relação a um pacote podman, resolvi arrancar: 


```sh
Unpacking docker-ce-cli (5:25.0.2-1~ubuntu.22.04~jammy) ...
dpkg: error processing archive /var/cache/apt/archives/docker-ce-cli_5%3a25.0.2-
1~ubuntu.22.04~jammy_amd64.deb (--unpack):
 trying to overwrite '/usr/bin/docker', which is also in package podman-docker 3
.4.4+ds1-1ubuntu1.22.04.2
dpkg-deb: error: paste subprocess was killed by signal (Broken pipe)
Errors were encountered while processing:
 /var/cache/apt/archives/docker-ce-cli_5%3a25.0.2-1~ubuntu.22.04~jammy_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

Realizei primeiro [esses passo indicados](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine), depois removi o tal do podman:

```sh
sudo apt remove podman
sudo apt autoclean && sudo apt autoremove
```