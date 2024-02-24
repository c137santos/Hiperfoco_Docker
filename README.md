<link rel="stylesheet" type="text/css" href="style.css">

# Repositório para Descomplicar o Docker

<div class="imagem-container">
  <img src="./imgs/santosindocker.jpeg">
</div>

Livro base é o [descomplicandoDocker](https://livro.descomplicandodocker.com.br/)

Conteiner não é Docker. Porém o Docker foi aquele populou essa forma de isolamento de processos. Por exemplo, o avó do Docker é o Shroot

## O que é conteiner?

### [Acesse o Resumo aqui](./oqueeconteiner.md)

1. Que problema o Docker resolve?
    * Uma alternativa a virtualização
2. Eficiência, escalabilidade e segurança


<div class="imagem-container">
  <img src="./imgs/image.png">
</div>


## O que são as Imagens?

### [Acesse o Resumo aqui](./oquesaoimagens.md)

1. As imagens tem camadas;
2. Apenas a última camada do docker pode ser escrita;
3. Caso queira escrever em qualquer outra camada, essa será a cópia e não original;

<div class="imagem-container">
  <img src="./imgs/ogrossaocomocebolas.jpeg">
</div>

## Namespace, cgroups e netfilter.

### [Acesse o Resumo aqui](./conceitosbasicos.md)

1. Conceitos imprescindíveis do Docker
    * NetFilter - iptables
    * Namespace - isolamento
    * Cgroups - recursos
2. Instalando o docker

<div class="imagem-container">
  <img src="./imgs/netfilter.jpeg">
</div>


## Comandos básicos

### [Acesse o Resumo aqui](./comandosbasicosdocker.md)

1. Attach vs Exec
2. Conceito de Entrepoint
3. Docker conteiner ls, ls-a e ps -ef.


![alt text](./imgs/entrepoint.jpeg)