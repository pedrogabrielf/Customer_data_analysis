README - Data Exploration do Dataset df_accounts

Este documento descreve as etapas de exploração de dados realizadas no dataset df_accounts, parte integrante do projeto de Data Science. Abaixo estão detalhadas as análises e transformações realizadas em cada coluna, bem como as justificativas e decisões tomadas durante o processo.

Sumário
	1.	Visão Geral
	2.	Estrutura do Dataset
	3.	Análises por Coluna
	•	account_sfid
	•	account_name
	•	account_created_date
	•	account_country
	•	account_industry
	4.	Transformações Realizadas
	•	Tratamento de Valores Nulos
	•	Conversão de Tipos de Dados
	•	Padronização de Categorias (Opcional)
	5.	Conclusões e Próximos Passos

## Visão Geral

O dataset df_accounts contém informações relacionadas às contas de clientes extraídas do Salesforce. O objetivo inicial foi explorar e entender a qualidade e a estrutura dos dados, identificando possíveis inconsistências, valores nulos e oportunidades de padronização.

## Estrutura do Dataset
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
	•	Análises Principais:
	•	Verificamos duplicatas (.duplicated().sum()) e valores nulos (.isnull().sum()): ambos retornaram 0, confirmando que todos os IDs são únicos e não há valores ausentes.
	•	Todos os valores possuem 64 caracteres, indicando padronização do sistema de origem (Salesforce).

Conclusão: Nenhuma alteração necessária. A coluna está íntegra e consistente.

### account_name
	•	Descrição: Nome descritivo da conta.
	•	Análises Principais:
	•	Verificamos quantidade de nomes únicos e valores nulos: 1.414 nomes únicos para 1.415 registros (há 1 duplicata). Nenhum valor nulo.
	•	O formato dos nomes segue um padrão tipo "Customer_<ID>", todos com 17 caracteres.
	•	Decidimos não alterar o nome duplicado, pois o account_sfid é a chave primária e permanece único.

Conclusão: Coluna sem problemas críticos. Documentamos a duplicidade, mas mantivemos como está.

### account_created_date
	•	Descrição: Data de criação da conta.
	•	Análises Principais:
	•	Convertida para datetime (pd.to_datetime()).
    •   Criamos 3 colunas para separar, ano, mes, dia da semana.
	•	Não há valores nulos.
	•	Descobrimos que as datas variam de 2007 até 2025, com apenas 1 registro em 2025 (possível dado de teste ou entrada futura).
	•	Identificamos alta criação de contas em alguns anos e meses (por exemplo, picos em novembro).

Conclusão: A conversão para datetime foi suficiente. Mantivemos o registro de 2025 e documentamos que pode ser uma anomalia ou teste.

### account_country
	•	Descrição: País de cadastro da conta.
	•	Análises Principais:
	•	Encontramos 7 valores nulos.
	•	Existem 72 países diferentes.
	•	Principais países: Estados Unidos (553), China (73), Canadá (68), etc.

Conclusão: Decidimos preencher (fillna("Unknown")) os 7 nulos com "Unknown" para manter consistência e não perder registros.

### account_industry
	•	Descrição: Indústria ou segmento a que a conta pertence.
	•	Análises Principais:
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


# Parte 1.1 Análise do segundo dataset 

1. Visão Geral
	•	Nome do Dataset: df_support_cases
	•	Objetivo: Analisar a qualidade do dataset de suporte (casos do Salesforce), identificando colunas com valores nulos, possíveis inconsistências, tipos de dados e frequências de valores.

2. Estrutura do Dataset

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

3. Análises Realizadas
	1.	Verificação de Valores Nulos
	•	Através de df_support_cases.isnull().sum(), identificamos:
	•	account_sfid: 1593 valores nulos.
	•	case_closed_date: 942 valores nulos.
	•	Demais colunas: 0 valores nulos.
	2.	Tratamento de account_sfid Nulo
	•	Decidimos padronizar os casos sem conta vinculada com o valor "Unknown".

4.	Frequência de Valores (value_counts)
	•	Para a maioria das colunas categóricas (ex.: case_status, case_priority, case_reason, case_type, case_category)

5. Observações Adicionais
	•	case_closed_date Nulo: Interpretamos como “caso em aberto”. Optamos por manter esses valores nulos, pois não há data de fechamento
	•	Consistência com o Dataset de Contas: O uso de account_sfid como chave (mesmo que algumas linhas estejam agora marcadas como "Unknown") será crucial na integração (JOIN) com o dataset de contas na próxima fase do projeto.
	•	Futura Análise de Métricas: Com datas em formato datetime, será possível calcular o tempo médio de resolução (para casos fechados) e acompanhar tendências de abertura de casos por período.


# Começando a PART 	 do projeto.
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

8. Evolução de contas criadas por mês e ano.
