---
description: Instalação Simplificada do Docker em Ambiente Linux
keywords: requirements, apt, installation, ubuntu, install, uninstall, upgrade, update
---

Uma outra possibilidade de instalação do Docker em sistemas Linix é utilizar o script de instalação disponível em:

[get.docker.com](get.docker.com)

O propósito do script de instalação é a conveniência de instalar rapidamente as versões mais recentes do Docker-CE nas distribuições Linux suportadas. Não é recomendado depender deste script para implantação em sistemas de produção. Para obter instruções mais completas sobre a instalação nas distros com suporte, consulte as [instruções de instalação](https://docs.docker.com/engine/installation/).

## Uso

Fazendo o download do script e executando.

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```{{execute}}

Executendo o script diretamente.
```bash
curl -fsSL https://get.docker.com | bash
```{{execute}}
