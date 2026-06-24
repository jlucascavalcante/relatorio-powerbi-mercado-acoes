# 📈 Análise do Mercado de Ações com Narrativa Inteligente — Power BI

Dashboard de análise do mercado de ações com foco em volume negociado, variação mensal de preço de fechamento e comparação entre empresas — combinando análise histórica com o visual de Narrativa Inteligente do Power BI para geração automática de insights em linguagem natural.

---

## 📸 Preview do Dashboard

![Preview do Dashboard](imagens/preview.png)


## 🎯 Problema de negócio

Analistas e investidores precisam monitorar simultaneamente o comportamento de múltiplos ativos ao longo do tempo, identificando tendências de volume, variações de fechamento e padrões sazonais. Este dashboard centraliza essas dimensões para responder: quais empresas apresentaram maior variação de fechamento mês a mês? O volume negociado está em tendência de crescimento ou queda? E quais períodos concentraram as maiores oscilações?

---

## 🔍 Principais insights

- **Pico de volume em janeiro de 2023**, com total próximo a 0,4 Bi de ações negociadas — destaque expressivo em relação à média do período, que gira em torno de 0,2 Bi.
- **Tesla apresenta a maior volatilidade na variação MoM de fechamento**, com oscilações que chegam próximo a -50% e +50% ao longo do ano — significativamente acima das demais empresas.
- **IBM, Microsoft, Oracle e Walmart mantêm variações MoM mais estáveis**, concentradas próximas a 0%, indicando comportamento mais previsível no período analisado.
- **A Narrativa Inteligente detectou aumento de 29,35% no volume** entre fevereiro de 2022 e fevereiro de 2023, além de registrar a queda iniciada em janeiro de 2023 (-22,73% em 31 dias) — padrões que seriam difíceis de identificar manualmente na série temporal.
- **A tabela de valores médios por mês** revela que os preços de fechamento (Close) variam entre R$ 148,71 (janeiro 2023) e R$ 197,47 (abril 2022), sugerindo uma tendência de queda nos preços médios ao longo do período.

---

## 📄 Estrutura do dashboard

O relatório é composto por uma única página com os seguintes visuais:

| Visual | Descrição |
|--------|-----------|
| **Narrativa Inteligente** | Visual de IA nativo do Power BI — gera automaticamente insights em linguagem natural sobre variações de volume e fechamento |
| **Total do Volume de Ações Negociadas ao Longo do Tempo** | Série temporal com volume total diário de janeiro de 2022 a fevereiro de 2023 |
| **Variação da Média de Fechamento MoM** | Gráfico de área com linhas por empresa — exibe a variação percentual mês a mês do preço de fechamento para as 5 empresas |
| **Tabela de Valores Médio por Mês** | Matriz com médias mensais de Open, High, Low, Close e Volume por ano e mês |

**Filtros disponíveis:** Empresa e Mês.

---

## 🛠️ Stack técnica

- **Ferramenta:** Power BI Desktop
- **Empresas analisadas:** IBM, Microsoft, Oracle, Tesla e Walmart
- **Transformação de dados (Power Query):** mudança de fonte, navegação, promoção de cabeçalhos e ajuste de tipos de dado
- **Visualizações:** Narrativa Inteligente (visual de IA nativo do Power BI), série temporal de volume, gráfico de área com múltiplas séries por empresa e matriz de valores médios mensais

**Medida DAX criada:**

```dax
Média de Close MoM% = 
IF(
    ISFILTERED('StockMarket'[Data]),
    ERROR("Medidas rápidas de inteligência de tempo somente podem ser agrupadas ou filtradas pela hierarquia de data fornecida pelo Power BI ou pela coluna de data primária."),
    VAR __PREV_MONTH =
        CALCULATE(
            AVERAGE('StockMarket'[Close]),
            DATEADD('StockMarket'[Data].[Date], -1, MONTH)
        )
    RETURN
        DIVIDE(AVERAGE('StockMarket'[Close]) - __PREV_MONTH, __PREV_MONTH)
)
```

Calcula a variação percentual da média de fechamento em relação ao mês anterior (MoM — Month over Month). Usa `DATEADD` para referenciar o mês anterior e `DIVIDE` para evitar erros em divisões por zero — base para o gráfico de variação MoM e para os insights gerados pela Narrativa Inteligente.

---

## 📊 Fonte dos dados

Dataset de mercado de ações com cobertura de janeiro de 2022 a fevereiro de 2023, contendo dados históricos de IBM, Microsoft, Oracle, Tesla e Walmart, originalmente utilizado em curso de formação em dados. O dashboard, os visuais, a formatação e a estrutura analítica foram desenvolvidos de forma independente — incluindo a escolha e configuração dos gráficos, uso da Narrativa Inteligente e cruzamentos exibidos.

---

## 🔗 Acesse o relatório

[Acesse o dashboard](https://app.powerbi.com/view?r=eyJrIjoiYzYwZTk2MzctMjZlZC00NGY5LWFiMjktOTRmY2RjNDNlNmJlIiwidCI6ImIyZTE2Mjk3LTJlZDYtNDFiOC1iODIyLWE5NTRlOTViZDJmMCIsImMiOjR9)

---

## 📌 Limitações e próximos passos

- A **Narrativa Inteligente gera texto automático com base nos filtros ativos** — sem interação, os insights refletem a visão geral de todas as empresas; filtrar por empresa individualmente revela narrativas mais específicas e acionáveis.
- O gráfico de variação MoM mostra a **média de fechamento por empresa**, mas não pondera pelo volume negociado — uma empresa com poucos negócios em determinado mês pode distorcer a variação percentual calculada.
- **Tesla domina visualmente o gráfico de variação MoM** pela alta volatilidade, o que pode obscurecer movimentos relevantes das demais empresas — uma próxima iteração com eixo separado por empresa tornaria as comparações mais equilibradas.
- Próximo passo: adicionar indicadores de **máxima e mínima histórica por empresa** para contextualizar os valores da tabela de médias mensais.
