Registro de análise com IA – Churn Beta Bank

**Etapa 1 – Formulação do problema**

O Beta Bank perdeu 2.080 dos seus 10.000 clientes em 2024, ou seja 20,8%. O problema de negócio não é entender o churn. É descobrir quem está saindo, quanto isso custa e onde vale a pena agir primeiro, porque o banco não tem orçamento nem equipe para tentar reter todo mundo ao mesmo tempo. Como reter é mais barato do que conquistar, a pergunta que o board precisa responder é onde investir para aumentar a retençāo.

Uma análise bem sucedida entrega três coisas: o perfil do cliente com maior risco, uma estimativa em euros do que se perde com ele e uma única ação que o board possa aprovar ou recusar. Não preciso de modelo preditivo e nem de lista de variáveis.

Perguntas que os dados precisam responder:
1 - Quais grupos de clientes saem muito acima da média de 20,8%?
2 - Quais variáveis realmente separam quem sai de quem fica, e quais não importam (salário, score e tempo de casa parecem não importar)?
3 - Quanto a receita anual do banco perde com o grupo mais crítico e quanto custaria repor esses clientes?
4 - Qual ação é concreta o bastante para ser testada em poucos meses?

**Etapa 2. Exploração dos dados**

Ferramenta: Claude Sonnet. Escolhi porque preciso de raciocínio sobre os números e de um texto em linguagem de negócio, não de execução de código. O CSV eu conferi à parte com Python, para ter um gabarito independente.

Prompt enviado: Persona, Contexto, Objetivo e Formato, adaptado do enunciado com os números do relatório interno

Você é um cientista de dados sênior especializado em análise de
risco de retenção no setor bancário.

Tenho um dataset com 10.000 clientes do Beta Bank. Taxa de churn
anual: 20,8% (2.080 saídas). Variáveis por cliente: pontuação de
crédito, país, gênero, idade, tempo de relacionamento, saldo,
número de produtos, posse de cartão, status de atividade e
salário estimado.

Dados do relatório interno:
- Churn por país: França 17,7%, Alemanha 33,2%, Espanha 14,6%
- Churn por idade: 18-30 8,7%; 31-40 12,3%; 41-50 17,0%;
  51-60 37,9%; acima de 60 65,9%
- Produtos: 1 produto 30,5%; 2 produtos 5,2%; 3 e 4 produtos 100%
- Saldo acima de €100 mil: 30,2%; sem saldo: 14,5%
- Inativos: 33,8% (≈79% de todos os cancelamentos); ativos: 8,9%
- Mulheres 31,3%; homens 12,3%
- Score de crédito, tempo de relacionamento e salário: sem
  relação relevante com o churn
- Cruzamentos: acima de 50 + saldo > €100 mil + inativo = 487
  clientes, churn ~70%; 1 produto + inativo = 2.218 clientes,
  ~45%; Alemanha + acima de 50 = 412 clientes, >55%

Amostra das primeiras linhas do CSV:

1	15414799	Dubois	663	France	Female	43	3	67146.18	1	0	0	107688.2	1		

2	15445942	Meyer	556	Spain	Female	37	2	24173.05	3	1	0	182209.78	1		

3	15222121	Oliveira	574	Germany	Female	32	1	0.0	1	1	0	199763.28	1		

4	15415413	Ferreira	405	Germany	Male	46	7	0.0	1	1	1	152155.0	0		

5	15139093	Robert	599	France	Male	46	2	27786.54	1	1	1	151296.95	0		

Faça uma análise exploratória. Identifique os três segmentos de
maior risco, as variáveis com maior poder explicativo e hipóteses
para o comportamento de cada segmento. Aponte também qualquer
dado que pareça inconsistente ou suspeito.

Formato: relatório com seções claras, linguagem de negócio, sem
estatística pesada.

Mudei duas coisas em relação ao prompt de referência do enunciado: usei 20,8% em vez de 20,4% (o relatório diz 20,8%) e acrescentei o pedido para apontar inconsistências.

Resumo do output: o modelo apontou como segmentos de risco (1) clientes acima de 50 anos, com saldo alto e inativos, (2) clientes com 1 produto e inativos, (3) clientes na Alemanha acima de 50 anos. Listou como variáveis mais relevantes idade, status de atividade, número de produtos, país e gênero, e descartou score, tempo de relacionamento e salário. Hipóteses levantadas: clientes mais velhos com saldo alto comparam ofertas de outros bancos; inativos já desengajaram antes de sair; alta saída na Alemanha pode vir de concorrência local ou de problema de produto.

**Sanity Check, o que aceitei, o que questionei e por quê:**
| Item | Decisão | Motivo |
|---|---|---|
| Perfis A, B e C | Aceito | Os números vêm direto do relatório e conferem com minha conta (487 × 70% ≈ 341 saídas; 2.218 × 45% ≈ 998; 412 × 55% ≈ 227). |
| Atividade, idade e produtos como variáveis centrais | Aceito | As diferenças são grandes. Inativos 33,8% contra ativos 8,9%. |
| "Vender mais produtos aumenta o churn" | **Questionado, não aceito** | 3 e 4 produtos somam só 326 clientes, com 100% de churn, o que é suspeito de artefato dos dados. Já 2 produtos têm o menor churn da base (5,2%). Concluir o oposto com base em 326 clientes seria erro. |
| "Score baixo ajuda a prever" | **Questionado, não aceito** | O relatório diz que o score não tem correlação forte e que o churn abaixo de 500 é só "ligeiramente superior". Tirei do texto. |
| Hipóteses de "concorrência" e "canais digitais" | Aceito como **hipótese**, não como fato | Nada nos dados confirma. Marquei como hipótese a testar com pesquisa de satisfação. |
| Gênero como driver | Aceito como fato descritivo, mas **não vou usar para segmentar a ação** | Feminino 31,3% contra masculino 12,3% é uma diferença grande, mas segmentar oferta por gênero tem risco regulatório e de reputação. |

**Etapa 3 – Aprofundamento por segmento**

**Segmento escolhido:** A (acima de 50 anos + saldo acima de €100 mil + inativo): 487 clientes, ~70% de churn.
Escolhi A e não B (que perde mais clientes em número, ~998) por três motivos: tem o maior churn, concentra clientes de maior saldo e é pequeno o suficiente para um piloto controlado. B fica como segunda fase.
**Ferramenta:** a mesma conversa do Claude (Sonnet), para o modelo manter o contexto do output anterior. Aqui o raciocínio numérico importa mais que na etapa 2, então não faria sentido trocar por um modelo menor.
**Custo estimado antes da chamada:** ~2.600 tokens de entrada (inclui o histórico da conversa) e ~900 de saída, cerca de US$ 0,02.
**Prompt encadeado:**
- **Persona:** você continua como o mesmo cientista de dados e agora também analista financeiro de um banco de varejo.
- **Contexto:** "Com base na sua análise anterior, vamos aprofundar o segmento A: 487 clientes (acima de 50 anos, saldo acima de €100 mil, inativos), churn de ~70%. Parâmetros financeiros: ticket médio de €250 por mês por cliente e custo de aquisição de €180 por novo cliente."
- **Objetivo:** (1) descrever o perfil completo desse grupo, (2) estimar clientes perdidos, receita anual perdida e custo de reposição, mostrando cada fórmula e (3) listar todas as premissas usadas. Não introduzir números que eu não tenha dado.
- **Formato:** perfil em tópicos curtos, depois uma tabela de cálculo com fórmula e resultado, e por último a lista de premissas.
**Output recebido (resumo):** perfil: clientes maduros, de alto saldo, sem movimentação recente, provavelmente os mais lucrativos por cliente. Cálculo: 487 × 70% = 341 saídas; 341 × €250 × 36 meses = ~€3,07 M; reposição de 341 × €180 = ~€61 mil; total ~€3,1 M. Citou ainda "taxa de retenção de mercado de 25%" para estimar o ganho de uma ação.

**Reverse Validation (conferi cada número contra o que eu forneci):**
| Número do output | Veio do contexto? | Ação |
|---|---|---|
| 487 clientes, 70% churn | Sim (relatório) | Mantido |
| 341 saídas (487 × 0,70 = 340,9) | Sim, cálculo | Mantido |
| €250/mês e €180 de CAC | Sim (enunciado) | Mantido |
| **36 meses de vida do cliente** | **Não, o modelo inventou** | Substituí por 12 meses (receita anual), que é o horizonte conservador e defensável. Receita perdida = 341 × €250 × 12 = **€1.023.000**. O valor de €3,07 M estava inflado quase 3x. |
| Reposição 341 × €180 = €61.380 | Sim, cálculo | Mantido |
| **Taxa de retenção de mercado de 25%** | **Não, o modelo inventou** | Descartada. Usei 30% como **meta hipotética de piloto**, dita assim no relatório. |
| Perfil "provavelmente os mais lucrativos" | Parcialmente | Mantido só como "saldo alto". O ticket de €250 é médio da base e não do segmento. |
**Números finais usados no relatório:**
- Saídas esperadas no segmento: 341.
- Receita anual em risco: €1.023.000 (341 × €250 × 12).
- Custo de reposição via aquisição: €61.380 (341 × €180).
- Meta de 30% de retenção: ~102 clientes (341 × 0,30 = 102,3), ou seja, 102 × €3.000 = **€306.000/ano** em receita preservada, mais €18.360 de aquisição evitada.
- Custo do piloto (hipótese minha, €60 por cliente × 487): **€29.220**.

**Etapa 5. Relatório executivo, validação e Matriz de Confiança**

**Ferramenta:** Claude (Sonnet), usando o prompt do enunciado com os campos preenchidos: churn de 20,8%; segmento A; principal fator preditivo: inatividade (79% das saídas) combinada com idade; impacto: €1,02 M/ano em receita em risco; ação: piloto de reativação de 90 dias para os 487 clientes. O prompt já tem os quatro pilares (persona de comunicação executiva, contexto dos dados, objetivo de relatório de uma página, formato com seções obrigatórias e tom).
**Custo estimado antes da chamada:** ~1.300 tokens de entrada e ~1.100 de saída (inclui as duas versões extras da etapa 6), cerca de US$ 0,02.
**Matriz de Confiança, o que verifiquei:**
| Afirmação do rascunho | Quadrante | Como verifiquei | Resultado |
|---|---|---|---|
| Churn de 20,8% (2.080 de 10.000) | Q1 (baixo risco, fácil de checar) | Comparei com o relatório | OK. O rascunho inicial trazia 20,4%, que era o erro do prompt de referência. Corrigi. |
| Receita anual em risco de €1,02 M | **Q3**: alto risco, verificável | Refiz a conta sozinho: 341 × 250 × 12 = 1.023.000 | OK |
| Custo de reposição de €61 mil | **Q3** | 341 × 180 = 61.380 | OK |
| Retorno de ~€306 mil com 30% de retenção | **Q3** | 102 × 3.000 = 306.000 | OK. O rascunho dizia "€310 mil" por arredondamento, e corrigi para o número exato. |
| Perfil: 487 clientes, ~70% de churn | **Q4**: alto risco, difícil de verificar | Triangulei com o relatório (cruzamento de 3 variáveis). Não consigo reproduzir sem processar o CSV completo. | Mantido, com a palavra "estimado" |
| "Clientes inativos concentram 79% das saídas" | Q4 | 4.849 × 33,8% = 1.639; 1.639/2.080 = 78,8% | OK |
| "Esses clientes saem por insatisfação com o atendimento" | Q4 | Nenhum dado sustenta | **Removida.** Troquei por "hipótese a validar". |
| "O piloto vai reduzir o churn em 30%" | Q4 | É uma meta, não uma previsão | Reescrita como meta do piloto |

**O que mudou depois da validação:** (1) 20,4% passou a 20,8%; (2) tirei a causa de "insatisfação"; (3) arredondamento de €310 mil para €306 mil; (4) acrescentei a limitação de que a análise usa um resumo estatístico; (5) acrescentei a divergência de receita encontrada nos dados (10.000 × €250 = €2,5 M por mês, mas o relatório diz €1,984 M).

**Técnicas da Matriz usadas e por quê:** Sanity Check na EDA (era o primeiro contato com o output e eu precisava filtrar o que contradizia os dados); Reverse Validation no cálculo financeiro (era onde o modelo mais tinha espaço para inventar premissas); recálculo independente e triangulação no rascunho final, como pede o enunciado para os quadrantes 3 e 4.

**Etapa 6. Adaptação para três audiências**

Tarefa de reescrita sobre um conteúdo já validado, feita na mesma chamada do relatório. Conferi que os três textos usam exatamente os mesmos números (487 clientes, 70%, €1,02 M, 30%, €306 mil). As versões estão em `relatorio_executivo.md`.

## Limitações do processo

Não processei o CSV completo. Usei as estatísticas do relatório e as primeiras linhas, como o enunciado permite. Os perfis de risco, portanto, são os do relatório e não foram recalculados por mim. Também não consegui explicar por que a receita total mensal do relatório (€1,984 M) não bate com 10.000 × €250.