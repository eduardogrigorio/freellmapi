<div align="center">

> **Tradução para português (Brasil) do README original do projeto [FreeLLMAPI](https://github.com/tashfeenahmed/freellmapi).**
> Créditos: projeto, texto e imagens originais por [Tashfeen Ahmed (@tashfeenahmed)](https://github.com/tashfeenahmed) e pelos colaboradores listados abaixo, sob licença [MIT](./LICENSE). Esta é uma tradução não oficial; em caso de divergência, vale o [README original em inglês](https://github.com/tashfeenahmed/freellmapi#readme).

# FreeLLMAPI

**7,4 bilhões de tokens por mês. 34 provedores de LLM gratuitos. 635 endpoints de modelos gratuitos. Um único endpoint compatível com a OpenAI.**

Reúna os planos gratuitos de dezenas de provedores, além de endpoints personalizados compatíveis com a OpenAI (chat, embeddings, imagem e áudio), atrás de uma única API `/v1`. As chaves ficam armazenadas criptografadas. Um roteador escolhe o melhor modelo disponível para cada requisição, passa para o próximo provedor quando um deles atinge o limite de uso e acompanha o consumo por chave para que você permaneça abaixo de todos os limites dos planos gratuitos.

[![CI](https://github.com/tashfeenahmed/freellmapi/actions/workflows/ci.yml/badge.svg)](https://github.com/tashfeenahmed/freellmapi/actions/workflows/ci.yml)
[![GitHub stars](https://img.shields.io/github/stars/tashfeenahmed/freellmapi?style=flat&logo=github&color=yellow)](https://github.com/tashfeenahmed/freellmapi/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](#contribuindo)
[![Docker image](https://img.shields.io/badge/ghcr.io-freellmapi-2496ED?logo=docker&logoColor=white)](https://github.com/tashfeenahmed/freellmapi/pkgs/container/freellmapi)
[![Ask DeepWiki](https://img.shields.io/badge/DeepWiki-Ask-blue)](https://deepwiki.com/tashfeenahmed/freellmapi)

**[freellmapi.co](https://freellmapi.co/?utm_source=github&utm_medium=readme&utm_campaign=repository&utm_content=readme_top)** · explore o catálogo completo: 474 famílias de modelos, 635 endpoints gratuitos

[English](README.md) · [简体中文](README.zh-cn.md) · [Русский](README.ru.md) · **Português (Brasil)**

<p align="center">
  <a href="https://github.com/tashfeenahmed/freellmapi/releases/latest"><img src="repo-assets/badges/macos.svg" height="48" alt="Baixar para macOS"></a>
  <a href="https://github.com/tashfeenahmed/freellmapi/releases/latest"><img src="repo-assets/badges/windows.svg" height="48" alt="Baixar para Windows"></a>
  <a href="docs/en/install/01-install.md#docker-compose"><img src="repo-assets/badges/docker.svg" height="48" alt="Hospede você mesmo com Docker"></a>
  <a href="https://play.google.com/store/apps/details?id=co.freellmapi.app"><img src="repo-assets/badges/play-store.svg" height="48" alt="Disponível no Google Play"></a>
  <a href="https://apps.apple.com/app/id6804648934"><img src="repo-assets/badges/app-store.svg" height="48" alt="Baixar na App Store"></a>
</p>

![Painel do FreeLLMAPI — página Modelos com o orçamento mensal de tokens](repo-assets/github-hero.png)


Seu roteador atualiza o próprio catálogo de modelos a partir de um feed assinado: novos modelos gratuitos, mudanças de cota e correções de compatibilidade chegam sem precisar de um `git pull`. As instalações gratuitas recebem o snapshot mensal, então um modelo chega até elas 30 dias depois de entrar no feed ao vivo; os roteadores premium o recebem no mesmo dia.
**[Entre no ar em freellmapi.co](https://freellmapi.co/?utm_source=github&utm_medium=readme&utm_campaign=premium&utm_content=readme_top#pricing)** (US$ 19/ano, cancele quando quiser).

</div>

---

## Sumário

- [Por que isto existe](#por-que-isto-existe)
- [Provedores suportados](#provedores-suportados)
- [CLIs e agentes de código compatíveis](#clis-e-agentes-de-código-compatíveis)
- [Como se compara](#como-se-compara)
- [Recursos](#recursos)
- [Início rápido](#início-rápido)
- [Aplicativo desktop](#aplicativo-desktop)
- [Funciona com clientes compatíveis com a OpenAI](#funciona-com-clientes-compatíveis-com-a-openai)
- [Idiomas](#idiomas)
- [Premium (catálogo ao vivo)](#premium-catálogo-ao-vivo)
- [Usando a API](#usando-a-api)
- [Capturas de tela](#capturas-de-tela)
- [Como funciona](#como-funciona)
- [Perguntas frequentes](#perguntas-frequentes)
- [Limitações](#limitações)
- [Contribuindo](#contribuindo)
- [Aviso legal](#aviso-legal)

**Guias** (em inglês): [Instalação e deploy](docs/en/install/01-install.md) · [Referência da API](docs/en/api/01-rest-api.md) · [Clientes e agentes de código](docs/en/clients/01-agent-clients.md) · [Compressão de prompts](docs/en/compression/01-compression-pipeline.md) · [Arquitetura e internos](docs/en/architecture/00-high-level-index.md) · [Índice da documentação](docs/en/README.md) · [Guia do colaborador](CONTRIBUTING.md)

## Por que isto existe

Todo laboratório de IA sério oferece hoje um plano gratuito: alguns milhões de tokens por mês, alguns milhares de requisições por dia. Isoladamente, cada plano é um brinquedo. Somados, chegam a cerca de **7,4 bilhões de tokens por mês** de capacidade de inferência utilizável, distribuídos em **474 famílias de modelos / 635 endpoints de provedores**, de pequenos e rápidos a razoavelmente capazes.

O problema é que combiná-los à mão é doloroso: trinta e quatro SDKs diferentes, trinta e quatro limites de uso diferentes, trinta e quatro lugares onde uma requisição pode falhar. O FreeLLMAPI condensa tudo isso em um único endpoint compatível com a OpenAI. Aponte qualquer biblioteca cliente da OpenAI para o seu servidor local e ele roteia de forma transparente entre os provedores para os quais você adicionou chaves.

Além disso, o cenário dos planos gratuitos muda toda semana: provedores lançam modelos, aposentam outros e alteram cotas sem aviso. O FreeLLMAPI acompanha tudo isso por você. O roteador baixa sozinho um catálogo de modelos assinado de [freellmapi.co](https://freellmapi.co), de modo que sua instalação se mantém atualizada sem `git pull`. Veja [Premium (catálogo ao vivo)](#premium-catálogo-ao-vivo) para saber com que rapidez ela acompanha.

![O plano gratuito, empilhado — ~7,4 bi de tokens de inferência gratuita por mês em 34 provedores](repo-assets/free-tier.png)

## Provedores suportados

<div align="center">
<table>
<tr>
<td align="center" width="150"><img src="repo-assets/providers/google.png" width="44" alt="Google"><br/><b>Google</b></td>
<td align="center" width="150"><picture><source media="(prefers-color-scheme: dark)" srcset="repo-assets/providers/groq-dark.png"><img src="repo-assets/providers/groq.png" width="44" alt="Groq"></picture><br/><b>Groq</b></td>
<td align="center" width="150"><img src="repo-assets/providers/cerebras.png" width="44" alt="Cerebras"><br/><b>Cerebras</b></td>
<td align="center" width="150"><picture><source media="(prefers-color-scheme: dark)" srcset="repo-assets/providers/opencode-dark.png"><img src="repo-assets/providers/opencode.png" width="44" alt="OpenCode Zen"></picture><br/><b>OpenCode Zen</b></td>
</tr>
<tr>
<td align="center"><img src="repo-assets/providers/mistral.png" width="44" alt="Mistral"><br/><b>Mistral</b></td>
<td align="center"><img src="repo-assets/providers/openrouter.png" width="44" alt="OpenRouter"><br/><b>OpenRouter</b></td>
<td align="center"><img src="repo-assets/providers/cloudflare.png" width="44" alt="Cloudflare"><br/><b>Cloudflare</b></td>
<td align="center"><img src="repo-assets/providers/cohere.png" width="44" alt="Cohere"><br/><b>Cohere</b></td>
</tr>
<tr>
<td align="center"><img src="repo-assets/providers/zhipu.png" width="44" alt="Z.ai (Zhipu)"><br/><b>Z.ai (Zhipu)</b></td>
<td align="center"><img src="repo-assets/providers/nvidia.png" width="44" alt="NVIDIA"><br/><b>NVIDIA</b></td>
<td align="center"><img src="repo-assets/providers/huggingface.png" width="44" alt="HuggingFace"><br/><b>HuggingFace</b></td>
</tr>
<tr>
<td align="center"><a href="https://modelscope.cn"><b>ModelScope</b><br/>Qwen3 · DeepSeek V4 · GLM-5 (requer vínculo com a Aliyun na China)</a></td>
</tr>
</table>

<i>… e mais 22 provedores gratuitos</i>

</div>

Há também um provedor **personalizado** — aponte modelos de chat, embeddings, imagem ou áudio para qualquer endpoint compatível com a OpenAI (llama.cpp, LM Studio, vLLM, um Ollama local ou um gateway remoto) pela página Chaves.

A lista completa e sempre atualizada está em **[freellmapi.co/models](https://freellmapi.co/models.html)**, com limites de uso por modelo, janelas de contexto e orçamentos de tokens gratuitos.

## CLIs e agentes de código compatíveis

<div align="center">
<table>
<tr>
<td align="center" width="150"><img src="repo-assets/agents/claude-code.png" width="44" alt="Claude Code"><br/><b>Claude Code</b></td>
<td align="center" width="150"><img src="repo-assets/agents/codex.png" width="44" alt="Codex CLI"><br/><b>Codex CLI</b></td>
<td align="center" width="150"><img src="repo-assets/agents/gemini-cli.png" width="44" alt="Gemini CLI"><br/><b>Gemini CLI</b></td>
<td align="center" width="150"><img src="repo-assets/agents/cursor.png" width="44" alt="Cursor"><br/><b>Cursor</b></td>
</tr>
<tr>
<td align="center"><img src="repo-assets/agents/cline.png" width="44" alt="Cline"><br/><b>Cline</b></td>
<td align="center"><img src="repo-assets/agents/roo-code.png" width="44" alt="Roo Code"><br/><b>Roo Code</b></td>
<td align="center"><img src="repo-assets/agents/opencode.png" width="44" alt="OpenCode"><br/><b>OpenCode</b></td>
<td align="center"><img src="repo-assets/agents/aider.png" width="44" alt="Aider"><br/><b>Aider</b></td>
</tr>
</table>

<i>… além de Continue, Goose, Qwen Code, Kilo Code, Crush, Zed, JetBrains AI, DeepSeek Harness, MiMo Code, AtomCode, OpenClaw, Hermes Agent, Pi, Reasonix, ZCode e qualquer cliente compatível com OpenAI, Anthropic, Gemini ou Ollama</i>

</div>

A maioria dos agentes se configura sozinha com um único comando que lê o seu catálogo ao vivo, faz backup da configuração existente e a mescla:

```bash
npx freellmapi setup-claude --url http://localhost:3001 --api-key <unified-key>
```

**[Todos os agentes suportados e seus comandos de configuração →](docs/en/clients/02-supported-agents.md)** · [Receitas por ferramenta, servidor MCP e tokens de URL →](docs/en/clients/01-agent-clients.md)

## Como se compara

![Comparação de recursos com OpenRouter, LiteLLM e Portkey](repo-assets/comparison.png)

Com base na documentação pública, julho de 2026 — correções são bem-vindas.

## Recursos

![Visão geral dos recursos](repo-assets/features.png)

- **Todas as superfícies no estilo OpenAI** — `/v1/chat/completions`, `/v1/responses` (necessário para o Codex CLI), `/v1/completions` (autocompletar "texto fantasma" em editores), `/v1/images/generations`, `/v1/videos/generations`, `/v1/audio/speech`, `/v1/audio/transcriptions`, `/v1/embeddings` e `/v1/models` — com e sem streaming, a partir dos SDKs oficiais ou de qualquer cliente compatível com a OpenAI. [Referência da API →](docs/en/api/01-rest-api.md)
- **API Anthropic Messages** — `/v1/messages` fala o formato de comunicação da Anthropic sobre o mesmo roteador, de modo que o **Claude Code** e os SDKs oficiais da Anthropic funcionam com o seu pool gratuito. [Detalhes →](docs/en/api/01-rest-api.md#anthropic--claude-clients)
- **Superfícies nativas do Gemini e do Ollama** — o Gemini CLI pode usar `/v1beta` (`generateContent`, streaming, contagem de tokens, modelos), enquanto a emulação opcional do Ollama serve chat/generate em NDJSON, tags, metadados e embeddings para Zed, JetBrains e outros clientes de modelos locais.
- **Fusion (síntese multimodelo)** — solicite o modelo virtual `fusion` e o roteador distribui seu prompt em paralelo para um painel de modelos gratuitos diversos; em seguida, um modelo juiz sintetiza uma única resposta a partir dos rascunhos. [Detalhes →](docs/en/api/01-rest-api.md#fusion-multi-model-synthesis)
- **Geração de imagem, vídeo e fala** — `/v1/images/generations`, `/v1/videos/generations` e `/v1/audio/speech` roteiam entre os provedores que oferecem modelos de mídia; imagens e fala também aceitam endpoints de mídia personalizados compatíveis com a OpenAI. Os jobs de vídeo são normalizados entre provedores síncronos e com fila e retornam um MP4 concluído.
- **Chamada de ferramentas e saídas estruturadas** — o `tools` no estilo OpenAI funciona entre provedores (chamadas de ferramenta em texto simples são resgatadas e convertidas em `tool_calls` reais), além de `response_format`, `seed`, `logprobs`, penalidades e os demais parâmetros de amostragem repassados por provedor.
- **Roteamento inteligente, seis estratégias** — pontuações ao vivo de velocidade, capacidade e confiabilidade por modelo ordenam sua cadeia; o fallover automático tenta o próximo modelo em caso de 429/5xx, com tempos de espera (cooldowns) e rotação de chaves. [Roteamento em detalhes →](docs/en/architecture/00-high-level-index.md#how-it-works)
- **Modelos unificados e perfis** — o mesmo modelo em vários provedores se funde em uma única entrada com failover estrito dentro do grupo; perfis de cadeia de fallback nomeados (uma cadeia para código, outra para visão) são alternados pelo painel ou por requisição via `auto:<perfil>`, e as cadeias personalizadas podem ser renomeadas diretamente no gerenciador de cadeias.
- **Controle de limites por chave** — contadores de RPM/RPD/TPM/TPD por `(plataforma, modelo, chave)` que aprendem os tetos informados pelos provedores, de modo que o roteamento sempre fica abaixo de todos os limites.
- **Catálogo de modelos com autoatualização** — o roteador sincroniza um catálogo assinado de freellmapi.co duas vezes por dia: novos modelos, mudanças de cota e correções de peculiaridades dos provedores chegam automaticamente. As instalações gratuitas acompanham o snapshot mensal, no qual cada modelo entra 30 dias depois de chegar ao feed ao vivo; os roteadores premium o recebem no mesmo dia. [Premium →](#premium-catálogo-ao-vivo)
- **Sessões fixas e transferência de contexto** — as conversas permanecem em um único modelo por 30 minutos; uma nota de transferência compacta e opcional mantém a conversa coerente quando ocorre uma troca no meio do chat. [Detalhes →](docs/en/clients/01-agent-clients.md#context-handoff)
- **Compressão de prompts (opcional)** — um pipeline de requisições compartilhado e tolerante a falhas pode deduplicar prompts, filtrar saídas de ferramentas, compactar JSON repetido e podar contexto antigo antes da consulta ao cache e do roteamento. [Detalhes →](docs/en/compression/01-compression-pipeline.md)
- **Chaves criptografadas, um único token de saída** — as chaves dos provedores são criptografadas com AES-256-GCM no SQLite e descriptografadas em memória a cada requisição; seus aplicativos só enxergam um único token bearer unificado `freellmapi-…`.
- **Painel administrativo e análises** — interface em React para gerenciar chaves, reordenar a cadeia, usar um playground e ler análises de p50/p95/TTFT em janelas de 24h a 90d; com login, temas claro/escuro e [60 idiomas](#idiomas).
- **Servidor MCP pronto para ChatGPT e documentação interativa** — o ChatGPT e outros clientes MCP podem chamar a inferência do FreeLLMAPI, listar modelos utilizáveis, inspecionar metadados de saúde/uso/cache/roteamento dos provedores e gerenciar a estratégia de roteamento via `/mcp`; um visualizador OpenAPI sem dependências fica em `/v1/docs`. [ChatGPT e agentes de código →](docs/en/clients/01-agent-clients.md#mcp-server)
- **Facilidades operacionais** — cache de respostas opcional, backups criptografados do banco de dados, verificações periódicas de saúde das chaves, importação/exportação de chaves em lote, configuração declarativa na inicialização. [Instalação e deploy →](docs/en/install/01-install.md)
- **Roda em qualquer lugar onde o Node 20+ rode** — Windows, macOS, servidores Linux ou uma pequena SBC ARM (incluindo Raspberry Pi). ~40 MB de RSS em repouso sob PM2 / systemd / o supervisor que você preferir.

O escopo é deliberadamente restrito — veja [o que ainda não é suportado](docs/en/architecture/00-high-level-index.md#not-yet-supported).

## Início rápido

**Comando único** (requer Docker — configura `~/freellmapi`, gera uma chave de criptografia, baixa a imagem e inicia o contêiner):

```bash
curl -fsSL https://freellmapi.co/install.sh | bash
```

Prefere ler antes de enviar para o bash? [O script está aqui](https://freellmapi.co/install.sh). Executá-lo novamente é seguro: seu `.env` (e a chave de criptografia) é preservado e o contêiner é atualizado para `:latest`.

Abra http://localhost:3001, adicione as chaves dos seus provedores na página **Chaves**, reordene a **Cadeia de Fallback** ao seu gosto e pegue sua chave de API unificada no cabeçalho da página **Chaves**. Essa chave unificada é para onde você aponta o seu SDK da OpenAI.

No Windows, o caminho mais fácil é o **[instalador `.exe` da área de Releases](https://github.com/tashfeenahmed/freellmapi/releases/latest)** do aplicativo desktop (abaixo). No Android, veja o [guia do Termux](docs/en/install/02-android-termux.md) (experimental).

Todo o resto — Docker Compose, desenvolvimento local, configuração declarativa na inicialização, builds de produção, acesso pela LAN e backups — está em **[docs/en/install/01-install.md](docs/en/install/01-install.md)**.

## Aplicativo desktop

Um aplicativo nativo de barra de menus fica em [`desktop/`](./desktop): o roteador e o painel inteiros rodando localmente a partir da bandeja do sistema, com um popover de vidro mostrando estatísticas de requisições ao vivo.

![Aplicativo desktop do FreeLLMAPI](repo-assets/desktop.png)

**[Baixe em Releases](https://github.com/tashfeenahmed/freellmapi/releases/latest)** — o `.dmg` para macOS e o instalador `.exe` para Windows acompanham cada versão. Não há conta nem senha para configurar: a única credencial de que você precisa é a chave de API unificada exibida no popover da bandeja. Passos para compilar a partir do código-fonte e onde ficam seus dados: [docs/en/install/01-install.md#desktop-app](docs/en/install/01-install.md#desktop-app).

Para macOS 12 Monterey ou posterior, escolha **arm64 (Apple Silicon)** ou **x64 (Intel)**. Ambas as versões para Mac também têm download em ZIP.

## Funciona com clientes compatíveis com a OpenAI

Qualquer coisa que consiga apontar para uma URL base compatível com a OpenAI funciona: defina-a como `http://localhost:3001/v1` com a chave unificada do painel. Todo gerador aceita `--dry-run`; `npx freellmapi launch` (Claude Code) e `launch-codex` (Codex) mantêm as credenciais totalmente fora dos arquivos de configuração. O roteador também funciona como servidor MCP que seus agentes podem consultar durante a sessão.

O **ChatGPT** se conecta ao mesmo roteador local por meio de um Secure MCP Tunnel privado. A ferramenta MCP
`ask_freellmapi` roteia uma tarefa solicitada pelo ChatGPT através de `/v1/chat/completions` e
retorna a resposta com metadados de modelo atendido, fallback, cache, execução e tokens. A URL
base e a autenticação bearer ficam na configuração do cliente do túnel; o modelo padrão é
definido por `MCP_INFERENCE_DEFAULT_MODEL` (`auto` quando não definido). Nenhuma chave deve ficar no Git ou na
URL do MCP. **[Configuração do ChatGPT →](docs/en/clients/01-agent-clients.md#chatgpt-private-secure-mcp-tunnel)**

As chaves dos provedores também podem ser gerenciadas pelo terminal, com um token de sessão do painel (`FREELLMAPI_DASHBOARD_TOKEN` ou `--token`): `npx freellmapi keys add|list|remove|test <plataforma>`, em que `keys test` verifica novamente as chaves armazenadas. Veja [cli/README.md](cli/README.md#provider-keys).

O FreeLLMAPI é local-first e de usuário único por design. Suas chaves de provedores ficam no seu banco SQLite, criptografadas em repouso, e as requisições vão da sua máquina diretamente para os provedores que você habilitou.

## Idiomas

O painel está disponível em **60 idiomas** (o menu da bandeja do desktop em 6). A interface
detecta automaticamente o idioma do navegador/sistema no primeiro acesso e você pode trocar a qualquer
momento em **⋯ → Configurações**; a escolha é lembrada. Os idiomas escritos da direita para a esquerda
(العربية, עברית, فارسی, اردو) invertem todo o layout automaticamente, e apenas o
dicionário do idioma ativo é carregado — os demais nunca consomem sua banda.

<img src="https://flagcdn.com/24x18/us.png" srcset="https://flagcdn.com/48x36/us.png 2x" width="24" height="18" alt="United States" title="United States"> <img src="https://flagcdn.com/24x18/cn.png" srcset="https://flagcdn.com/48x36/cn.png 2x" width="24" height="18" alt="China" title="China"> <img src="https://flagcdn.com/24x18/es.png" srcset="https://flagcdn.com/48x36/es.png 2x" width="24" height="18" alt="Spain" title="Spain"> <img src="https://flagcdn.com/24x18/fr.png" srcset="https://flagcdn.com/48x36/fr.png 2x" width="24" height="18" alt="France" title="France"> <img src="https://flagcdn.com/24x18/br.png" srcset="https://flagcdn.com/48x36/br.png 2x" width="24" height="18" alt="Brazil" title="Brazil"> <img src="https://flagcdn.com/24x18/it.png" srcset="https://flagcdn.com/48x36/it.png 2x" width="24" height="18" alt="Italy" title="Italy"> <img src="https://flagcdn.com/24x18/in.png" srcset="https://flagcdn.com/48x36/in.png 2x" width="24" height="18" alt="India" title="India"> <img src="https://flagcdn.com/24x18/sa.png" srcset="https://flagcdn.com/48x36/sa.png 2x" width="24" height="18" alt="Saudi Arabia" title="Saudi Arabia"> <img src="https://flagcdn.com/24x18/bd.png" srcset="https://flagcdn.com/48x36/bd.png 2x" width="24" height="18" alt="Bangladesh" title="Bangladesh"> <img src="https://flagcdn.com/24x18/ru.png" srcset="https://flagcdn.com/48x36/ru.png 2x" width="24" height="18" alt="Russia" title="Russia"> <img src="https://flagcdn.com/24x18/pk.png" srcset="https://flagcdn.com/48x36/pk.png 2x" width="24" height="18" alt="Pakistan" title="Pakistan"> <img src="https://flagcdn.com/24x18/id.png" srcset="https://flagcdn.com/48x36/id.png 2x" width="24" height="18" alt="Indonesia" title="Indonesia"> <img src="https://flagcdn.com/24x18/de.png" srcset="https://flagcdn.com/48x36/de.png 2x" width="24" height="18" alt="Germany" title="Germany"> <img src="https://flagcdn.com/24x18/jp.png" srcset="https://flagcdn.com/48x36/jp.png 2x" width="24" height="18" alt="Japan" title="Japan"> <img src="https://flagcdn.com/24x18/ke.png" srcset="https://flagcdn.com/48x36/ke.png 2x" width="24" height="18" alt="Kenya" title="Kenya"> <img src="https://flagcdn.com/24x18/tr.png" srcset="https://flagcdn.com/48x36/tr.png 2x" width="24" height="18" alt="Türkiye" title="Türkiye"> <img src="https://flagcdn.com/24x18/vn.png" srcset="https://flagcdn.com/48x36/vn.png 2x" width="24" height="18" alt="Vietnam" title="Vietnam"> <img src="https://flagcdn.com/24x18/kr.png" srcset="https://flagcdn.com/48x36/kr.png 2x" width="24" height="18" alt="South Korea" title="South Korea"> <img src="https://flagcdn.com/24x18/ir.png" srcset="https://flagcdn.com/48x36/ir.png 2x" width="24" height="18" alt="Iran" title="Iran"> <img src="https://flagcdn.com/24x18/th.png" srcset="https://flagcdn.com/48x36/th.png 2x" width="24" height="18" alt="Thailand" title="Thailand"> <img src="https://flagcdn.com/24x18/pl.png" srcset="https://flagcdn.com/48x36/pl.png 2x" width="24" height="18" alt="Poland" title="Poland"> <img src="https://flagcdn.com/24x18/ua.png" srcset="https://flagcdn.com/48x36/ua.png 2x" width="24" height="18" alt="Ukraine" title="Ukraine"> <img src="https://flagcdn.com/24x18/mm.png" srcset="https://flagcdn.com/48x36/mm.png 2x" width="24" height="18" alt="Myanmar" title="Myanmar"> <img src="https://flagcdn.com/24x18/ro.png" srcset="https://flagcdn.com/48x36/ro.png 2x" width="24" height="18" alt="Romania" title="Romania"> <img src="https://flagcdn.com/24x18/nl.png" srcset="https://flagcdn.com/48x36/nl.png 2x" width="24" height="18" alt="Netherlands" title="Netherlands"> <img src="https://flagcdn.com/24x18/my.png" srcset="https://flagcdn.com/48x36/my.png 2x" width="24" height="18" alt="Malaysia" title="Malaysia"> <img src="https://flagcdn.com/24x18/ph.png" srcset="https://flagcdn.com/48x36/ph.png 2x" width="24" height="18" alt="Philippines" title="Philippines"> <img src="https://flagcdn.com/24x18/ng.png" srcset="https://flagcdn.com/48x36/ng.png 2x" width="24" height="18" alt="Nigeria" title="Nigeria"> <img src="https://flagcdn.com/24x18/et.png" srcset="https://flagcdn.com/48x36/et.png 2x" width="24" height="18" alt="Ethiopia" title="Ethiopia"> <img src="https://flagcdn.com/24x18/uz.png" srcset="https://flagcdn.com/48x36/uz.png 2x" width="24" height="18" alt="Uzbekistan" title="Uzbekistan"> <img src="https://flagcdn.com/24x18/az.png" srcset="https://flagcdn.com/48x36/az.png 2x" width="24" height="18" alt="Azerbaijan" title="Azerbaijan"> <img src="https://flagcdn.com/24x18/lk.png" srcset="https://flagcdn.com/48x36/lk.png 2x" width="24" height="18" alt="Sri Lanka" title="Sri Lanka"> <img src="https://flagcdn.com/24x18/np.png" srcset="https://flagcdn.com/48x36/np.png 2x" width="24" height="18" alt="Nepal" title="Nepal"> <img src="https://flagcdn.com/24x18/kh.png" srcset="https://flagcdn.com/48x36/kh.png 2x" width="24" height="18" alt="Cambodia" title="Cambodia"> <img src="https://flagcdn.com/24x18/gr.png" srcset="https://flagcdn.com/48x36/gr.png 2x" width="24" height="18" alt="Greece" title="Greece"> <img src="https://flagcdn.com/24x18/cz.png" srcset="https://flagcdn.com/48x36/cz.png 2x" width="24" height="18" alt="Czechia" title="Czechia"> <img src="https://flagcdn.com/24x18/hu.png" srcset="https://flagcdn.com/48x36/hu.png 2x" width="24" height="18" alt="Hungary" title="Hungary"> <img src="https://flagcdn.com/24x18/se.png" srcset="https://flagcdn.com/48x36/se.png 2x" width="24" height="18" alt="Sweden" title="Sweden"> <img src="https://flagcdn.com/24x18/il.png" srcset="https://flagcdn.com/48x36/il.png 2x" width="24" height="18" alt="Israel" title="Israel"> <img src="https://flagcdn.com/24x18/dk.png" srcset="https://flagcdn.com/48x36/dk.png 2x" width="24" height="18" alt="Denmark" title="Denmark"> <img src="https://flagcdn.com/24x18/fi.png" srcset="https://flagcdn.com/48x36/fi.png 2x" width="24" height="18" alt="Finland" title="Finland"> <img src="https://flagcdn.com/24x18/no.png" srcset="https://flagcdn.com/48x36/no.png 2x" width="24" height="18" alt="Norway" title="Norway"> <img src="https://flagcdn.com/24x18/sk.png" srcset="https://flagcdn.com/48x36/sk.png 2x" width="24" height="18" alt="Slovakia" title="Slovakia"> <img src="https://flagcdn.com/24x18/bg.png" srcset="https://flagcdn.com/48x36/bg.png 2x" width="24" height="18" alt="Bulgaria" title="Bulgaria"> <img src="https://flagcdn.com/24x18/hr.png" srcset="https://flagcdn.com/48x36/hr.png 2x" width="24" height="18" alt="Croatia" title="Croatia"> <img src="https://flagcdn.com/24x18/rs.png" srcset="https://flagcdn.com/48x36/rs.png 2x" width="24" height="18" alt="Serbia" title="Serbia"> <img src="https://flagcdn.com/24x18/lt.png" srcset="https://flagcdn.com/48x36/lt.png 2x" width="24" height="18" alt="Lithuania" title="Lithuania"> <img src="https://flagcdn.com/24x18/tw.png" srcset="https://flagcdn.com/48x36/tw.png 2x" width="24" height="18" alt="Taiwan" title="Taiwan"> <img src="https://flagcdn.com/24x18/pt.png" srcset="https://flagcdn.com/48x36/pt.png 2x" width="24" height="18" alt="Portugal" title="Portugal"> <img src="https://flagcdn.com/24x18/ge.png" srcset="https://flagcdn.com/48x36/ge.png 2x" width="24" height="18" alt="Georgia" title="Georgia">

A lista completa de idiomas está em
[`client/src/i18n/locale-config.ts`](./client/src/i18n/locale-config.ts).

Os seis idiomas originais foram revisados por humanos; os mais novos foram traduzidos
automaticamente e melhoram conforme falantes nativos enviam correções — um PR de uma única
string é uma ótima primeira contribuição.

As traduções ficam em [`client/src/i18n/locales/`](./client/src/i18n/locales) como
arquivos JSON simples. Para corrigir uma string, edite o valor no arquivo JSON do idioma. Para
adicionar um idioma, copie `en.json`, traduza os valores e registre o idioma em
`client/src/i18n/locale-config.ts` (e em `desktop/src/i18n.ts` para as strings da
bandeja); `npm test` verifica a paridade de chaves/placeholders em todos os idiomas — PRs
são bem-vindos.

## Premium (catálogo ao vivo)

O roteador mantém seu catálogo de modelos atualizado por conta própria: ele baixa um catálogo
assinado de [freellmapi.co](https://freellmapi.co) duas vezes por dia e aplica novos modelos,
mudanças de cota e correções de peculiaridades dos provedores ao seu banco de dados local. Suas próprias
escolhas de habilitar/desabilitar e seus provedores personalizados nunca são alterados, e cada
download é verificado com uma chave Ed25519 fixada antes de ser aplicado.

O catálogo acompanha atualmente **34 provedores**, **474 famílias de modelos**, **635
endpoints provedor/modelo gratuitos** (584 de chat, 41 de embeddings, 7 de transcrição, 3 de
vídeo) e cerca de **7,4 bilhões de tokens por mês** de capacidade gratuita listada. Explore o conjunto completo em
**[freellmapi.co/models](https://freellmapi.co/models.html)**.

As instalações gratuitas baixam o mesmo catálogo assinado, mas do snapshot mensal: um
modelo entra nele 30 dias depois de chegar ao feed ao vivo, então a versão gratuita
está atualmente cerca de 303 modelos atrás. Nada expira e nada é limitado — apenas
chega mais tarde.

O Premium mantém esse catálogo assinado ao vivo em cada roteador que você executa. Quando um provedor
lança um bom modelo gratuito, aperta discretamente uma cota ou quebra um formato de comunicação,
os roteadores no feed ao vivo recebem a atualização no mesmo dia em que a publicamos.

**[Entre no ar em freellmapi.co →](https://freellmapi.co/?utm_source=github&utm_medium=readme&utm_campaign=premium&utm_content=readme_bottom#pricing)**

- US$ 19/ano ou US$ 49 uma única vez, vitalício. Checkout via Stripe; cancele quando quiser, por conta própria.
- Uma chave `fla_` cobre todos os roteadores que você executa: desktop, homelab, Raspberry Pi.
- Ative no painel em **Premium**; cancele ou gerencie a cobrança por conta própria
  em [freellmapi.co/manage](https://freellmapi.co/manage).
- O roteador em si continua licenciado sob MIT e totalmente gratuito, para sempre. O Premium é apenas
  o feed ao vivo, e é ele que financia os testes diários de modelos e a manutenção do catálogo
  que mantêm o catálogo funcionando.

O servidor do catálogo nunca vê seus prompts, respostas ou chaves de provedores — o roteador
continua totalmente hospedado por você em qualquer dos casos.

## Usando a API

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:3001/v1",
    api_key="freellmapi-your-unified-key",
)

resp = client.chat.completions.create(
    model="auto",  # deixe o roteador escolher; ou "auto:fast", "auto:smart", um perfil, ou um id de modelo
    messages=[{"role": "user", "content": "Summarise the fall of Rome in one sentence."}],
)
print(resp.choices[0].message.content)
print("Routed via:", resp.headers.get("x-routed-via"))
```

Streaming, as estratégias de roteamento `auto:*`, chamada de ferramentas, entrada de visão, grounding com Google Search do Gemini, embeddings e a superfície Anthropic Messages — com exemplos em curl e Python para cada uma — estão em **[docs/en/api/01-rest-api.md](docs/en/api/01-rest-api.md)**. Toda resposta traz um cabeçalho `X-Routed-Via: <plataforma>/<modelo>` para que você veja qual provedor realmente a atendeu.

## Capturas de tela

### Modelos

Escolha uma estratégia de roteamento e veja o orçamento mensal de tokens se preencher em toda a frota de provedores. Cada modelo mostra pontuações ao vivo de confiabilidade, velocidade e inteligência — a ordem abaixo é como as requisições são roteadas agora.

![Página Modelos](repo-assets/models.png)

### Chaves

Gerencie as credenciais dos provedores e pegue a chave de API unificada com a qual seus aplicativos se conectam. Cada chave mostra um indicador de status e quando foi verificada pela última vez.

![Página Chaves](repo-assets/keys.png)

### Playground

Envie uma chat completion pelo roteador e veja qual provedor a atendeu, com o ID do modelo e a latência impressos na própria mensagem. Anexe arquivos por botão, arrastar e soltar ou colar: imagens (PNG/JPEG/WebP/GIF) são reduzidas no navegador e enviadas como partes de conteúdo de imagem a um modelo com capacidade de visão, e arquivos de texto (TXT/MD/CSV/JSON/LOG) são inseridos no prompt como blocos de código.

![Página Playground](repo-assets/playground.png)

### Análises

Volume de requisições, taxa de sucesso, tokens de entrada e saída, latência média e detalhamento por provedor em janelas de 24h / 7d / 30d / 90d.

![Página Análises](repo-assets/analytics.png)

## Como funciona

![Uma requisição entra, o melhor modelo gratuito sai — a cadeia de fallback com pontuações ao vivo, cooldowns e controle de cotas](repo-assets/router-flow.png)

Uma requisição entra, o melhor modelo gratuito sai: o roteador escolhe o modelo de maior prioridade com uma chave saudável que esteja abaixo de todos os seus limites, descriptografa a chave em memória e chama o provedor — em caso de 429/5xx, ele coloca essa chave em espera e tenta o próximo modelo da sua cadeia. O passo a passo dos componentes, os internos do roteamento e os detalhes operacionais estão em **[docs/en/architecture/00-high-level-index.md](docs/en/architecture/00-high-level-index.md)**.

## Perguntas frequentes

**Preciso de uma senha?** Não no aplicativo desktop — o painel faz login sozinho com uma conta local oculta, então não há nada para configurar nem para esquecer. Abra-o pelo ícone da bandeja → **Open Dashboard**, ou em qualquer navegador no mesmo computador (por exemplo http://127.0.0.1:31415); ambos entram automaticamente. Outro dispositivo que acesse o aplicativo pela LAN ainda vê o formulário de login. As instalações em servidor (Docker, comando único, `npm run dev`) têm uma conta com e-mail e senha.

**Esqueci a senha em uma instalação de servidor.** Clique em **Forgot password?** na página de login. Como não há e-mail para enviar um link, o código de uso único é impresso no log do servidor — leia-o com `docker compose logs -f freellmapi` (ou no terminal em que o servidor está rodando, ou no arquivo de log do desktop) e digite-o no formulário de redefinição. O código dura 15 minutos.

**Onde ficam os logs?** No log do contêiner no Docker, no terminal em uma execução a partir do código-fonte e em `<diretório de dados>/logs/freeapi.log` no aplicativo desktop — acessível pelo menu da bandeja em **Open Logs Folder**.

**Como desinstalo?** Remova o aplicativo (Lixeira no macOS, *Configurações → Aplicativos* no Windows, `docker compose down -v` no Docker) e depois apague o diretório de dados: `%APPDATA%\FreeLLMAPI\`, `~/Library/Application Support/FreeLLMAPI/` ou `~/.config/FreeLLMAPI/`. A desinstalação nunca mexe nessa pasta por conta própria.

Respostas mais longas, por método de instalação: **[docs/en/install/01-install.md#faq-passwords-logs-uninstall](docs/en/install/01-install.md#faq-passwords-logs-uninstall)**.

## Limitações

Empilhar planos gratuitos tem contrapartidas reais: sem modelos de ponta, latência variável, sem SLA — e a inteligência efetiva do endpoint cai no fim do dia conforme os melhores modelos atingem seus limites diários, voltando ao normal à meia-noite UTC. Leia a lista honesta em **[docs/en/architecture/00-high-level-index.md#limitations](docs/en/architecture/00-high-level-index.md#limitations)** antes de construir qualquer coisa séria em cima disso.

## Contribuindo

Contribuidores são muito bem-vindos! Veja [CONTRIBUTING.md](CONTRIBUTING.md) para o ciclo de desenvolvimento, as expectativas para PRs e a política sobre contribuições assistidas por IA/LLM (resumindo: são bem-vindas, com o mesmo padrão de qualidade de qualquer outro PR). Bons primeiros PRs:

- **Adicionar um provedor** — copie `server/src/providers/openai-compat.ts` como modelo, ligue-o em `server/src/providers/index.ts`, popule seus modelos em `server/src/db/index.ts` e adicione um teste em `server/src/__tests__/providers/`.
- **Adicionar um endpoint** — outras superfícies compatíveis com a OpenAI (por exemplo `/v1/realtime`). A classe base de provedor pode ganhar novos métodos; os adaptadores declaram quais suportam.
- **Melhorar o roteador** — roteamento sensível a custo (compromissos entre mais barato, saudável e rápido), melhor prioridade ponderada por latência, fixação regional.
- **Polimento do painel** — gráficos na página Análises, UX de rotação de chaves, importação em lote de chaves a partir de `.env`.
- **Documentação** — mais exemplos, trechos para bibliotecas cliente em Go/Rust/etc., uma receita de deploy para Docker ou Fly.

`npm install && npm run dev` coloca o servidor em :3001 e o painel em :5173, ambos com HMR. Para uma configuração repetível, use `./scripts/dev-bootstrap.sh` no Bash ou `.\scripts\dev-bootstrap.ps1` no PowerShell; cada um preserva um `.env` existente. Os PRs devem incluir um teste, manter a suíte existente verde (`npm test`) e seguir os padrões de `.editorconfig` / tsconfig já presentes no repositório. O fluxo de migração de banco de dados e o ciclo completo do colaborador estão em [CONTRIBUTING.md](./CONTRIBUTING.md).

### Colaboradores

Agradecimentos a todas as pessoas que construíram o FreeLLMAPI. Veja também o [gráfico oficial de colaboradores](https://github.com/tashfeenahmed/freellmapi/graphs/contributors).

<a href="https://github.com/moaaz12-web"><img src="https://images.weserv.nl/?url=github.com/moaaz12-web.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@moaaz12-web" /></a>
<a href="https://github.com/lukasulc"><img src="https://images.weserv.nl/?url=github.com/lukasulc.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@lukasulc" /></a>
<a href="https://github.com/VinhPhamAI"><img src="https://images.weserv.nl/?url=github.com/VinhPhamAI.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@VinhPhamAI" /></a>
<a href="https://github.com/deadc"><img src="https://images.weserv.nl/?url=github.com/deadc.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@deadc" /></a>
<a href="https://github.com/zhangyu1324"><img src="https://images.weserv.nl/?url=github.com/zhangyu1324.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@zhangyu1324" /></a>
<a href="https://github.com/kentpan"><img src="https://images.weserv.nl/?url=github.com/kentpan.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@kentpan" /></a>
<a href="https://github.com/stephenzwj"><img src="https://images.weserv.nl/?url=github.com/stephenzwj.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@stephenzwj" /></a>
<a href="https://github.com/chongjiazhen"><img src="https://images.weserv.nl/?url=github.com/chongjiazhen.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@chongjiazhen" /></a>
<a href="https://github.com/vjsai"><img src="https://images.weserv.nl/?url=github.com/vjsai.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@vjsai" /></a>
<a href="https://github.com/long2ice"><img src="https://images.weserv.nl/?url=github.com/long2ice.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@long2ice" /></a>
<a href="https://github.com/sadesguy"><img src="https://images.weserv.nl/?url=github.com/sadesguy.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@sadesguy" /></a>
<a href="https://github.com/hodlmybeer69-bit"><img src="https://images.weserv.nl/?url=github.com/hodlmybeer69-bit.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@hodlmybeer69-bit" /></a>
<a href="https://github.com/phoenixikkifullstack"><img src="https://images.weserv.nl/?url=github.com/phoenixikkifullstack.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@phoenixikkifullstack" /></a>
<a href="https://github.com/jtbrennan-git"><img src="https://images.weserv.nl/?url=github.com/jtbrennan-git.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@jtbrennan-git" /></a>
<a href="https://github.com/praveenkumarpranjal"><img src="https://images.weserv.nl/?url=github.com/praveenkumarpranjal.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@praveenkumarpranjal" /></a>
<a href="https://github.com/nordbyte"><img src="https://images.weserv.nl/?url=github.com/nordbyte.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@nordbyte" /></a>
<a href="https://github.com/mybropro"><img src="https://images.weserv.nl/?url=github.com/mybropro.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@mybropro" /></a>
<a href="https://github.com/danscMax"><img src="https://images.weserv.nl/?url=github.com/danscMax.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@danscMax" /></a>
<a href="https://github.com/jhash"><img src="https://images.weserv.nl/?url=github.com/jhash.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@jhash" /></a>
<a href="https://github.com/JammyJames1234"><img src="https://images.weserv.nl/?url=github.com/JammyJames1234.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@JammyJames1234" /></a>
<a href="https://github.com/coffcoe"><img src="https://images.weserv.nl/?url=github.com/coffcoe.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@coffcoe" /></a>
<a href="https://github.com/Sumit4codes"><img src="https://images.weserv.nl/?url=github.com/Sumit4codes.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Sumit4codes" /></a>
<a href="https://github.com/meliani"><img src="https://images.weserv.nl/?url=github.com/meliani.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@meliani" /></a>
<a href="https://github.com/thedavidweng"><img src="https://images.weserv.nl/?url=github.com/thedavidweng.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@thedavidweng" /></a>
<a href="https://github.com/bharvey42"><img src="https://images.weserv.nl/?url=github.com/bharvey42.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@bharvey42" /></a>
<a href="https://github.com/yuvrxj-afk"><img src="https://images.weserv.nl/?url=github.com/yuvrxj-afk.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@yuvrxj-afk" /></a>
<a href="https://github.com/Tushar49"><img src="https://images.weserv.nl/?url=github.com/Tushar49.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Tushar49" /></a>
<a href="https://github.com/nicyoong"><img src="https://images.weserv.nl/?url=github.com/nicyoong.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@nicyoong" /></a>
<a href="https://github.com/Aldo-f"><img src="https://images.weserv.nl/?url=github.com/Aldo-f.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Aldo-f" /></a>
<a href="https://github.com/Tazrif-Raim"><img src="https://images.weserv.nl/?url=github.com/Tazrif-Raim.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Tazrif-Raim" /></a>
<a href="https://github.com/m1nuzz"><img src="https://images.weserv.nl/?url=github.com/m1nuzz.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@m1nuzz" /></a>
<a href="https://github.com/suantea"><img src="https://images.weserv.nl/?url=github.com/suantea.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@suantea" /></a>
<a href="https://github.com/OhOkThisIsFine"><img src="https://images.weserv.nl/?url=github.com/OhOkThisIsFine.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@OhOkThisIsFine" /></a>
<a href="https://github.com/LoneRifle"><img src="https://images.weserv.nl/?url=github.com/LoneRifle.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@LoneRifle" /></a>
<a href="https://github.com/ita333"><img src="https://images.weserv.nl/?url=github.com/ita333.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@ita333" /></a>
<a href="https://github.com/barbotkonv"><img src="https://images.weserv.nl/?url=github.com/barbotkonv.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@barbotkonv" /></a>
<a href="https://github.com/Naster17"><img src="https://images.weserv.nl/?url=github.com/Naster17.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Naster17" /></a>
<a href="https://github.com/StealthTensor"><img src="https://images.weserv.nl/?url=github.com/StealthTensor.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@StealthTensor" /></a>
<a href="https://github.com/EmranAhmed"><img src="https://images.weserv.nl/?url=github.com/EmranAhmed.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@EmranAhmed" /></a>
<a href="https://github.com/itsfuad"><img src="https://images.weserv.nl/?url=github.com/itsfuad.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@itsfuad" /></a>
<a href="https://github.com/RobinHoodO"><img src="https://images.weserv.nl/?url=github.com/RobinHoodO.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@RobinHoodO" /></a>
<a href="https://github.com/hmm183"><img src="https://images.weserv.nl/?url=github.com/hmm183.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@hmm183" /></a>
<a href="https://github.com/duemilionidieuro-bot"><img src="https://images.weserv.nl/?url=github.com/duemilionidieuro-bot.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@duemilionidieuro-bot" /></a>
<a href="https://github.com/cagedbird043"><img src="https://images.weserv.nl/?url=github.com/cagedbird043.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@cagedbird043" /></a>
<a href="https://github.com/jasnoorgill"><img src="https://images.weserv.nl/?url=github.com/jasnoorgill.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@jasnoorgill" /></a>
<a href="https://github.com/Joey9024"><img src="https://images.weserv.nl/?url=github.com/Joey9024.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Joey9024" /></a>
<a href="https://github.com/AskingConical"><img src="https://images.weserv.nl/?url=github.com/AskingConical.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@AskingConical" /></a>
<a href="https://github.com/ProAlit"><img src="https://images.weserv.nl/?url=github.com/ProAlit.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@ProAlit" /></a>
<a href="https://github.com/hjhhoni"><img src="https://images.weserv.nl/?url=github.com/hjhhoni.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@hjhhoni" /></a>
<a href="https://github.com/immanuelsavio"><img src="https://images.weserv.nl/?url=github.com/immanuelsavio.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@immanuelsavio" /></a>
<a href="https://github.com/Slyker"><img src="https://images.weserv.nl/?url=github.com/Slyker.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Slyker" /></a>
<a href="https://github.com/wells1013"><img src="https://images.weserv.nl/?url=github.com/wells1013.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@wells1013" /></a>
<a href="https://github.com/evgkrsk"><img src="https://images.weserv.nl/?url=github.com/evgkrsk.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@evgkrsk" /></a>
<a href="https://github.com/aaronjmars"><img src="https://images.weserv.nl/?url=github.com/aaronjmars.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@aaronjmars" /></a>
<a href="https://github.com/Robs87"><img src="https://images.weserv.nl/?url=github.com/Robs87.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Robs87" /></a>
<a href="https://github.com/dashitongzhi"><img src="https://images.weserv.nl/?url=github.com/dashitongzhi.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@dashitongzhi" /></a>
<a href="https://github.com/QingJ01"><img src="https://images.weserv.nl/?url=github.com/QingJ01.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@QingJ01" /></a>
<a href="https://github.com/3215"><img src="https://images.weserv.nl/?url=github.com/3215.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@3215" /></a>
<a href="https://github.com/saifulaiub123"><img src="https://images.weserv.nl/?url=github.com/saifulaiub123.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@saifulaiub123" /></a>
<a href="https://github.com/PietFourie"><img src="https://images.weserv.nl/?url=github.com/PietFourie.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@PietFourie" /></a>
<a href="https://github.com/mhmdkrmabd"><img src="https://images.weserv.nl/?url=github.com/mhmdkrmabd.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@mhmdkrmabd" /></a>
<a href="https://github.com/DemeulemeesterxMaxime"><img src="https://images.weserv.nl/?url=github.com/DemeulemeesterxMaxime.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@DemeulemeesterxMaxime" /></a>
<a href="https://github.com/HoodBlah"><img src="https://images.weserv.nl/?url=github.com/HoodBlah.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@HoodBlah" /></a>
<a href="https://github.com/SeanPedersen"><img src="https://images.weserv.nl/?url=github.com/SeanPedersen.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@SeanPedersen" /></a>
<a href="https://github.com/andersmmg"><img src="https://images.weserv.nl/?url=github.com/andersmmg.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@andersmmg" /></a>
<a href="https://github.com/chirag127"><img src="https://images.weserv.nl/?url=github.com/chirag127.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@chirag127" /></a>
<a href="https://github.com/allababbot"><img src="https://images.weserv.nl/?url=github.com/allababbot.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@allababbot" /></a>
<a href="https://github.com/johan-droid"><img src="https://images.weserv.nl/?url=github.com/johan-droid.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@johan-droid" /></a>
<a href="https://github.com/redenfire"><img src="https://images.weserv.nl/?url=github.com/redenfire.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@redenfire" /></a>
<a href="https://github.com/itzpingcat"><img src="https://images.weserv.nl/?url=github.com/itzpingcat.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@itzpingcat" /></a>
<a href="https://github.com/kairwang01"><img src="https://images.weserv.nl/?url=github.com/kairwang01.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@kairwang01" /></a>
<a href="https://github.com/gongjurenzhangwei"><img src="https://images.weserv.nl/?url=github.com/gongjurenzhangwei.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@gongjurenzhangwei" /></a>
<a href="https://github.com/jsonring"><img src="https://images.weserv.nl/?url=github.com/jsonring.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@jsonring" /></a>
<a href="https://github.com/1029734570"><img src="https://images.weserv.nl/?url=github.com/1029734570.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@1029734570" /></a>
<a href="https://github.com/86TheCactus"><img src="https://images.weserv.nl/?url=github.com/86TheCactus.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@86TheCactus" /></a>
<a href="https://github.com/AmiroKD"><img src="https://images.weserv.nl/?url=github.com/AmiroKD.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@AmiroKD" /></a>
<a href="https://github.com/ecryptomillionaire-dev"><img src="https://images.weserv.nl/?url=github.com/ecryptomillionaire-dev.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@ecryptomillionaire-dev" /></a>
<a href="https://github.com/4riful"><img src="https://images.weserv.nl/?url=github.com/4riful.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@4riful" /></a>
<a href="https://github.com/fix2015"><img src="https://images.weserv.nl/?url=github.com/fix2015.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@fix2015" /></a>
<a href="https://github.com/iisyw"><img src="https://images.weserv.nl/?url=github.com/iisyw.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@iisyw" /></a>
<a href="https://github.com/xsfhacg"><img src="https://images.weserv.nl/?url=github.com/xsfhacg.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@xsfhacg" /></a>
<a href="https://github.com/noobix"><img src="https://images.weserv.nl/?url=github.com/noobix.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@noobix" /></a>
<a href="https://github.com/nandukmelath"><img src="https://images.weserv.nl/?url=github.com/nandukmelath.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@nandukmelath" /></a>
<a href="https://github.com/NirvanaCh7"><img src="https://images.weserv.nl/?url=github.com/NirvanaCh7.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@NirvanaCh7" /></a>
<a href="https://github.com/Mohamed3nan"><img src="https://images.weserv.nl/?url=github.com/Mohamed3nan.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Mohamed3nan" /></a>
<a href="https://github.com/Arman-Espiar"><img src="https://images.weserv.nl/?url=github.com/Arman-Espiar.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Arman-Espiar" /></a>
<a href="https://github.com/MetaMysteries8"><img src="https://images.weserv.nl/?url=github.com/MetaMysteries8.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@MetaMysteries8" /></a>
<a href="https://github.com/lujun880726"><img src="https://images.weserv.nl/?url=github.com/lujun880726.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@lujun880726" /></a>
<a href="https://github.com/qq97693453"><img src="https://images.weserv.nl/?url=github.com/qq97693453.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@qq97693453" /></a>
<a href="https://github.com/emv33"><img src="https://images.weserv.nl/?url=github.com/emv33.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@emv33" /></a>
<a href="https://github.com/ousamabenyounes"><img src="https://images.weserv.nl/?url=github.com/ousamabenyounes.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@ousamabenyounes" /></a>
<a href="https://github.com/yfdyh000"><img src="https://images.weserv.nl/?url=github.com/yfdyh000.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@yfdyh000" /></a>
<a href="https://github.com/s-uryansh"><img src="https://images.weserv.nl/?url=github.com/s-uryansh.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@s-uryansh" /></a>
<a href="https://github.com/arsalanyavari"><img src="https://images.weserv.nl/?url=github.com/arsalanyavari.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@arsalanyavari" /></a>
<a href="https://github.com/RoboMWM"><img src="https://images.weserv.nl/?url=github.com/RoboMWM.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@RoboMWM" /></a>
<a href="https://github.com/gaurang-py"><img src="https://images.weserv.nl/?url=github.com/gaurang-py.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@gaurang-py" /></a>
<a href="https://github.com/ddy4633"><img src="https://images.weserv.nl/?url=github.com/ddy4633.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@ddy4633" /></a>
<a href="https://github.com/UrbsKali"><img src="https://images.weserv.nl/?url=github.com/UrbsKali.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@UrbsKali" /></a>
<a href="https://github.com/hb-0"><img src="https://images.weserv.nl/?url=github.com/hb-0.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@hb-0" /></a>
<a href="https://github.com/xyblue135"><img src="https://images.weserv.nl/?url=github.com/xyblue135.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@xyblue135" /></a>
<a href="https://github.com/Icesenator"><img src="https://images.weserv.nl/?url=github.com/Icesenator.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Icesenator" /></a>
<a href="https://github.com/ZER0-auto"><img src="https://images.weserv.nl/?url=github.com/ZER0-auto.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@ZER0-auto" /></a>
<a href="https://github.com/tashdroid"><img src="https://images.weserv.nl/?url=github.com/tashdroid.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@tashdroid" /></a>
<a href="https://github.com/Patrickleondev"><img src="https://images.weserv.nl/?url=github.com/Patrickleondev.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Patrickleondev" /></a>
<a href="https://github.com/hiiamwaffledev"><img src="https://images.weserv.nl/?url=github.com/hiiamwaffledev.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@hiiamwaffledev" /></a>
<a href="https://github.com/w0fv1"><img src="https://images.weserv.nl/?url=github.com/w0fv1.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@w0fv1" /></a>
<a href="https://github.com/oppih"><img src="https://images.weserv.nl/?url=github.com/oppih.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@oppih" /></a>
<a href="https://github.com/n3dhir"><img src="https://images.weserv.nl/?url=github.com/n3dhir.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@n3dhir" /></a>
<a href="https://github.com/ksp2000"><img src="https://images.weserv.nl/?url=github.com/ksp2000.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@ksp2000" /></a>
<a href="https://github.com/quabynahdavis"><img src="https://images.weserv.nl/?url=github.com/quabynahdavis.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@quabynahdavis" /></a>
<a href="https://github.com/qinghuanandejiangshi"><img src="https://images.weserv.nl/?url=github.com/qinghuanandejiangshi.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@qinghuanandejiangshi" /></a>
<a href="https://github.com/qatcod"><img src="https://images.weserv.nl/?url=github.com/qatcod.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@qatcod" /></a>
<a href="https://github.com/CooperSheroy"><img src="https://images.weserv.nl/?url=github.com/CooperSheroy.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@CooperSheroy" /></a>
<a href="https://github.com/shahidbeig-a11y"><img src="https://images.weserv.nl/?url=github.com/shahidbeig-a11y.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@shahidbeig-a11y" /></a>
<a href="https://github.com/Kaban15"><img src="https://images.weserv.nl/?url=github.com/Kaban15.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Kaban15" /></a>
<a href="https://github.com/efcunha"><img src="https://images.weserv.nl/?url=github.com/efcunha.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@efcunha" /></a>
<a href="https://github.com/sukaimi"><img src="https://images.weserv.nl/?url=github.com/sukaimi.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@sukaimi" /></a>
<a href="https://github.com/rome-xi"><img src="https://images.weserv.nl/?url=github.com/rome-xi.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@rome-xi" /></a>
<a href="https://github.com/bsi-bcp"><img src="https://images.weserv.nl/?url=github.com/bsi-bcp.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@bsi-bcp" /></a>
<a href="https://github.com/rodion-gudz"><img src="https://images.weserv.nl/?url=github.com/rodion-gudz.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@rodion-gudz" /></a>
<a href="https://github.com/bjornmage"><img src="https://images.weserv.nl/?url=github.com/bjornmage.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@bjornmage" /></a>
<a href="https://github.com/kenanlabs"><img src="https://images.weserv.nl/?url=github.com/kenanlabs.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@kenanlabs" /></a>
<a href="https://github.com/xzyj50609"><img src="https://images.weserv.nl/?url=github.com/xzyj50609.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@xzyj50609" /></a>
<a href="https://github.com/Ahmedtahoon2"><img src="https://images.weserv.nl/?url=github.com/Ahmedtahoon2.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Ahmedtahoon2" /></a>
<a href="https://github.com/Inference1"><img src="https://images.weserv.nl/?url=github.com/Inference1.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Inference1" /></a>
<a href="https://github.com/yzhkali"><img src="https://images.weserv.nl/?url=github.com/yzhkali.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@yzhkali" /></a>
<a href="https://github.com/levonk"><img src="https://images.weserv.nl/?url=github.com/levonk.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@levonk" /></a>
<a href="https://github.com/tripstar6000"><img src="https://images.weserv.nl/?url=github.com/tripstar6000.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@tripstar6000" /></a>
<a href="https://github.com/alkank"><img src="https://images.weserv.nl/?url=github.com/alkank.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@alkank" /></a>
<a href="https://github.com/Yi-111-a"><img src="https://images.weserv.nl/?url=github.com/Yi-111-a.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Yi-111-a" /></a>
<a href="https://github.com/The88788"><img src="https://images.weserv.nl/?url=github.com/The88788.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@The88788" /></a>
<a href="https://github.com/ac1982"><img src="https://images.weserv.nl/?url=github.com/ac1982.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@ac1982" /></a>
<a href="https://github.com/agammann"><img src="https://images.weserv.nl/?url=github.com/agammann.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@agammann" /></a>
<a href="https://github.com/Claven07"><img src="https://images.weserv.nl/?url=github.com/Claven07.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Claven07" /></a>
<a href="https://github.com/AymenJeddou"><img src="https://images.weserv.nl/?url=github.com/AymenJeddou.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@AymenJeddou" /></a>
<a href="https://github.com/Thibaultjaigu"><img src="https://images.weserv.nl/?url=github.com/Thibaultjaigu.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Thibaultjaigu" /></a>
<a href="https://github.com/silvertakana"><img src="https://images.weserv.nl/?url=github.com/silvertakana.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@silvertakana" /></a>
<a href="https://github.com/sidhunt"><img src="https://images.weserv.nl/?url=github.com/sidhunt.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@sidhunt" /></a>
<a href="https://github.com/rubicon"><img src="https://images.weserv.nl/?url=github.com/rubicon.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@rubicon" /></a>
<a href="https://github.com/JoAdo07"><img src="https://images.weserv.nl/?url=github.com/JoAdo07.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@JoAdo07" /></a>
<a href="https://github.com/igormutton"><img src="https://images.weserv.nl/?url=github.com/igormutton.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@igormutton" /></a>
<a href="https://github.com/georgeatparallel"><img src="https://images.weserv.nl/?url=github.com/georgeatparallel.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@georgeatparallel" /></a>
<a href="https://github.com/BNTiyan"><img src="https://images.weserv.nl/?url=github.com/BNTiyan.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@BNTiyan" /></a>
<a href="https://github.com/ChaosCom"><img src="https://images.weserv.nl/?url=github.com/ChaosCom.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@ChaosCom" /></a>
<a href="https://github.com/Alexis12119"><img src="https://images.weserv.nl/?url=github.com/Alexis12119.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@Alexis12119" /></a>
<a href="https://github.com/kevin9327"><img src="https://images.weserv.nl/?url=github.com/kevin9327.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@kevin9327" /></a>
<a href="https://github.com/ilya-bogin-keenable"><img src="https://images.weserv.nl/?url=github.com/ilya-bogin-keenable.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@ilya-bogin-keenable" /></a>
<a href="https://github.com/harsHdokyc"><img src="https://images.weserv.nl/?url=github.com/harsHdokyc.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@harsHdokyc" /></a>
<a href="https://github.com/ALANDVO"><img src="https://images.weserv.nl/?url=github.com/ALANDVO.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@ALANDVO" /></a>
<a href="https://github.com/HyperionBurn"><img src="https://images.weserv.nl/?url=github.com/HyperionBurn.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@HyperionBurn" /></a>
<a href="https://github.com/emirhantasli"><img src="https://images.weserv.nl/?url=github.com/emirhantasli.png&w=40&h=40&fit=cover&mask=circle" width="40" alt="@emirhantasli" /></a>

## Aviso legal

**Este projeto é para experimentação e aprendizado pessoal, não para produção.** Os planos gratuitos existem para que desenvolvedores façam protótipos com eles; não são uma base de inferência estável e com suporte, e não devem ser tratados como tal. Se você construir algo real em cima do FreeLLMAPI, troque por uma API paga antes de lançar. Sua relação com cada provedor é regida pelos termos que você aceitou ao criar sua conta — esses termos continuam valendo quando o tráfego passa por este projeto, e você é responsável por cumpri-los.

Como o termo de serviço de cada provedor encara um proxy pessoal de usuário único — analisado provedor por provedor em maio de 2026 — está em [docs/en/architecture/00-high-level-index.md#terms-of-service-review](docs/en/architecture/00-high-level-index.md#terms-of-service-review).

## Licença

[MIT](./LICENSE)

---

<div align="center">

**Créditos:** projeto original [FreeLLMAPI](https://github.com/tashfeenahmed/freellmapi) de [Tashfeen Ahmed](https://github.com/tashfeenahmed) e colaboradores, licenciado sob MIT. Tradução para o português (Brasil) não oficial, feita a partir do README original em inglês.

</div>
