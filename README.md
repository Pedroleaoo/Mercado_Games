# Mercado_Games

Dashboard de Análise de Dados e BI sobre o mercado de games na Steam.

[![Abrir no Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1zxashxjXKafUrXWxSuXLLzlACwRNDOFf?usp=sharing)

## Equipe

- Pedro Vítor Leão
- Bruno Silva Sena
- Matheus Gabriel

## Objetivo

Ajudar uma publisher a decidir **onde investir no próximo lançamento** e **o que evitar** em cada mercado (inglês, alemão, japonês e chinês simplificado), a partir do público estimado e das notas dos jogos da Steam. O projeto segue as etapas do DEILAS.

## Tecnologias

- Python, Pandas 2.2.3, NumPy 2.4.6 e SciPy 1.16.3
- Plotly 5.24.1 (gráficos interativos e dashboard em HTML)
- Google Colab (execução e versionamento do notebook)

## Bases de dados

| Base | Fonte | Uso |
|------|-------|-----|
| `games.csv` | Steam Games Dataset (Kaggle) | Principal: 125.855 jogos publicados até novembro de 2026 (125.854 após a limpeza) |
| `Language.csv` | Steam Hardware & Software Survey (Valve) | Apoio: pesquisa de abril de 2022, comparada com a de outubro de 2026 |

As bases são cruzadas **por idioma**, não por jogo.

## Regras de negócio

**Público e hits**
- O público é medido em **donos estimados**, informados em faixas; nos cálculos usa-se o ponto médio de cada faixa.
- Um jogo é **hit** quando tem mais de 100 mil donos estimados. Taxa de hits da base: **7,0%** (8.819 jogos).
- Só entram nos rankings grupos com pelo menos 30 jogos (30 lançamentos por período, no caso das tags).

**Mercados de idioma**
- Sem vendas por região, o idioma suportado pelo jogo representa o mercado: inglês (América do Norte e global), alemão (Europa), japonês (Japão) e chinês simplificado (China e Ásia).
- Um jogo conta em todos os idiomas e gêneros que possui, então as participações somam mais de 100%. Na comparação Japão × Ocidente os grupos são exclusivos.

**Faixas de preço**
- Gratuito (26.660 jogos), até US$ 10 (86.864), de US$ 10 a US$ 30 (11.184) e acima de US$ 30 (1.146).

**Modo de jogo**
- Definido pelas tags Singleplayer, Multiplayer e Co-op, disponíveis em 83.353 jogos (66,2%). Um jogo pode ter mais de uma tag.

**Nota e avaliações**
- Medida principal: % de avaliações positivas na Steam, para jogos com pelo menos 50 avaliações (30.624 jogos).
- Complemento: nota do Metacritic (abaixo de 60, de 60 a 79 e 80+), disponível em só 4.258 jogos (3,4%). A relação com o público é medida pela correlação de Spearman.

**Tendências**
- Comparação dos lançamentos de 2020–2022 com os de 2023–2025, pelo número de lançamentos (os donos são acumulados e favorecem jogos antigos). Jogos de 2026 ficam fora.
- Na Survey, compara-se abril de 2022 com outubro de 2026; variação acima de 2 p.p. é considerada relevante.

## Limpeza dos dados

A limpeza gerou `games_clean.csv` (125.854 jogos, 41 colunas) e `language_clean.csv` (29 idiomas).

- **Linha removida:** 1 jogo gratuito, sem público estimado, com Playtest ou Demo no nome e sem gênero nem tags (efeito desprezível).
- **Cabeçalho corrigido** (colunas "Discount" e "DLC count" coladas) e faixas de donos convertidas em número, com indicador de hit.
- **Novas colunas:** faixa de preço, ano de lançamento, período 2020–2025, % de avaliações positivas, quatro indicadores de idioma e grupo de idade.
- **Gêneros:** mantidos só os gêneros de jogo. 1.711 jogos com apenas rótulos de loja ou software (como Utilities e Education) ficaram sem gênero, totalizando 10.133 jogos sem gênero.
- **Idiomas (Survey):** coluna "Laguage" corrigida, nomes padronizados e variação convertida em número. Os valores de outubro de 2026 foram digitados no notebook, a partir da página da Valve.
- **Mantidos:** playtests, demos, nomes repetidos, jogos sem avaliação e jogos de audiência gigante, com o efeito controlado pelo uso da mediana.

## Perguntas e resultados

| # | Pergunta | Resultado |
|---|----------|-----------|
| 1 | Gênero por mercado | Action, Adventure e Indie lideram nos quatro mercados. Só o chinês simplificado tem oferta de jogos (27,6%) próxima do peso entre usuários (25,6%); o inglês tem 89,1% dos jogos para 35,8% dos usuários. |
| 2 | Nota e público | Na Steam, a relação é muito fraca (ρ = 0,01). No Metacritic é moderada (ρ = 0,31): jogos com nota 80+ têm 9,7 vezes o público médio dos de nota abaixo de 60. |
| 3 | Gênero e preço | Maiores taxas de hits: Massively Multiplayer gratuito (21,7%), Massively Multiplayer de US$ 10 a 30 (19,0%) e Action de US$ 10 a 30 (14,1%), contra 7,0% da base. |
| 4 | Multijogador × um jogador | Taxa de hits de 32,4% no Co-op, 30,7% no Multiplayer e 12,5% no Singleplayer; mediana de donos de 35 mil contra 10 mil. |
| 5 | Quem cresce e cai | Simulation (+2,54 p.p.), RPG (+1,45) e Strategy (+1,12) ganharam espaço; Action (−2,25), Indie (−1,54) e Adventure (−1,16) perderam. Entre os usuários, o inglês subiu 3,23 p.p. e o chinês simplificado caiu 3,65 p.p. |
| 6 | Japão × Ocidente | Action, Adventure, Indie, Strategy e Casual têm participação mais de 5 p.p. diferente no grupo japonês (30.216 jogos) em relação a inglês e alemão sem japonês. |

## Recomendações para a publisher

- Investir em jogos com **multijogador ou cooperativo**: a taxa de hits é mais que o dobro da do Singleplayer.
- Escolher gênero e preço pela taxa de hits: **Massively Multiplayer, Action, Strategy e RPG**, de preferência na faixa de **US$ 10 a US$ 30**.
- Acompanhar **Simulation, RPG e Strategy**, que ganham espaço nos lançamentos.
- Não depender só da nota dos jogadores: ela quase não se relaciona com o público; divulgação e alcance pesam mais.
- **Evitar** Casual e Indie acima de US$ 30 e Casual até US$ 10, que têm as menores taxas de hits (1,2%, 2,9% e 3,8%).
- Diferenciar-se em Action, Indie e Adventure: perderam participação nos lançamentos (mais concorrência), mas continuam liderando em público.


## Como usar

- **Notebook:** `dashboard_steam_v2.ipynb`. Abra pelo botão "Abrir no Colab" acima e execute as células em ordem.
- **Dashboard:** `dashboard_steam.html`. Abra no navegador, use **F11** para tela cheia e as setas **← →** para navegar.
