---
title: Ferramentas e serviços suportados
layout: singlePage
sectionid: supporting
---

Essa página descreve ferramentas e serviços que atualmente suportam o Development Container Specification, incluindo o formato `devcontainer.json`. Um arquivo `devcontainer.json` no seu projeto diz às ferramentas e serviços que suportam essa *spec* a como acessar (ou criar) um *dev container* com uma stack de ferramenta e runtime bem definida.

Enquanto maior parte das [propriedades](implementors/json_reference) se aplicam a qualquer ferramenta ou serviço que suporte um `devcontainer.json`, algumas são específicas para certas ferramentas, que estão descritas abaixo.

## <a href="#editors" name="editors" class="anchor"> Editores </a>

### <a href="#visual-studio-code" name="visual-studio-code" class="anchor"> Visual Studio Code </a>

Propriedades específicas para o Visual Studio Code vão em `vscode` dentro de `customizations`.

```json
"customizations": {
	// Configure properties specific to VS Code.
	"vscode": {
		// Set *default* container specific settings.json values on container create.
		"settings": {},
		"extensions": [],
	}
}
```

| Property | Type  | Description |
|:------------------|:------------|:------------|
| `extensions` | array | Um array com os IDs das extensões que devem ser instaladas no contêiner quando ele é criado. O padrão é `[]`. |
| `settings` | object |

Adiciona os valores de um `settings.json` em um arquivo de configuração específico do contêiner/máquina. O padrão é `{}`. |
{: .table .table-bordered .table-responsive}

Note que as extensões [Dev Containers](#dev-containers) e [GitHub Codespaces](#github-codespaces) suportam essas propriedades do VS Code.

### <a href="#visual-studio" name="visual-studio" class="anchor"> Visual Studio </a>

O Visual Studio passou a suportar *dev container* no Visual Studio 2022 17.4 para projetos C++ usando CMake Presets. Isso faz parte do workload de Linux e desenvolvimento integrado com C++, então garanta que isso foi selecionado na sua instalação do VS. O Visual Studio gerencia o ciclo de vida dos *dev containers* que ele utiliza a medida que você trabalha, mas ele os trata como *targets* remotos de maneira semelhante a outros Linux ou *targets* WSL.

Você pode aprender mais no [post de anúncio no blog](https://devblogs.microsoft.com/cppblog/dev-containers-for-c-in-visual-studio/).

### <a href="#intellij" name="intellij" class="anchor"> IntelliJ IDEA </a>

IntelliJ IDEA suporta *dev containers* que podem rodar remotamente via SSH ou localmente usando Docker.

Você pode aprender mais no [post de anúncio no blog](https://blog.jetbrains.com/idea/2023/06/intellij-idea-2023-2-eap-6/#SupportforDevContainers).

## <a href="#tools" name="tools" class="anchor"> Ferramentas </a>

### <a href="#devcontainer-cli" name="devcontainer-cli" class="anchor"> Dev Container CLI </a>

A interface de linha de comando (CLI) do Dev Container é uma implementação de referência para a *Dev Container Spec*. Está em desenvolvimento no repositório [devcontainers/cli](https://github.com/devcontainers/cli). A ideia é ser utilizada tanto diretamente quanto por ferramentas e serviços que querem dar suporte à spec.

O CLI pode pegar um `devcontainer.json` e criar e configurar um *dev container* a partir disso. Isso permite pré-buildar configurações de *dev container* usando uma *CI* ou produto *DevOps* tipo *Github Actions*. Pode detectar e incluir funcionalidades e aplicá-las ao runtime do contêiner, e rodar [lifecycle scripts](implementors/json_reference/#lifecycle-scripts) tipo `postCreateCommand`, provendo mais poder que um simples `docker build` e `docker run`.

#### <a href="#dev-containers-cli" name="dev-containers-cli" class="anchor"> VS Code extension CLI </a>

A [extensão Dev Containers para VS Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) inclui uma variação do *Dev Container CLI* que adiciona a habilidade de abrir um *dev container* no VS Code. Também é automaticamente atualizado quando a extenção é atualizada. 

Pressione <kbd>cmd/ctrl</kbd>+<kbd>shift</kbd>+<kbd>p</kbd> ou <kbd>F1</kbd> e selecione o comando **Dev Containers: Install devcontainer CLI** para instalá-la.

### <a href="#cachix-devenv" name="cachix-devenv" class="anchor"> Cachix devenv </a>

Cachix's **[devenv](https://devenv.sh/)** agora suporta automaticamente gerando um arquivo `.devcontainer.json`. Isso te dá uma maneira mais conveniente e consistente de usar o [Nix](https://nixos.org/) com qualquer ferramenta ou serviço que suporte o *Dev Container Spec*!

Veja a [documentação do devenv](https://devenv.sh/integrations/codespaces-devcontainer/) para detalhes. 

### <a href="#jetify-devbox" name="jetify-devbox" class="anchor"> Jetify Devbox </a>

[Jetify](https://jetify.com) (antigo jetpack.io) é um serviço para deploy de aplicações baseado em [Nix](https://nixos.org/). O [DevBox](https://www.jetify.com/devbox/) fornece uma forma de usar o Nix para gerar um ambiente de desenvolvimento. A [extensão de VS Code Jetify](https://marketplace.visualstudio.com/items?itemName=jetpack-io.devbox) permite que você rapidamente tire vantagem do DevBox em qualquer ferramenta ou serviços que suporte o *Dev Container Spec*.

Pressione <kbd>cmd/ctrl</kbd>+<kbd>shift</kbd>+<kbd>p</kbd> ou <kbd>F1</kbd> e selecione o comando **Generate Dev Container files** para começar!

### <a href="#dev-containers" name="dev-containers" class="anchor"> Extensão VS Code Dev Containers </a>

A [extensão Dev Containers do Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) te deixa usar um [contêiner Docker](https://docker.com) como um ambiente de desenvolvimento completo. Te permite abrir qualquer pasta (dentro ou montada) em um contêiner e se aproveita de todas as funcionalidades do VS Code. Mais informações na [documentação](https://code.visualstudio.com/docs/remote/containers) do *Dev Containers*.

> **Dica:** Se você fizer alguma mudança no seu *dev container* depois de buildar e se conectar à ele, garanta de rodar  **Dev Containers: Rebuild Container** no Command Palette (<kbd>cmd/ctrl</kbd>+<kbd>shift</kbd>+<kbd>p</kbd> ou <kbd>F1</kbd>) para pegar qualquer mudança que você tenha feito.

#### <a href="#product-specific-properties" name="product-specific-properties" class="anchor"> Propriedades específicas de produtos</a>

A extenção Dev Containers implementa as [properties específicas do VS Code](#visual-studio-code).

#### <a href="#dev-containers-code-specific-limitations" name="dev-containers-specific-limitations" class="anchor"> Limitações específicas de Produtos </a>

Alguns produtos também podem ter certas limitações na extensão Dev Containers.

| Property or variable | Type  | Description |
|:------------------|:------------|:------------|
| `workspaceMount` | string | Ainda não suportada quando usando *Clone Repository* em um *Container Volume*. |
| `workspaceFolder` | string | Ainda não suportada quando usando *Clone Repository* em um *Container Volume*. |
| `${localWorkspaceFolder}` | Any | Ainda não suportada quando usando *Clone Repository* em um *Container Volume*. |
| `${localWorkspaceFolderBasename}` | Any | Ainda não suportada quando usando *Clone Repository* em um *Container Volume*. |
{: .table .table-bordered .table-responsive}

## <a href="#services" name="services" class="anchor"> Serviços</a>

### <a href="#github-codespaces" name="github-codespaces" class="anchor"> GitHub Codespaces </a>

Um [codespace](https://docs.github.com/en/codespaces/overview) é um ambiente de desenvolvimento na nuvem. Codespaces rodam em uma variedade de opções de computação baseadas em VM hospedadas no GitHub.com, em que você pode configurar de 2 até 32 core machines. Você pode conectar no seu codespace a partir do navegador ou usando localmente o Visual Studio Code.

> **Dica:** Se você fizer alguma mudança no seu dev container depois de tê-lo buildado e conectado ao seu codespace, garanta de rodar **Codespaces: Rebuild Container** no Command Palette (<kbd>cmd/ctrl</kbd>+<kbd>shift</kbd>+<kbd>p</kbd> ou <kbd>F1</kbd>) para pegar qualquer mudança que você tenha feito.

#### <a href="#codespaces-specific-properties" name="codespaces-specific-properties" class="anchor"> Propriedades específicas do Produto </a>

GitHub Codespaces funciona integrado com um número crescente de ferramentas e, onde aplicável, suas propriedades `devcontainer.json`. Por exemplo, conectando via web editor ou VS COde te habilita o uso de [propriedades VS Code](#visual-studio-code).

Se os Codespaces do seu projeto precisam de permissões adicionais para outros repositórios, você pode configurar isso nas propriedades `repositories` e `permissions`. Você pode aprender mais sobre isso na [documentação do Codespaces](https://docs.github.com/en/codespaces/managing-your-codespaces/managing-repository-access-for-your-codespaces). Como outras ferramentas, propriedades específicas do Codespaces estão localizadas em um namespace `codespace` dentro da propriedade `customizations`. 

```json
"customizations": {
	// Configure properties specific to Codespaces.
	"codespaces": {
		"repositories": {
			"my_org/my_repo": {
				"permissions": {
					"issues": "write"
				}
			}
		}
	}
}
```

Você pode customzar quais arquivos são inicialmente abertos quando o codepsace é criado:
```json
"customizations": {
	// Configure properties specific to Codespaces.
	"codespaces": {
		"openFiles": [
			"README"
			"src/index.js"
		]
	}
}
```
Os *paths* são relativos à raiz do repositório. Eles serão aberto em ordem, com o primeiro arquivo ativo.

> **Note** que atualmente Codespaces lêem essas propriedades do `devcontainer.json`, não dos metadados da imagem. 

#### <a href="#codespaces-specific-limitations" name="codespaces-specific-limitations" class="anchor"> Limitações Específicas do Produto </a>

Algumas propriedades podem ser aplicadas diferentemente aos codespaces.

| Property or variable | Type | Description |
|----------|---------|----------------------|
| `mounts` | array | Codespaces ignora "bind" mounts com exceção do Docker socket. Montagem de volumes ainda são permitidas.|
| `forwardPorts` | array | Codespaces ainda não suportam a variação `"host:port"` dessa propriedade. |
| `portsAttributes` | object | Codespaces ainda não suportam a variação `"host:port"` dessa propriedade.|
| `shutdownAction` | enum | Não se aplica a Codespaces. |
| `${localEnv:VARIABLE_NAME}` | Any | Para Codespaces, o host está na nuvem ao invés da sua máquina local.|
| `customizations.codespaces` | object | Codespaces lêem essas propriedades do `devcontainer.json`, não dos metadados da imagem. |
| `hostRequirements` | object | Codespaces lêem essas propriedades do `devcontainer.json`, não dos metadados da imagem. |
{: .table .table-bordered .table-responsive}

### <a href="#codesandbox" name="codesandbox" class="anchor"> CodeSandbox </a>

[CodeSandbox](https://codesandbox.io/) fornece ambientes de desenvolvimento cloud rodando em uma arquitetura microVM. As especificações de VM começam em 2 vCPUs + 2GB de RAM por ambiente (free tier) e pode subir para 16 vCPUs + 32 GB de RAM.

Quando você importa um respositório do GitHub no CodeSandbox, ele vai automaticamente provisionar um ambiente dedicado para cada branch. Graças à *memory snapshotting*, o CodeSandbox então retoma e ramifica um ambiente em menos de dois segundos.

CodeSandbox oferece suporte a multiplos editores, então você pode codar usando o web editor do próprio CodeSandbox, VS Code, ou o app CodeSandbox no iOS. 

**Dica:** Depois de importar um repositório no CodeSandbox, você opde usar a UI para configurar o ambiente usando *dev containers*.

#### <a href="#codesandbox-specific-properties" name="codesandbox-specific-properties" class="anchor"> Propriedades específicas do Produto </a>
CodeSandbox tem suporte para qualquer linguagem de programação e suporta imagens Debian e Ubuntu-based.

Todas as propriedades específicas para o CodeSandbox se encontram em uma pasta `.codesandbox` na raiz. Tipicamente vai conter um arquivo `tasks.json`, que define os comandos a serem executados no startup ou com um click.

Mais detalhes sobre isso pode ser encontrado na [documentação](https://codesandbox.io/docs/learn/repositories/task) do CodeSandbox.

#### <a href="#codesandbox-specific-limitations" name="codesandbox-specific-limitations" class="anchor"> Limitações Específicas do Produto </a>

O CodeSandbox roda *dev containers* usando Podman *rootless* ao invés do Docker. CodeSandbox também usa o [devcontainers/cli](https://github.com/devcontainers/cli) para gerenciar *dev containers*. Então quaisquer limitações do Podman e do Dev Container CLI devem se aplicar ao CodeSandbox.

As seguintes propriedades se aplicam diferentemente ao CodeSandbox.

| Property or variable | Type | Description |
|----------|---------|----------------------|
| `forwardPorts` | array | CodeSandbox não precias dessa propriedade. Todas as portas abertas em *dev containers* serão automaticamente mapeadas para uma URL pública. |
| `portsAttributes` | object | CodeSandbox ainda não suporta essa propriedade. Portas são ligadas a tasks configuradas em `.codesandbox/tasks.json` e são atribuidas à task.|
| `otherPortsAttributes` | object | CodeSandbox ainda não suporta essa propriedade. |
| `remoteUser` | string | CodeSandbox atualmente ignora essa proriedade e a sobrescreve como `root`. CodeSandbox usa Podman rootless para rodar contêineres. Executar com usuário não-root remoto é o mesmo que rodar como usuário root remoto num Podman rootless, do ponto de vista da segurança. CodeSandbox planeja suportar isso no futuro.|
| `shutdownAction` | string | Não se aplica ao CodeSandbox. |
| `capAdd` | array | CodeSandbox não suporta adicionar recursos do docker. Como os contêineres rodam como usuário não-root, recursos que precisam de acesso de root não vão funcionar. |
| `features` | object | CodeSandbox automaticamente adiciona docker-cli ao contêiner e conecta ao socket do host. Funcionalidades como `docker-in-docker` e `docker-outside-of-docker` funcionarão um pouco diferente. Como o docker-cli e o socket do host são acessíveis do container, maior parte dos casos de uso devem funcionar como esperado. |
| `${localEnv:VARIABLE_NAME}` | Any | Para o CodeSandbox, o host está na nuvem ao invés da sua máquina local.|
| `hostRequirements` | object | CodeSandbox ainda não suporta essa propriedade. |
{: .table .table-bordered .table-responsive}

### <a href="#devpod" name="devpod" class="anchor"> DevPod </a>

O [DevPod](https://github.com/loft-sh/devpod) é uma ferramenta client-only para criar ambientes de desenvolvimento reproduzíveis baseado em um `devcontainer.json` em qualquer backend. Cada ambiente de desenvolvimento roda em um contêiner e é especificado através de um `devcontainer.json`. Através dos provedores do DevPod esses ambientes podem ser criados em qualquer backend, desde a máquina local, um cluster Kubernets, qualquer máquina remota acessível ou em uma VM na nuvem.  

### <a href="#schema" name="schema" class="anchor"> Schema </a>

Você pode explorar a spec da [implementação VS Code ](implementors/json_schema) do dev container.
