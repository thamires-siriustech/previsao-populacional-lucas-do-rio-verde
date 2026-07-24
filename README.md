# Projeção populacional e demanda habitacional — Lucas do Rio Verde (MT), 2026–2035

Análise de dados que estima quantos novos moradores Lucas do Rio Verde deve receber até 2035 e quantas unidades habitacionais serão necessárias para abrigá-los, a partir de dados públicos do IBGE.

> **Pergunta de negócio:** Se Lucas do Rio Verde mantiver seu ritmo de crescimento, quantos novos moradores chegam por ano até 2035, e quantas moradias serão necessárias para abrigá-los?

---

## Contexto

Lucas do Rio Verde saiu de 6.693 habitantes em 1991 para 95.792 em 2025 — crescimento superior a 1.100% em três décadas. Em 2025 foi o município que mais cresceu no Centro-Oeste e o segundo do Brasil entre cidades com mais de 50 mil habitantes. O motor é a agroindústria, que atrai trabalhadores de outras regiões de forma sustentada.

Esse ritmo pressiona diretamente o mercado imobiliário local, mas o setor opera sem uma quantificação pública de quanta moradia a cidade precisará nos próximos anos. Este projeto preenche essa lacuna usando apenas fontes oficiais.

## Principais resultados

- **Lucas do Rio Verde deve cruzar 100 mil habitantes até 2027** em qualquer cenário de crescimento razoável.
- Em 2035, a população projetada fica entre **128.736** (Conservador) e **149.618** (Otimista), com cenário central em torno de **140.000**.
- A demanda por novas moradias entre 2026 e 2035 fica entre **10.300 e 19.200 unidades** — média de **1.100 a 1.800 unidades por ano**.
- **Validação cruzada de dois métodos independentes:** a regressão log-linear (143.401) e o CAGR intercensal (140.303) convergem para 2035 com diferença de apenas **2,2%**.

## Metodologia

O projeto segue um pipeline analítico completo:

1. **Extração** — série populacional 2001–2025 direto da API do SIDRA/IBGE (tabela 6579), de forma reprodutível.
2. **Tratamento e consolidação** — verificação de qualidade, tratamento de anos ausentes e incorporação dos censos de 2010 e 2022, com identificação do padrão de rebaseamento censitário.
3. **Cálculo de taxas (CAGR)** — decomposição do crescimento por sub-período para isolar a taxa livre de contaminação metodológica (intercensal 2011–2021).
4. **Cenários de projeção** — três trajetórias (Conservador, Base, Otimista) ancoradas em taxas observadas da própria série.
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

No Jupyter, use **Kernel → Restart & Run All** para executar todo o pipeline do zero. Os dados são baixados automaticamente da API do IBGE — não é necessário nenhum download manual.

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

- O parâmetro de moradores por domicílio (3,0) é provisório, a confirmar no Censo 2022 para o município. A análise de sensibilidade mostra que seu impacto é secundário (3,5× menor que o cenário de crescimento).
- Os números refletem **demanda bruta por crescimento populacional** — não o déficit habitacional acumulado, nem descontam a oferta já em construção.
- Os cenários supõem taxa de crescimento constante; choques econômicos locais podem alterar a trajetória.

## Fonte dos dados

IBGE — SIDRA tabela 6579 (População residente estimada) e Censos Demográficos de 2010 e 2022.

---

**Autora:** Thamires Martins da Silva
Projeto de portfólio em análise de dados.
