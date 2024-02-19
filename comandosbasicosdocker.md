## Comandos básicos
![alt text](./imgs/entrepoint.jpeg)

Primeiro, saiba que o Docker Client é a interface que você usa para interagir com o Docker. Permite executar comandos como docker run, docker build, docker ps e docker images. Já o Docker Daemon é o serviço que faz o trabalho pesado de gerenciar containers. Responsável por baixar imagens, criar containers, iniciar e parar containers, gerenciar redes e volumes, e executa os comandos recebidos do docker client.

Então:

```sh
docker run
```

1. O comando "docker" se comunica com o daemon do Docker informando a ação desejada.

2. O daemon do Docker verifica se a imagem "hello-world" existe em seu host; caso ainda não, o Docker faz o download da imagem diretamente do Docker Hub.

3. O daemon do Docker cria um novo container utilizando a imagem que você acabou de baixar.

4. O daemon do Docker envia a saída para o comando "docker", que imprime a mensagem em seu terminal. [Descomplicando o Docker](https://livro.descomplicandodocker.com.br/chapters/chapter_04.html)

```sh
docker container ls
docker container ls -a
```

Mostra apenas aqueles conteiners que estão em execução. Com -a mostra todos os conteiners que já foram executados. 


```sh
docker run -it ubuntu
```

Nesse caso, você estará realizando o donwload da imagem e logo em seguida estará em iteranção com ele pelo bash Portanto se você fizer ps -ef, apenas verá dois processos, o que é diferente da sua máquina. 

O docker costuma matar conteiner quando não há mais o processo principal rodando, ou seja, não existe seu entrepoint. Todo conteiner tem um entrepoint, o principal processo do conteiner. Por exemplo, se você mata o terminal ubutun, você mata o container, pois o terminal é o entrepoint da aplicação.
Para sair sem matar o bash e etc, saia com `ctrl + pq`


Tecnicamente, para voltar para dentro de um container rodando, você realiza o attach.

```sh
docker container attach <id containers>
```

Estranhamento esse comando congelou meu o terminal em alguns container que usei. Mas por quê? Porque existem container que o entrepoint dele não é o bash, é o próprio processo. Todo container tem seu principal processo rodando em primeiro plano. Por isso, em alguns containers como, por exemplo, nginx, não existe um bash para você ter uma iteratividade . Nem com attach nem com -it.

Para evitar isso, pode usar o exec. 

```sh
docker container exec -it <id containers> bash
```

ou

```sh
docker compose exec -it <id containers> bash
```

O que é rodar como deamon? Não trava o seu terminal, você roda com -d, sem iteratividade. 

```sh
docker container run -d nginx
```

