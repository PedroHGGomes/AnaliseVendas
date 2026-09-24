**Sistema de Vendas e Análise de Entregas**

Projeto desenvolvido com o objetivo de registrar vendas, acompanhar prazos de entrega e analisar atrasos de produção por meio de dados históricos.

A aplicação utilizará C# com ASP.NET Core para a interface e regras de negócio, banco de dados para armazenamento das informações e Python com pandas para análise dos dados de vendas e entregas.
________________________________________
**Objetivo do Projeto**

Criar um sistema web capaz de:
<br>
Registrar projetos e pedidos.
<br>
Cadastrar as peças vendidas.
<br>
Registrar valores, quantidades, clientes e notas fiscais.
<br>
Controlar datas de início e entrega.
<br>
Identificar atrasos automaticamente.
<br>
Analisar quais tipos de peças apresentam mais atrasos.
<br>
Identificar quais clientes são mais impactados por atrasos.
<br>
Comparar prazos estimados com prazos reais.
<br>
Gerar indicadores e dashboards para apoio à tomada de decisão.
_______________________________________
**Tecnologias**

Backend e aplicação web
<br>
C#
<br>
.NET
<br>
ASP.NET Core
<br>
Entity Framework Core
<br>
REST API
<br>
Banco de dados
<br>
Inicialmente:
<br>
SQL Server
<br>
Possíveis alternativas:
<br>
PostgreSQL
<br>
Oracle
<br>
Análise de dados
<br>
Python
<br>
pandas
<br>
Possíveis evoluções:
<br>
matplotlib
<br>
Plotly
<br>
scikit-learn
<br>
_______________________________________
**Estrutura do Sistema**

1. Projetos / Pedidos

Cada projeto deverá possuir as seguintes informações:

ID

Nome ou número do projeto

Cliente

Data de início

Data estimada de entrega

Data real de entrega

Status

Observações

Possíveis status

Em produção

Aguardando material

Em inspeção

Pronto para entrega

Entregue

Atrasado

Cancelado

2. Peças Vendidas

Cada projeto poderá possuir uma ou mais peças.

Para cada peça serão armazenadas as seguintes informações:

Tipo da peça

Subproduto

Valor unitário

Quantidade

Valor total

Cliente

Número da Nota Fiscal

Projeto relacionado

O valor total será calculado automaticamente:

Valor Total = Valor Unitário × Quantidade

Tipos de Peças

Inicialmente o sistema deverá possuir as seguintes categorias:

Válvula

Filtro padrão

Peças sobressalentes

Usinagem Especial

Subprodutos

Cada categoria poderá possuir seus próprios subprodutos.

Exemplo:

Usinagem Especial
├── Carcaças
├── Painéis
└── Geral

Outras categorias poderão receber novos subprodutos futuramente.

Exemplo:

Válvula
├── Esfera
├── Borboleta
├── Retenção
└── Geral

Os subprodutos deverão ser armazenados no banco de dados, permitindo adicionar novos itens sem alterar o código da aplicação.

Estrutura Inicial do Banco de Dados

Projeto

Projeto
-------------------------
Id
NomeProjeto
ClienteId
DataInicio
DataEntregaEstimada
DataEntregaReal
Status
Observacao

Cliente

Cliente
-------------------------
Id
Nome

Categoria

Categoria
-------------------------
Id
Nome

Subproduto

Subproduto
-------------------------
Id
CategoriaId
Nome

Peça Vendida

PecaVendida
-------------------------
Id
ProjetoId
CategoriaId
SubprodutoId
ValorUnitario
Quantidade
NumeroNF

Dashboard

A página inicial deverá apresentar os principais indicadores da operação.

Exemplos:

Faturamento do mês

Quantidade de peças vendidas

Projetos em andamento

Projetos entregues

Projetos atrasados

Taxa de entregas no prazo

Atraso médio

Valor médio dos projetos

Exemplo:

-------------------------------------

FATURAMENTO DO MÊS
R$ 125.000

PROJETOS ENTREGUES
18

PROJETOS ATRASADOS
4

TAXA DE ATRASO
18,2%

ATRASO MÉDIO
4,7 dias

-------------------------------------

Análise de Dados com Python e pandas

Os dados armazenados pelo sistema serão utilizados para gerar análises por meio do Python.

O pandas será utilizado para identificar padrões relacionados a vendas e entregas.

Dias de Atraso

A diferença entre a data real de entrega e a data estimada será calculada automaticamente.

Exemplo:

Entrega estimada: 15/09/2026
Entrega real:      20/09/2026

Atraso: 5 dias

Caso o produto seja entregue antes da data prevista:

Entrega estimada: 20/09/2026
Entrega real:      18/09/2026

Atraso: 0 dias

Exemplo em pandas:

df["DiasAtraso"] = (
    df["DataEntregaReal"] -
    df["DataEntregaEstimada"]
).dt.days

df["DiasAtraso"] = df["DiasAtraso"].clip(lower=0)

Análises Planejadas

Qual tipo de peça mais atrasa?

Agrupar os projetos por tipo de peça e calcular:

Quantidade de pedidos

Quantidade de pedidos atrasados

Percentual de atraso

Média de dias de atraso

Total de dias de atraso

Exemplo:

Usinagem Especial

Pedidos:             25
Pedidos atrasados:   14
Taxa de atraso:      56%
Atraso médio:        8,4 dias

Qual subproduto mais atrasa?

Exemplo:

Usinagem Especial

Carcaças
Atraso médio: 9,3 dias

Painéis
Atraso médio: 5,2 dias

Geral
Atraso médio: 2,8 dias

Qual cliente sofre mais com atrasos?

Analisar:

Quantidade total de pedidos

Quantidade de pedidos atrasados

Percentual de pedidos atrasados

Média de dias de atraso

Total de dias de atraso
