# Sistema de Vendas e Análise de Entregas

Projeto desenvolvido com o objetivo de registrar vendas, acompanhar prazos de entrega e analisar atrasos de produção por meio de dados históricos.

A aplicação utilizará **C# com ASP.NET Core** para a interface e regras de negócio, banco de dados para armazenamento das informações e **Python com pandas** para análise dos dados de vendas e entregas.

---

## Objetivo do Projeto

Criar um sistema web capaz de:

- Registrar projetos e pedidos.
- Cadastrar as peças vendidas.
- Registrar valores, quantidades, clientes e notas fiscais.
- Controlar datas de início e entrega.
- Identificar atrasos automaticamente.
- Analisar quais tipos de peças apresentam mais atrasos.
- Identificar quais clientes são mais impactados por atrasos.
- Comparar prazos estimados com prazos reais.
- Gerar indicadores e dashboards para apoio à tomada de decisão.

---

## Tecnologias

### Backend e aplicação web

- C#
- .NET
- ASP.NET Core
- Entity Framework Core
- REST API

### Banco de dados

- SQL Server

### Análise de dados

- Python
- pandas

### Possíveis evoluções

- matplotlib
- Plotly
- scikit-learn

---

# Estrutura do Sistema

## 1. Projetos / Pedidos

Cada projeto deverá possuir as seguintes informações:

- ID
- Nome ou número do projeto
- Cliente
- Data de início
- Data estimada de entrega
- Data real de entrega
- Status
- Observações

### Possíveis status

- Em produção
- Aguardando material
- Em inspeção
- Pronto para entrega
- Entregue
- Atrasado
- Cancelado

---

## 2. Peças Vendidas

Cada projeto poderá possuir uma ou mais peças.

Para cada peça serão armazenadas as seguintes informações:

- Tipo da peça
- Subproduto
- Valor unitário
- Quantidade
- Valor total
- Cliente
- Número da Nota Fiscal
- Projeto relacionado

O valor total será calculado automaticamente:

```text
Valor Total = Valor Unitário × Quantidade
```

---

# Tipos de Peças

Inicialmente o sistema deverá possuir as seguintes categorias:

- Válvula
- Filtro padrão
- Peças sobressalentes
- Usinagem Especial

---

# Subprodutos

Cada categoria poderá possuir seus próprios subprodutos.

Exemplo:

```text
Usinagem Especial
├── Carcaças
├── Painéis
└── Geral
```

Outras categorias poderão receber novos subprodutos futuramente.

Exemplo:

```text
Válvula
├── Esfera
├── Borboleta
├── Retenção
└── Geral
```

Os subprodutos deverão ser armazenados no banco de dados, permitindo adicionar novos itens sem alterar o código da aplicação.

---

# Estrutura Inicial do Banco de Dados

## Projeto

```text
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
```

## Cliente

```text
Cliente
-------------------------
Id
Nome
```

## Categoria

```text
Categoria
-------------------------
Id
Nome
```

## Subproduto

```text
Subproduto
-------------------------
Id
CategoriaId
Nome
```

## Peça Vendida

```text
PecaVendida
-------------------------
Id
ProjetoId
CategoriaId
SubprodutoId
ValorUnitario
Quantidade
NumeroNF
```

---

# Dashboard

A página inicial deverá apresentar os principais indicadores da operação.

Exemplos:

- Faturamento do mês
- Quantidade de peças vendidas
- Projetos em andamento
- Projetos entregues
- Projetos atrasados
- Taxa de entregas no prazo
- Atraso médio
- Valor médio dos projetos

Exemplo:

```text
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
```

---

# Análise de Dados com Python e pandas

Os dados armazenados pelo sistema serão utilizados para gerar análises por meio do Python.

O pandas será utilizado para identificar padrões relacionados a vendas e entregas.

---

## Dias de Atraso

A diferença entre a data real de entrega e a data estimada será calculada automaticamente.

Exemplo:

```text
Entrega estimada: 15/09/2026
Entrega real:      20/09/2026

Atraso: 5 dias
```

Caso o produto seja entregue antes da data prevista:

```text
Entrega estimada: 20/09/2026
Entrega real:      18/09/2026

Atraso: 0 dias
```

Exemplo em pandas:

```python
df["DiasAtraso"] = (
    df["DataEntregaReal"] -
    df["DataEntregaEstimada"]
).dt.days

df["DiasAtraso"] = df["DiasAtraso"].clip(lower=0)
```

---

# Análises Planejadas

## Qual tipo de peça mais atrasa?

Agrupar os projetos por tipo de peça e calcular:

- Quantidade de pedidos
- Quantidade de pedidos atrasados
- Percentual de atraso
- Média de dias de atraso
- Total de dias de atraso

Exemplo:

```text
Usinagem Especial

Pedidos:             25
Pedidos atrasados:   14
Taxa de atraso:      56%
Atraso médio:        8,4 dias
```

---

## Qual subproduto mais atrasa?

Exemplo:

```text
Usinagem Especial

Carcaças
Atraso médio: 9,3 dias

Painéis
Atraso médio: 5,2 dias

Geral
Atraso médio: 2,8 dias
```

---

## Qual cliente sofre mais com atrasos?

Analisar:

- Quantidade total de pedidos
- Quantidade de pedidos atrasados
- Percentual de pedidos atrasados
- Média de dias de atraso
- Total de dias de atraso

Exemplo:

```text
Cliente: --

Pedidos:             14
Pedidos atrasados:    8
Taxa de atraso:      57%
Atraso médio:        6,2 dias
```

---

# Cruzamento de Dados

O sistema deverá permitir análises combinando:

```text
Cliente
+
Tipo da peça
+
Subproduto
+
Período
```

Exemplo de resultado:

```text
Cliente: --

Usinagem Especial
└── Carcaças

Pedidos: 12
Pedidos atrasados: 8
Taxa de atraso: 66%
Atraso médio: 9,3 dias
```

---

# Análise dos Prazos

Além do atraso, também será analisado o tempo planejado de produção.

## Prazo planejado

```text
Data de início
→
Data estimada de entrega
```

## Prazo real

```text
Data de início
→
Data real de entrega
```

Exemplo:

```text
Produto:
Usinagem Especial - Carcaças

Prazo planejado médio:
18 dias

Prazo real médio:
27 dias

Diferença média:
+9 dias
```

Essa informação poderá ajudar a identificar situações em que o prazo comercial informado ao cliente não corresponde ao prazo histórico real de fabricação.

---

# Páginas Planejadas

```text
Dashboard

Projetos

Peças Vendidas

Clientes

Notas Fiscais

Análise de Entregas

Análise de Vendas
```

---

# Filtros

As páginas de análise deverão permitir filtros como:

```text
Período

Cliente

Tipo da peça

Subproduto

Status
```

Exemplo:

```text
Período:
01/01/2026 até 31/12/2026

Cliente:
Todos

Tipo:
Usinagem Especial

Subproduto:
Carcaças
```

---

# Arquitetura Planejada

```text
                  ASP.NET Core
                       |
                       |
                Aplicação Web
                       |
                       |
                  .NET API
                       |
                       |
                 SQL Server
                       |
          -------------------------
          |                       |
          |                       |
      Aplicação               Python
        .NET                  FastAPI
                                  |
                                  |
                                pandas
                                  |
                                  |
                         Análise de dados
                                  |
                                  |
                           Dashboard .NET
```

---

# Fluxo do Sistema

```text
Usuário cadastra projeto
        |
        v
Cadastra as peças vendidas
        |
        v
Sistema salva no banco
        |
        v
Projeto é finalizado
        |
        v
Data real de entrega é informada
        |
        v
Sistema calcula atraso
        |
        v
Python + pandas analisam histórico
        |
        v
Dashboard apresenta indicadores
```

---

# Futuras Melhorias

Após a primeira versão do sistema, poderão ser adicionadas funcionalidades como:

- Previsão de atraso utilizando Machine Learning.
- Previsão de prazo de produção.
- Identificação de produtos com maior risco de atraso.
- Análise de clientes mais impactados.
- Histórico de vendas por cliente.
- Ranking de produtos por faturamento.
- Gráficos de faturamento mensal.
- Gráficos de atraso por produto.
- Exportação de relatórios para Excel.
- Exportação de relatórios para PDF.
- Autenticação de usuários.
- Controle de permissões.
- Integração com sistemas internos.

---

# Objetivo de Aprendizado

Este projeto também será utilizado como projeto de portfólio, permitindo aplicar conhecimentos em:

- C#
- ASP.NET Core
- .NET
- Entity Framework Core
- APIs REST
- SQL
- Modelagem de banco de dados
- Python
- pandas
- Análise de dados
- Dashboards
- Integração entre C# e Python

---

## Status do Projeto

```text
Planejamento inicial
```

### Próxima etapa

```text
Criar solução .NET
↓
Configurar banco de dados
↓
Criar Models
↓
Criar primeiro CRUD de Projetos
↓
Criar cadastro de Peças
↓
Criar Dashboard
↓
Adicionar Python + pandas
```
