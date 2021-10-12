# O que é Docker?

Docker é uma plataforma Open Source que possibilita o empacotamento de uma aplicação dentro de um container. Uma aplicação consegue se adequar e rodar em qualquer máquina que tenha essa tecnologia instalada.

De acordo com o www.docker.com, um container é uma unidade padrão de software que empacota o código e todas as suas dependências para que o aplicativo seja executado de maneira rápida e confiável de um ambiente de computação para outro. Uma imagem de container do Docker é um pacote de software leve, autônomo e executável que inclui tudo o que é necessário para executar um aplicativo: código, tempo de execução, ferramentas do sistema, bibliotecas do sistema e configurações. É uma maneira de garantir que outros desenvolvedores, que poderão trabalhar em seu projeto, de máquinas diferentes, possam trabalhar exatamente no mesmo ambiente que você, independente da máquina utilizada, todos terão os mesmo recursos, por exemplo, mesma versão do PHP, MySQL, recursos computacionais e etc…

![Estrutura Docker Conteiners](/docker_structure.png)

# Passo a Passo

1. Instalar o Docker (www.docker.com)

2. Dockerfile

“O Dockerfile é um arquivo de texto que contém as instruções necessárias para criar uma nova imagem de contêiner. Essas instruções incluem a identificação de uma imagem existente a ser usada como uma base, comandos a serem executados durante o processo de criação da imagem e um comando que será executado quando novas instâncias da imagem de contêiner forem implantadas.”

Neste arquivo a instrução `FROM` define a imagem de container que será usada durante o processo de criação de nova imagem. 

Exemplo: ` FROM wyveo/nginx-php-fpm:latest ` aonde  `wyveo/nginx-php-fpm` é o endereço da imagem e `latest` indica que queremos a versão mais atual dessa imagem.

`WORKDIR` “A instrução WORKDIR define um diretório de trabalho para outras instruções Dockerfile, como `RUN`, `CMD` e também o diretório de trabalho para executar instâncias da imagem do container.”

`RUN`
“A instrução RUN especifica os comandos a serem executados e capturados na nova imagem de contêiner. Esses comandos podem incluir itens como a instalação de software, a criação de arquivos e diretórios e a criação da configuração do ambiente.” 

O Exemplo abaixo implementa um container com a imagem do `NGINX/PHP-FPM` e realiza a cópia dos arquivos e diretórios locais para o sistema de arquivos do container. Isto se faz necessário caso desejarmos realizar o build da imagem para o nosso Docker Hub incluindo o código fonte por exemplo.
    
    FROM wyveo/nginx-php-fpm:latest
    WORKDIR /usr/share/nginx/
    RUN rm -rf /usr/share/ngix/html
    COPY . /usr/share/nginx
    RUN chmod -R 775 /usr/share/nginx/storage/*
    RUN ln -s public html

3. Docker Compose (docker-compose.yml)

O docker-compose é uma ferramenta do Docker que, a partir de diversas especificações, permite subir diversos containeres e relacioná-los através de redes internas. No arquivo docker-compose, podemos declarar por exemplo a seguinte estrutura:

`version` : declara a versão do docker compose
`services`: declara quais serviços serão rodados, nesse caso, chamaremos de laravel-app.
`build`: declara o nome da imagem, ou, no caso, se declararmos o ., ele irá “chamar” a imagem declarada no Dockerfile.
`ports`: realiza a liberação das portas. Na minha máquina eu quero que libere a porta 8080, porém, lá no meu container eu quero que seja liberada a porta 80, ou seja, toda vez que eu acessar o meu localhost com a porta 8080 o Docker irá redirecionar para a porta 80 o nginx criado no container.

4. Comandos Básicos Docker:

`$ docker ps` : lista os conteiners existentes.

`$ docker images` : lista as imagens existentes.

`$ docker-compose up -d` Inicia um container, sendo que O `docker-compose up` irá rodar o docker compose, baseado em nosso `docker-compose.yaml` e com o `-d` o container é inicializado em segundo plano e podemos utilizar o nosso terminal para outros comandos.

`$ docker-compose up -d --build` Inicia um container, reconstruindo a sua imagem.

`$ docker-compose down` Interrompe um container

