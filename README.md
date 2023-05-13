# Introdução

4 projetos de Power BI desenvolvidos durante um curso intensivo ministrado pelo professor Ítalo Teotônio em março de 2023, com carga horária de 10 horas. 

# Dashboards

## Dashboard de Estoque

### Desafio

Você recebeu algumas bases de dados com movimentos de estoque, do ano de 2020, de uma empresa do segmento de vestuário. Você tem o desafio de analisar estas bases de dados e construir um relatório que:

- Apresente a análise de estoque por loja x categoria.
- Apresente a análise de estoque por período.
- Apresente a análise de movimentação de estoque por categoria x subcategoria.
- Apresente a análise de estoque por subcategoria e por produto.
- Apresente o estoque atual, bem como informações gerais sobre Entrada, Saída e Devoluções.
- Permita a interação entre as diversas informações, com o intuito de realização de análises diversas.

### Base de Dados

Arquivo no formato .xlsx, com 3 banco de dados disponíveis:

Estoque
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
Produtos
```
schema = (
    código produto,
    descrição do produto-categoria-subcategoria,
    custo unitário,
    preço unitário
)
```
Calendário
```
schema = (
    data
)
```

### Dashboard

Algumas transformações foram realizadas via Power Query, tais como: 

O campo "descrição do produto-categoria-subcategoria" da tabela Calendário foi alterado para trazer as informações de Produto, Categoria e Subcategoria separadamente, uma vez que estavam delimitados por um hífen ("-") na coluna
```
= Table.SplitColumn(#"Tipo Alterado", "DESCRIÇÃO DO PRODUTO-CATEGORIA-SUBCATEGORIA", Splitter.SplitTextByDelimiter("-", QuoteStyle.Csv), {"DESCRIÇÃO DO PRODUTO-CATEGORIA-SUBCATEGORIA.1", "DESCRIÇÃO DO PRODUTO-CATEGORIA-SUBCATEGORIA.2", "DESCRIÇÃO DO PRODUTO-CATEGORIA-SUBCATEGORIA.3"})
```
A tabela Calendário traz algumas informações adicionais como Ano, Trimestre, Mês, Nome do Mês e Nome do Mês Abreviado. 
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
A tabela Calendário se relaciona com a Estoque pelo campo "Data", em uma relação de 1 para muitos. A tabela Produtos se relaciona com a tabela Estoque através do campo "Código Produto" em uma relação de 1 para muitos também.

A parte visual do Dash se divide em três principais grupos: filtros, big numbers e gráficos interativos. 

Os filtros funcionam para todos os visuais e permitem filtrar por algum mês específico ou loja específica.

Os Big Numbers apresentados referem-se a estoque acumulado, entradas, saídas e devoluções.

4 informações são apresentadas de forma gráfica:

Itens em Estoque por Subcategoria e Produto, apresentado a partir de um treemap que apresenta a subcategoria e uma dica de ferramenta personalizada que apresenta os produtos que pertencem a categoria e os valores de cada um.

Estoque Acumulado por Mês, trazendo uma visão mês a mês da soma de estoque.

Análise de Movimentações de Itens de Estoque, representado por uma árvore hierárquica que permite analisar o estoque com maior granularidade, a partir do tipo (entrada, devolução ou saída), categoria e subcategoria.

Por fim, Itens em Estoque por Loja x Categoria, representado por um simples gráfico de colunas empilhadas.

## Dashboard de Recursos Humanos

### Desafio

O gestor de uma empresa está analisando informações a respeito dos colaboradores da organização e lhe enviou um e-mail com alguns questionamentos e uma base de dados dos funcionários da empresa. Os questionamentos são:

- Quantos funcionários existem atualmente na empresa?
- Atualmente, existem mais funcionários do gênero masculino ou do gênero feminino?
- Quantos funcionários existem em cada setor da organização?
- Quantos funcionários já fizeram pós-graduação, mestrado ou doutorado?
- Quantos funcionários do gênero feminino estão em cargos de direção, gerência ou supervisão?
- Qual a cidade que possui a maior quantidade de funcionários?
- Quais os dois setores em que existe maior custo com salário?
- Quantos estagiários existem na organização e qual o valor gasto atualmente com o salário destes estagiários?
- Qual a média salarial dos Analistas III?
- Qual a quantidade de funcionários e o valor gasto de salários por faixa etária?
- Qual o total de salários gasto em cada cidade?
- Qual a média salarial do gênero masculino e do gênero feminino?

### Base de dados

Arquivo no formato .xlsx, com 1 banco de dados disponível:
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

Algumas transformações foram realizadas via Power Query, tais como: A base de dados trazia uma idade estática, da qual foi substituida por uma coluna calculada em que traz a idade real.
```
= Table.AddColumn(#"Colunas Removidas1", "Idade.1", each Date.From(DateTime.LocalNow()) - [Data de Nascimento], type duration)
= Table.TransformColumns(#"Colunas Reordenadas",{{"Idade.1", each Duration.TotalDays(_) / 365, type number}})
= Table.TransformColumns(#"Total de Anos Calculado",{{"Idade.1", Number.RoundDown, Int64.Type}})
```

A parte visual do Dash se divide em quatro telas: 

MENU: traz uma tela nomeando o assunto tratado no projeto, bem como botões de links externos para minhas redes pessoais, como Instagram e Linkedin, além do site da empresa de treinamentos.

ANÁLISE PESSOAL: há uma tarja na lateral esquerda, que apresentam botões de ação para navegação entre as telas e botões de link externos, presentes em ambas as telas de informações. Há também uma área destinada a filtros de setor, gênero, escolaridade, cargo, faixa etária e estado, também presentes em ambas telas e com filtros sincronizados, ou seja, a alteração de algum deles em uma página é replicada nas demais. A tela também traz gráficos de análise de funcionários por escolaridade, setor, gênero, faixa etária, cargo e cidade.

ANÁLISE FINANCEIRA: além dos itens já citados anteriormente, a tela apresenta big numbers de total de salários, média salarial e total de funcionários. Graficamente há a representação das informações de total de salários por escolaridade, setor, cargo, faixa etária e cidade. 

PÁGINA DE CONTATOS: nessa tela é novamente apresentado os botões de link externos, juntamente com a exposição de uma foto pessoal e um botão que possibilita o retorno à tela home.

## Dashboard de Resultados Financeiro

### Desafio

Você recebeu algumas bases de dados, para que possa realizar a análise dos resultados de uma empresa, no ano de 2020. Você tem o desafio de analisar estas bases de dados e construir um relatório que:

- Apresente a receita da empresa por período, durante todo o ano de 2020.
- Apresente um comparativo de produtos, que demonstre a relação de receita x quantidade de itens vendidos daquele produto.
- Apresente uma análise de receita e lucro, por supervisor e por vendedor.
- Apresente um indicador que compare a receita com a respectiva meta, de forma que seja possível analisar por vendedor, período e produto.

### Base de Dados

Arquivos no formato .xlsx, com 5 banco de dados disponíveis:

Relatório
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
Pessoas
```
schema = (
    cod_vendedor,
	vendedor,
	supervisor
)
```
Produtos
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
Calendário 2020
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
Metas
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

Algumas transformações foram realizadas via Power Query, tais como: 

Foi criada uma coluna na tabela Calendário que traz o último dia do mês 
```
= Table.AddColumn(#"Tipo Alterado", "Fim do Mês", each Date.EndOfMonth([DATA]), type date)
```

Esse Dashboard apresenta uma relação entre as tabelas um pouco mais complexa que os demais, uma vez que possui 2 tabelas fato e 3 tabelas dimensão. A tabela Calendário se relaciona com a tabela de Metas pelo campo "Apuração" em uma relação de 1 para muitos e com a tabela de Vendas pelo campo "Data_NF" novamente em uma relação de 1 para muitos. A tabela Produtos se relaciona com a tabela de Metas pelo campo "Produto" em uma relação de 1 para muitos e com a tabela de Vendas pelo campo "Cod_Produto" novamente em uma relação de 1 para muitos. A tabela Vendedores se relaciona com a tabela de Metas pelo campo "Vendedor" em uma relação de 1 para muitos e com a tabela de Vendas pelo campo "Cod_Vendedor" novamente em uma relação de 1 para muitos. 

A parte visual do Dash se divide em dois principais grupos: big numbers e gráficos interativos. 

Os Big Numbers apresentados referem-se a receita total e lucro total    .

4 informações são apresentadas de forma gráfica:

Análise por Período, apresentado a partir de um gráfico de colunas e um de linhas trazendo a receita total por mês.

Análise de Produtos, representado por um gráfico de dispersão que correlaciona o total de itens vendidos e a receita total, porém com a possibilidade de avaliar também o lucro total, representado pelo tamanho de cada item.

Uma matriz que permite fazer a análise por supervisor e por colaborador, avaliando a receita total e o lucro total. 

Por fim, um gráfico de indicador (velocímetro) que compara a receita total com uma meta previamente definida e, a partir de uma formatação condicional, apresenta o indicador na cor verde ou vermelha de acordo com a meta.
```
COR DO VELOCÍMETRO = 
SWITCH(
    TRUE(),
    [Receita Total] >= MEDIDAS[Meta Total], "#00E6BB",
    [Receita Total] < MEDIDAS[Meta Total], "#e6002b"
)
```

## Dashboard de Pesquisa de Satisfação

### Desafio

Você trabalha em uma Instituição Financeira, que realizou recentemente uma pesquisa para analisar a satisfação dos clientes. Esta pesquisa é baseada em uma metodologia denominada Net Promoter Score (NPS), que avalia o grau de fidelidade dos clientes.

Tem-se os seguintes parâmetros:

- Notas 1, 2, 3, 4 e 5 = Clientes Detratores 
- Notas 6, 7 e 8 = Clientes Neutros
- Notas 9 e 10 = Clientes Promotores

### Base de Dados

Arquivo no formato .xlsx, com 4 banco de dados disponíveis:

Resposta Clientes NPS
```
schema = (
    data_resposta2, 
    cód_cliente, 
    nota_final_nps, 
    sentimento
)
```
Respostas Clientes Atributos
```
schema = (
    id_cliente2, 
    id_cliente2, 
    cidade, 
    uf, 
    região
)
```
Informações Clientes
```
schema = (
    cod_cliente2,
    nome_cliente, 
    gênero, 
    gerente, 
    tempo_relacionamento 
)
```
Localidades Clientes
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

Algumas transformações foram realizadas via Power Query, tais como: 

Foi realizado um join da tabela de Informações Clientes com a de Localidades Clientes, para trazer o endereço de cada cliente
```
= Table.NestedJoin(#"Tipo Alterado1", {"COD_CLIENTE"}, Cadastro_ClienteLocalidade, {"ID_CLIENTE"}, "Cadastro_ClienteLocalidade", JoinKind.LeftOuter)
= Table.ExpandTableColumn(#"Consultas Mescladas", "Cadastro_ClienteLocalidade", {"CIDADE", "UF", "REGIÃO"}, {"CIDADE", "UF", "REGIÃO"})
```
Foi adicionada uma coluna na tabela Resposta Clientes NPS que identifica o status de acordo com a nota final
 ```
= Table.AddColumn(#"Tipo Alterado", "STATUS_NPS", each if [NOTA_FINAL_NPS] >= 9 then "PROMOTOR" else if [NOTA_FINAL_NPS] <= 5 then "DETRATOR" else "NEUTRO")
 ```    
Foi também criada uma tabela Calendário a partir do Power Query
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

Esse Dashboard apresenta 2 tabelas fato e 2 tabelas dimensão. A tabela Calendário se relaciona com a tabela de Respostas Clientes Atributos pelo campo "Data Resposta" em uma relação de 1 para muitos e com a tabela de Resposta Clientes NPS pelo campo "Data_Resposta" novamente em uma relação de 1 para muitos. A tabela Clientes se relaciona com a tabela de Respostas Clientes Atributos pelo campo "Cod_Cliente" em uma relação de 1 para muitos e com a tabela de Resposta Clientes NPS pelo campo "Cod_Cliente" novamente em uma relação de 1 para muitos. 

A parte visual do Dash se divide em três principais grupos: filtros, big numbers para análise de satisfação geral e gráficos interativos. 

Os filtros referem-se aos gerentes, estados e datas.

A parte de análise de satisfação geral apresenta big numbers de total de respostas, total de promotores, média do NPS, total de neutros, total de detratores e um farol definido pela média do NPS. Ainda nessa parte, há um gráfico de donut apresentando o percentual de cada status e uma nuvem de palavras a partir do sentimento definido na pesquisa.

4 informações são apresentadas de forma gráfica:

Análise Notas dos Atributos, representado por um gráfico importado do tipo Radar Chart, que avalia a média de nota de acordo com cada atributo: Aplicativo, Inovação, Tarifas, Serviços, Suporte e Confiabilidade.

Análise NPS por Período, representado por um gráfico de área que traz a média do NPS por mês.

Por fim, dois gráficos de barras para análise do NPS por região e por gerente.