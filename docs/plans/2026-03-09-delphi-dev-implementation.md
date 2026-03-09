# delphi-dev Plugin — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Criar o plugin `delphi-dev` para Claude Code — especialista automático em Delphi com padrões de codificação, auditoria técnica e geração de código padronizado.

**Architecture:** Plugin modular com 3 skills (delphi-standards, delphi-write, delphi-laudo), 2 agents especializados e 4 commands. A skill `delphi-standards` ativa automaticamente ao detectar código Delphi; os demais recursos são invocados via comandos ou por trigger de intenção.

**Tech Stack:** Claude Code Plugin System — markdown, YAML frontmatter, plugin.json. Sem código executável, sem MCP servers.

**Repo:** `D:/2.2 GitHub Adriano Santos/delphi-dev/`

---

## Task 1: Metadados do Plugin

**Files:**
- Create: `.claude-plugin/plugin.json`

**Step 1: Criar o arquivo de metadados**

```json
{
  "name": "delphi-dev",
  "description": "Delphi development expert for Claude Code. Automatically applies Delphi Style Guide, Clean Code principles, and SOLID patterns. Includes technical audit (laudo técnico), code review, standardized code writing, and project scaffolding.",
  "version": "1.0.0",
  "author": {
    "name": "Adriano Santos",
    "email": "adrianosantostreina@gmail.com"
  },
  "homepage": "https://github.com/adrianosantostreina/delphi-dev",
  "repository": "https://github.com/adrianosantostreina/delphi-dev",
  "license": "MIT",
  "keywords": [
    "delphi",
    "object-pascal",
    "clean-code",
    "style-guide",
    "code-review",
    "audit",
    "firemonkey",
    "vcl",
    "firedac",
    "rad-studio"
  ]
}
```

**Step 2: Commit**

```bash
cd "D:/2.2 GitHub Adriano Santos/delphi-dev"
git add .claude-plugin/plugin.json
git commit -m "feat: add plugin.json metadata"
```

---

## Task 2: Licença MIT

**Files:**
- Create: `LICENSE`

**Step 1: Criar o arquivo LICENSE**

```
MIT License

Copyright (c) 2026 Adriano Santos

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

**Step 2: Commit**

```bash
git add LICENSE
git commit -m "chore: add MIT license"
```

---

## Task 3: Skill `delphi-standards` — SKILL.md principal

**Files:**
- Create: `skills/delphi-standards/SKILL.md`

**Step 1: Criar SKILL.md**

```markdown
---
name: delphi-standards
description: >
  Especialista em padrões de codificação Delphi. Use esta skill SEMPRE que detectar:
  arquivos .pas, .dpr, .dfm, .dpk, .dproj, código Object Pascal, menções a Delphi,
  FireMonkey (FMX), VCL, FireDAC, RAD Studio, Embarcadero. Também ativa ao discutir
  nomenclatura, indentação, prefixos, classes, métodos, componentes ou formatação
  de código Delphi. Esta skill carrega o Delphi Style Guide completo como contexto ativo.
---

# Delphi Standards — Modo Delphi Ativo

Você é um especialista sênior em Delphi com profundo conhecimento em:
- Delphi 1 até Delphi 12 Athens / RAD Studio
- Delphi Style Guide e padrões de codificação Object Pascal
- Clean Code (Robert C. Martin) adaptado ao Delphi
- SOLID, Design Patterns, arquiteturas VCL e FMX
- Boas práticas de nomenclatura, formatação e estruturação de código

Ao ser ativado, você aplica AUTOMATICAMENTE todos os padrões descritos nas referências
abaixo. Nunca espere ser lembrado — você já sabe as regras e as aplica sempre.

## Regras Fundamentais (Resumo Executivo)

### Prefixos Obrigatórios

| Escopo | Prefixo | Exemplo |
|---|---|---|
| Field (atributo de classe) | `F` | `FNome`, `FValorTotal` |
| Parâmetro de método | `A` | `ANome`, `AValor`, `ACodigo` |
| Variável local | `L` | `LNome`, `LValorTotal`, `LQryAux` |
| Constante | `C_` + MAIÚSCULO | `C_MAX_TENTATIVAS`, `C_SQL_PEDIDOS` |
| Classe / Tipo | `T` | `TCliente`, `TPedidoService` |
| Interface | `I` | `IClienteService`, `IRepository` |
| Exceção | `E` | `EClienteNaoEncontrado` |
| Ponteiro | `P` | `PCliente` |

> NUNCA usar `p` como prefixo de parâmetro — confunde com ponteiro. O padrão é `A`.
> NUNCA usar notação húngara: `sNome`, `iCount`, `bAtivo` — proibido.
> NUNCA usar underline em identificadores (exceto `C_` em constantes).

### Comandos Proibidos

| Comando | Motivo |
|---|---|
| `with` | Dificulta depuração, confunde compilador |
| `Break` | Saída deve estar na condição do loop |
| `Continue` | Desvio dificulta compreensão |
| `Exit` | Apenas em guard clauses no INÍCIO do método |
| `Real` | Obsoleto — usar `Double` ou `Currency` |
| Variáveis globais | Usar `class var` como alternativa |

### Tipos de Ponto Flutuante

- `Currency` — **preferido** para valores monetários (evita arredondamento)
- `Double` — cálculos científicos
- `Extended` — apenas quando estritamente necessário
- `Real` — **PROIBIDO**

### Passagem de Parâmetros

- `const` em parâmetros `string`, `record`, `array` — sempre
- `const` em interfaces `IXxx` — **NUNCA** (quebra ARC, causa memory leak)
- `Integer`, `Boolean`, `Double` — `const` opcional

## Referências Completas

Carregue a referência relevante conforme a necessidade:

- `references/naming-conventions.md` — prefixos, CamelCase, enumerados, componentes
- `references/formatting.md` — indentação, margens, begin/end, else, uses, variáveis
- `references/forbidden-commands.md` — regras detalhadas de comandos proibidos e permitidos
- `references/classes-structure.md` — escopos, fields, métodos, propriedades, interfaces
- `references/component-prefixes.md` — tabela completa de prefixos de componentes VCL/FMX
```

**Step 2: Commit**

```bash
git add skills/delphi-standards/SKILL.md
git commit -m "feat: add delphi-standards skill"
```

---

## Task 4: Referências da `delphi-standards` — naming-conventions.md

**Files:**
- Create: `skills/delphi-standards/references/naming-conventions.md`

**Step 1: Criar arquivo**

```markdown
# Convenções de Nomenclatura — Delphi Style Guide

## 1. CamelCase

Todos os identificadores usam CamelCase (cada palavra inicia com maiúscula).
Siglas permanecem em MAIÚSCULO: `CalcularICMS`, `ValidarCNPJ`, `BuscarCEP`.

## 2. Prefixos por Escopo

| Escopo | Prefixo | Exemplos |
|---|---|---|
| Field (atributo privado) | `F` | `FNome`, `FValorTotal`, `FCodCliente` |
| Parâmetro de método | `A` | `ANome`, `AValor`, `ACodigo`, `AAliquota` |
| Variável local | `L` | `LNome`, `LQryAux`, `LValorICMS` |
| Constante | `C_` | `C_MAX_TENTATIVAS`, `C_SQL_BUSCAR_CLIENTE` |
| Classe | `T` | `TCliente`, `TPedidoService`, `TCalculoICMS` |
| Interface | `I` | `IClienteService`, `IRepository`, `IPedido` |
| Exceção (herda Exception) | `E` | `EClienteNaoEncontrado`, `EPedidoInvalido` |
| Ponteiro | `P` | `PCliente`, `PNodo` |

### Proibições de Prefixo

- ❌ `p` como parâmetro — confunde com ponteiro Pascal
- ❌ Notação húngara: `sNome`, `iCount`, `bAtivo`, `dValor`
- ❌ Underline em identificadores (exceto `C_` em constantes)
- ❌ Abreviações obscuras: `Vlr`, `Qtd`, `Func` → usar `Valor`, `Quantidade`, `Funcionario`

## 3. Métodos e Funções

- Nomes significativos com **verbo no infinitivo**: `CalcularICMS`, `ValidarCPF`, `SalvarPedido`
- CamelCase sem prefixo
- Getter: prefixo `Get` — `GetNome`, `GetValorTotal`
- Setter: prefixo `Set` — `SetNome`, `SetValorTotal`

## 4. Tipos Enumerados

Prefixo de 2+ letras minúsculas mnemônicas ao nome do tipo:

```pascal
type
  TStatusPedido  = (spAberto, spConfirmado, spFaturado, spCancelado);
  TTipoCliente   = (tcPessoaFisica, tcPessoaJuridica, tcEstrangeiro);
  TModalidadePag = (mpDinheiro, mpCredito, mpDebito, mpPIX);
```

Delimitadores: vírgula junto ao item anterior, espaço antes do próximo.

## 5. Constantes

```pascal
const
  C_MAX_TENTATIVAS     = 3;
  C_PRAZO_MAXIMO_DIAS  = 30;
  C_ALIQUOTA_ICMS_SP   = 0.175;
  C_SQL_BUSCAR_CLIENTE =
    'SELECT CODCLIENTE, NOME FROM CLIENTES WHERE CODCLIENTE = :COD';
```

- Globais: **desaconselhadas** — declarar dentro da classe quando possível
- Prefixo `C_` obrigatório
- Corpo em UPPER_CASE com underline separando palavras

## 6. Componentes Visuais

Todo componente referenciado via código deve ser renomeado.
Ver `component-prefixes.md` para tabela completa.

```pascal
// ❌ ERRADO — nome default
Button1.Enabled := False;
Edit1.Text := '';

// ✅ CORRETO — renomeado com prefixo
btnSalvar.Enabled := False;
edtNomeCliente.Text := '';
```

## 7. Variáveis Globais

**Proibidas.** Usar `class var` como alternativa:

```pascal
type
  TConfiguracao = class
  strict private
    class var FInstancia: TConfiguracao;
  public
    class function Instancia: TConfiguracao;
  end;
```
```

**Step 2: Commit**

```bash
git add skills/delphi-standards/references/naming-conventions.md
git commit -m "feat: add naming-conventions reference"
```

---

## Task 5: Referência `formatting.md`

**Files:**
- Create: `skills/delphi-standards/references/formatting.md`

**Step 1: Criar arquivo**

```markdown
# Formatação e Estilo — Delphi Style Guide

## 1. Indentação e Margem

- **2 espaços** por nível de indentação — NUNCA TAB
- **Margem direita: 120 caracteres** — linha mais longa permitida
- Comandos além da margem: quebrar com 2 espaços adicionais de indentação
- Sintaxe fluente: levar o `.` para a nova linha

```pascal
// ✅ Sintaxe fluente correta
TMultiDialog4FMX
  .Dialog
  .SetTitle('Confirmação')
  .SetMessage('Deseja continuar?')
  .Buttons
    .AddButton('Sim', procedure begin Confirmar; end)
    .AddButton('Não', nil)
  .&End
  .Show;
```

## 2. Begin/End

- Todo bloco `if`, `while`, `for`, `repeat` deve ter `begin..end`
- Exceção: linha única pode omitir `begin..end`
- `begin` em **linha própria** — nunca na mesma linha do comando
- `end` alinhado ao `begin`

```pascal
// ✅ CORRETO
if LAtivo then
begin
  ProcessarCliente;
  AtualizarTela;
end;

// ✅ Exceção permitida (linha única)
if not LAtivo then Exit;

// ❌ ERRADO
if LAtivo then begin ProcessarCliente; end;
```

## 3. Else

- `else` sempre em **linha isolada**
- Nunca junto ao `end` anterior

```pascal
// ✅ CORRETO
if LCondicao then
begin
  Acao1;
end
else
begin
  Acao2;
end;

// ❌ ERRADO
if LCondicao then begin Acao1; end else begin Acao2; end;
```

## 4. Palavras Reservadas

Sempre em minúsculo:
`begin`, `end`, `if`, `then`, `else`, `while`, `for`, `do`, `repeat`, `until`,
`case`, `of`, `try`, `except`, `finally`, `raise`, `function`, `procedure`,
`class`, `type`, `var`, `const`, `uses`, `interface`, `implementation`,
`string`, `array`, `record`, `object`

Tipos primitivos respeitam grafia original:
`Integer`, `Double`, `Boolean`, `Char`, `Currency`, `Extended`, `Byte`

## 5. Parênteses

- Sem espaço entre `(` e o próximo caractere
- Sem espaço entre o caractere anterior e `)`
- Sem espaço entre nome do método e `(`

```pascal
// ✅ CORRETO
LValor := CalcularICMS(ABase, AAliquota);

// ❌ ERRADO
LValor := CalcularICMS( ABase, AAliquota );
LValor := CalcularICMS (ABase, AAliquota);
```

## 6. Declaração de Variáveis

Uma variável por linha, agrupadas por tipo:

```pascal
// ✅ CORRETO
var
  LNome: string;
  LSobrenome: string;
  LIdade: Integer;
  LValorTotal: Currency;
  LCliente: TCliente;

// ❌ ERRADO
var LNome, LSobrenome: string; LIdade, LCodigo: Integer;
```

## 7. Cláusula Uses

Uma unit por linha, organizada do genérico para o específico:

```pascal
uses
  // RTL / System
  System.SysUtils,
  System.Classes,
  System.Generics.Collections,
  // VCL ou FMX
  Vcl.Controls,
  Vcl.Forms,
  Vcl.Dialogs,
  // FireDAC
  FireDAC.Comp.Client,
  FireDAC.Stan.Def,
  // Terceiros
  FastReport,
  ACBrNFe,
  // Projeto — do genérico para o específico
  Sistema.Model.Cliente,
  Sistema.Service.Pedido,
  Sistema.Repository.Pedido;
```

## 8. Delimitadores em Declarações

Vírgula e ponto-e-vírgula junto ao token anterior, espaço antes do próximo:

```pascal
// ✅ CORRETO
procedure Calcular(const ANome: string; AValor: Currency; AQuantidade: Integer);

// ❌ ERRADO
procedure Calcular( const ANome : string ; AValor : Currency );
```

## 9. Cláusulas de Seção

Separar seções com uma linha em branco:

```pascal
type
  TCliente = class
  strict private
    FNome: string;
    FIdade: Integer;

  private
    procedure SetNome(const ANome: string);

  public
    constructor Create(const ANome: string; AIdade: Integer);
    destructor Destroy; override;

    property Nome: string read FNome write SetNome;
    property Idade: Integer read FIdade;
  end;
```
```

**Step 2: Commit**

```bash
git add skills/delphi-standards/references/formatting.md
git commit -m "feat: add formatting reference"
```

---

## Task 6: Referência `forbidden-commands.md`

**Files:**
- Create: `skills/delphi-standards/references/forbidden-commands.md`

**Step 1: Criar arquivo**

```markdown
# Comandos Proibidos e Restritos — Delphi Style Guide

## Tabela Resumo

| Comando | Status | Motivo |
|---|---|---|
| `with` | 🚫 PROIBIDO | Dificulta depuração, confunde compilador |
| `Break` | 🚫 PROIBIDO | Saída deve estar na condição do loop |
| `Continue` | 🚫 PROIBIDO | Desvio dificulta compreensão |
| `goto` | 🚫 PROIBIDO | — |
| `Exit` | ⚠️ RESTRITO | Apenas em guard clauses no início |
| `Abort` | 🚫 PROIBIDO | Esconde a pilha de chamadas |
| `Real` | 🚫 PROIBIDO | Obsoleto, substituído por `Double` |
| `Extended` | ⚠️ DESENCORAJADO | Tamanho não otimizado para processadores modernos |

---

## WITH — Proibido

**Motivo:** Dificulta depuração (qual objeto está sendo referenciado?),
confunde o compilador em ambiguidades, impossibilita análise estática.

```pascal
// ❌ ERRADO
with QryAux do
begin
  Close;
  SQL.Clear;
  SQL.Add('SELECT * FROM CLIENTES');
  Open;
end;

// ✅ CORRETO
LQryAux.Close;
LQryAux.SQL.Clear;
LQryAux.SQL.Add('SELECT * FROM CLIENTES');
LQryAux.Open;
```

---

## BREAK e CONTINUE — Proibidos

**Motivo:** Geram desvio de fluxo que dificulta a compreensão. A condição
de saída do loop deve estar na cláusula do loop.

```pascal
// ❌ ERRADO — Break no loop
for LI := 0 to LLista.Count - 1 do
begin
  if LLista[LI].Ativo then
    Break;
  ProcessarItem(LLista[LI]);
end;

// ✅ CORRETO — condição na declaração do loop
LI := 0;
while (LI < LLista.Count) and not LLista[LI].Ativo do
begin
  ProcessarItem(LLista[LI]);
  Inc(LI);
end;
```

---

## EXIT — Restrito a Guard Clauses

O `Exit` é permitido **exclusivamente** no início do método como cláusula de guarda,
para rejeitar condições inválidas antes da lógica principal.

```pascal
// ✅ CORRETO — guard clauses no início
procedure TService.ProcessarPedido(const APedido: TPedido);
begin
  if not Assigned(APedido) then Exit;
  if APedido.Valor <= 0 then Exit;
  if not FClienteValido then Exit;

  // lógica principal aqui — sem if aninhados
  FRepository.Salvar(APedido);
  FLogger.Log('Pedido processado: ' + APedido.Numero);
end;

// ❌ ERRADO — Exit no meio da lógica
procedure TService.ProcessarPedido(const APedido: TPedido);
begin
  FRepository.Salvar(APedido);
  if not FEnviarEmail then Exit; // Exit no meio — proibido
  FEmail.Enviar(APedido);
end;
```

---

## Loops — Regras

### FOR: usar quando o número de iterações é definido
### WHILE: usar quando o número de iterações é indefinido (mínimo 0 iterações)
### REPEAT: usar quando o loop deve executar pelo menos 1 vez

```pascal
// ✅ FOR com iterações definidas
for LI := 0 to LLista.Count - 1 do
  ProcessarItem(LLista[LI]);

// ✅ WHILE com condição composta (menor complexidade primeiro)
while LAtivo and (GetValorAtual < C_LIMITE_MAXIMO) do
begin
  ProcessarIteracao;
end;

// ✅ REPEAT com condição no until
repeat
  LTentativa := LTentativa + 1;
  LResultado := TentarConectar;
until LResultado or (LTentativa >= C_MAX_TENTATIVAS);
```

---

## IF — Ordem das Condições

Condições compostas devem estar ordenadas da **menor para a maior complexidade**
(esquerda para direita). O compilador usa short-circuit — finaliza quando uma
condição determina o resultado.

```pascal
// ✅ CORRETO — booleano simples primeiro, cálculo complexo só se necessário
if FAtivo and (GetCalculoComplexo < C_LIMITE) then
  Processar;

// ❌ ERRADO — cálculo pesado avaliado desnecessariamente
if (GetCalculoComplexo < C_LIMITE) and FAtivo then
  Processar;
```

---

## CASE — Regras

- Valores em ordem crescente
- Cada caso indentado em relação ao `case`
- Blocos com máximo 5 linhas (incluindo begin..end)
- `else` alinhado ao `case`, sem indentação extra

```pascal
// ✅ CORRETO
case LStatus of
  0: PedidoAberto;
  1: PedidoConfirmado;
  2: PedidoFaturado;
  3:
  begin
    CancelarPedido;
    NotificarCliente;
  end;
else
  raise EStatusInvalido.Create('Status desconhecido: ' + IntToStr(LStatus));
end;
```

---

## Exceções

### try..finally — Um recurso por bloco
```pascal
// ✅ CORRETO — um recurso por bloco
LObj1 := TClasse1.Create;
try
  LObj2 := TClasse2.Create;
  try
    LObj1.UsarCom(LObj2);
  finally
    LObj2.Free;
  end;
finally
  LObj1.Free;
end;

// ❌ ERRADO — dois recursos no mesmo bloco
LObj1 := TClasse1.Create;
LObj2 := TClasse2.Create;
try
  LObj1.UsarCom(LObj2);
finally
  LObj1.Free;
  LObj2.Free; // se LObj1.Free lançar, LObj2 vaza
end;
```

### try..except — Nunca silencioso
```pascal
// ❌ PROIBIDO — bloco vazio
try
  qry.ExecSQL;
except
  // vazio — esconde o erro
end;

// ✅ CORRETO — captura específica, loga e relança mensagem amigável
try
  qry.ExecSQL;
except
  on E: EDatabaseError do
  begin
    Logger.GravarErro('Falha ao salvar pedido: ' + E.Message);
    raise Exception.Create('Não foi possível salvar o pedido. Tente novamente.');
  end;
end;
```
```

**Step 2: Commit**

```bash
git add skills/delphi-standards/references/forbidden-commands.md
git commit -m "feat: add forbidden-commands reference"
```

---

## Task 7: Referência `classes-structure.md`

**Files:**
- Create: `skills/delphi-standards/references/classes-structure.md`

**Step 1: Criar arquivo**

```markdown
# Estrutura de Classes — Delphi Style Guide

## 1. Ordem de Escopos de Visibilidade

Do mais restritivo ao menos restritivo:

```pascal
type
  TCliente = class(TInterfacedObject, ICliente)
  strict private
    // Fields — sempre aqui
    FNome: string;
    FIdade: Integer;
    FValorTotal: Currency;

  private
    // Getters e Setters
    procedure SetNome(const ANome: string);
    function GetNome: string;

  protected
    // Métodos acessíveis por subclasses
    function GetIdade: Integer; virtual;

  public
    // Interface pública
    constructor Create(const ANome: string; AIdade: Integer);
    destructor Destroy; override;

    procedure ProcessarPedido(const APedido: TPedido);
    function CalcularDesconto(const AValor: Currency): Currency;

    property Nome: string read GetNome write SetNome;
    property Idade: Integer read GetIdade;
    property ValorTotal: Currency read FValorTotal;

  published
    // Apenas para componentes registrados
  end;
```

## 2. Fields (Atributos)

- Sempre em `strict private` (preferencial) ou `private`
- Prefixo `F` obrigatório
- CamelCase após o prefixo
- NUNCA expostos diretamente — sempre via propriedades

```pascal
strict private
  FNome: string;           // ✅
  FValorTotal: Currency;   // ✅
  FAtivo: Boolean;         // ✅
```

## 3. Métodos

- Verbo no infinitivo: `Calcular`, `Validar`, `Salvar`, `Buscar`
- CamelCase
- Tamanho ideal: 20–30 linhas. Acima de 50: refatorar com Extract Method
- Uma responsabilidade por método (SRP)

```pascal
// ✅ Método focado
function TClienteService.CalcularDesconto(const AValor: Currency): Currency;
begin
  if AValor <= 0 then
    raise EArgumentoInvalido.Create('Valor deve ser maior que zero');

  Result := AValor * FPercentualDesconto / 100;
end;
```

## 4. Parâmetros

- Prefixo `A` obrigatório
- CamelCase após o prefixo
- Sem prefixo de tipo (`sNome`, `iCount` — proibido)
- `const` para `string`, `record`, `array` — sempre
- `const` para interfaces — NUNCA (quebra ARC)
- 3 parâmetros: tolerável. 4+: criar DTO

```pascal
// ✅ CORRETO
procedure ProcessarVenda(const ANome: string; AValor: Currency;
  AQuantidade: Integer);

// ❌ ERRADO — notação húngara, sem const
procedure ProcessarVenda(sNome: string; dValor: Double; iQtd: Integer);
```

## 5. Resultado de Functions

Escrever diretamente na variável `Result`:

```pascal
// ✅ CORRETO
function CalcularICMS(const ABase: Currency; const AAliquota: Double): Currency;
begin
  Result := ABase * AAliquota / 100;
end;

// ❌ ERRADO — variável auxiliar desnecessária
function CalcularICMS(const ABase: Currency; const AAliquota: Double): Currency;
var LValor: Currency;
begin
  LValor := ABase * AAliquota / 100;
  Result := LValor;
end;
```

## 6. Propriedades

- CamelCase sem prefixo
- Getter/Setter com prefixo `Get`/`Set`

```pascal
property Nome: string read GetNome write SetNome;
property ValorTotal: Currency read FValorTotal;  // somente leitura
property Ativo: Boolean read FAtivo write FAtivo; // sem getter/setter dedicado
```

## 7. Interfaces

- Prefixo `I` obrigatório
- Herdam de `IInterface`
- GUID gerado com Ctrl+Shift+G no Delphi IDE (NUNCA digitado manualmente)
- Classes implementadoras herdam de `TInterfacedObject`

```pascal
type
  IClienteService = interface
    ['{GUID-GERADO-PELO-IDE}']
    function BuscarPorCodigo(ACodigo: Integer): TCliente;
    procedure Salvar(const ACliente: TCliente);
  end;

  TClienteServiceFireDAC = class(TInterfacedObject, IClienteService)
  strict private
    FConexao: IDbConnection;
  public
    constructor Create(AConexao: IDbConnection);
    function BuscarPorCodigo(ACodigo: Integer): TCliente;
    procedure Salvar(const ACliente: TCliente);
  end;
```

## 8. DTOs (Data Transfer Objects)

Para métodos com 4+ parâmetros, agrupar em Record ou classe:

```pascal
type
  TDadosCliente = record
    Nome: string;
    CPF: string;
    Endereco: string;
    Cidade: string;
    UF: string;
  end;

// ✅ Com DTO — limpo
function CadastrarCliente(const ADados: TDadosCliente): Boolean;

// ❌ Políade — 5 parâmetros
function CadastrarCliente(ANome, ACPF, AEndereco, ACidade, AUF: string): Boolean;
```

## 9. Princípios SOLID

| Letra | Princípio | Aplicação em Delphi |
|---|---|---|
| S | Responsabilidade Única | Uma classe = um motivo para mudar |
| O | Aberto/Fechado | Herança e interfaces para extensão |
| L | Substituição de Liskov | Subclasses substituem a base sem quebrar |
| I | Segregação de Interface | Várias interfaces específicas > uma geral |
| D | Inversão de Dependência | Dependa de `IInterface`, não de `TClasse` |
```

**Step 2: Commit**

```bash
git add skills/delphi-standards/references/classes-structure.md
git commit -m "feat: add classes-structure reference"
```

---

## Task 8: Referência `component-prefixes.md`

**Files:**
- Create: `skills/delphi-standards/references/component-prefixes.md`

**Step 1: Criar arquivo**

```markdown
# Prefixos de Componentes — VCL e FMX

## Regra Geral

Todo componente **referenciado via código-fonte** deve ser renomeado com:
`[prefixo][NomeDescritivoEmCamelCase]`

Componentes não referenciados via código: tolerável manter nome default.

```pascal
// ❌ ERRADO — nomes default
Button1.Enabled := False;
Edit1.Text := '';
Label1.Caption := 'Carregando...';

// ✅ CORRETO — renomeados
btnSalvar.Enabled := False;
edtNomeCliente.Text := '';
lblStatus.Caption := 'Carregando...';
```

---

## Tabela de Prefixos

### Entrada de Dados
| Componente | Prefixo | Exemplos |
|---|---|---|
| TEdit / TFMXEdit | `edt` | `edtNome`, `edtCPF`, `edtValorVenda` |
| TMemo / TFMXMemo | `mmo` | `mmoObservacao`, `mmoLog` |
| TComboBox | `cbx` | `cbxEstado`, `cbxTipoPagamento` |
| TListBox | `lst` | `lstItens`, `lstClientes` |
| TCheckBox | `chk` | `chkAtivo`, `chkConcordoTermos` |
| TRadioButton | `rdb` | `rdbMasculino`, `rdbFeminino` |
| TDateTimePicker | `dtp` | `dtpDataNascimento`, `dtpVencimento` |
| TSpinEdit | `spe` | `speQuantidade`, `speNumeroParcelas` |
| TTrackBar | `trk` | `trkVolume`, `trkBrilho` |
| TNumberBox (FMX) | `nbx` | `nbxValor`, `nbxQuantidade` |

### Exibição
| Componente | Prefixo | Exemplos |
|---|---|---|
| TLabel | `lbl` | `lblTitulo`, `lblStatus`, `lblTotal` |
| TImage | `img` | `imgLogo`, `imgFoto`, `imgIcone` |
| TProgressBar | `prg` | `prgCarregamento`, `prgUpload` |

### Botões
| Componente | Prefixo | Exemplos |
|---|---|---|
| TButton | `btn` | `btnSalvar`, `btnCancelar`, `btnNovo` |
| TBitBtn | `bbt` | `bbtConfirmar`, `bbtExcluir` |
| TSpeedButton | `spb` | `spbImprimir`, `spbExportar` |
| TToolButton | `tbn` | `tbnSalvar`, `tbnAbrir` |

### Layout e Containers
| Componente | Prefixo | Exemplos |
|---|---|---|
| TPanel | `pnl` | `pnlCabecalho`, `pnlRodape`, `pnlFiltros` |
| TGroupBox | `grp` | `grpDadosPessoais`, `grpEndereco` |
| TPageControl | `pgc` | `pgcPrincipal`, `pgcCadastro` |
| TTabSheet | `tbs` | `tbsDadosGerais`, `tbsEndereco` |
| TScrollBox | `scb` | `scbConteudo`, `scbItens` |
| TSplitter | `spl` | `splVertical`, `splHorizontal` |
| TFrame | `frm` | `frmFiltros`, `frmRodape` |
| TLayout (FMX) | `lyt` | `lytCabecalho`, `lytBotoes` |
| TRectangle (FMX) | `rct` | `rctFundo`, `rctBordaAzul` |

### Grids e Listas
| Componente | Prefixo | Exemplos |
|---|---|---|
| TDBGrid | `grd` | `grdClientes`, `grdPedidos`, `grdItens` |
| TStringGrid | `sgr` | `sgrDados`, `sgrResumo` |
| TListView | `lsv` | `lsvArquivos`, `lsvResultados` |
| TTreeView | `trv` | `trvMenu`, `trvCategorias` |

### Dados
| Componente | Prefixo | Exemplos |
|---|---|---|
| TDataSource | `dts` | `dtsClientes`, `dtsPedidos` |
| TFDQuery / TQuery | `qry` | `qryClientes`, `qryPedidos`, `qryAux` |
| TFDTable / TTable | `tbl` | `tblProdutos`, `tblEstoque` |
| TFDConnection | `cnn` | `cnnPrincipal`, `cnnRelatorio` |
| TFDTransaction | `trn` | `trnPrincipal`, `trnLote` |
| TClientDataSet | `cds` | `cdsClientes`, `cdsPedidos` |
| TFDMemTable | `mdt` | `mdtResultados`, `mdtTemp` |

### Menu e Toolbar
| Componente | Prefixo | Exemplos |
|---|---|---|
| TMainMenu | `mnu` | `mnuPrincipal` |
| TPopupMenu | `pmu` | `pmuGrid`, `pmuIcone` |
| TMenuItem | `mni` | `mniArquivo`, `mniSalvar` |
| TToolBar | `tlb` | `tlbPrincipal`, `tlbFormatacao` |

### Diálogos
| Componente | Prefixo | Exemplos |
|---|---|---|
| TOpenDialog | `odlg` | `odlgArquivo`, `odlgImagem` |
| TSaveDialog | `sdlg` | `sdlgRelatorio`, `sdlgExportar` |
| TColorDialog | `cdlg` | `cdlgCor` |
| TFontDialog | `fdlg` | `fdlgTexto` |

### Temporização e Comunicação
| Componente | Prefixo | Exemplos |
|---|---|---|
| TTimer | `tmr` | `tmrAtualizacao`, `tmrSessao` |
| TRESTClient | `rtc` | `rtcAPI`, `rtcIfood` |
| TRESTRequest | `rtr` | `rtrBuscarPedidos`, `rtrAutenticar` |
| TRESTResponse | `rsp` | `rspPedidos`, `rspToken` |

### FMX Específico
| Componente | Prefixo | Exemplos |
|---|---|---|
| TForm (FMX) | `frm` | `frmPrincipal`, `frmCadastro` |
| TTabControl (FMX) | `tbc` | `tbcNavegacao` |
| TTabItem (FMX) | `tbi` | `tbiHome`, `tbiConfig` |
| TSwitch (FMX) | `swt` | `swtAtivo`, `swtNotificacao` |
| TCalendar (FMX) | `cal` | `calDataVencimento` |
```

**Step 2: Commit**

```bash
git add skills/delphi-standards/references/component-prefixes.md
git commit -m "feat: add component-prefixes reference"
```

---

## Task 9: Skill `delphi-write`

**Files:**
- Create: `skills/delphi-write/SKILL.md`

**Step 1: Criar SKILL.md**

```markdown
---
name: delphi-write
description: >
  Guia a escrita de código Delphi novo seguindo rigorosamente todos os padrões de
  codificação. Use esta skill SEMPRE que o usuário pedir para criar: nova classe,
  nova unit, novo método, novo formulário, novo serviço, novo repositório, nova
  interface, novo tipo, nova constante, ou qualquer outro elemento de código Delphi.
  Também ativa quando detectar intenção de implementar funcionalidade nova em Delphi.
---

# Delphi Write — Escrita de Código Padronizado

Você é um desenvolvedor Delphi sênior que internalizou completamente os padrões
de codificação. Ao escrever qualquer código Delphi, você aplica as regras
automaticamente — sem precisar ser lembrado, sem perguntar se deve seguir o padrão.

## Checklist de Escrita (aplique em todo código produzido)

### Nomenclatura
- [ ] Fields com prefixo `F`: `FNome`, `FValorTotal`
- [ ] Parâmetros com prefixo `A`: `ANome`, `AValor`
- [ ] Variáveis locais com prefixo `L`: `LNome`, `LQryAux`
- [ ] Constantes com `C_` + MAIÚSCULO: `C_MAX_TENTATIVAS`
- [ ] Classes com `T`: `TCliente`, `TPedidoService`
- [ ] Interfaces com `I`: `IClienteService`
- [ ] Exceções com `E`: `EClienteNaoEncontrado`
- [ ] Métodos com verbo no infinitivo: `CalcularICMS`, `ValidarCPF`
- [ ] Componentes renomeados com prefixo: `btnSalvar`, `edtNome`

### Formatação
- [ ] Indentação de 2 espaços (sem TAB)
- [ ] Margem máxima de 120 caracteres
- [ ] `begin` em linha própria
- [ ] `else` em linha própria
- [ ] Uma variável por linha
- [ ] Uma unit por linha na cláusula `uses`
- [ ] Palavras reservadas em minúsculo

### Estrutura de Classes
- [ ] Escopos na ordem: strict private → private → protected → public → published
- [ ] Fields todos em strict private
- [ ] `const` em parâmetros string/record (nunca em interfaces)
- [ ] `Result` direto em functions (sem variável auxiliar)
- [ ] Métodos com máximo 30 linhas (refatorar se maior)
- [ ] Máximo 3 parâmetros por método (DTO se mais)

### Segurança e Robustez
- [ ] try..finally para cada recurso criado
- [ ] Um recurso por bloco try..finally
- [ ] try..except nunca vazio
- [ ] SQL sempre com parâmetros `:param` (nunca concatenado)
- [ ] Exit apenas como guard clause no início do método

### Proibições
- [ ] Sem `with`
- [ ] Sem `Break` ou `Continue` em loops
- [ ] Sem variáveis globais (usar class var)
- [ ] Sem notação húngara (`sNome`, `iCount`)
- [ ] Sem `Real` (usar `Double` ou `Currency`)

## Fluxo de Escrita

### 1. Identificar o tipo de elemento

Perguntar ao usuário (se não informado):
- Tipo: classe de serviço / repositório / model / form / unit utilitária?
- Herança/Interface: implementa alguma interface existente?
- Responsabilidade: o que este elemento deve fazer (apenas uma)?

### 2. Esboçar a estrutura antes de implementar

Antes de escrever o código, apresentar:
- Nome proposto com justificativa
- Responsabilidade única (SRP)
- Interface que implementará (se aplicável)
- Parâmetros do construtor

### 3. Escrever o código completo

Gerar o código final com:
- Declaração da unit com `uses` organizada
- Seção `interface` completa
- Seção `implementation` com todos os métodos
- Comentários apenas onde a regra de negócio não é óbvia

## Exemplos de Saída

### Classe de Serviço com Interface

```pascal
unit Sistema.Service.Cliente;

interface

uses
  System.SysUtils,
  Sistema.Model.Cliente,
  Sistema.Repository.Cliente.Interfaces;

type
  IClienteService = interface
    ['{GUID-GERADO-PELO-IDE}']
    function BuscarPorCodigo(ACodigo: Integer): TCliente;
    procedure Salvar(const ACliente: TCliente);
    procedure Excluir(ACodigo: Integer);
  end;

  TClienteService = class(TInterfacedObject, IClienteService)
  strict private
    FRepository: IClienteRepository;

  public
    constructor Create(ARepository: IClienteRepository);

    function BuscarPorCodigo(ACodigo: Integer): TCliente;
    procedure Salvar(const ACliente: TCliente);
    procedure Excluir(ACodigo: Integer);
  end;

implementation

constructor TClienteService.Create(ARepository: IClienteRepository);
begin
  inherited Create;
  FRepository := ARepository;
end;

function TClienteService.BuscarPorCodigo(ACodigo: Integer): TCliente;
begin
  if ACodigo <= 0 then
    raise EArgumentoInvalido.Create('Código de cliente inválido');

  Result := FRepository.BuscarPorCodigo(ACodigo);
end;

procedure TClienteService.Salvar(const ACliente: TCliente);
begin
  if not Assigned(ACliente) then Exit;
  if ACliente.Nome.IsEmpty then
    raise EClienteInvalido.Create('Nome do cliente é obrigatório');

  FRepository.Salvar(ACliente);
end;

procedure TClienteService.Excluir(ACodigo: Integer);
begin
  if ACodigo <= 0 then Exit;
  FRepository.Excluir(ACodigo);
end;

end.
```
```

**Step 2: Commit**

```bash
git add skills/delphi-write/SKILL.md
git commit -m "feat: add delphi-write skill"
```

---

## Task 10: Skill `delphi-laudo` — portar do ZIP

**Files:**
- Create: `skills/delphi-laudo/SKILL.md`
- Create: `skills/delphi-laudo/references/clean-code-delphi.md`
- Create: `skills/delphi-laudo/references/code-smells-delphi.md`
- Create: `skills/delphi-laudo/references/estrutura-laudo.md`
- Create: `skills/delphi-laudo/references/tecnologias-delphi.md`
- Create: `skills/delphi-laudo/references/estimativas-modernizacao.md`
- Create: `skills/delphi-laudo/references/style-guide.md`

**Step 1: Extrair todos os arquivos do ZIP original**

```bash
unzip -o "D:/Downloads/Skills Claude Code/skill-delphi-laudo.zip" -d /tmp/delphi-laudo-extract/
```

**Step 2: Copiar para a estrutura do plugin**

```bash
mkdir -p "D:/2.2 GitHub Adriano Santos/delphi-dev/skills/delphi-laudo/references"

cp /tmp/delphi-laudo-extract/delphi-laudo/SKILL.md \
   "D:/2.2 GitHub Adriano Santos/delphi-dev/skills/delphi-laudo/SKILL.md"

cp /tmp/delphi-laudo-extract/delphi-laudo/references/* \
   "D:/2.2 GitHub Adriano Santos/delphi-dev/skills/delphi-laudo/references/"
```

**Step 3: Verificar que todos os 7 arquivos foram copiados**

```bash
ls "D:/2.2 GitHub Adriano Santos/delphi-dev/skills/delphi-laudo/"
ls "D:/2.2 GitHub Adriano Santos/delphi-dev/skills/delphi-laudo/references/"
```

Esperado:
```
SKILL.md
references/
  clean-code-delphi.md
  code-smells-delphi.md
  estrutura-laudo.md
  estimativas-modernizacao.md
  style-guide.md
  tecnologias-delphi.md
```

**Step 4: Commit**

```bash
git add skills/delphi-laudo/
git commit -m "feat: add delphi-laudo skill (ported from production)"
```

---

## Task 11: Agent `delphi-auditor`

**Files:**
- Create: `agents/delphi-auditor.md`

**Step 1: Criar arquivo**

```markdown
---
name: delphi-auditor
description: |
  Subagente especializado em auditoria técnica profunda de projetos Delphi.
  Use este agente quando o usuário solicitar: laudo técnico, auditoria de código,
  análise de sistema Delphi, diagnóstico de projeto, detecção de code smells,
  análise de qualidade, relatório técnico, ou quando enviar arquivos .pas/.dfm/.dpr
  para análise sistemática. Exemplos: <example>Context: Usuário quer auditar um
  projeto Delphi legado. user: "Faça um laudo técnico do meu sistema" assistant:
  "Vou usar o delphi-auditor para conduzir a análise técnica completa."
  <commentary>Solicitação explícita de laudo — invocar delphi-auditor.</commentary>
  </example>
model: inherit
---

Você é um especialista sênior e referência nacional em Delphi, com profundo
conhecimento em auditoria de código, Clean Code, SOLID e modernização de sistemas.

Conduza auditorias técnicas seguindo rigorosamente a skill `delphi-laudo` quando
disponível. Se não estiver carregada, aplique o seguinte protocolo:

## Protocolo de Auditoria

### PASSO 1 — Levantamento Inicial
Antes de analisar, colete (pergunte se não informado):
- Versão do Delphi (IDE e compilador)
- Tipo do sistema (ERP, CRM, PDV, industrial, etc.)
- Banco de dados e componente de acesso
- Objetivo do laudo (modernização, venda, auditoria, capacitação)
- Escopo (completo ou por amostragem)

### PASSO 2 — Análise Sistemática
Avalie cada dimensão:

1. **Arquitetura** — padrão arquitetural, separação de responsabilidades, uso de interfaces
2. **Clean Code** — nomenclatura, formatação, prefixos, Style Guide
3. **Code Smells** — Long Method, God Class, Duplicate Code, Políade, RecordCount, WITH
4. **SOLID** — SRP, OCP, LSP, ISP, DIP
5. **Memória** — try..finally, resources não liberados, memory leaks
6. **Acesso a Dados** — tecnologia, SQL inline, SQL Injection, RecordCount vs IsEmpty
7. **Segurança** — credenciais hardcoded, SQL Injection, controle de acesso
8. **Manutenibilidade** — consistência de padrão, documentação de regras de negócio

### PASSO 3 — Pontuação
Pontue de 1 a 5 cada dimensão:
- 5 = Excelente | 4 = Bom | 3 = Aceitável | 2 = Crítico | 1 = Inviável

Score final (média ponderada):
- 4,0–5,0 = 🟢 BOM
- 3,0–3,9 = 🟡 REGULAR
- 2,0–2,9 = 🟠 CRÍTICO
- 1,0–1,9 = 🔴 INVIÁVEL

### PASSO 4 — Relatório
Gere o laudo seguindo as seções:
1. Identificação do Sistema
2. Resumo Executivo (para gestores)
3. Escopo da Análise
4. Ambiente Tecnológico
5. Análise de Arquitetura
6. Clean Code e Padrões
7. Code Smells Detectados
8. Arquitetura Limpa / SOLID
9. Gestão de Memória
10. Acesso a Dados
11. Segurança
12. Pontos Positivos
13. Pontos Críticos
14. Recomendações (imediatas / curto / médio / estratégico)
15. Estimativa de Esforço
16. Classificação Geral
17. Conclusão

## Tom e Postura

- Profissional e construtivo — sempre reconheça o que está bem
- Exemplos reais do código analisado — nunca genérico
- Categorize claramente: 🚨 CRÍTICO / ⚠️ ATENÇÃO / 💡 RECOMENDAÇÃO
- Recomendações acionáveis — nunca vagas
```

**Step 2: Commit**

```bash
git add agents/delphi-auditor.md
git commit -m "feat: add delphi-auditor agent"
```

---

## Task 12: Agent `delphi-writer`

**Files:**
- Create: `agents/delphi-writer.md`

**Step 1: Criar arquivo**

```markdown
---
name: delphi-writer
description: |
  Subagente especializado em escrever código Delphi novo seguindo rigorosamente
  todos os padrões de codificação. Use quando o usuário pedir para criar: nova
  classe, unit, serviço, repositório, formulário, interface ou qualquer elemento
  de código Delphi do zero. Exemplos: <example>Context: Usuário quer uma nova
  classe de serviço. user: "Crie um serviço de pedidos em Delphi" assistant:
  "Vou usar o delphi-writer para criar o serviço com todos os padrões aplicados."
  <commentary>Criação de código novo — invocar delphi-writer.</commentary>
  </example>
model: inherit
---

Você é um desenvolvedor Delphi sênior que internalizou completamente os padrões
de codificação do Delphi Style Guide e Clean Code. Você escreve código como
um profissional de alto nível — limpo, focado e pronto para produção.

## Regras Invioláveis

Você NUNCA produz código que:
- Use `with`, `Break`, `Continue` ou `goto`
- Tenha variáveis globais (use `class var`)
- Use notação húngara (`sNome`, `iCount`)
- Tenha métodos com mais de 50 linhas sem refatoração
- Misture responsabilidades em uma mesma classe
- Concatene SQL com input do usuário
- Libere dois recursos no mesmo `try..finally`
- Use `const` em parâmetros de interface (ARC)
- Deixe blocos `except` vazios
- Use `Real` como tipo de ponto flutuante

## Processo de Escrita

### 1. Entender antes de escrever
- Clarificar a responsabilidade única do elemento
- Identificar dependências e interfaces necessárias
- Definir nome e estrutura antes do código

### 2. Esboço → Código completo
Sempre apresentar a estrutura antes do código final:
```
Proposta: TClienteService
- Implementa: IClienteService
- Depende de: IClienteRepository (injeção de dependência)
- Responsabilidade: regras de negócio do cliente
- Métodos: BuscarPorCodigo, Salvar, Excluir, ValidarDados
```

### 3. Código completo e funcional
Nunca entregue código parcial ou com `// TODO`. O código deve compilar.

## Padrões Aplicados Automaticamente

- Prefixos: F (fields), A (params), L (locals), C_ (constants)
- Tipos: T (classes), I (interfaces), E (exceptions)
- Escopos: strict private → private → protected → public
- `begin` em linha própria, `else` em linha própria
- 2 espaços de indentação, 120 chars de margem
- Uses organizado: RTL → VCL/FMX → FireDAC → Third-party → Projeto
- `Result` direto em functions
- Guard clauses com Exit no início
- try..finally por recurso
- DTOs para 4+ parâmetros
```

**Step 2: Commit**

```bash
git add agents/delphi-writer.md
git commit -m "feat: add delphi-writer agent"
```

---

## Task 13: Commands

**Files:**
- Create: `commands/audit.md`
- Create: `commands/review.md`
- Create: `commands/write.md`
- Create: `commands/new-project.md`

**Step 1: Criar `commands/audit.md`**

```markdown
---
description: Gera laudo técnico profissional completo de um projeto Delphi
---

Execute um laudo técnico completo do projeto Delphi atual ou dos arquivos
informados pelo usuário.

Siga este protocolo:

1. **Levantamento** — colete: versão do Delphi, banco de dados, componente de
   acesso, tipo do sistema, objetivo do laudo (modernização / auditoria / venda).

2. **Análise** — avalie as 8 dimensões: Arquitetura, Clean Code, Code Smells,
   SOLID, Memória, Acesso a Dados, Segurança, Manutenibilidade.

3. **Pontuação** — score 1–5 por dimensão, média ponderada final.
   - 4,0–5,0 = 🟢 BOM
   - 3,0–3,9 = 🟡 REGULAR
   - 2,0–2,9 = 🟠 CRÍTICO
   - 1,0–1,9 = 🔴 INVIÁVEL

4. **Relatório** — gere o laudo completo com: resumo executivo, análise por
   dimensão, pontos críticos, recomendações (imediatas / curto / médio / estratégico)
   e estimativa de esforço para modernização.

Use o agente `delphi-auditor` para análise profunda quando disponível.
Carregue a skill `delphi-laudo` para estrutura detalhada do laudo.
```

**Step 2: Criar `commands/review.md`**

```markdown
---
description: Revisão rápida de código Delphi — detecta violações de padrão e sugere correções
---

Realize uma revisão rápida do código Delphi fornecido pelo usuário.

Analise e reporte:

**Nomenclatura**
- Prefixos corretos (F, A, L, C_, T, I, E)?
- Notação húngara detectada?
- Nomes significativos ou abreviações obscuras?
- Componentes renomeados corretamente?

**Formatação**
- Indentação de 2 espaços (sem TAB)?
- Margem de 120 chars respeitada?
- `begin` em linha própria?
- `else` em linha própria?

**Comandos Proibidos**
- Uso de `with`?
- `Break` ou `Continue` em loops?
- `Exit` fora de guard clause?
- `Real` como tipo?
- SQL concatenado (SQL Injection)?

**Estrutura**
- Métodos com mais de 50 linhas?
- Múltiplas responsabilidades na mesma classe/método?
- try..finally com múltiplos recursos?
- `const` em parâmetros de interface?

**Formato de saída:**

Para cada violação encontrada:
- Linha aproximada e descrição do problema
- Classificação: 🚨 CRÍTICO / ⚠️ ATENÇÃO / 💡 SUGESTÃO
- Exemplo correto de como deveria ser escrito
```

**Step 3: Criar `commands/write.md`**

```markdown
---
description: Inicia sessão de escrita de código Delphi novo com todos os padrões aplicados
---

Inicie uma sessão de escrita de código Delphi padronizado.

**Passo 1 — Entender o que será criado**

Pergunte ao usuário (se não informado):
- Tipo de elemento: classe de serviço / repositório / model / form / unit utilitária / interface?
- Responsabilidade principal (deve ser única — SRP)?
- Implementa alguma interface? Qual?
- Depende de outras classes? Quais serão injetadas?
- Nome sugerido ou você pode propor?

**Passo 2 — Propor estrutura**

Antes de escrever código, apresente:
```
Proposta: [TNomeClasse]
- Herda de: [TInterfacedObject / TObject]
- Implementa: [INomeInterface]
- Injeta via construtor: [IOutraDependencia]
- Responsabilidade: [uma frase]
- Métodos públicos: [lista]
```

Aguarde aprovação antes de continuar.

**Passo 3 — Escrever código completo**

Gere o código completo, compilável, com:
- Unit header com uses organizado
- Seção interface completa
- Seção implementation com todos os métodos
- Todos os padrões aplicados automaticamente

Use o agente `delphi-writer` para implementação.
```

**Step 4: Criar `commands/new-project.md`**

```markdown
---
description: Scaffold de novo projeto Delphi com estrutura de pastas e arquivos base padronizados
---

Crie a estrutura inicial de um novo projeto Delphi seguindo boas práticas de arquitetura.

**Passo 1 — Levantar requisitos**

Pergunte ao usuário:
- Nome do sistema/projeto?
- Tipo: VCL / FMX / REST API / Library?
- Banco de dados? Componente de acesso?
- Arquitetura desejada: simples (Form + DataModule) / camadas (Model, Service, Repository)?
- Frameworks de terceiros? (ACBr, Horse, FastReport, etc.)

**Passo 2 — Propor estrutura de pastas**

Exemplo para projeto em camadas:
```
NomeProjeto/
├── src/
│   ├── model/          # entidades e DTOs
│   ├── interfaces/     # contratos (IService, IRepository)
│   ├── service/        # regras de negócio
│   ├── repository/     # acesso a dados
│   ├── presentation/   # forms e frames
│   └── shared/         # utilitários, constantes, exceções
├── tests/              # DUnitX
├── docs/
└── NomeProjeto.dproj
```

**Passo 3 — Gerar arquivos base**

Crie os arquivos iniciais:
- `NomeProjeto.dpr` — project file limpo
- Unit de exceções customizadas do domínio
- Interface base de repositório (ICRUDRepository)
- DataModule de conexão (se aplicável)
- Form principal com nomenclatura correta

Aplique todos os padrões do Delphi Style Guide em cada arquivo gerado.
```

**Step 5: Commit**

```bash
git add commands/
git commit -m "feat: add all commands (audit, review, write, new-project)"
```

---

## Task 14: README Bilíngue

**Files:**
- Create: `README.md`

**Step 1: Criar README completo**

```markdown
# delphi-dev — Claude Code Plugin

> 🇧🇷 [Português](#português) | 🇺🇸 [English](#english)

---

## Português

### O que é

Plugin para Claude Code que transforma o assistente em um especialista sênior
em Delphi. Ao detectar código Delphi, o Claude aplica automaticamente os padrões
do Delphi Style Guide, Clean Code e boas práticas — sem precisar ser solicitado.

### Funcionalidades

| Recurso | Descrição |
|---|---|
| **Modo Delphi Automático** | Ao abrir qualquer `.pas`, `.dpr` ou `.dfm`, o Claude já conhece e aplica todos os padrões |
| **`/delphi-audit`** | Gera laudo técnico profissional completo com score e recomendações |
| **`/delphi-review`** | Revisão rápida de código — detecta violações e sugere correções |
| **`/delphi-write`** | Escreve código novo com todos os padrões aplicados automaticamente |
| **`/delphi-new`** | Scaffold de novo projeto com estrutura de pastas padronizada |

### Instalação

**Via GitHub (antes do marketplace):**
```bash
/plugin install delphi-dev@github:adrianosantostreina/delphi-dev
```

**Via marketplace Claude Code (quando disponível):**
```bash
/plugin install delphi-dev
```

### Padrões Aplicados

- ✅ Prefixos obrigatórios: `F` (fields), `A` (params), `L` (locals), `C_` (constantes)
- ✅ Prefixos de tipo: `T` (classes), `I` (interfaces), `E` (exceções)
- ✅ CamelCase em todos os identificadores
- ✅ Indentação de 2 espaços, margem de 120 caracteres
- ✅ `begin` e `else` em linhas próprias
- ✅ Comandos proibidos: `with`, `Break`, `Continue`, `Real`
- ✅ `Exit` apenas em guard clauses
- ✅ `const` correto (nunca em interfaces — ARC)
- ✅ try..finally por recurso (nunca dois recursos no mesmo bloco)
- ✅ SQL sempre parametrizado
- ✅ Prefixos de componentes: `btn`, `edt`, `lbl`, `grd`, `qry`, etc.
- ✅ Uses organizada: RTL → VCL/FMX → Third-party → Projeto

### Baseado em

- Normas e Padronização de Codificação Delphi v4.0.1 — Adriano Santos
- Código Limpo e Boas Práticas em Delphi — Adriano Santos
- Clean Code — Robert C. Martin
- Delphi Style Guide — Embarcadero

### Licença

MIT © 2026 Adriano Santos

---

## English

### What is it

A Claude Code plugin that turns the assistant into a senior Delphi expert.
When Delphi code is detected, Claude automatically applies the Delphi Style Guide,
Clean Code principles, and best practices — without being asked.

### Features

| Feature | Description |
|---|---|
| **Auto Delphi Mode** | Opening any `.pas`, `.dpr`, or `.dfm` file activates full coding standards context |
| **`/delphi-audit`** | Generates a complete professional technical audit with scoring and recommendations |
| **`/delphi-review`** | Quick code review — detects violations and suggests corrections |
| **`/delphi-write`** | Writes new code with all standards automatically applied |
| **`/delphi-new`** | Scaffolds a new project with standardized folder structure |

### Installation

**Via GitHub (before marketplace):**
```bash
/plugin install delphi-dev@github:adrianosantostreina/delphi-dev
```

**Via Claude Code marketplace (when available):**
```bash
/plugin install delphi-dev
```

### Standards Applied

- ✅ Mandatory prefixes: `F` (fields), `A` (params), `L` (locals), `C_` (constants)
- ✅ Type prefixes: `T` (classes), `I` (interfaces), `E` (exceptions)
- ✅ CamelCase for all identifiers
- ✅ 2-space indentation, 120-character margin
- ✅ `begin` and `else` on their own lines
- ✅ Prohibited commands: `with`, `Break`, `Continue`, `Real`
- ✅ `Exit` only as guard clauses
- ✅ Correct `const` usage (never on interfaces — ARC)
- ✅ One resource per try..finally block
- ✅ Always parameterized SQL (no string concatenation)
- ✅ Component prefixes: `btn`, `edt`, `lbl`, `grd`, `qry`, etc.
- ✅ Organized uses clause: RTL → VCL/FMX → Third-party → Project

### Based on

- Delphi Coding Standards v4.0.1 — Adriano Santos
- Clean Code and Best Practices in Delphi — Adriano Santos
- Clean Code — Robert C. Martin
- Delphi Style Guide — Embarcadero

### License

MIT © 2026 Adriano Santos
```

**Step 2: Commit**

```bash
git add README.md
git commit -m "docs: add bilingual README (PT-BR + EN)"
```

---

## Task 15: Verificação Final

**Step 1: Verificar estrutura completa**

```bash
find "D:/2.2 GitHub Adriano Santos/delphi-dev" -not -path "*/.git/*" | sort
```

Esperado:
```
.claude-plugin/plugin.json
agents/delphi-auditor.md
agents/delphi-writer.md
commands/audit.md
commands/new-project.md
commands/review.md
commands/write.md
docs/plans/2026-03-09-delphi-dev-design.md
docs/plans/2026-03-09-delphi-dev-implementation.md
LICENSE
README.md
skills/delphi-laudo/references/clean-code-delphi.md
skills/delphi-laudo/references/code-smells-delphi.md
skills/delphi-laudo/references/estimativas-modernizacao.md
skills/delphi-laudo/references/estrutura-laudo.md
skills/delphi-laudo/references/style-guide.md
skills/delphi-laudo/references/tecnologias-delphi.md
skills/delphi-laudo/SKILL.md
skills/delphi-standards/references/classes-structure.md
skills/delphi-standards/references/component-prefixes.md
skills/delphi-standards/references/forbidden-commands.md
skills/delphi-standards/references/formatting.md
skills/delphi-standards/references/naming-conventions.md
skills/delphi-standards/SKILL.md
skills/delphi-write/SKILL.md
```

**Step 2: Verificar git log**

```bash
cd "D:/2.2 GitHub Adriano Santos/delphi-dev" && git log --oneline
```

Esperado: 14+ commits limpos.

**Step 3: Commit final do plano de implementação**

```bash
git add docs/plans/2026-03-09-delphi-dev-implementation.md
git commit -m "docs: add implementation plan"
```

---

## Próximos Passos (Pós-Implementação)

1. **Instalar localmente para testar:**
   ```bash
   /plugin install delphi-dev@local:"D:/2.2 GitHub Adriano Santos/delphi-dev"
   ```

2. **Criar repositório no GitHub:**
   - `https://github.com/new` → nome: `delphi-dev`
   - Público, sem README (já temos)

3. **Push:**
   ```bash
   git remote add origin https://github.com/adrianosantostreina/delphi-dev.git
   git push -u origin master
   ```

4. **Submeter ao marketplace:**
   - `https://clau.de/plugin-directory-submission`
