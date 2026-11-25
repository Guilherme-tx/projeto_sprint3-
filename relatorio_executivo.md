# Relatório executivo: Churn no Beta Bank

## Para o board

**Conclusão:** o Beta Bank perde 1 em cada 5 clientes por ano, e a maior perda de valor está num grupo pequeno e identificável. Propomos um piloto de reativação de 90 dias, que custa cerca de €29 mil e pode preservar ~€306 mil por ano.

**1. Situação atual.** Em 2024, 2.080 dos 10.000 clientes encerraram a conta (20,8%). Quase 8 em cada 10 saídas (79%) vieram de clientes inativos, ou seja, clientes que pararam de usar o banco antes de ir embora.

**2. O que descobrimos.** O grupo mais crítico reúne clientes com mais de 50 anos, saldo acima de €100 mil e sem atividade recente: são 487 clientes e cerca de 70% deles saem, mais de três vezes a média do banco. O que mais pesa é a inatividade, seguida da idade e do número de produtos (clientes com um só produto saem mais, e quem tem dois produtos é o mais estável, com 5,2%). Salário, score de crédito e tempo de casa não explicam as saídas.

**3. Impacto financeiro (estimativa).** Cerca de 341 clientes desse grupo devem sair, o que representa **€1,02 milhão por ano** de receita (341 × €250 × 12). Repor esses clientes custaria outros €61 mil em aquisição.

**4. Recomendação.** Um piloto de 90 dias com os 487 clientes: contato direto do gerente de relacionamento, revisão do portfólio e oferta de um segundo produto, com um grupo de controle para medir o efeito. Como meta, reter 30% (~102 clientes), o que preservaria ~€306 mil por ano. Custo estimado do piloto: €29 mil (€60 por cliente, estimativa nossa).

**5. Decisão necessária do board.** Aprovar o orçamento de ~€29 mil e o início do piloto no próximo trimestre. Se a meta de 30% se confirmar, voltamos ao board com a proposta de estender a ação aos 2.218 clientes com um produto e inativos (~45% de churn).

**Nota de confiança.** Este relatório foi estruturado com suporte de IA generativa a partir dos dados resumidos fornecidos pelo Beta Bank. As estimativas de impacto financeiro são projeções baseadas no ticket médio e custo de aquisição informados, devem ser tratadas como indicativas e validadas contra os dados reais do sistema financeiro antes de embasar decisões de orçamento. A análise foi conduzida sobre um resumo estatístico da base, não sobre os dados individuais dos clientes.
Limitações específicas: (a) a meta de 30% e o custo de €60 por cliente são hipóteses a validar no piloto; (b) assumimos que cada cliente perdido geraria receita por 12 meses; (c) os perfis de risco se sobrepõem e não devem ser somados; (d) os números mostram associação e não causa; (e) a receita mensal total do relatório interno (€1,984 M) não bate com 10.000 × €250 (€2,5 M), e isso precisa ser explicado pela área financeira. Confiança geral: **alta** na priorização do grupo, **média** nos valores em euros.

## Para o gerente de relacionamento

Nos próximos 90 dias você vai ligar para uma lista de 487 clientes que têm mais de 50 anos, saldo acima de €100 mil e estão sem atividade. Cerca de 7 em cada 10 saem se ninguém fizer nada. A ligação não é para vender: é para entender se o cliente ainda quer o banco, revisar com ele os produtos que tem e, quando fizer sentido, apresentar um segundo produto (quem tem dois produtos é o perfil que menos sai). Comece pelos clientes da Alemanha, onde o churn é maior. Registre o resultado de cada contato (reativou, está avaliando, vai sair) e o motivo que o cliente disser. Meta: segurar 30% dos clientes da lista, cerca de 100, e a sua anotação dos motivos é o que vai nos dizer se a hipótese de concorrência ou de atendimento está certa. Metade da lista fica como grupo de controle, sem contato, para medirmos o efeito real.

## Para a equipe de dados

Acompanhem mensalmente: churn geral (referência: 20,8% ao ano, ou ~1,7% ao mês), churn do segmento crítico (acima de 50 anos, saldo acima de €100 mil, inativos; base de 487 clientes, ~70%), proporção de inativos na base (hoje 48,5%) e distribuição por número de produtos. Propomos como alertas: churn mensal geral acima de 2,0%; proporção de inativos acima de 52%; e qualquer queda de produtos por cliente nos clientes de saldo alto. Esses limites são sugestões, e vale revisá-los depois dos 3 primeiros meses. Pendências: reproduzir os perfis de risco no CSV completo (os números do relatório são estimados), checar a sobreposição entre os três segmentos, investigar os 100% de churn em clientes com 3 e 4 produtos (326 clientes, suspeito de erro de dados), reconciliar a receita total com o financeiro e, depois, avaliar um modelo preditivo com os dados individuais.
