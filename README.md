# Introduction

4 Power BI projects developed during an intensive course taught by Professor Ítalo Teotônio in March 2023, with a workload of 10 hours.

# Dashboards

## Stock Dashboard

### Challenge

You have received some databases with stock movements from 2020 from a company in the clothing segment. You have the challenge of analyzing these databases and building a report that:

- Presents the stock analysis by store x category.
- Presents the stock analysis by period.
- Presents the stock movement analysis by category x subcategory.
- Presents the stock analysis by subcategory and by product.
- Presents the current stock, as well as general information about Input, Output and Returns.
- Allows interaction between the various pieces of information, in order to perform various analyses.

### Database

File in .xlsx format, with 3 databases available:

Stock
```
schema = (
    cod movimento,
    cod produto, 
    data date,
    id loja,
    estoque,
    tipo
)
```
Products
```
schema = (
    código produto,
    descrição do produto-categoria-subcategoria,
    custo unitário,
    preço unitário
)
```
Calendar
```
schema = (
    data
)
```

### Dashboard

ASome transformations were performed via Power Query, such as:

The "product description-category-subcategory" field in the Calendar table was changed to bring the Product, Category and Subcategory information separately, since they were delimited by a hyphen ("-") in the column
```
= Table.SplitColumn(#"Tipo Alterado", "DESCRIÇÃO DO PRODUTO-CATEGORIA-SUBCATEGORIA", Splitter.SplitTextByDelimiter("-", QuoteStyle.Csv), {"DESCRIÇÃO DO PRODUTO-CATEGORIA-SUBCATEGORIA.1", "DESCRIÇÃO DO PRODUTO-CATEGORIA-SUBCATEGORIA.2", "DESCRIÇÃO DO PRODUTO-CATEGORIA-SUBCATEGORIA.3"})
```
The Calendar table provides some additional information such as Year, Quarter, Month, Month Name and Abbreviated Month Name.
```
= Table.AddColumn(#"Tipo Alterado", "Ano", each Date.Year([DATA]), Int64.Type)
```
```
= Table.AddColumn(#"Ano Inserido", "Trimestre", each Date.QuarterOfYear([DATA]), Int64.Type)
```
```
= Table.AddColumn(#"Trimestre Inserido", "Mês", each Date.Month([DATA]), Int64.Type)
```
```
= Table.AddColumn(#"Mês Inserido", "Nome do Mês", each Date.MonthName([DATA]), type text)
```
```
= Table.AddColumn(#"Colocar Cada Palavra Em Maiúscula", "Primeiros caracteres", each Text.Start(Text.Upper([Nome do Mês]), 3), type text)
= Table.RenameColumns(#"Primeiros Caracteres Inseridos",{{"Primeiros caracteres", "Mês Abreviado"}})
```
The Calendar table relates to the Inventory table through the "Date" field, in a 1-to-many relationship. The Products table relates to the Inventory table through the "Product Code" field in a 1-to-many relationship as well.

The visual part of Dash is divided into three main groups: filters, big numbers and interactive graphs.

The filters work for all visuals and allow you to filter by a specific month or store.

The Big Numbers presented refer to accumulated stock, entries, exits and returns.

4 pieces of information are presented graphically:

Items in Stock by Subcategory and Product, presented from a treemap that shows the subcategory and a personalized tooltip that shows the products that belong to the category and the values ​​of each one.

Accumulated Stock by Month, providing a month-by-month view of the total stock.

Analysis of Stock Item Movements, represented by a hierarchical tree that allows you to analyze the stock with greater granularity, based on the type (entry, return or exit), category and subcategory.

Finally, Items in Stock by Store x Category, represented by a simple stacked column graph.

## Human Resources Dashboard

### Challenge

The manager of a company is analyzing information about the organization's employees and sent you an email with some questions and a database of the company's employees. The questions are:

- How many employees are currently in the company?
- Are there currently more male or female employees?
- How many employees are in each sector of the organization?
- How many employees have completed postgraduate, master's or doctoral degrees?
- How many female employees are in management, supervisory or supervisory positions?
- Which city has the largest number of employees?
- Which two sectors have the highest salary costs?
- How many interns are there in the organization and how much is currently spent on their salaries?
- What is the average salary for Analysts III?
- How many employees and how much is spent on salaries by age group?
- What is the total salary spent in each city?
- What is the average salary for male and female employees?

### Database

File in .xlsx format, with 1 database available:
```
schema = (
    registro2, 
    nome_completo, 
    estado civil, 
    escolaridade, 
    genero, 
    data de nascimento, 
    idade, 
    data_contratação, 
    data saída, 
    status, 
    codcargo, 
    cargo, 
    setor, 
    salário base, 
    extra cidade, 
    extra titulação, 
    salário bruto, 
    cidade, 
    estado, 
    desempenho_2018, 
    desempenho_2019, 
    desempenho_2020, 
    desempenho_geral
)
```

### Dashboard

Some transformations were performed via Power Query, such as: The database had a static age, which was replaced by a calculated column showing the real age.
```
= Table.AddColumn(#"Colunas Removidas1", "Idade.1", each Date.From(DateTime.LocalNow()) - [Data de Nascimento], type duration)
= Table.TransformColumns(#"Colunas Reordenadas",{{"Idade.1", each Duration.TotalDays(_) / 365, type number}})
= Table.TransformColumns(#"Total de Anos Calculado",{{"Idade.1", Number.RoundDown, Int64.Type}})
```

The visual part of the Dash is divided into four screens:

MENU: brings up a screen naming the subject covered in the project, as well as buttons for external links to my personal networks, such as Instagram and LinkedIn, in addition to the training company's website.

PERSONAL ANALYSIS: there is a strip on the left side, which presents action buttons for navigation between screens and external link buttons, present on both information screens. There is also an area for filters by sector, gender, education, position, age group and state, also present on both screens and with synchronized filters, that is, changing any of them on one page is replicated on the others. The screen also presents graphs analyzing employees by education, sector, gender, age group, position and city.

FINANCIAL ANALYSIS: in addition to the items previously mentioned, the screen presents big numbers for total salaries, average salary and total employees. Graphically, there is a representation of the information on total salaries by education, sector, position, age group and city. 

CONTACTS PAGE: this screen displays the external link buttons again, along with a personal photo and a button that allows you to return to the home screen.

## Financial Results Dashboard

### Challenge

You have received some databases so that you can analyze a company's results in 2020. You have the challenge of analyzing these databases and creating a report that:

- Presents the company's revenue by period, throughout 2020.
- Presents a product comparison, demonstrating the relationship between revenue and quantity of items sold for that product.
- Presents an analysis of revenue and profit, by supervisor and by salesperson.
- Presents an indicator that compares revenue with the respective target, so that it is possible to analyze by salesperson, period and product.
  
### Database

Files in .xlsx format, with 5 databases available:

Report
```
schema = (
    data_nf,
    núm_nf,
    cod_produto,
    cod_vendedor,
    canal_vendas,
    qtde_itens,
    custo_unitário,
    preço unitário
)
```
People
```
schema = (
    cod_vendedor,
	vendedor,
	supervisor
)
```
Products
```
schema = (
    cod_produto,
    grupo_produto,
    categoria_produto,
    produto,
    custo_unitário,
    preço_unitário
)
```
Calendar 2020
```
schema = (
    data,
    semestre,
    trimestre,
    número mês,
    nome mês,
    mês abreviado
)
```
Goals
```
schema = (
    apuração,
    produto,
    vendedor,
    supervisor,
    canal_vendas,
    meta
)
```

### Dashboard

Some transformations were performed via Power Query, such as:

A column was created in the Calendar table that shows the last day of the month
```
= Table.AddColumn(#"Tipo Alterado", "Fim do Mês", each Date.EndOfMonth([DATA]), type date)
```

This Dashboard presents a slightly more complex relationship between tables than the others, since it has 2 fact tables and 3 dimension tables. The Calendar table is related to the Goals table through the "Aclaração" field in a 1-to-many relationship and to the Sales table through the "Date_NF" field, again in a 1-to-many relationship. The Products table is related to the Goals table through the "Produto" field in a 1-to-many relationship and to the Sales table through the "Cod_Produto" field, again in a 1-to-many relationship. The Salespeople table is related to the Goals table through the "Vendedor" field in a 1-to-many relationship and to the Sales table through the "Cod_Vendedor" field, again in a 1-to-many relationship.

The visual part of Dash is divided into two main groups: big numbers and interactive graphs.

The Big Numbers presented refer to total revenue and total profit.

4 pieces of information are presented graphically:

Analysis by Period, presented using a column graph and a line graph showing total revenue per month.

Product Analysis, represented by a scatter plot that correlates the total number of items sold and total revenue, but with the possibility of also evaluating total profit, represented by the size of each item.

A matrix that allows analysis by supervisor and employee, evaluating total revenue and total profit.

Finally, an indicator graph (speedometer) that compares total revenue with a previously defined target and, based on conditional formatting, presents the indicator in green or red according to the target.

```
COR DO VELOCÍMETRO = 
SWITCH(
    TRUE(),
    [Receita Total] >= MEDIDAS[Meta Total], "#00E6BB",
    [Receita Total] < MEDIDAS[Meta Total], "#e6002b"
)
```

## Satisfaction Survey Dashboard

### Challenge

You work at a Financial Institution that recently conducted a survey to analyze customer satisfaction. This survey is based on a methodology called Net Promoter Score (NPS), which assesses the degree of customer loyalty.

The following parameters are used:

- Scores 1, 2, 3, 4 and 5 = Detractor Customers
- Scores 6, 7 and 8 = Neutral Customers
- Scores 9 and 10 = Promoter Customers
  
### Database

File in .xlsx format, with 4 databases available:

NPS Customer Response
```
schema = (
    data_resposta2, 
    cód_cliente, 
    nota_final_nps, 
    sentimento
)
```
Customer Responses Attributes
```
schema = (
    id_cliente2, 
    id_cliente2, 
    cidade, 
    uf, 
    região
)
```
Customer Information
```
schema = (
    cod_cliente2,
    nome_cliente, 
    gênero, 
    gerente, 
    tempo_relacionamento 
)
```
Customer Locations
```
schema = (
    id_cliente2,
    id_cliente, 
    cidade, 
    uf, 
    região
)
```
### Dashboard

Some transformations were performed via Power Query, such as:

A join was performed between the Customer Information table and the Customer Locations table to retrieve the address of each customer
```
= Table.NestedJoin(#"Tipo Alterado1", {"COD_CLIENTE"}, Cadastro_ClienteLocalidade, {"ID_CLIENTE"}, "Cadastro_ClienteLocalidade", JoinKind.LeftOuter)
= Table.ExpandTableColumn(#"Consultas Mescladas", "Cadastro_ClienteLocalidade", {"CIDADE", "UF", "REGIÃO"}, {"CIDADE", "UF", "REGIÃO"})
```
A column has been added to the NPS Customer Response table that identifies the status according to the final score
 ```
= Table.AddColumn(#"Tipo Alterado", "STATUS_NPS", each if [NOTA_FINAL_NPS] >= 9 then "PROMOTOR" else if [NOTA_FINAL_NPS] <= 5 then "DETRATOR" else "NEUTRO")
 ```    
A Calendar table was also created from Power Query
``` 
DataMin = List.Min(fRespostasNPS[DATA_RESPOSTA]),
DataMax = List.Max(fRespostasNPS[DATA_RESPOSTA]),
QtdeDias = Duration.Days (DataMax - DataMin) + 1,
ListarDatas = List.Dates(DataMin, QtdeDias, #duration (1,0,0,0)),
ConverterTabelaDatas = Table.FromList(ListarDatas, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
#"Colunas Renomeadas" = Table.RenameColumns(ConverterTabelaDatas,{{"Column1", "DATA"}}),
#"Tipo Alterado" = Table.TransformColumnTypes(#"Colunas Renomeadas",{{"DATA", type date}}),
#"Trimestre Inserido" = Table.AddColumn(#"Tipo Alterado", "Trimestre", each Date.QuarterOfYear([DATA]), Int64.Type),
#"Prefixo Adicionado" = Table.TransformColumns(#"Trimestre Inserido", {{"Trimestre", each "TRI 0" & Text.From(_, "pt-BR"), type text}}),
#"Mês Inserido" = Table.AddColumn(#"Prefixo Adicionado", "Mês", each Date.Month([DATA]), Int64.Type),
#"Nome do Mês Inserido" = Table.AddColumn(#"Mês Inserido", "Nome do Mês", each Date.MonthName([DATA]), type text),
#"Primeiros Caracteres Inseridos" = Table.AddColumn(#"Nome do Mês Inserido", "Primeiros caracteres", each Text.Start(Text.Upper([Nome do Mês]), 3), type text),
#"Colunas Renomeadas1" = Table.RenameColumns(#"Primeiros Caracteres Inseridos",{{"Primeiros caracteres", "Mês Abreviado"}}),
#"Semana do Mês Inserida" = Table.AddColumn(#"Colunas Renomeadas1", "Semana do Mês", each Date.WeekOfMonth([DATA]), Int64.Type),
#"Semana do Ano Inserida" = Table.AddColumn(#"Semana do Mês Inserida", "Semana do Ano", each Date.WeekOfYear([DATA]), Int64.Type),
#"Dia do Ano Inserido" = Table.AddColumn(#"Semana do Ano Inserida", "Dia do Ano", each Date.DayOfYear([DATA]), Int64.Type),
#"Nome do Dia Inserido" = Table.AddColumn(#"Dia do Ano Inserido", "Nome do Dia", each Date.DayOfWeekName([DATA]), type text),
#"Coluna Condicional Adicionada" = Table.AddColumn(#"Nome do Dia Inserido", "Semestre", each if [Mês] <= 6 then "SEM 01" else "SEM 02"),
#"Colunas Reordenadas" = Table.ReorderColumns(#"Coluna Condicional Adicionada",{"DATA", "Semestre", "Trimestre", "Mês", "Nome do Mês", "Mês Abreviado", "Semana do Mês", "Semana do Ano", "Dia do Ano", "Nome do Dia"})
```  

This Dashboard presents 2 fact tables and 2 dimension tables. The Calendar table is related to the Customer Responses Attributes table by the "Response Date" field in a 1-to-many relationship and to the Customer Responses NPS table by the "Response_Date" field again in a 1-to-many relationship. The Customers table is related to the Customer Responses Attributes table by the "Customer_ID" field in a 1-to-many relationship and to the Customer Responses NPS table by the "Customer_ID" field again in a 1-to-many relationship.

The visual part of the Dash is divided into three main groups: filters, big numbers for general satisfaction analysis and interactive charts.

The filters refer to managers, states and dates.

The general satisfaction analysis part presents big numbers of total responses, total promoters, average NPS, total neutrals, total detractors and a lighthouse defined by the average NPS. Also in this section, there is a donut chart showing the percentage of each status and a word cloud based on the sentiment defined in the survey.

Four pieces of information are presented graphically:

Attribute Score Analysis, represented by an imported Radar Chart chart, which evaluates the average score according to each attribute: Application, Innovation, Rates, Services, Support and Reliability.

NPS Analysis by Period, represented by an area chart that shows the average NPS per month.

Finally, two bar charts for analyzing NPS by region and by manager.
