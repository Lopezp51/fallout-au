---
description: Preservação de estatísticas do plugin Keep the Rhythm e manipulação de arquivos no Obsidian
always_on: true
---

# Regra do Workspace: Integridade do Plugin "Keep the Rhythm"

Este cofre do Obsidian utiliza o plugin comunitário **Keep the Rhythm** (configurado em `.obsidian/plugins/keep-the-rhythm/`), responsável por registrar as metas diárias de escrita, histórico de produtividade, sequência (streak) e calorias de palavras.

## 1. O Problema do Rastreamento por Caminho de Arquivo
* O plugin associa as palavras escritas ao caminho exato do arquivo (`filePath`).
* Se arquivos forem renomeados, movidos de pasta ou reorganizados externamente (por scripts, terminal ou comandos de sistema) enquanto o Obsidian está ativo, o plugin interpreta a alteração como a **exclusão do arquivo original**, gerando registros negativos de palavras (ex: `-6.000 words`) e riscando os títulos no painel *Entries Today*.

## 2. Diretrizes Obrigatórias para Qualquer Agente
Sempre que for criar, renomear, mover ou reorganizar arquivos neste cofre:

1. **Configuração de Segurança:** Garantir que no arquivo `.obsidian/plugins/keep-the-rhythm/data.json` a propriedade `"ignoreDeletedFiles"` permaneça definida como `true`.
2. **Atualização de Caminhos após Mover Arquivos:** Sempre que mover arquivos de diretório, atualize as entradas em `data.json` (`stats.dailyActivity`) substituindo o caminho antigo pelo novo caminho relativo, sem registrar eventos de subtração de palavras.
3. **Preservação de Métricas Positivas:** Jamais permitir que reorganizações estruturais reduzam o saldo diário de palavras ou quebrem a sequência (*streak*) do usuário.
