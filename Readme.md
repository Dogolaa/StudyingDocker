# Introdução ao Docker

## O que são containers?

<br>

"Um container é uma unidade padronizada de software que empacota o código e todas as suas dependências para que um software seja executado de forma rápida e consistente em qualquer ambiente."

<br>

Principais Características:

- Imutabilidade
- Isolamente de processos e recursos computacionais
- Leves - É executado com um processo no sistema operacional.
- Utilizam os recursos do Kernel do SO - Não possui a necessidade de instalar um novo SO
- Rápido de iniciar e de ser removido ou parado - Não há necessidade de 'boot'
- Utilizam "Imagens" (imutáveis) para serem executados - Algo similar a um snapshot
- Linux é rei
- "Na minha máquina funciona"

<br>

## Containers vs Máquinas Virtuais

![alt text](./readmeImages/containersVsVm.png)

## Docker no cmd

```sh
# run executa um container, e depois passa-se o nome da imagem, no caso, hello-world
docker run hello-world

# Mostra os container em execução
docker ps

# Mostra todos os containers que temos, independente se estão sendo executados ou não
docker ps -a

# Toda vez que se usa o comando run cria-se uma nova imagem, mesmo usando a mesma imagem
docker run --name mycontainer hello-world

# Para descobrir comandos como o --name acima, podemos executar
docker run --help
# Como o comando abaixo filtra-se apenas os que tem a palavra "name"
docker run --help | grep name

# Para facilitar a visualização podemos usar um sinal de "=" em cada parâmetro
docker run --name=mycontainer hello-world


```
