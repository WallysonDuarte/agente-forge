<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=e67e22&height=220&section=header&text=FORGE&fontColor=ffffff&fontSize=72&fontAlignY=38&desc=Arquitetura%20de%20Software&descSize=22&descAlignY=60" width="100%" />

<img src="./assets/portrait.png" width="200" alt="Forge" />

# 🔧 Forge · Arquitetura de Software

**Define a estrutura que sustenta o sistema: camadas, limites, integracoes e desempenho — com a decisao registrada antes do codigo.**

![Skills](https://img.shields.io/badge/skills-5-e67e22?style=for-the-badge)
![CLIs](https://img.shields.io/badge/CLIs-8-2980b9?style=for-the-badge)
![Idioma](https://img.shields.io/badge/pt--BR-nativo-1abc9c?style=for-the-badge)

</div>

---

## Por que usar este agente

- **Decisao rastreavel.** Seis meses depois se sabe por que a escolha foi aquela e o que ela custou.
- **Camadas que resistem.** Dependencia unidirecional mantem o dominio testavel sem infraestrutura.
- **Desempenho por medicao.** Perfilamento antes de mexer: otimiza o gargalo real, nao o suspeito.
- **Integracao confiavel.** Nova tentativa, interrupcao de circuito, idempotencia e fila de descarte desde o desenho.
- **Divida visivel.** Catalogo com custo e plano incremental, em vez de surpresa trimestral.

---

## Skills

| Skill | Titulo | O que resolve |
|---|---|---|
| [`arquitetura-em-camadas`](skills/arquitetura-em-camadas/SKILL.md) | Arquitetura em camadas | Estruturar o sistema em camadas com dependencia unidirecional, separacao de responsabilidade e testabilidade por injecao. |
| [`registro-de-decisao`](skills/registro-de-decisao/SKILL.md) | Registro de decisao arquitetural | Documentar decisoes com contexto, alternativas, consequencias e reversibilidade, em formato curto e durave. |
| [`desempenho-e-escala`](skills/desempenho-e-escala/SKILL.md) | Desempenho e escala | Medir antes de otimizar, encontrar o gargalo real e aplicar cache, paginacao, indice e compressao onde a medicao aponta. |
| [`integracoes`](skills/integracoes/SKILL.md) | Integracoes e mensageria | Integrar sistemas com nova tentativa, interrupcao de circuito, idempotencia, verificacao de assinatura e fila de descarte. |
| [`divida-tecnica`](skills/divida-tecnica/SKILL.md) | Divida tecnica e refatoracao | Catalogar, priorizar e pagar divida tecnica de forma incremental, sem parar a entrega de valor. |

Cada skill e um arquivo markdown autocontido (`skills/<slug>/SKILL.md`): pode ser instalada
isolada, sem o resto do repositorio.

---

## Instalacao rapida

```bash
git clone https://github.com/WallysonDuarte/agente-forge.git
cd agente-forge

# Claude Code — skills nativas
cp -r skills/* ~/.claude/skills/

# Qualquer outra CLI — arquivo unico de instrucao
cat AGENT.md skills/*/SKILL.md > AGENTS.md
```

| CLI | Instalacao (Windows / PowerShell) | Onde colocar as skills | Rodar |
|---|---|---|---|
| **Claude Code** | `irm https://claude.ai/install.ps1 \| iex` | `~/.claude/skills/` | `claude -p "<pedido>"` |
| **Codex** | `irm https://chatgpt.com/codex/install.ps1 \| iex` | `AGENTS.md na raiz do projeto` | `codex exec --skip-git-repo-check "<pedido>"` |
| **Grok** | `irm https://x.ai/cli/install.ps1 \| iex` | `passado no prompt` | `grok -p "<pedido>"` |
| **Antigravity** | `irm https://antigravity.google/cli/install.ps1 \| iex` | `passado no prompt` | `agy -p "<pedido>"` |
| **Hermes** | `iex (irm https://hermes-agent.nousresearch.com/install.ps1)` | `passado no prompt` | `hermes` |
| **OpenClaw** | `& ([scriptblock]::Create((iwr -useb https://openclaw.ai/install.ps1))) -NoOnboard` | `~/.openclaw/skills/` | `openclaw agent --local -m "<pedido>"` |
| **OpenCode** | `irm https://opencode.ai/install \| iex` | `AGENTS.md na raiz do projeto` | `opencode` |
| **Kimi Code** | `irm https://code.kimi.com/kimi-code/install.ps1 \| iex` | `passado no prompt` | `kimi -p "<pedido>"` |

Passo a passo por CLI, incluindo macOS/Linux: **[install/README.md](install/README.md)**

---

## Estrutura

```
agente-forge/
├── AGENT.md                 definicao canonica do agente
├── .agent/agent.json        metadados legiveis por maquina
├── skills/
│   ├── arquitetura-em-camadas/SKILL.md
│   ├── registro-de-decisao/SKILL.md
│   ├── desempenho-e-escala/SKILL.md
│   ├── integracoes/SKILL.md
│   └── divida-tecnica/SKILL.md
├── install/README.md        instalacao nas 8 CLIs
└── assets/portrait.png
```

---

## Ecossistema

Este agente faz parte de um time de 9 agentes especializados:

[![Master](https://img.shields.io/badge/Master-00ff41?style=flat-square)](https://github.com/WallysonDuarte/agente-master) [![Nova](https://img.shields.io/badge/Nova-9b59b6?style=flat-square)](https://github.com/WallysonDuarte/agente-nova) [![Atlas](https://img.shields.io/badge/Atlas-1abc9c?style=flat-square)](https://github.com/WallysonDuarte/agente-atlas) [![Pixel](https://img.shields.io/badge/Pixel-e91e63?style=flat-square)](https://github.com/WallysonDuarte/agente-pixel) [![Forge](https://img.shields.io/badge/Forge-e67e22?style=flat-square)](https://github.com/WallysonDuarte/agente-forge) [![Cipher](https://img.shields.io/badge/Cipher-c0392b?style=flat-square)](https://github.com/WallysonDuarte/agente-cipher) [![Luna](https://img.shields.io/badge/Luna-f39c12?style=flat-square)](https://github.com/WallysonDuarte/agente-luna) [![Lira](https://img.shields.io/badge/Lira-f0932b?style=flat-square)](https://github.com/WallysonDuarte/agente-lira) [![Rank](https://img.shields.io/badge/Rank-2980b9?style=flat-square)](https://github.com/WallysonDuarte/agente-rank)

[![Hub](https://img.shields.io/badge/ver%20o%20time%20completo-e67e22?style=for-the-badge)](https://github.com/WallysonDuarte/softwarehub-agentes)

---

<div align="center">

## Apoie este trabalho

Os 9 agentes sao mantidos de forma independente e publicados abertamente.
Se este material te poupou tempo, voce pode retribuir com um cafe:

<a href="https://buy.stripe.com/bJefZi3wJ4JH18WeuEgA800">
<img src="https://img.shields.io/badge/Apoie--me-R$%2010-e67e22?style=for-the-badge&logo=stripe&logoColor=white" alt="Apoie-me com R$ 10" />
</a>

## Links

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/dududev/)
[![Do Zero ao Junior](https://img.shields.io/badge/Do%20Zero%20ao%20Junior-1abc9c?style=for-the-badge)](https://dozeroaojunior.com.br/)
[![Curitiba Software](https://img.shields.io/badge/Curitiba%20Software-2980b9?style=for-the-badge)](https://curitibasoftware.com.br/)
[![SoftwareHub](https://img.shields.io/badge/SoftwareHub-00ff41?style=for-the-badge)](https://softwarehub.com.br/)


</div>

<img src="https://capsule-render.vercel.app/api?type=waving&color=e67e22&height=100&section=footer" width="100%" />
