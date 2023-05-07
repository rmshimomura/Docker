# Docker

### Comandos básicos:
- ```docker ps``` : Lista os containers em execução
    - ```docker ps -a``` : Lista todos os containers rodados (junto com os que estão rodando no momento)
- ```docker run <imagem>``` : Cria um novo container
    - ```docker run -it <imagem>``` : Cria um novo container deixando ele interativo no terminal
    - ```docker run -d <imagem>``` : Cria um novo container deixando ele "detached", ou seja rodando no background, sem travar o terminal
    - ```docker run --name <nome> <imagem>``` : Cria um novo container com o nome passado depois da flag ```--name```
    - ```docker run -p <porta_do_host:porta_do_container> <imagem>``` : Cria um container e expõe a porta dele para o mundo afora, lembrando que tem que a ordem importa e tem que colocar os dois pontos entre os números das portas
- ```docker start <id>``` : "Liga" o container novamente
- ```docker rm <id>``` : Remove um container
    - ```docker rm -f <id>``` : Força a remoção de um container
- ```docker stop <id>``` : Para um container sem removê-lo
- ```docker images```: Lista as imagens guardadas em cache
    - ```docker image rm <id>``` : Remove uma imagem específica
- ```docker logs <id>``` : Mostra o que aconteceu em um container específico
    - ```docker logs -f <id>``` : Parecido com o anterior, porém todas as operações que aconteceram no container vão estar aparecendo em tempo real no terminal