# Custo da análise de churn

## Como estimei
- Texto em português: 600 palavras ≈ 1.000 tokens (regra do enunciado). Medi o tamanho de cada prompt e da resposta esperada e fiz o cálculo antes de enviar cada um.
- Preços usados para o Claude Sonnet: **US$ 3 por milhão de tokens de entrada e US$ 15 por milhão de saída**. Como a tabela de preços muda, vale conferir o valor atual antes de usar esses números para orçamento.
- Fórmula: custo = (tokens de entrada × 3 + tokens de saída × 15) / 1.000.000.

## Tabela de custo

| Etapa | Ferramenta | Modelo | Tokens entrada (est.) | Tokens saída (est.) | Custo (USD) | Justificativa do modelo |
|---|---|---|---:|---:|---:|---|
| Formulação do problema | Nenhuma (trabalho próprio) | n/a | 0 | 0 | 0,0000 | Feita à mão antes de abrir a IA, de propósito. |
| EDA e segmentação | Claude | Sonnet | 1.400 | 1.000 | 0,0192 | Precisa cruzar variáveis e levantar hipóteses. Um modelo menor costuma ficar superficial. |
| Aprofundamento por segmento | Claude (mesma conversa) | Sonnet | 2.600 | 900 | 0,0213 | Entrada maior porque carrega o histórico. Tem cálculo financeiro e o risco de erro é maior. |
| Geração do relatório executivo (inclui as 3 versões) | Claude | Sonnet | 1.300 | 1.100 | 0,0204 | O texto vai para o board, e a qualidade da escrita importa. |
| **Total** | | | **5.300** | **3.000** | **≈ 0,061** | |

Conferência dos cálculos: EDA 1.400×3 + 1.000×15 = 4.200 + 15.000 = 19.200 → US$ 0,0192. Aprofundamento 7.800 + 13.500 = 21.300 → US$ 0,0213. Relatório 3.900 + 16.500 = 20.400 → US$ 0,0204. Soma: US$ 0,0609.

(Obs.: a coluna de saída soma 3.000 tokens: 1.000 + 900 + 1.100.)

Observação: não incluí as conferências que fiz à mão (recálculos, Matriz de Confiança) porque não geraram chamadas de IA. Se eu tivesse refeito alguma chamada por causa de um erro, o custo seria somado.

## Pergunta de reflexão

**Custo se a análise fosse repetida toda semana.** Uma execução custa ~US$ 0,061. Com ~4,3 semanas por mês, são cerca de **US$ 0,26 por mês** (~US$ 3,2 por ano). Quase nada, e mesmo com 10 vezes mais dados de entrada o custo continuaria abaixo de US$ 3 por mês. O custo de tokens, portanto, não é o que decide se a análise vale a pena. O que pesa é o tempo de validação humana de cada resultado.

**O modelo se justifica em todas as etapas?** Não em todas. Penso assim:
- **Formulação:** sem IA, e foi a decisão certa, porque a direção do problema tem de vir de quem entende o negócio.
- **Aprofundamento financeiro e relatório:** mantenho o Sonnet. Foi exatamente aqui que o modelo inventou premissas (36 meses de vida do cliente, 25% de retenção), e um modelo mais fraco provavelmente erraria mais. O erro nesses pontos custaria muito mais do que os centavos economizados.
- **EDA em rodadas semanais:** depois da primeira vez, o roteiro já está definido e o trabalho vira atualizar números e comparar com a semana anterior. Aí daria para usar um modelo mais econômico (classe Haiku, a ~US$ 1/US$ 5 por milhão) sem perda de qualidade, e a validação humana continuaria a mesma. Estimo que isso reduziria o custo semanal para ~US$ 0,034 (cerca de US$ 0,15 por mês), com o Sonnet só no relatório.

Conclusão: o modelo escolhido se justifica onde há cálculo e comunicação para o board. A economia possível está nas rodadas de monitoramento, e em ordem de grandeza ela é pequena, então eu priorizaria a qualidade.
