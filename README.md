# Projeção populacional e demanda habitacional — Lucas do Rio Verde (MT), 2026–2035

Análise de dados que estima quantos novos moradores Lucas do Rio Verde deve receber até 2035 e quantas unidades habitacionais serão necessárias para abrigá-los, a partir de dados públicos do IBGE.

> **Pergunta de negócio:** Se Lucas do Rio Verde mantiver seu ritmo de crescimento, quantos novos moradores chegam por ano até 2035, e quantas moradias serão necessárias para abrigá-los?

---

## Contexto

Lucas do Rio Verde passou de 95.792 habitantes estimados em 2025 para **99.291 habitantes estimados em 2026**, segundo o IBGE, com referência em 1º de julho. Isso representa crescimento de aproximadamente 3,65% em um ano.

Esse crescimento pressiona a demanda por moradia. O projeto usa a série histórica do SIDRA/IBGE, os Censos de 2010 e 2022 e a estimativa oficial de 2026 para construir cenários — deixando explícito o que é dado oficial e o que é projeção do modelo. Fonte: [IBGE — Estimativas da População](https://www.ibge.gov.br/estatisticas/sociais/populacao/9103-estimativas-de-populacao.html).

## Principais resultados

- **Base observada mais recente:** 99.291 habitantes na estimativa oficial do IBGE para 2026.
- O teste de um passo, feito a partir da base de 2025, colocou a estimativa oficial de 2026 dentro da faixa dos três cenários; o cenário Base ficou cerca de **0,23%** acima do valor oficial.
- Para 2035, os cenários projetam **129.552** habitantes (Conservador), **139.982** (Base) e **148.320** (Otimista).
- A demanda bruta por moradias gerada pelo crescimento populacional entre 2027 e 2035 fica em torno de **10.086 unidades** no cenário Conservador, **13.565** no Base e **16.342** no Otimista, usando provisoriamente 3 moradores por domicílio.
- Variando também o parâmetro domiciliar de 2,8 a 3,2 moradores, a matriz de sensibilidade fica entre **9.457 e 17.512 unidades**. Esses valores não representam o déficit habitacional acumulado nem descontam a oferta já em obra.
- A projeção log-linear histórica resulta em cerca de 143,4 mil habitantes em 2035, próxima ao cenário Base recalibrado; essa proximidade é uma checagem complementar, não uma garantia de precisão.

## Metodologia

O projeto segue um pipeline analítico completo:

1. **Extração** — série histórica 2001–2025 via API do SIDRA/IBGE (tabela 6579); a série é limitada explicitamente a 2025 para separar o histórico da estimativa oficial mais recente.
2. **Tratamento e consolidação** — verificação de qualidade, tratamento de anos ausentes e incorporação dos censos de 2010 e 2022, com identificação do padrão de rebaseamento censitário.
3. **Cálculo de taxas (CAGR)** — decomposição do crescimento por sub-período para isolar a taxa livre de contaminação metodológica (intercensal 2011–2021).
4. **Validação e projeção** — compara o cenário calculado a partir da base de 2025 com a estimativa oficial de 2026 e, em seguida, ancora a projeção 2027–2035 no valor oficial mais recente.
5. **Conversão em demanda habitacional** — com análise de sensibilidade cruzando cenário de crescimento e razão de moradores por domicílio.
6. **Modelagem estatística** — regressão linear e log-linear com validação temporal, confrontando a abordagem determinística com modelos estatísticos.

## Stack técnico

- **Python** — pandas, numpy
- **Visualização** — matplotlib
- **Extração** — requests (API SIDRA/IBGE)
- **Modelagem** — scikit-learn (regressão linear e log-linear)
- **Ambiente** — Jupyter Notebook

## Como executar

```bash
# Clonar o repositório
git clone https://github.com/thamires-siriustech/previsao-populacional-lucas-do-rio-verde.git

# Instalar dependências
pip install -r requirements.txt

# Abrir o notebook
jupyter notebook previsao_populacao_lucas_do_rio_verde.ipynb
```

Execute o notebook a partir da raiz do repositório. No Jupyter, use **Kernel → Restart & Run All** para executar todo o pipeline. A série histórica é baixada da API do IBGE; a estimativa oficial de 2026 é informada separadamente a partir da publicação anual do IBGE e deve ser atualizada quando uma nova edição for divulgada.

## Estrutura do repositório

```
previsao-populacional-lucas-do-rio-verde/
├── previsao_populacao_lucas_do_rio_verde.ipynb   # análise completa
├── dados/
│   └── populacao_lucas_do_rio_verde.csv          # série consolidada (gerada pelo notebook)
├── requirements.txt
└── README.md
```

## Limitações

- O parâmetro de moradores por domicílio (3,0) é provisório, a confirmar no Censo 2022 para o município. A análise de sensibilidade mostra que seu impacto é secundário (3,4× menor que a variação entre cenários).
- Os números refletem **demanda bruta por crescimento populacional** — não o déficit habitacional acumulado, nem descontam a oferta já em construção.
- Os cenários supõem taxa de crescimento constante; choques econômicos locais podem alterar a trajetória.

## Fonte dos dados

IBGE — SIDRA tabela 6579 (série histórica de população residente estimada), Censos Demográficos de 2010 e 2022 e [Estimativas da População 2026](https://www.ibge.gov.br/estatisticas/sociais/populacao/9103-estimativas-de-populacao.html) (referência em 1º de julho de 2026).

---

**Autora:** Thamires Martins da Silva
Projeto de portfólio em análise de dados.
