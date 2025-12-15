# FINANCE360-Análise e Previsão Financeira Corporativa Avançada

Projeto de portfólio sênior em Finanças Corporativas, Analytics e Forecast Financeiro, desenvolvido para simular uma solução analítica real usada por empresas de médio e grande porte.

🔎 Visão Geral do Projeto

O FINANCE360 é um sistema analítico completo voltado para análise financeira corporativa, com foco em:

Demonstração de resultados

Análise de custos e despesas

Forecast financeiro (receita e custos)

Análise de clientes, produtos e regiões

Indicadores financeiros avançados (EBITDA, Margem, CAC, LTV)

Tomada de decisão executiva baseada em dados

O projeto foi desenvolvido com Power BI, utilizando modelo estrela, DAX avançado e base de dados simulada realista.

🎯 Objetivos do Projeto

Objetivo Principal

Criar uma solução analítica realista, escalável e executiva, que represente como empresas analisam seu desempenho financeiro.

Objetivos Específicos

Prever receita, custos e lucro

Identificar tendências e sazonalidades

Avaliar eficiência operacional

Analisar rentabilidade por cliente e produto

Simular cenários financeiros futuros

Comunicar insights de forma executiva

🏗️ Arquitetura da Solução

Ferramentas Utilizadas

Power BI — Visualização e modelagem analítica

Python — Geração de dados simulados

DAX — Medidas financeiras e análises avançadas

Figma — Design e layout do dashboard

GitHub — Versionamento e documentação

🧩 Modelo de Dados

O projeto utiliza modelo estrela clássico, padrão em BI corporativo.

Dimensões

Dim_Tempo

Dim_Produto

Dim_Cliente

Dim_UnidadeNegocio

Dim_CentroCusto

Dim_Regional

Fatos

Fato_Financeiro

Fato_Receita

Fato_Despesas

Fato_Caixa

Relacionamentos

Cardinalidade 1:N

Direção de filtro Single

Chave substituta em todas as dimensões

✔️ Modelo otimizado para performance

✔️ Estrutura preparada para escalabilidade


📈 Indicadores Financeiros Implementados

KPIs Principais

Receita Total

Custo Total

Lucro Líquido

EBITDA

Margem (%)

Burn Rate

KPIs Avançados

CAC (Custo de Aquisição de Cliente)

LTV (Lifetime Value)

Receita Prevista

Custo Previsto

Variância Absoluta e Percentual

Receita YoY

Receita MoM

Esses indicadores simulam métricas reais usadas por áreas financeiras, FP&A e estratégia.


🔮 Forecast Financeiro


O projeto implementa forecast de receita e custos utilizando:

Tendência histórica

Sazonalidade mensal

Projeções lineares em DAX

Comparação entre valores reais e previstos


📌 O forecast foi desenhado para análise gerencial, não para modelos estatísticos complexos, simulando o uso comum em áreas financeiras corporativas.


📐 DAX — Medidas Financeiras do Projeto FINANCE360

Todas as medidas abaixo foram desenvolvidas considerando modelo estrela, direção single, e uso da tabela Fato_Financeiro como fato principal.

1️⃣ Receita Total
Receita Total :=
SUM ( Fato_Financeiro[Receita] )

2️⃣ Custo Total
Custo Total :=
SUM ( Fato_Financeiro[Custo] )

3️⃣ Lucro Líquido
Lucro Líquido :=
[Receita Total] - [Custo Total]

4️⃣ EBITDA

EBITDA = Receita − Custos Operacionais (sem impostos, juros e depreciação)

EBITDA :=
CALCULATE (
    [Receita Total] - [Custo Total],
    Fato_Financeiro[Tipo_Custo] <> "Depreciação"
)

5️⃣ Margem (%)
Margem (%) :=
DIVIDE (
    [Lucro Líquido],
    [Receita Total],
    0
)

6️⃣ Burn Rate

Quanto de caixa é consumido por período

Burn Rate :=
CALCULATE (
    SUM ( Fato_Caixa[CaixaValor] ),
    Fato_Caixa[Tipo_Movimento] = "Saída"
)

7️⃣ CAC — Custo de Aquisição de Cliente
CAC :=
DIVIDE (
    CALCULATE (
        SUM ( Fato_Despesas[Valor_Despesa] ),
        Fato_Despesas[Tipo_Despesa] = "Marketing"
    ),
    DISTINCTCOUNT ( Dim_Cliente[ClienteID] ),
    0
)

8️⃣ LTV — Lifetime Value
LTV :=
AVERAGEX (
    VALUES ( Dim_Cliente[ClienteID] ),
    CALCULATE ( [Receita Total] )
)

9️⃣ Receita Prevista (Forecast em DAX)

Forecast linear simples baseado em tendência histórica

Receita Prevista :=
VAR UltimaData =
    MAX ( Dim_Tempo[Data] )

VAR ReceitaMediaMensal =
    AVERAGEX (
        VALUES ( Dim_Tempo[AnoMes] ),
        [Receita Total]
    )

RETURN
IF (
    MAX ( Dim_Tempo[Data] ) > UltimaData,
    ReceitaMediaMensal,
    [Receita Total]
)


📌 Observação:
Este forecast simula o uso comum em FP&A corporativo, não um modelo estatístico avançado.

🔟 Custo Previsto
Custo Previsto :=
VAR CustoMedioMensal =
    AVERAGEX (
        VALUES ( Dim_Tempo[AnoMes] ),
        [Custo Total]
    )

RETURN
CustoMedioMensal

1️⃣1️⃣ Variância Absoluta
Variância Absoluta :=
[Receita Total] - [Receita Prevista]

1️⃣2️⃣ Variância %
Variância % :=
DIVIDE (
    [Variância Absoluta],
    [Receita Prevista],
    0
)

1️⃣3️⃣ Receita YoY
Receita YoY :=
CALCULATE (
    [Receita Total],
    SAMEPERIODLASTYEAR ( Dim_Tempo[Data] )
)

1️⃣4️⃣ Receita MoM
Receita MoM :=
CALCULATE (
    [Receita Total],
    DATEADD ( Dim_Tempo[Data], -1, MONTH )
)

📊 Medidas Auxiliares (Usadas em Gráficos Avançados)
Receita Acumulada (para Curva ABC)
Receita Acumulada :=
CALCULATE (
    [Receita Total],
    FILTER (
        ALLSELECTED ( Dim_Produto ),
        [Receita Total]
            >= CALCULATE ( [Receita Total] )
    )
)

% Acumulado (Curva ABC)
% Receita Acumulada :=
DIVIDE (
    [Receita Acumulada],
    CALCULATE ( [Receita Total], ALL ( Dim_Produto ) )
)

Classificação ABC
Classificação ABC :=
SWITCH (
    TRUE (),
    [% Receita Acumulada] <= 0.8, "A",
    [% Receita Acumulada] <= 0.95, "B",
    "C"


📊 Dashboard — Estrutura das Páginas


1️⃣ Resumo Executivo Financeiro

<img width="581" height="794" alt="pag1" src="https://github.com/user-attachments/assets/f7fa8bf0-d6b2-44a7-9113-898fb1c2fe18" />

Visão geral do negócio:

KPIs financeiros

Receita por Região

Receita real vs prevista

Top clientes

Drivers de resultado


2️⃣ Previsão Financeira (Forecast)

<img width="1161" height="645" alt="pag2" src="https://github.com/user-attachments/assets/dae22ad5-f205-442a-80c3-71caefc46090" />

Receita Total

Custo Previsto

Heatmap de Receita

3️⃣ Análise de Custos e Despesas

<img width="952" height="794" alt="pag3" src="https://github.com/user-attachments/assets/ef74213b-69fe-4631-baf8-bee2bbff1ef1" />

Custos por centro de custo

Custos por unidade de negócio

Fixos vs variáveis

Tabela de Despesas


4️⃣ Análise de Clientes

<img width="949" height="796" alt="pag4" src="https://github.com/user-attachments/assets/b32f1093-551f-40d1-8c76-8036803b5b2f" />

Receita por cliente

CAC e LTV

Rentabilidade

Churn financeiro


5️⃣ Análise de Produtos

<img width="1152" height="696" alt="pag5" src="https://github.com/user-attachments/assets/34ad4c67-96f4-4a86-b298-f61fa3f1d3f2" />

Receita por Produto

Margem por produto

Curva ABC


🎨 Design & Experiência do Usuário


O layout segue o tema:

Fintech / Data Product

Visual moderno e profissional

Uso estratégico de cores

Layout em grid

Alta legibilidade

Foco em storytelling executivo

O design foi prototipado no Figma antes da implementação no Power BI.


🧠 Análises e Insights Gerados


O dashboard FINANCE360 foi desenhado para responder perguntas reais de gestão financeira, indo além da visualização e entregando diagnóstico e apoio à decisão.

Abaixo estão os principais questionamentos estratégicos e onde exatamente o projeto responde a cada um deles.


📈 A empresa está crescendo de forma sustentável?


Onde encontrar a resposta:


Página 1 — Resumo Executivo Financeiro


KPIs: Receita Total, Lucro Líquido, EBITDA e Margem (%)

Gráfico: Receita Real vs Receita Prevista


Página 2 — Forecast Financeiro


Tendência de receita futura

Comparação entre crescimento histórico e projeções

Como o insight é gerado:

Crescimento sustentável é avaliado pela combinação de aumento de receita + manutenção ou melhoria da margem.

O forecast permite verificar se o crescimento atual se mantém nos próximos períodos ou se há desaceleração projetada.


💰 Onde estão os maiores custos?


Onde encontrar a resposta:


Página 3 — Análise de Despesas e Custos


Gráficos:

Custo por Centro de Custo

Custo por Unidade de Negócio

Despesas Fixas vs Variáveis

Tabela detalhada de despesas

Como o insight é gerado:

A decomposição dos custos permite identificar quais centros, unidades ou tipos de despesa concentram maior impacto financeiro.

A separação entre fixos e variáveis ajuda a entender rigidez da estrutura de custos.


📦 Quais produtos geram mais margem


Onde encontrar a resposta:


Página 5 — Análise de Produtos


Gráficos:

Receita por Produto

Margem por Produto

Curva ABC de Produtos

Como o insight é gerado:

A margem por produto permite identificar produtos com alto volume, mas baixa rentabilidade.

A Curva ABC ajuda a priorizar produtos estratégicos (Classe A) versus produtos de baixo impacto.


👥 Quais clientes são mais rentáveis?


Onde encontrar a resposta:


Página 4 — Análise de Clientes


Gráficos:

Receita por Cliente

LTV por Cliente

Rentabilidade por Segmento

Scatter: LTV x CAC

Como o insight é gerado:

A análise cruza receita, LTV e CAC, permitindo identificar clientes que:

Geram alto retorno

Custam menos para adquirir

Segmentação ajuda a identificar perfis de cliente mais estratégicos.


⚠️ Existe risco financeiro no curto prazo?


Onde encontrar a resposta:


Página 1 — Resumo Executivo


KPIs: Burn Rate, Lucro Líquido, EBITDA


Página 2 — Forecast Financeiro


Projeção de receita e custos

Variância entre real e previsto


Página 3 — Custos


Peso de custos fixos

Como o insight é gerado:

Burn Rate alto combinado com queda de margem ou aumento de custos fixos indica risco de caixa.

Forecast negativo ou desaceleração sinaliza potenciais problemas no curto prazo.


📆 Como a sazonalidade impacta o resultado?


Onde encontrar a resposta:


Página 2 — Forecast Financeiro


Gráfico de Sazonalidade Mensal

Heatmap financeiro por mês e região


Página 1 — Resumo Executivo


Comparação MoM e YoY

Como o insight é gerado:

A análise mensal evidencia picos e vales de receita e custos.

O heatmap facilita identificar períodos críticos ou mais rentáveis, apoiando planejamento financeiro e orçamentário.

O projeto enfatiza análise, não apenas visualização.


⚙️ Geração da Base de Dados


A base foi gerada via Python, com:

250.000 registros

Dados temporais (2023–2024)

Sazonalidade realista

Coerência entre fatos e dimensões

Estrutura pronta para BI corporativo


📌 Os dados são 100% simulados, criados apenas para fins educacionais e de portfólio.


🚀 Diferenciais do Projeto


✔️ Modelo estrela profissional

✔️ KPIs financeiros reais

✔️ Forecast integrado ao dashboard

✔️ Storytelling executivo

✔️ Design moderno

✔️ Projeto completo de ponta a ponta


Este projeto foi desenhado para demonstrar senioridade em BI, finanças e analytics, indo além de dashboards básicos.


📌 Próximos Passos (Evolução do Projeto)


Inclusão de cenários (best / base / worst)

Simulação de orçamento (Budget vs Actual)

Integração com dados reais

Versionamento de métricas

Automação de refresh


👤 Autor - Guilherme Alencar


Projeto desenvolvido para fins de portfólio profissional, demonstrando habilidades em:

Business Intelligence

Finanças Corporativas

Análise de Dados

Storytelling Executivo
