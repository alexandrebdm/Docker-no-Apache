# Docker-no-Apache
Rotina de instalção do docker no Apache

Como funciona o Docker.

Docker é um programa que que funciona como um contêiner, ou seja você vai instalar aplicações e configurar em docker e posteriormente vai baixar o docker com a aplicação já funcionando no estado que foi deixada independente do sistema operacional do host e sem necessidade de configura-la novamente.

Tutorial de instaláção no Ubuntu 16.04.

Pré-requisitos

Droplet (servidor privado e virtual) do Ubuntu 16.04 64 bits Usuário root. O Docker requer uma versão de 64 bits do Ubuntu, bem como uma versão de kernel maior ou igual a 3.10. O Droplet padrão de 64 bits do Ubuntu 16.04.

Realize atualização do repositório oficial.

Atualizando os pacotes do Ubuntu :

>apt-get update

Ative a chave GPG oficial do repositório do Docker:

> apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

Adicione às fontes do APT:

> apt-add-repository 'deb https://apt.dockerproject.org/repo ubuntu-xenial main'

Atualizar os pacotes do sistema novamente para que os pacotes do Docker instalados sejam adicionados.

> apt-get update

Verifique se está instalando a partir do repositório do Docker em vez do repositório padrão do Ubuntu 16.04:

> apt-cache policy docker-engine

Você deverá ver uma saída semelhante à essa:

                                docker-engine:

Installed: (none)

Candidate: 1.11.1-0xenial 500

500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages

1.11.0-0~xenial 500

500 https://apt.dockerproject.org/repo ubuntu-xenial/main amd64 Packages

Observe que o docker-engine não está instalado, mas o candidato para instalação é do repositório Docker do Ubuntu 16.04. O número da versão do docker-engine pode ser diferente.
O sistema está pronto para receber o Docker.

> apt-get install -y docker-engine

O Docker foi instalado, e o daemon iniciado, e o processo habilitado para iniciar no boot. Verifique que ele está executando:

> systemctl status docker

A saída do comando anterior deve ser similar a essa, mostrando que o serviço está ativo e em execução:

                              [ secondary_label Output]
                              
● docker.service - Docker Application Container 

Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)

Active: active (running) since Sun 2016-05-01 06:53:52 CDT; 1 weeks 3 days ago

Docs: https://docs.docker.com

Main PID: 749 (docker)

Se sua saída foi semelhante a essa a instalação está correta. Agora vamos usar uma imagem docker pronta.

> docker run -dit --name apache-app --publish=9081:80 chicocx/docker-apache

Vamos configurar externamente à imagem docker o local onde o apache salvará o site, utilize:

> docker run -dit --name apache-app --publish=9081:80 -v "$PWD":/usr/local/apache2/htdocs/ chicocx/docker-apache

Configuração do diretório de onde o comando docker é executado como sendo o ponto de montagem do diretório /usr/local/apache2/htdocs/ da imagem criada. No caso, "$PWD" retorna o diretório atual.

> -v "$PWD":/usr/local/apache2/htdocs/

Podemos modificar esse diretório da seguinte forma:

> -v /diretorio/do/host:/usr/local/apache2/htdocs/

Dessa forma, /diretorio/do/host passa a ser o diretório do host que conterá os arquivos lidos pela imagem em /usr/local/apache2/htdocs/

Alguns comandos úteis.

Para entrar na máquina docker/imagem:

 > docker exec -it redmine1 bash
Para sair do docker sem encerrar a máquina:

> ctrl p q

Listando os status dos containers

> docker ps -a

Excluindo os containers que estão parados

> docker rm $(docker ps -q -f status=exited)

Listando as imagens baixadas

> docker images

Excluindo alguma imagem

> docker rmi f72216345d97

Onde f72216345d97 é o código da imagem instalada.
