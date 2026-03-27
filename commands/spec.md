---
description: Creates a complete software specification document (SPEC) for a Delphi project or module
---

Inicie a criacao de um documento de especificacao de software (SPEC) para o projeto ou
modulo informado pelo usuario.

**Escopo:** A SPEC cobre o projeto inteiro ou um modulo de negocio completo.
Nunca para uma unica unit ou classe.

**Idioma:** Detecte o idioma do usuario e responda sempre nesse idioma.
Padrao: portugues brasileiro.

Siga este protocolo:

1. **Levantamento** — Conduza uma entrevista estruturada, uma secao por vez:
   - Contexto: nome do sistema, objetivo, usuarios
   - Funcionalidades: o que faz, o que esta fora do escopo, regras de negocio
   - Tecnico: Delphi, banco de dados, integracoes, restricoes

2. **Mapeamento** — Organize e apresente ao usuario para validacao:
   - Requisitos Funcionais (RF-001...)
   - Requisitos Nao Funcionais (RNF-001...)
   - Regras de Negocio (RN-001...)
   - Atores e casos de uso

3. **Geracao** — Produza o documento completo seguindo o template padrao com
   todas as 13 secoes obrigatorias.

4. **Revisao** — Oferecer ajuste secao por secao e verificar se ha requisitos faltando.

Use o agente `delphi-spec-writer` para conduzir o levantamento e gerar o documento.
Carregue a skill `delphi-spec` e o template em `references/spec-template.md`.
