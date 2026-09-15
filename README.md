# Sentinel QA Cloud + Runner

Central local para cadastrar aplicações, gravar fluxos Playwright, executar testes de regressão e realizar exploração automática com relatórios ligados à branch e ao commit testados.

## Arquitetura híbrida

- **Sentinel Cloud:** painel de projetos, métricas, equipe, histórico, configurações e relatórios.
- **Sentinel Runner:** execução próxima ao código e às aplicações privadas, com Playwright, Git e integrações de IA.
- **Adaptadores de IA:** detectam Claude Code e Codex autenticados localmente. As credenciais não são enviadas ao painel.

Em desenvolvimento, Cloud e Runner podem iniciar juntos. Eles são processos independentes: o Cloud cria trabalhos autenticados, o Runner envia heartbeat, assume a fila e sincroniza apenas resultados e artefatos.

## Iniciar

```bash
npm install
npx playwright install chromium
npm run dev
```

Abra `http://localhost:5173`.

No primeiro acesso, a interface solicitará a criação do workspace e do usuário administrador.

## Executar separadamente

Cloud/API, após `npm run build`:

```bash
npm run start:cloud
```

Runner na máquina do QA:

```bash
set SENTINEL_CLOUD_URL=https://seu-sentinel.example
set SENTINEL_RUNNER_TOKEN=seu-token-do-dispositivo
npm run start:runner
```

Em PowerShell, substitua `set` por `$env:NOME="valor"`.

## Hospedar somente o Cloud

Copie `.env.example`, defina um `SENTINEL_RUNNER_TOKEN` forte e execute:

```bash
docker compose up -d --build
```

O contêiner não instala navegador nem executa IA. Playwright, Git, Claude Code e Codex ficam nos Runners dos QAs.

## Fluxo de uso

1. Cadastre um projeto com nome e URL, como `http://localhost:8001`.
2. Escolha **Projeto local** para informar a pasta e detectar branch/commit, ou **Ambiente remoto** para testar STG/QA apenas pela URL.
3. Use **Gravar fluxo**, realize as ações no navegador e feche a janela ao terminar.
4. Abra o teste gerado para editar ou executar.
5. Use **Explorar com IA** para inspecionar páginas, links, console, rede e acessibilidade.
6. Consulte as evidências na área de relatórios.

## O que já funciona

- Vários projetos e portas diferentes.
- Verificação de disponibilidade das aplicações.
- Detecção de branch e commit Git.
- Gravação real com Playwright Codegen.
- Editor de código dos testes gravados.
- Execução de regressão com relatório HTML.
- Exploração automática segura com screenshots, erros de console, rede, links e verificações básicas de acessibilidade.
- Campo para cadastrar uma aplicação Lovable de referência.
- Interface de execuções com filtros e contexto Git.
- Gestão visual de Runner e provedores de IA.
- Configurações de sincronização, privacidade e segurança.
- Área de equipe com papéis de administrador, QA e visualizador.
- Primeiro acesso, login, derivação segura de senha e sessões.
- Cloud e Runner em processos independentes.
- Fila autenticada, heartbeat e identidade por dispositivo.
- Upload de relatórios e screenshots pelo Runner.
- Execução opcional do Claude Code local para enriquecer a exploração.
- Imagem Docker somente para o Cloud.

## Próxima evolução

A exploração usa regras autônomas locais sem custo de API e, quando o Claude Code estiver instalado e autenticado, pede uma segunda análise da interface. Quando existe uma URL Lovable, o Runner captura as duas telas na mesma resolução, calcula o percentual de diferença e inclui imagens da aplicação, referência e diff no relatório. Diferenças visuais são tratadas como suspeitas para revisão, não como bugs confirmados.
