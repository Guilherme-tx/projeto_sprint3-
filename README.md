# Projeto Sprint 3: Análise de churn do Beta Bank com IA

Projeto Sprint 3. O Beta Bank perdeu 2.080 dos 10.000 clientes em 2024 (churn de 20,8%). A ideia foi usar ferramentas de IA, com validação crítica, para chegar a um relatório que o board consiga ler e usar para decidir.

## Resultado em uma frase
O grupo mais crítico é formado por clientes com mais de 50 anos, saldo acima de €100 mil e inativos: 487 clientes, com cerca de 70% de churn e ~€1,02 milhão por ano de receita em risco. A recomendação é um piloto de reativação de 90 dias, que custa cerca de €29 mil.

## Arquivos

| Arquivo | O que contém |
|---|---|
| [analise_processo.md](analise_processo.md) | Formulação do problema, prompts (Persona, Contexto, Objetivo, Formato), outputs, Sanity Check, Reverse Validation e Matriz de Confiança |
| [relatorio_executivo.md](relatorio_executivo.md) | Relatório de uma página para o board, com nota de confiança, e as versões para o gerente de relacionamento e para a equipe de dados |
| [custo_analise.md](custo_analise.md) | Estimativa de custo de tokens por etapa, justificativa dos modelos e reflexão sobre uso semanal |

## Observações
- A análise usa as estatísticas do relatório interno e a estrutura do CSV (primeiras linhas). Não foi feito processamento do CSV completo.
- O CSV com os dados dos clientes não está neste repositório.
- As estimativas em euros dependem de premissas (meta de retenção de 30%, custo de €60 por cliente) e devem ser validadas com dados financeiros reais.
