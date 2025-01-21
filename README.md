# README - Data Science Project: Salesforce Data Analysis

# Pedro Gabriel Fonseca
# Curitiba, Paraná, Brazil
# (41) 99129-2213
# E-mail: pedrofons8@gmail.com


## Visão Geral

O projeto tem como objetivo realizar uma análise de dados extraídos do Salesforce, processando e transformando os dados usando Python e SQL, para gerar insights valiosos que possam apoiar decisões de negócios. Este README descreve as etapas realizadas nas partes 1, 2, 3 e 4 do projeto, com ênfase em análise exploratória, transformação de dados, visualizações e insights para a empresa.

## O projeto foi dividido nas seguintes partes:

### Parte 1: Data Exploration
	•	Carregar e explorar os dados dos dois datasets fornecidos (df_accounts e df_support_cases), para entender sua estrutura, conteúdo e identificar inconsistências, valores nulos e outras características importantes.

### Parte 2: Data Processing
	•	Usar Python e SQL para juntar os datasets e derivar métricas significativas para os negócios. A ênfase foi dada ao uso de SQL para agregações e transformações.

### Parte 3: Data Visualization
	•	Criar visualizações utilizando bibliotecas Python como Matplotlib, Seaborn e Plotly, com base nos KPIs definidos, para apresentar insights valiosos e facilitar a interpretação dos dados.

### Parte 4: Business Insights
	•	Derivar insights chave dos dados e propor recomendações acionáveis que possam ser aplicadas no contexto de negócios.

# Parte 1: Data Exploration  - df_accounts
		•	Total de linhas: 1.415
		•	Total de colunas: 5 (após criarmos algumas colunas auxiliares para análise, mantivemos as colunas originais intactas).
		•	Colunas originais:
		1.	account_sfid
		2.	account_name
		3.	account_created_date
		4.	account_country
		5.	account_industry

	## Análises por Coluna
	### account_sfid
		•	Descrição: Identificador único de cada conta.
		•	Verificamos duplicatas (.duplicated().sum()) e valores nulos (.isnull().sum()): ambos retornaram 0, confirmando que todos os IDs são únicos e não há valores ausentes.
		•	Todos os valores possuem 64 caracteres, indicando padronização do sistema de origem (Salesforce).

	Conclusão: Nenhuma alteração necessária. A coluna está íntegra e consistente.

	### account_name
		•	Descrição: Nome descritivo da conta.
		•	Verificamos quantidade de nomes únicos e valores nulos: 1.414 nomes únicos para 1.415 registros (há 1 duplicata). Nenhum valor nulo.
		•	O formato dos nomes segue um padrão tipo "Customer_<ID>", todos com 17 caracteres.
		•	Decidimos não alterar o nome duplicado, pois o account_sfid é a chave primária e permanece único.

	Conclusão: Coluna sem problemas críticos. Documentada a duplicidade, mas mantive como está.

	### account_created_date
		•	Descrição: Data de criação da conta.
		•	Convertida para datetime (pd.to_datetime()).
		•   Criamos 3 colunas para separar, ano, mes, dia da semana.
		•	Não há valores nulos.
		•	Descobrimos que as datas variam de 2007 até 2025, com apenas 1 registro em 2025 (possível dado de teste ou entrada futura).
		•	Identificamos alta criação de contas em alguns anos e meses (por exemplo, picos em novembro).

	Conclusão: A conversão para datetime foi suficiente.

	### account_country
		•	Descrição: País de cadastro da conta.
		•	Encontramos 7 valores nulos.
		•	Existem 72 países diferentes.
		•	Principais países: Estados Unidos (553), China (73), Canadá (68), etc.

	Conclusão: Decidimos preencher (fillna("Unknown")) os 7 nulos com "Unknown" para manter consistência e não perder registros.

	### account_industry
		•	Descrição: Indústria ou segmento a que a conta pertence.
		•	Encontramos 13 valores nulos.
		•	Existência de 22 indústrias diferentes.
		•	Principais: Pharmaceuticals (421), Printing (265), Packaging and Containers (252), etc.

	Conclusão: Preenchemos os 13 valores nulos com "Unknown".

	### Transformações Realizadas

	Tratamento de Valores Nulos
		•	account_country: Substituímos 7 valores nulos por "Unknown".
		•	account_industry: Substituímos 13 valores nulos por "Unknown".

	Conversão de Tipos de Dados
		•	account_created_date: Convertido de object para datetime64[ns].


# Parte 1: Data Exploration  - df_support_cases

Através do comando df_support_cases.info(), observamos:
	•	Total de Colunas: 16 (ex.: case_sfid, account_sfid, case_number, case_created_date, etc.)
	•	Total de Registros: 10k

Principais Colunas
	1.	case_sfid - Identificador único do caso.
	2.	account_sfid - Identificador da conta associada.
	3.	case_number - Número do caso, usado para referência.
	4.	case_created_date - Data de criação do caso.
	5.	case_closed_date - Data de fechamento do caso, se houver.
	6.	case_status, case_priority, case_severity, case_reason, case_type, case_category - Metadados que descrevem a natureza e a urgência do caso.

# Análises Realizadas
	1.	Verificação de Valores Nulos
		•	Através de df_support_cases.isnull().sum(), identificamos:
		•	account_sfid: 1593 valores nulos.
		•	case_closed_date: 942 valores nulos.
		•	Demais colunas: 0 valores nulos.
	2.	Tratamento de account_sfid Nulo
		•	Decidimos padronizar os casos sem conta vinculada com o valor "Unknown".

# Frequência de Valores (value_counts)
	•	Para a maioria das colunas categóricas (ex.: case_status, case_priority, case_reason, case_type, case_category)

# Observações Adicionais
	•	case_closed_date Nulo: Interpretamos como “caso em aberto”. Optamos por manter esses valores nulos, pois não há data de fechamento
	•	Consistência com o Dataset de Contas: O uso de account_sfid como chave (mesmo que algumas linhas estejam agora marcadas como "Unknown") será crucial na integração (JOIN) com o dataset de contas na próxima fase do projeto.
	•	Futura Análise de Métricas: Com datas em formato datetime, será possível calcular o tempo médio de resolução (para casos fechados) e acompanhar tendências de abertura de casos por período.


# Parte 2: Data Processing
Insights:

	1. Número de Casos por País
		Insight: Identificamos os países com maior demanda de suporte. Países com mais casos indicam maior necessidade de recursos ou processos de suporte mais complexos, podendo ser alvos de ações específicas.

	2. Número de Casos por Indústria
		Insight: Determinamos quais indústrias demandam mais suporte, ajudando a priorizar esforços de suporte especializado ou desenvolvimento de soluções personalizadas para indústrias com maior volume de casos.

	3. Número de Casos por Conta (Top 5)
		Insight: Descobrimos as contas mais exigentes em termos de suporte, permitindo direcionar esforços para essas contas, que podem precisar de um atendimento mais intensivo ou preventivo.

	4. Tempo Médio de Resolução por Conta
		Insight: Identificamos as contas com o maior tempo médio de resolução, revelando áreas onde o suporte pode estar sendo ineficiente ou onde mais recursos podem ser necessários.

	5. Distribuição de Casos por Severidade
		Insight: A análise da severidade dos casos mostrou quais níveis de severidade predominam, permitindo otimizar a alocação de recursos de acordo com a urgência dos casos.

	6. Distribuição de Casos por Prioridade
		Insight: Identificamos a distribuição dos casos por prioridade, o que pode ajudar a ajustar a alocação de recursos de forma mais eficiente, priorizando casos de alta urgência.

	7. Tempo Médio de Resolução por País
		Insight: A comparação do tempo médio de resolução entre países revelou quais regiões demandam mais tempo para resolver casos. Isso pode ajudar a identificar ineficiências regionais e melhorar processos de suporte específicos para essas regiões.

# Parte 3: Data Visualization

## Seção 1 – Visão Geral (Gráficos Genéricos)
	1.1 – Número de casos por país
	1.2 – Distribuição de casos por país (Pizza – TOP 5 + OUTROS)
	1.3 – Distribuição de Contas por País
	1.4 – Número de Contas por País

## Seção 2 – Análise de Severidade e Prioridade
	2.1 – Distribuição de casos por Severidade
	2.3 – Distribuição de Casos por Prioridade
	2.4 – Distribuição de Casos Graves por País (EUA Disparado)
	2.5 – Distribuição de Casos Graves por Indústria nos EUA

## Seção 3 - Análise Específica de Indústrias e Tipos de Chamados

	3.1 - Indústrias Relacionadas ao Top 3 Países
	3.2 - Número de casos por Indústria
	3.3 - Distribuição de Casos por Tipo de Chamado e Severidade (Top 10 Tipos)
	3.4 - Distribuição de Casos por Tipo de Chamado e Severidade (Top 3 Tipos)
	3.5 - Tempo Médio de Resolução por Indústria
		3.5.1 - Tempo médio resolução por ano
	3.6 - Tempo Médio de Resolução de Casos nos Estados Unidos por Ano de Criação das Contas
	3.7 - Países com maior tempo de resolução
		3.7.1 - Distribuição de Casos por Severidade nos Países com Maior Tempo de Resolução de Casos
	3.8 - Top 10 Contas com Maior Tempo Médio de Resolução # REVER SE DEIXO OU NÃO
	3.9 - Evolução de Contas Criadas por Mês e Ano
	3.10 - Número de casos por contas (Top 5)

## Seção 4 - Análise de Performance e Tendência

	4.1 - Tendência de Tempo de Resolução (Média, Máximo, Mínimo)
	4.2 - Severidade por casos
	4.3 - Distribuição de Casos por País para as Top 3 Indústrias
	4.4 - Média, máximo e mínimo resoluções dos casos EUA com 5 indústrias mais presentes
	4.5 - Tendência de criação de contas até 2030

## Seção 5 - Análise por países com mais contas (EUA, CHINA, CANADA)
	5.1 - EUA
	5.2 - CHINA
	5.3 - CANADA

# Part 4: Business Insights
	Based on your findings, answer the following questions:

	1. What are the key insights you derived from the data and visualizations?

		1 - EUA possui a maior demanda de suporte, seguido por china e canadá. 

		2 - A tabela abaixo mostra rapidamente o tempo médio de resolução do suporte por tipo de caso nos EUA

		| Case Type  | Tempo médio |
		| ------------- | ------------- |
		| License Activation  | +2 days  |
		| Software Performance  | +10 days  |
		| User Access issue | +4 days |

		3 - Top 3 Industrias com mais casos. Farmaceutica, Information Technology e Printing. A tabela abaixo mostra o tempo médio de resolução dos casos de cada um.

		| Industry  | Tempo médio |
		| ------------- | ------------- |
		| Farmaceutica  | +6 days  |
		| Information Technology  | +-1 day  |
		| Printing | +5 days |

		4 - Distribuição de Casos Graves: Casos graves são mais prevalentes nos EUA, especialmente na indústria farmacêutica.

		5 - Através dos graficos que foram gerados, observa-se que os estados unidos é lider em casos graves, especialmente na indústria farmacêutica, que ademais, possui um tempo médio de resolução de mais de 6 dias, o que é um absurdo e precisa melhorar.

		6 - Pude observar uma inconsistência na resolução de problemas relacionados a tecnologia. O Canadá tem praticamente casos relacionados a industria da tecnologia que possui um excelente tempo médio de resolução (Aproximadamente 1 dia) porém, os estados unidos que tem dois problemas relacionados a tecnologia, "Software Performace" e "User Access Issue", possuindo respectivamente +10 dias e +4 dias, como são problemas relacionados a tecnologia, deveriam ser resolvidos mais rapidamente !

		7 - O canadá possui um problema sério na parte de documentar e especificar o tipo de caso que está sendo analisado, falta dado para concluirmos dados melhores.


	2. Propose two actionable recommendations that the business could take based on these insights.
		1.	Alocar Mais Recursos de Suporte para os EUA: A demanda de casos nos EUA é significativamente maior, o que pode justificar o aumento da equipe de suporte ou a automação de processos. 40,2% dos casos são dos EUA, e com uma demora de +10 days para problemas de Software Performance e +4 days para User Access Issue, é nítido que a equipe de T.I precisa urgentemente ser revisada para diminuirmos esse tempo consideravelmente. A industria farmaceutica representa 42,2% dos casos por industria dos estados unidos, e tem um tempo médio de resolução de +6 days, o que demonstra mais uma vez que os EUA tem um sério problema com tempo de resolução dos suportes abertos.


