# PROJETO 1 - FICHA TÉCNICA 

# Ficha Técnica: Projeto Análise de Dados 1

## Título do Projeto: **Segmentando “O Mercado”**

---

## Objetivo

Preparar a base de dados da empresa “O Mercado”, aplicar a segmentação de clientes através do RFM (Recência, Frequência e Valor Monetário), compreender o resultado da segmentação e tirar conclusões para tomada de decisão da empresa.
Analisar o perfil de clientes da empresa para traçar estratégias de fidelização através da personalização do serviço para manter e aumentar os rendimentos 

---

## Equipe

Débora Vasconcellos

---

## Ferramentas e Tecnologias

Google Sheets e Looker Studio

---

## Processamento e Análises

---

### Limpeza dos dados

***Tabela “transações”***:
7 casos de valores ausentes na variável “id_cliente”, por compreender que essa seria minha variável chave para junção dos dados, optei por excluí-los.

***Tabela “dados_cliente”***:
24 casos de valores ausentes na variável “salario_anual_dolar”, optei por substituir os campos vazios pelo valor da mediana ($51373) que alterou a moda (anteriormente $7500) passando a ser o valor das substituições, mas a média permaneceu estável. Também foi identificado na mesma variável um valor discrepante de “$666.666” que optei por excluir do banco de dados. Se tornando a variável “salario_anual_dolar_corrigida”.
3 casos de valores discrepantes na variável “ano_nascimento”, sendo clientes nascidos nos anos de 1893,1899 e 1900, o que resultaria em pessoas com mais de 120 anos. Optei pela exclusão dos dados.
10 casos de clientes que quando cruzados com a tabela de transações foram identificadas nenhuma movimentação, optei por excluí-los.

***Tabela “resumo_compra”***:
9 casos duplicados que foram retirados através da função de limpeza de dados do Google Sheets

---

### Novas variáveis/Segmentação RFM/Análise Coorte

### Variáveis Gerais

***“idade”***: Apesar do banco de dados ser referente ao ano de 2022, optei por calcular a idade com a fórmula “=YEAR=(TODAY())- (ano de nascimento)” para que o dado seja sempre atual quando a tabela for consultado.

***“faixa_etaria”***: Optei por dividir as faixar etárias em intervalos de +/-10 anos compreendendo que o consumo pode variar de acordo com o período geracional dos clientes. Estabeleci as seguintes faixas:

28-34 anos; 35-44 anos; 45-54 anos; 55-64 anos; 65-74 anos; 75 anos ou mais. (Como parâmetro aglutinei duas faixas da pirâmide etária estabelecida pelo IBGE: [Pirâmide etária | Educa | Jovens - IBGE](https://educa.ibge.gov.br/jovens/conheca-o-brasil/populacao/18318-piramide-etaria.html))

***“renda_faixa”***: Para facilitar a visualização das faixas de renda optei por dividir os dados em quartis estabelecendo os seguintes valores por faixa:

Faixa 1 (4428-35683) / Faixa 2 (35684 – 51382) / Faixa 3 (51383 – 68147) / Faixa 4 (68148 – 162397)

***“total_criança”***: Somatório das variáveis “crianças_ate_dez_anos” e “crianças_mais_dez_anos”.

***“tem_criancas”***: Variável nominal informando se a pessoa tem ou não filhos criança.

---

### Segmentação RFM

A técnica demanda a análise de três aspectos: o quão recente foi a última compra do cliente, quantas vezes o cliente comprou conosco e quanto ele gastou no total. Para cada um dos três pontos é atribuída uma nota de 1 a 5. Para isso foi necessário construir as seguintes variáveis:

***“frequencia_transação”***: Através da fórmula =COUNTIF calculei a quantidade de vezes que cada cliente comprou no mercado.

***“data_compra_recente”***: Através de uma =QUERY selecionei apenas a data de transação mais recente de cada cliente.

***“dias_desde_ultima_compra”***: A partir da variável anterior, utilizei a fórmula =DAYS(TODAY() para calcular a distância do tempo desde a última compra de cada cliente.

***“total_compras_cliente”***: Somatorio de todo o consumo dos clientes.

***“nota_recencia”***: A partir de percentis (20%) calculei uma nota de 1 a 5, sendo 1 para as compras mais distantes da data atual e 5 para as compras mais próximas da data atual.

***“nota_frequencia”***: A partir de percentis (20%) calculei uma nota de 1 a 5, sendo 1 para as menores frequências e 5 para as maiores frequências.

***“nota_monetario”***: A partir de percentis (20%) calculei uma nota de 1 a 5, sendo 1 para os menores valores e 5 para os maiores valores.

***“media_fm”***: Tirei uma média do somatório dos valores de frequência e monetário.

***“segmentação”***: Seguindo a tabela e cálculo do RFM sugerida por pares, presente no link abaixo:

[O que é RFM e como aplicá-lo ao seu time de Customer Success | by Paulo Vasconcellos | MaxMilhas Tech | Medium](https://medium.com/maxmilhas-tech/o-que-%C3%A9-rfm-e-como-aplic%C3%A1-lo-ao-seu-time-de-customer-service-b9c35817ed01)

Segmentei os clientes em 11 categorias (Campeão, Cliente Leal, Potencialmente Leal, Cliente novo, Não posso perdê-lo, Risco de Perda, Hibernando, Precisa de Atenção, Sonolento, Promissor, Perdido)

***“agrupamento_segmento”***: Para facilitar a visualização em grupos menores estabeleci 4 status de observação:

Assíduo (Cliente Leal, Potencialmente Leal, Campeão) / Promissor e Novo (Promissor e Cliente novo) / Resgatar (Risco de Perda, Precisa de Atenção, Não posso perdê-lo) / Distante (Hibernando, Perdido e Sonolento)

---

### Análise Coorte

***“mes_ano_transacoes”***: Concatenei ano e mês das transações

***“mes_ano_entrada”***: Concatenei ano e mês de entrada dos clientes no sistema

***“meses_entrada_transações”***: Calculei com =DATEIF a diferença de meses entre as transações e a data de cadastro dos clientes.

Disponibilizei as informações em uma tabela dinâmica utilizando formatação condicional para melhor visualização dos dados

---

## Resultados e Conclusões

A maior parte do público é composto por adultos mais maduros ou idosos. Sendo, 80% dos clientes com idade de 45 anos ou mais, estabelecendo uma idade média de 55 anos. A maior parte dos clientes são casados ou em uma União Estável (somando 65%). 71,7% têm filhos crianças (apesar de que também podem ter sido contabilizado netos). Seguindo esse perfil mais maduro observamos que os clientes possuem um alto nível de escolaridade 88% possui Ensino Superior (incluindo 38,3% com Pós-Graduação). Isso se reflete em um público com uma renda anual mais elevada, apresentando uma média de $51.382. Poucas pessoas responderam a campanha do marketing (15%), para maior efetividade de ações futuras precisamos analisar como os nossos clientes estão configurados. 

Para isso, analisamos os dados através da técnica de segmentação RFM (Recency-Frequency-Monetary). Essa técnica permite entender melhor o comportamento do cliente verificando quando foi sua última compra, quantas vezes ele comprou e quanto gastou em sua empresa. Assim poderemos atuar de forma mais efetiva para cada perfil de cliente. Valorizando e fidelizando aqueles que já amam a marca e reativando aqueles que podem estar se distanciando e que são importantes para o seu crescimento.
Através da Segmentação obtivemos os seguintes resultados:

- Campeão (6,56%)
- Cliente Leal (27,76%)
- Potencialmente Leal (12,4%)
- Risco de Perda (15,86%)
- Não posso perdê-lo (2,74%)
- Precisa de Atenção (2,61%)
- Promissor (2,7%)
- Cliente novo (1,66%)
- Sonolento (6,33%)
- Hibernando (13,75%)
- Perdido (7,64%)

Para facilitar a visualização também aglutinamos e apresentamos os seguintes status:

- Assíduo (47%)
- Resgatar (21%)
- Promissor e Novo (4%)
- Distante (28%)

Quando observamos o resumo de compras, percebemos que em todos os segmentos de clientes temos o vinho como principal receita de faturamento (total de $679.921). Seguido por Carnes, Outros, Peixes, Doces e por último Frutas. Faturamento total nos três anos analisados totaliza em $1.349.698. Entendemos que vinhos e carnes são o carro chefe da loja, com maior detalhamento podemos direcionar sugestões de compras para os clientes. Entendendo que o público é mais maduro, podemos alinhar aniversários, datas festivas e recomendar uma carta de vinhos selecionados conjuntamente com cortes especiais de carne personalizando a experiência de acordo com o consumo do cliente.
Os clientes com maior gasto médio são:
Não posso perdê-lo ($1.373)
Campeão ($1.287)
Cliente Leal ($1.065)
Risco de Perda ($859)

Recomendo que haja uma campanha de resgate para os clientes “Não posso perdê-lo” e “Risco de Perda”, os dois possuem um histórico interessante de gastos com a loja, porém faz muito tempo desde as suas últimas compras.
Já para os clientes “Campeão” e “Cliente Leal” seria mais interessante realizar uma ação voltada para a fidelização e manutenção desse cliente na maneira que já usufruem do serviço. Personalizar a experiência trazendo ofertas exclusivas ou seleções de produtos especiais pode ajudar na perpetuação desse vínculo.
Inclusive, quando observamos as respostas da última campanha, os clientes leais foram aqueles que mais responderam, seguido pelos que estão em risco de perda, depois pelos hibernando e os não posso perdê-lo. Seria interessante rever a última campanha para evitar os mesmos erros. Por ora, recomendo traçar estratégias diferentes para cada status de cliente.
Assíduos – Já possuem uma boa frequência e estão recentemente consumindo, fidelizar e mantê-los parece interessante.
Resgatar – São importantes para nós, mas estão ficando distantes desde a data da última compra. Reativá-los com campanhas direcionadas para o intuito de reestabelecer o vínculo se torna urgente.
Promissor e Novo – Ainda não estabeleceram um vínculo com a marca, pode ser interessante apresentá-los os serviços e mostrar possibilidades ainda desconhecidas por eles.
Distantes – Investir menos esforços se comparado com os anteriores. 
A loja tem um certo público cativo, entretanto há poucos clientes conhecendo a marca compondo menos de 2% de nosso banco de dados.
Pensando no aspecto de reativação e retenção, quando realizamos a análise de coorte percebemos que a loja está com dificuldade de reter clientes. Após o primeiro mês de cadastro já há uma queda em torno de 50% de consumação dos clientes em todos os meses, o que se acentua ao passar dez meses desde os seus cadastros. Em uma visão geral, os clientes têm abandonado a marca mais rapidamente se comparados com anos anteriores na mesma passagem de meses. 

No total de transações (22.099), 12.959 foram na loja física e 9.140 no online, apesar de ter um maior número de vendas na loja física há um certo equilíbrio entre as modalidades. Quando olhamos para o ponto mais alto de vendas percebemos que foi no mês de maio (2022). Separando as modalidades percebemos que na modalidade online o pico foi em outubro de 2021, enquanto na modalidade física foi em maio de 2022. De modo geral, a empresa iniciou com um crescimento contínuo, entretanto nos últimos meses registrados (entre setembro e dezembro de 2022) são marcados por uma forte queda nas transações, além disso não há cadastro de novos clientes desde junho de 2022. No caminho atual, a empresa não se encontra em um bom momento nem de receita, nem no surgimento de novos clientes. O público atual é mais maduro, casado, com filhos e classe social alta, investir na seleção dos produtos personalizando a experiência, proporcionando uma atenção exclusiva pode cativar esse tipo de consumidor. Além disso, é recomendado direcionar campanhas de resgate dos clientes “Risco de Perda” e “Não posso perdê-lo”  e campanhas de fidelização aos clientes “Campeão” e “Cliente Leal”. 

---

## Limitações/Próximos Passos

Percebi que o banco de dados que possui certas limitações que podem ser melhoradas para uma análise robusta e direcionada às campanhas:

- Data de Nascimento Completa: Ajudaria a saber a data de aniversário dos clientes e acioná-los com campanhas direcionadas.
- Gênero: Não sabemos qual a identidade de gênero dos clientes o que poderia ajudar em um maior refinamento do perfil do público além de influenciar no direcionamento de campanhas (Ex: Dia das Mães, Dia dos Pais, Dia Internacional da Mulher)
- Maior detalhamento dos produtos: Dados específicos dos itens que saem do estoque facilitaria a ação de personalizada indicando produtos de maior saída para determinados clientes. Além de possibilitar a disponibilidade de itens correlatos como sugestão para clientes que já compraram na loja.
- Maior detalhamento das transações: Assim como é importante saber quais itens estão sendo mais comprados também é interessante saber quais itens são comprados normalmente juntos, quando esses itens estão sendo comprados. Por ser uma importadora, certos itens podem ter uma maior saída quando há festividades (Ano Novo, Natal, São João, Páscoa), principalmente os vinhos que são as maiores vendas.

 Mais dados sobre o comportamento de consumo dos clientes facilitará uma atuação mais efetiva, direcionada e voltada para a experiência do cliente. 

---

## Links de interesse

**Bando de Dados**: 

https://docs.google.com/spreadsheets/d/13datOjuBjMZXcQM93vVCDcda99E_ZmD02EX9LTbP8kI/edit?usp=sharing

**Dashboard**: 

[Segmentação "O Mercado" - Projeto 1 Laboratoria](https://lookerstudio.google.com/reporting/0e4d9dc7-e993-4251-890c-03674de801e7)

**Slide**:

https://docs.google.com/presentation/d/1OH8Q06010JVgeq5iXdiS6QnOySP44M82XIZvdE86Rpo/edit?usp=sharing

**Video**:

[Projeto de Segmentação - "O Mercado" - Laboratoria (Débora Vasconcellos)](https://www.loom.com/share/cb27f27cdd8f462a8e742cdb33ce14db?sid=db55f5fc-3e6a-4b47-829d-5b75fe53cf28)

---

---
