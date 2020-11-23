---
description: Etapas de pós-instalação para Linux
keywords: Docker, Docker documentation, requirements, apt, installation, ubuntu, install, uninstall, upgrade, update
title: Etapas de pós-instalação para Linux
---

Esta seção contém procedimentos opcionais para configurar hosts Linux para funcionar melhor com Docker.

## Gerenciar Docker como um usuário não root

O daemon Docker se liga a um soquete Unix em vez de uma porta TCP. Por padrão o soquete Unix é propriedade do usuário `root` e outros usuários só podem acessá-lo usando `sudo`. O daemon Docker sempre é executado como o usuário `root`.

Se você não quiser usar o comando `docker` com` sudo`, crie um grupo chamado `docker` e adicionar usuários nele. Quando o daemon do Docker é iniciado, o socket Unix acessível aos membros do grupo `docker`.

> Aviso
>
> O grupo `docker` concede privilégios equivalentes ao` root` usuário.
> Para obter detalhes sobre como isso afeta a segurança do seu sistema, consulte
> [* Docker Daemon Attack Surface *](https://docs.docker.com/engine/security/#docker-daemon-attack-surface).
{: .warning}

> **Nota**:
>
> Para executar o Docker sem privilégios de root, consulte
> [Execute o daemon do Docker como um usuário não root (modo Rootless)](https://docs.docker.com/engine/security/rootless/).
>
> O modo sem root está atualmente disponível como um recurso experimental.

Para criar o grupo `docker` e adicionar seu usuário:

1.  Crie o grupo `docker`.

    ```bash
    sudo groupadd docker
    ```{{execute}}

2.  Adicione seu usuário ao grupo `docker`.

    ```bash
    sudo usermod -aG docker $USER
    ```{{execute}}

3. Faça logout e login novamente para que sua associação ao grupo seja reavaliada.

    Se estiver testando em uma máquina virtual, pode ser necessário reiniciar a máquina virtual para que as alterações tenham efeito.

    Em um ambiente de desktop Linux, como X Windows, saia da sessão completamente e faça login novamente.

    No Linux, você também pode executar o seguinte comando para ativar as alterações nos grupos:

    ```bash
    newgrp docker 
    ```{{execute}}

4. Verifique se você pode executar comandos `docker` sem` sudo`.

    ```bash
    docker run hello-world
    ```{{execute}}

    Este comando baixa uma imagem de teste e a executa em um contêiner. Quando o contêiner é executado, ele imprime uma mensagem informativa e sai.

     Se você inicialmente executou comandos Docker CLI usando `sudo` antes de adicionarseu usuário para o grupo `docker`, você pode ver o seguinte erro, que indica que seu diretório `~/.docker /` foi criado com permissões incorretas devido aos comandos `sudo`.

    ```none
    WARNING: Error loading config file: /home/user/.docker/config.json -
    stat /home/user/.docker/config.json: permission denied
    ```

    Para corrigir este problema, remova o diretório `~/.docker /`
    (é recriado automaticamente, mas todas as configurações personalizadas
    são perdidos) ou altere sua propriedade e permissões usando o
    seguintes comandos:

    ```bash
    sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
    sudo chmod g+rwx "$HOME/.docker" -R
    ```{{execute}}

## Configure o Docker para iniciar na inicialização

A maioria das distribuições Linux atuais (RHEL, CentOS, Fedora, Ubuntu 16.04 e superior) use `systemd` para gerenciar quais serviços iniciam quando o sistema inicializa. Ubuntu 14.10 e abaixo usam `upstart`.

### `systemd`

```bash
sudo systemctl enable docker
```{{execute}}

Para desabilitar esse comportamento, use `disable`.

```bash
sudo systemctl disable docker
```{{execute}}

Se você precisar adicionar um proxy HTTP, definir um diretório ou partição diferente para o arquivos do Docker ou fazer outras personalizações, consulte [personalize as opções de daemon do Docker do systemd](https://docs.docker.com/config/daemon/systemd/).

### `upstart`

O Docker é configurado automaticamente para iniciar na inicialização usando `upstart`. Para desativar esse comportamento, use o seguinte comando:

```bash
echo manual | sudo tee /etc/init/docker.override
```{{execute}}

### `chkconfig`

```bash
sudo chkconfig docker on
```{{execute}}