---
name: storyline-codex-vault
description: >-
  Use this skill whenever creating, modifying, validating, or auditing Codex entries
  within the StoryLine plugin (Characters, Locations, Items, Creatures, Lore, Organizations,
  Culture, Systems, Worldbuilding) or managing metadata frontmatter and bi-directional
  wikilinks in the Fallout AU vault (folders 00 to 99).
---

# StoryLine Codex & Vault Management Guide

Este guia documenta o padrão técnico de gerenciamento do **StoryLine Codex** e das notas de cânone/apoio (pastas `00` a `99`) no vault **Fallout AU**. Use estas diretrizes para manter a consistência do grafo, a compatibilidade com a UI do StoryLine e a rastreabilidade total de dados.

---

## 1. Estrutura de Pastas e Tipos Oficiais do StoryLine

O projeto principal ativo está localizado em:
`StoryLine/Fallout - Arco 1/`

Todas as entidades do Codex residem em:
`StoryLine/Fallout - Arco 1/Codex/<Categoria>/`

### Regras Críticas de `type` (YAML Frontmatter)
> [!IMPORTANT]
> O plugin StoryLine diferencia estritamente **singular** e **plural** em `type`. Um valor incorreto faz a nota não ser reconhecida na categoria correta da barra lateral do plugin.

| Categoria | Pasta no Codex | `type` Exato | Principais Campos Nativos Suportados |
| :--- | :--- | :--- | :--- |
| **Characters** | `Characters/` | `character` | `name`, `nickname`, `age`, `role`, `occupation`, `residency`, `appearance`, `distinguishingFeatures`, `style`, `quirks`, `personality`, `internalMotivation`, `externalMotivation`, `strengths`, `flaws`, `fears`, `belief`, `misbelief`, `formativeMemories`, `startingPoint`, `goal`, `expectedChange`, `habits`, `props`, `tagline`, `relations`, `relationHistory`, `custom` |
| **Locations** | `Locations/` | `location` | `name`, `world`, `parent`, `description`, `aliases`, `caseSensitive`, `excludeTerms` |
| **Items** | `Items/` | `items` | `name`, `itemType`, `description`, `origin`, `history`, `owner`, `previousOwners`, `properties`, `limitations`, `significance` |
| **Creatures** | `Creatures/` | `creatures` | `name`, `creatureType`, `description`, `habitat`, `diet`, `abilities`, `weaknesses`, `behavior`, `mythology` |
| **Lore** | `Lore/` | `lore` | `name`, `loreType`, `description`, `fullText`, `sources`, `significance`, `relatedEntries` |
| **Organizations**| `Organizations/`| `organizations` | `name`, `orgType`, `description`, `leadership`, `members`, `goals`, `methods`, `founded`, `history` |
| **Culture** | `Culture/` | `culture` | `name`, `description`, `traditions`, `taboos`, `socialStructure`, `values`, `arts`, `language` |
| **Systems** | `Systems/` | `systems` | `name`, `systemType`, `description`, `rules`, `limitations`, `practitioners`, `impact` |
| **Worldbuilding**| `Worldbuilding/`| `worldbuilding` | `name`, `description`, campos padrão de linking |

---

## 2. Padrões Universais do Codex

### A. Metadados Básicos & Linking
Todos os arquivos no Codex devem conter:
```yaml
created: YYYY-MM-DD
modified: YYYY-MM-DD
entryType: <Rótulo ou Sub-tipo>   # Ex: Protagonista, Veículo, Refinaria, Cidade
aliases:                           # Opcional, exceto em Characters que usa nickname
  - Outro Nome
caseSensitive: false               # Padrão: false para autolink em cenas
excludeTerms: ""                   # Termos ambíguos para não gerar falso link
```

### B. Mídia
```yaml
image: StoryLine/Fallout - Arco 1/Images/exemplo.jpeg
gallery:
  - path: StoryLine/Fallout - Arco 1/Images/exemplo.jpeg
    caption: Descrição da foto
```

### C. Relações entre Entidades
Para ligar personagens e entidades, use a lista `relations`:
```yaml
relations:
  - category: family   # family, ally, enemy, colleague, etc.
    type: parent       # parent, child, spouse, friend, mentor, etc.
    target: "[[Nome do Alvo]]"
```

---

## 3. Padrão de Metadados para as Pastas 00 a 99 (Fora do StoryLine)

As pastas numeradas contêm pesquisas, linhas do tempo, dossiês densos e capítulos. Para integrá-las ao grafo do Obsidian e aos registros do Codex, todo arquivo dessas pastas deve ter o seguinte frontmatter:

```yaml
---
tipo: dossie              # dossie, timeline, worldbuilding, pesquisa, capitulo, rascunho
status: ativo             # ativo, em_revisao, canonico, arquivo
arco: "[[Fallout - Arco 1]]"
periodo_lore: "2047-2069" # Opcional: intervalo temporal dos eventos

# Links bidirecionais diretos para o Codex:
personagens:
  - "[[Texalia Veight]]"
locais:
  - "[[Condado de Garza]]"
itens:
  - "[[Maverick]]"
organizacoes:
  - "[[Petro-Chico]]"

# Link para a nota equivalente do Codex (quando aplicável):
codex_ref: "[[Texalia Veight]]"

tags:
  - fallout-au/arco1
  - canon/dossie
aliases:
  - Nome Alternativo para Busca
---
```

---

## 4. Checklist de Consistência e Auditoria

Sempre que criar ou atualizar uma nota no vault, verifique:
1. **O `type` está correto?** (ex: `items`, `creatures`, `organizations`, `systems` no plural; `character`, `location`, `lore`, `culture` no singular).
2. **Os links foram escritos no formato `[[Nome]]`?** O vault usa `writeFieldsAsWikilinks: true`. Nunca use links estáticos de texto puro quando o alvo for uma nota existente.
3. **A nota do Codex possui equivalente em 00-99?** Se houver um dossiê expandido (ex: `Dossie Maverick.md`), referencie-o ou garanta que os detalhes essenciais estejam sincronizados.
4. **Links Quebrados:** Certifique-se de que os nomes entre `[[...]]` correspondam exatamente ao nome do arquivo sem extensão ou que haja um alias configurado.

---

## 5. Coesão Narrativa e Comunicação Obrigatória de Inconsistências

> [!IMPORTANT]
> **O usuário preza profundamente pela coesão da história.** Nenhuma contradição factual, temporal ou relacional deve passar despercebida ou ser alterada silenciosamente.

Ao trabalhar em **arquivos novos** ou atualizar arquivos existentes:

1. **Cruzamento de Palavras e Fatos (Cross-Check)**:
   - Pesquise termos-chave, datas, eventos e nomes em outros arquivos do vault (especialmente nas pastas `01 - Linhas do Tempo & Cânone`, `02 - Personagens (Codex)` e `03 - Worldbuilding & Tecnologia`).
   - Verifique se o que está sendo dito no novo arquivo não contradiz o cânone estabelecido.

2. **Validação de Entidades Associadas**:
   - Analise se os itens, personagens, facções e locais vinculados nas propriedades fazem sentido narrativo naquele momento específico (ex: cronologia de posse do Maverick, idade da Texalia em determinado ano, relacionamentos familiares/afetivos, se um personagem já havia falecido, etc.).

3. **Uso de Tags e Linha do Tempo**:
   - Utilize as tags e o campo `periodo_lore` para garantir que o evento se encaixa exatamente no arco correto (`Arco 1: 2047-2069`, `Arco 2: 2070-2077`, `Pós-Guerra: 2287+`).

4. **Comunicação Proativa com o Usuário**:
   - Se for identificada **qualquer inconsistência**, divergência de datas, contradição de personalidade/atitude ou conflito de fatos entre notas:
     - **Avise o usuário imediatamente.**
     - Destaque com precisão onde está a divergência (ex: *"Na nota X consta ano 2060, mas na linha do tempo Y consta 2062"*).
     - Apresente opções de harmonização e aguarde a decisão do usuário antes de consolidar a mudança.

---

## 6. Pilares Canônicos Fundamentais do Fallout AU

Estes pontos são **regras canônicas absolutas** deste universo e devem ser estritamente respeitados em qualquer criação ou revisão:

1. **Protagonista**:
   - É **Texalia "Tex" Veight** (nunca usar "Nora", que não existe como protagonista nesta história).
2. **O Filho (Jessie, NUNCA Shaun)**:
   - O filho de Texalia Veight e Nate Howard é **Jessie** (batizado em homenagem viva ao avô Jesse Veight).
   - **Shaun NÃO existe neste universo.**
   - O garotinho sequestrado por Kellogg no Vault 111 é Jessie. A busca da Texalia no pós-guerra é exclusivamente por **Jessie**.
3. **Núcleo Familiar de Origem**:
   - **Clara Veight** (mãe): Rigorosa, auditora na Petro-Chico, faleceu de fibrose química quando Tex tinha 13 anos.
   - **Jesse Veight** (pai): Mestre mecânico afetuoso da *Veight Auto*, ensinou a Tex mecânica bruta de motores V8.
4. **O Veículo Símbolo**:
   - **Ford Maverick 1974 V8**: Veículo analógico a combustão, reconstruído por Jesse e Texalia, mantido no pré-guerra em Boston com gasolina racionada.

