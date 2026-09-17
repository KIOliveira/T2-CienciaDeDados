# Vantagem de Mando no Campeonato Brasileiro

Trabalho Final de Ciência de Dados (UNESP — Faculdade de Ciências, Bauru). Tema 8 — Desempenho de times no Campeonato Brasileiro.

**Pergunta central:** jogar em casa realmente muda a probabilidade de vitória, e por quanto?

**Recorte:** Campeonato Brasileiro de 2003 a 2023, 8.405 partidas em 21 temporadas, agregadas ao nível de partida com mandante e visitante lado a lado.

**Resposta:** sim. Entre as partidas com resultado decidido, o mandante vence **67,6%** (IC 95% [66,4%; 68,7%]). O mando vale **0,78 ponto por partida**, o que soma aproximadamente **14,7 pontos** ao longo dos 19 jogos em casa de uma temporada.

## Estrutura

```
├── Datasets/
│   ├── campeonato-brasileiro-full.csv               # partidas: placar, mando, arena (bruto)
│   ├── campeonato-brasileiro-estatisticas-full.csv  # chutes, posse, faltas por time (bruto)
│   ├── campeonato-brasileiro-gols.csv               # gols por atleta e tipo (bruto)
│   ├── campeonato-brasileiro-cartoes.csv            # cartões por atleta (bruto)
│   ├── campeonato_brasileiro_tratado.csv            # gerado pela EDA: 8.405 x 37, nível partida
│   └── Legenda.txt                                  # dicionário de dados das 5 bases
├── notebooks/
│   ├── EDA.ipynb                    # tratamento, análise exploratória e geração do dataset tratado
│   └── Testes_de_Hipoteses.ipynb    # testes formais, tamanho de efeito e mecanismo
├── figuras/                         # 10 figuras geradas pelos notebooks (PNG)
├── documentacao/
│   ├── sn-article.tex               # relatório técnico (template Springer)
│   └── sn-jnl.cls, sn-*.bst         # arquivos de classe e bibliografia do template
└── requirements.txt
```

## Como rodar

```bash
pip install -r requirements.txt
jupyter notebook
```

Execute os notebooks nesta ordem:

1. **`notebooks/EDA.ipynb`** — carrega as bases brutas, aplica o tratamento (Seção 4), exporta `Datasets/campeonato_brasileiro_tratado.csv` e gera as figuras em `figuras/`.
2. **`notebooks/Testes_de_Hipoteses.ipynb`** — parte do dataset tratado e conduz os testes formais.

O segundo depende do dataset gerado pelo primeiro. Como esse arquivo já está versionado, também é possível executar apenas o segundo.

### O que cada notebook contém

| `EDA.ipynb` | |
|---|---|
| Seções 1–3 | Carga, limpeza e diagnóstico de dados faltantes |
| Seção 4 | **Tratamento** e exportação do dataset consolidado |
| Seção 5 | Estatística descritiva |
| Seções 6–9 | Análises: vantagem de mando, tendência temporal, assimetria dos placares, estatísticas de jogo e heterogeneidade entre clubes |
| Seção 10 | Síntese |

| `Testes_de_Hipoteses.ipynb` | Instrumento |
|---|---|
| Seção 3 | Teste binomial exato — o mando muda a probabilidade de vitória? |
| Seção 4 | Diferença de proporções e pontos esperados — e por quanto? |
| Seção 5 | Teste t pareado — o mandante joga diferente? |
| Seção 6 | Qui-quadrado de homogeneidade — de onde vem a vantagem? |

## Fonte de dados

- **Organização:** Adão Duque — compilação de dados públicos da CBF, publicada no Kaggle
- **Link de acesso:** https://www.kaggle.com/datasets/adaoduque/campeonato-brasileiro-de-futebol
- **Data de download:** 15 de setembro de 2026
- **Recorte utilizado:** 2003–2023 (as bases contêm registros posteriores, descartados na carga para manter critério de coleta consistente)

As bases brutas de gols e cartões contêm a coluna `atleta`, com nomes de jogadores. Essa coluna **não entra no dataset tratado**, que é integralmente agregado por partida. Nenhuma análise deste trabalho utiliza dados em nível individual.

O dicionário completo das cinco bases, com as janelas de cobertura de cada uma e as regras de tratamento aplicadas, está em `Datasets/Legenda.txt`.

## Principais achados

- O mandante vence **49,7%** das partidas contra **23,9%** do visitante — vantagem positiva em **todas as 21 temporadas**.
- O mando acrescenta **25,9 pontos percentuais** à probabilidade de vitória (IC 95% [24,5; 27,3]).
- O mandante **ataca 31% mais**, mas com a **mesma taxa de conversão** e posse quase simétrica: joga mais adiantado, não joga melhor.
- No período de **portões fechados** (ago/2020–set/2021), a taxa de vitória do mandante caiu de **50,7% para 42,4%** (p = 3,2×10⁻⁴) — a torcida é componente relevante, mas não exclusivo.
- A vantagem é **independente da força do clube** (r = −0,02): times fortes e fracos extraem o mesmo bônus.

## Decisões metodológicas

- **Recorte em 2003–2023.** Testamos completar a série com uma segunda fonte, mas as métricas divergiam entre elas (chutes no alvo +28,5%, passes −11,5%), indicando critérios de coleta distintos. Optamos por fonte única.
- **Zeros estruturais convertidos em ausentes.** As bases auxiliares têm janelas de cobertura diferentes (estatísticas 2015–2023, gols e cartões 2014–2023) e gravam ausência como zero. Mantidos, diluiriam as médias por um fator de ~2,4×.
- **Desenhos pareados.** Mandante e visitante de uma mesma partida não são observações independentes, então a comparação principal não usa tabela de contingência.
- **Sem KNN.** O tema sugere classificação do resultado, mas um classificador não produz estimativa de efeito nem intervalo de confiança, que é o que a pergunta central exige. A justificativa está no relatório.

## Autores

Igor dos Reis Gomes e Caio César Souza Oliveira.
