---
description: Instruções para instalação do Docker em Ambiente Linux
keywords: requirements, apt, installation, ubuntu, install, uninstall, upgrade, update
---

Para começar a usar o Docker Engine no Ubuntu, certifique-se de atender aos pré-requisitos e instale o Docker.

## Prerequisites

### Requisitos de sistema operacional

Para instalar o Docker Engine, você precisa da versão de 64 bits de uma dessas versões:

- Ubuntu Focal 20.04 (LTS)
- Ubuntu Bionic 18.04 (LTS)
- Ubuntu Xenial 16.04 (LTS)

O Docker Engine é compatível com as arquiteturas `x86_64` (ou `amd64`), `armhf` e `arm64`.

### Desinstalar versões antigas

Versões mais antigas do Docker eram chamadas de `docker`,` docker.io` ou `docker-engine`. Se estiverem instalados, desinstale-os:

```bash
sudo apt-get update -y
```{{execute}}

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc -y
```{{execute}}

Tudo bem se o `apt-get` relatar que nenhum desses pacotes está instalado.

O conteúdo de `/var/lib/docker/`, incluindo imagens, contêineres, volumes e redes, são preservados. Se você não precisa salvar seus dados existentes e deseja comece com uma instalação limpa, consulte **Desinstalar Docker Engine** seção na parte inferior desta página.

### Drivers de armazenamento suportados

O Docker Engine no Ubuntu suporta drivers de armazenamento `overlay2`, `aufs` e `btrfs`.

O Docker Engine usa o driver de armazenamento `overlay2` por padrão. Se você precisar usar `aufs` em vez disso, precisará configurá-lo manualmente. Consulte a [Documentação do Docker](https://docs.docker.com/storage/storagedriver/aufs-driver/).

## Métodos de instalação

Você pode instalar o Docker Engine de diferentes maneiras, dependendo de suas necessidades:

- Maioria dos usuários
  Configurar e instalar os repositórios do Docker para facilitar as tarefas de e atualização. Isto é o abordagem recomendada.

### Instale usando repositório

Antes de instalar o Docker Engine pela primeira vez em uma nova máquina host, você precisa configurar o repositório Docker. Depois disso, você pode instalar e atualizar o Docker do repositório.

#### Configure o repositório

URL_BASE="https://download.docker.com/linux/ubuntu"

1.  Atualize o índice do pacote `apt` e instale os pacotes para permitir que o `apt` use um repositório sobre HTTPS:

    ```bash
    sudo apt-get update

    sudo apt-get install \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg-agent \
        software-properties-common
    ```{{execute}}

2.  Adicione a chave GPG oficial do Docker:

    ```bash
    curl -fsSL $URL_BASE/gpg | sudo apt-key add -
    ```{{execute}}

    Verifique se agora você tem a chave com a impressão digital: <span><code>9DC8 5822 9FC7 DD38 854A&nbsp;&nbsp;E2D8 8D81 803C 0EBF CD88</code></span>, procurando pelo últimos 8 caracteres da impressão digital.

    ```bash
    sudo apt-key fingerprint 0EBFCD88

    pub   rsa4096 2017-02-22 [SCEA]
          9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
    uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
    sub   rsa4096 2017-02-22 [S]
    ```

3.  Use o seguinte comando para configurar o repositório ** estável **.

    > **Nota**: O comando `lsb_release -cs` abaixo retorna o nome da sua
    > Distribuição Ubuntu, como `xenial`. Às vezes, em uma distribuição
    > como o Linux Mint, você pode precisar alterar `(lsb_release -cs)`
    > para sua distribuição pai do Ubuntu. Por exemplo, se você estiver usando
    > `Linux Mint Tessa`, você pode usar `bionic`. O Docker não oferece nenhuma garantia em
    > distribuições não testados e sem suporte.

    ```bash
    sudo add-apt-repository \
       "deb [arch=amd64] $URL_BASE \
       $(lsb_release -cs) \
       stable"
    ```{{execute}}

#### Instale o Docker Engine

1. Atualize o índice do pacote `apt` e instale a _última versão_ do Docker Engine e containerd ou vá para a próxima etapa para instalar uma versão específica:

    ```bash
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io
    ```{{execute}}

    > Tem vários repositórios Docker?
    >
    > Se você tiver vários repositórios Docker ativados, instalando
    > ou atualizando sem especificar uma versão no `apt-get install` ou
    > O comando `apt-get update` sempre instala a versão mais alta possível,
    > que pode não ser apropriado para suas necessidades de estabilidade.

2.  Para instalar uma _versão específica_ do Docker Engine, liste as versões disponíveis no repo, selecione e instale::

    a. Liste as versões disponíveis em seu repo:

    ```bash
    apt-cache madison docker-ce

      docker-ce | 5:18.09.1~3-0~ubuntu-xenial | {{ download-url-base }}  xenial/stable amd64 Packages
      docker-ce | 5:18.09.0~3-0~ubuntu-xenial | {{ download-url-base }}  xenial/stable amd64 Packages
      docker-ce | 18.06.1~ce~3-0~ubuntu       | {{ download-url-base }}  xenial/stable amd64 Packages
      docker-ce | 18.06.0~ce~3-0~ubuntu       | {{ download-url-base }}  xenial/stable amd64 Packages
      ...
    ```

    b. Instale uma versão específica usando a string de versão da segunda coluna, por exemplo, `5: 18.09.1 ~ 3-0 ~ ubuntu-xenial`.

    ```bash
    sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
    ```{{execute}}

3.  Verifique se o Docker Engine está instalado corretamente executando a imagem `hello-world`.

    ```bash
    sudo docker run hello-world
    ```{{execute}}

    Este comando baixa uma imagem de teste e a executa em um contêiner. Quando o o contêiner é executado, ele imprime uma mensagem informativa e sai.

O Docker Engine está instalado e em execução. O grupo `docker` é criado, mas nenhum usuário
é adicionado a ele. Você precisa usar `sudo` para executar comandos do Docker.
Continue para **Etapas de pós-instalação para Linux** para permitir que usuários não privilegiados
sejam capaz de executar comandos do Docker e para outras etapas de configuração opcionais.

#### Atualizar Docker Engine

Para atualizar o Docker Engine, primeiro execute `sudo apt-get update` e siga **Instruções de Instalação**, escolhendo o novo versão que você deseja instalar.

## Desinstalar Docker Engine

1.  Desinstale os pacotes Docker Engine, CLI e Containerd:

    ```bash
    sudo apt-get purge docker-ce docker-ce-cli containerd.io
    ```{{execute}}

2.  Imagens, contêineres, volumes ou arquivos de configuração personalizados em seu host não são removidos automaticamente. Para excluir todas as imagens, contêineres e volumes:

    ```bash
    sudo rm -rf /var/lib/docker
    ```{{execute}}

Você deve excluir quaisquer arquivos de configuração editados manualmente.

## Etapas de pós-instalação para Linux

- Continue to [Post-installation steps for Linux](linux-postinstall.md).
