---
title: Visão Geral
layout: singlePage
sectionid: overview
---

## <a href="#overview" name="overview" class="anchor"> O que são contêineres de desenvolvimento? </a>
A medida que conteinerização de cargas de produção se torna comum, mais desenvolvedores estão usando contêineres para cenários que vão além de deploy, incluindo integração contínua, automatização de testes e até mesmo ambientes de codificação completos.

As necessidades de cenário podem variar de ambientes com um simples contêiner, para setups mais complexos com multi-contêineres orquestrados. Ao invés de tentar criar outro formato orquestrador, a *Especificação de Contêinerers de Desenvolvimento* (ou Dev Container Spec) busca encontrar maneiras de enriquecer formatos existentes com metadados para parametrizações, ferramentas e configurações comuns de desenvolvimento.

### <a href="#metadata-format" name="metadata-format" class="anchor"> Um formato de metadados estruturado </a>

Assim como o [Language Server Protocol](https://microsoft.github.io/language-server-protocol/), o primeiro formato desta especificação, [`devcontainer.json`](implementors/json_reference), nasceu da necessidade. São metadados em um formato *JSON with Comments* (jsonc) que outras ferramentas podem usar para armazenar qualquer configuração necessária para *codar* em um contêiner local ou na nuvem. 

Desde que a spec foi inicialmente publicada, os metadados de dev container podem agora ser armazenados de labels de imagem a em blocos de metadados reutilizáveis e instalar scripts conhecidos como [Dev Container Features](../features). Prevemos que esses mesmos dados estruturados possam ser incorporados em outros formatos – tudo isso mantendo um mesmo modelo de objeto para um processamento consistente.

### <a href="#Development-vs-production" name="Development-vs-production" class="anchor"> Desenvolvimento vs produção </a>

Um contêiner de desenvolvimento define um ambiente no qual você desenvolve sua aplicação antes que você esteja pronto para disponibilizá-la. Enquanto os contêineres de deploy e desenvolvimento possam se parecer um com o outro, você pode querer não incluir, em uma imagem de deploy, certas ferramentas que são usadas durante o desenvolvimento.   

<img alt="Diagram of inner and outer loop of container-based development" src="/assets/img/dev-container-stages.png"/>

### <a href="#build-and-test" name="build-and-test" class="anchor"> Build e teste </a>

Para além da configuração repetitiva, esses mesmo contêineres de desenvolvimentos proveem consistência para evitar problemas específicos de ambiente entre desenvolvedores e centraliza serviços de build e automatização de testes. O [CLI reference implementation](https://github.com/devcontainers/cli) open-source pode ser usado diretamente ou integrando tanto com um Docker Compose quanto com uma opção simplificada de um contêiner, não orquestrado – então assim eles podem ser usados como ambientes de codificação ou integração contínua e testes. 

Estão disponíveis uma Action do Github e uma Task do Azure DevOps em [devcontainers/ci](https://github.com/devcontainers/ci) para rodar um *dev container* de um repositório no build de uma integração contínua. Isso permite reutilizar a mesma configuração de desenvolvimento no build e nos testes de código do CI. 

### <a href="#supporting" name="supporting" class="anchor"> Ferramentas suportadas </a>

Você pode [aprender mais](/supporting.md) sobre como outras ferramentas e serviços suportam a *Dev Container Spec*. 