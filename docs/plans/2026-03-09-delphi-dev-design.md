# Design: Plugin `delphi-dev` para Claude Code

**Data:** 2026-03-09
**Autor:** Adriano Santos
**Status:** Aprovado

---

## Visão Geral

Plugin para Claude Code que transforma o assistente em um especialista sênior em Delphi.
Ao detectar código Delphi, o Claude automaticamente aplica os padrões de codificação do
autor — sem precisar ser solicitado. Comandos explícitos permitem auditorias, revisões e
geração de código padronizado.

Destinado a ser publicado como open source no marketplace oficial do Claude Code.

---

## Problema que Resolve

Desenvolvedores Delphi — individualmente ou em equipe — frequentemente violam padrões
de nomenclatura, formatação e arquitetura por falta de um guardrail automático. O Claude,
sem contexto especializado, não conhece o Delphi Style Guide, os prefixos obrigatórios
(F, A, L, C_, T, I), comandos proibidos (with, Break, Continue) nem como estruturar
laudos técnicos profissionais.

---

## Fontes de Conhecimento

Os padrões do plugin são baseados em:

- **Adriano Santos — Normas e Padronização de Codificação v4.0.1** (documento primário de estilo)
- **Código Limpo e Boas Práticas em Delphi** (apostila baseada em Clean Code + SOLID + Delphi Style Guide)
- **Guia de Boas Práticas na Escrita de Código v1.0**
- **Padrões de Excelência Delphi — Código Limpo**
- **Clean Code — Robert C. Martin** (referência universal adaptada ao Delphi)
- **Skill delphi-laudo** (skill de auditoria profissional já utilizada em produção)

---

## Estrutura de Arquivos

```
delphi-dev/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   ├── delphi-standards/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── naming-conventions.md
│   │       ├── formatting.md
│   │       ├── forbidden-commands.md
│   │       ├── classes-structure.md
│   │       └── component-prefixes.md
│   ├── delphi-write/
│   │   └── SKILL.md
│   └── delphi-laudo/
│       ├── SKILL.md
│       └── references/
│           ├── clean-code-delphi.md
│           ├── code-smells-delphi.md
│           ├── estrutura-laudo.md
│           ├── tecnologias-delphi.md
│           ├── estimativas-modernizacao.md
│           └── style-guide.md
├── agents/
│   ├── delphi-auditor.md
│   └── delphi-writer.md
├── commands/
│   ├── audit.md
│   ├── review.md
│   ├── write.md
│   └── new-project.md
├── docs/
│   └── plans/
│       └── 2026-03-09-delphi-dev-design.md
├── LICENSE
└── README.md
```

---

## Skills

### `delphi-standards` — O Modo Delphi Automático

**Propósito:** Skill sempre presente ao trabalhar com Delphi. Carrega todo o conhecimento
de padrões de codificação como contexto ativo.

**Trigger automático:** Detecta arquivos `.pas`, `.dpr`, `.dfm`, `.dpk`, `.dproj`,
menções a Delphi, FireMonkey, VCL, FireDAC, RAD Studio.

**Responsabilidades:**
- Prefixos obrigatórios: `F` (fields), `A` (parâmetros), `L` (locais), `C_` (constantes)
- Prefixos de tipos: `T` (classes/tipos), `I` (interfaces), `E` (exceções), `P` (ponteiros)
- Enumerados: prefixo mnemônico de 2+ letras minúsculas
- Indentação: 2 espaços, sem TAB, margem de 120 chars
- `begin` em linha própria, `else` em linha própria
- Palavras reservadas em minúsculo (`begin`, `end`, `if`, `string`)
- Sem underline em identificadores (exceto `C_` em constantes)
- Sem variáveis globais (usar `class var`)
- Sem notação húngara nos parâmetros (`sNome`, `iCount` → proibido)
- `const` em parâmetros string/record; NUNCA `const` em interfaces (ARC)
- Tipos monetários: `Currency` preferido; `Real` proibido

**References:** 5 arquivos de referência temáticos para não sobrecarregar o contexto principal.

---

### `delphi-write` — Escrita de Código Novo

**Propósito:** Guia a escrita de código Delphi novo do zero, garantindo que vícios
nunca entrem no código antes de acontecerem.

**Trigger automático:** Detecta intenção de criar nova classe, unit, método, formulário,
serviço, repositório ou interface em Delphi.

**Responsabilidades:**
- Estruturar classes na ordem correta (strict private → private → protected → public)
- Funções pequenas e focadas (20–30 linhas, máximo)
- Result diretamente em functions (sem variável auxiliar)
- DTOs para métodos com 4+ parâmetros
- Guard clauses com Exit no início (único uso permitido de Exit)
- try..finally por recurso (nunca liberar dois recursos no mesmo bloco)
- SQL sempre parametrizado, nunca concatenado
- Uses organizada: RTL → VCL/FMX → FireDAC → Third-party → Projeto

---

### `delphi-laudo` — Auditoria Técnica Profissional

**Propósito:** Gerar laudos técnicos profissionais de projetos Delphi.
Portado integralmente da skill `delphi-laudo` já validada em produção.

**Trigger automático:** Detecta pedidos de análise, auditoria, laudo, diagnóstico,
code smells, arquivos `.pas`/`.dfm`/`.dpr` enviados para análise.

**Responsabilidades:**
- Levantamento inicial (versão Delphi, banco, componente de acesso, objetivo)
- 8 dimensões de análise: Arquitetura, Clean Code, Code Smells, SOLID,
  Memória, Acesso a Dados, Segurança, Manutenibilidade
- Sistema de pontuação 1–5 com classificação: BOM / REGULAR / CRÍTICO / INVIÁVEL
- Flags automáticos: CRÍTICO, ATENÇÃO, RECOMENDAÇÃO
- Saídas: laudo completo, resumo executivo, checklist, plano de modernização

**References:** 6 arquivos de referência (portados do ZIP original).

---

## Agents

### `delphi-auditor`

Subagente especializado em auditoria profunda. Invocado pelo comando `/delphi-audit`
ou automaticamente pela skill `delphi-laudo` quando a análise exige varredura
sistemática de múltiplos arquivos.

Perfil: Especialista sênior Delphi com experiência em Delphi 1–12, conhecimento de
todos os frameworks e componentes do ecossistema, familiaridade com sistemas legados
e modernos.

---

### `delphi-writer`

Subagente especializado em escrita de código padronizado. Invocado pelo comando
`/delphi-write` ou pela skill `delphi-write`.

Perfil: Escreve código Delphi como um desenvolvedor sênior que internalizou todos os
padrões — nunca viola regras, nunca precisa ser lembrado, produz código pronto para
produção e revisão por pares.

---

## Commands

| Comando | Descrição |
|---|---|
| `/delphi-audit` | Laudo técnico completo — 8 dimensões, score, recomendações |
| `/delphi-review` | Revisão rápida de trecho de código — violações e correções |
| `/delphi-write` | Inicia sessão de escrita padronizada de código novo |
| `/delphi-new` | Scaffold de novo projeto Delphi com estrutura correta |

---

## Repositório GitHub

- **URL:** `https://github.com/adrianosantostreina/delphi-dev`
- **Licença:** MIT
- **README:** Bilíngue (PT-BR e EN) para alcance internacional
- **Instalação local:** `/plugin install delphi-dev@github:adrianosantostreina/delphi-dev`
- **Marketplace:** Submeter em `https://clau.de/plugin-directory-submission` após validação

---

## Critérios de Qualidade para Aprovação no Marketplace

- [x] Estrutura compatível com `claude-plugins-official`
- [x] `plugin.json` com todos os campos obrigatórios
- [x] README bilíngue com exemplos de uso reais
- [x] Licença MIT explícita
- [x] Sem MCP servers (perfil mais seguro para aprovação)
- [x] Sem execução de código externo
- [x] Triggers de skills bem definidos e não ambíguos
- [x] Skills com references modulares (não sobrecarregam contexto)
- [x] Agents com system prompts completos e focados
- [x] Commands com instruções claras de execução

---

## O Que NÃO está no escopo (v1.0)

- Integração com LSP Delphi
- Análise estática via ferramentas externas
- Geração de testes unitários automatizados (DUnitX)
- Suporte a outras linguagens além de Delphi/Object Pascal
