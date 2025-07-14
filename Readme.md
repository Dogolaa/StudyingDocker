# Introdução ao Docker

## Containers

### O que são containers?

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
- Resolve o famoso "Na minha máquina funciona"

<br>

### Containers vs Máquinas Virtuais

![alt text](./readmeImages/containersVsVm.png)

### Docker no CMD

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
# Com o comando abaixo filtra-se apenas os que tem a palavra "name"
docker run --help | grep name

# Para facilitar a visualização podemos usar um sinal de "=" em cada parâmetro
docker run --name=mycontainer hello-world

# Para remover um container, ele deve estar "saido" ou parado, não é possível remover um container em execução
docker rm ID_DO_CONTAINER
# Ou
docker rm NOME_DO_CONTAINER

# Para parar um container
docker stop NOME_DO_CONTAINER

# Para iniciar um container
docker start NOME_DO_CONTAINER

# Para remover um container em execução
docker rm -f NOME_DO_CONTAINER

# Para rodar um container em segundo plano usa-se o comando abaixo, -d significa dettached, "desemprende" do terminal
docker run -d nginx

# Para prender um terminal em um container, usa-se
docker attach ID_DO_CONTAINER

# Para executar comandos dentro de um container, podemos utilizar os comandos abaixo, no primeiro exemplo estamos executando o comando "ls" dentro do container
docker exec NOME_DO_CONTAINER ls

# Para "entrar" no terminal dentro do container
docker exec -it ID_DO_CONTAINER bash

# Para o container se autoremover quando parado ou "saido"
docker run --rm nginx

# Para remover multíplos containers, usamos um subcomando
docker rm -f $(docker ps -a -q)

# Para expor uma porta usa-se o comando abaixo, basicamente está dizendo "pegue a porta 80 do container e mapeie-a para a minha porta 8080"
docker run -p 8080:80 nginx
```

## Volumes e Bind-Mounts

### Volumes vs Bind-Mounts

"Bind-Mount, basicamente, pega uma pasta do seu computador e duplica para dentro do container, poder gerar conflitos, mas tem latência baixa, ideal para ambientes de desenvolvimento, já o volume é algo gerenciado pelo docker, não é compartilhada um pasta usual do computador, usa-se quando queremos salvar em um lugar onde o computador não tem acesso, aumentando a segurança, usado também para persistência de dados, ideal para ambientes de produção."

### Bind-Mounts

```sh
# -v serve para indicar a pasta que será compartilhada e a de destino no container
docker run -d -p 8080:80 -v $(pwd)/my_nginx_html:/usr/share/nginx/html nginx
```

Posso remover o container sem perder os arquivos de dentro do meu computador.

<br>

> Caminho mais completo

```sh
docker run -d -p 8080:80 --mount type=bind,source=$(pwd)/my_nginx_html,target=/usr/share/nginx/html nginx
```

### Volumes

```sh
# Comando para criar um volume
docker volume create my_volume

# Exibir os volumes da máquinas
docker volume ls

# Apagar todos os volumes
docker volume prune

# Para verificar todas as informações de um volume
docker volume inspect my_volume

# Para compartilhar pastas com do container através do volume
docker run -d -p 808080 -v my_volume:/usr/share/nginx/html nginx
# Copiando o arquivo de dentro do computador e jogando para o container
docker cp $(pwd)/my_nginx_html/index.html NOME_DO_CONTAINER:/usr/share/nginx/html
```

Posso remover o container, os dados que estão no volume não serão perdidos.

<br>

```sh
# Para realizar backups
docker run --rm -v my_volumes:/data -v $(pwd)/Module1/backup_host:/backup busybox tar czf /backup/backup.tar.gz / data
# Traduzindo o comando acima: vá para a pasta /data que é um volume montado, compacte-a dentro do /backup/backup.tar.gz e ele será exibido na pasta indicada do meu computador como bind-mount

# Para restaurar o backup
docker run --rm -v my_volumes:/data -v $(pwd)/Module1/backup_host:/backup busybox tar xzf /backup/backup.tar.gz -C /
```

## Imagens

Dica: Não use latest, use a versão específica, busque imagens leves, além de leves são menos vulneráveis.

```sh
# Para exibir todas as imagens criadas
docker images

# Para remover imagens, necessário matar o container vinculado a ela
docker rmi NOME_DA_IMAGEM

# Para remover imagens de maneira forçada
docker rmi -f NOME_DA_IMAGEM

# Para verificar informações importantes de images
docker inspect NOME_DA_IMAGEM

# Para verificar informações específicas
docker inspect --format='{{.id}}' NOME_DA_IMAGEM

# Exibe todas as imagens do registro de containers do Docker
docker search nginx

# Para baixar imagens
docker pull nginx:1.20

# Apagar todas as imagens sem containers associados a elas
docker image prune

# Apagar todas as imagens
docker image prune -a
```

### DockerHub

https://hub.docker.com/
