# Brasileirão Data Analytics: O Impacto do Clima e da Tabela nos Resultados

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Lab-orange.svg)](https://jupyter.org/)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED.svg)](https://www.docker.com/)

## Sobre o Projeto

Este projeto de Ciência de Dados tem como objetivo analisar os resultados do Campeonato Brasileiro (Série A), cruzando dados esportivos com informações meteorológicas históricas.

A motivação principal foi responder a uma hipótese comum no futebol brasileiro: **"Os times do Nordeste e do Rio de Janeiro possuem vantagem ao jogar em casa sob forte calor?"**. Além disso, o projeto expandiu para analisar o impacto dos horários das partidas, a dificuldade das viagens inter-regionais e a probabilidade estatística de "zebras" baseada na posição da tabela.

## Tecnologias Utilizadas

- **Linguagem:** Python
- **Web Scraping:** `requests`, `BeautifulSoup` (Coleta de dados do Transfermarkt)
- **APIs:** [Open-Meteo API](https://open-meteo.com/) (Coleta de dados climáticos históricos de forma gratuita)
- **Manipulação de Dados:** `pandas`, `numpy`, `re` (Expressões Regulares)
- **Visualização de Dados:** `matplotlib`, `seaborn`
- **Infraestrutura:** Docker & Docker Compose (Ambiente Jupyter Lab isolado)

## Perguntas de Pesquisa

O projeto foi estruturado para responder às seguintes perguntas fundamentais:

1. **O Fator Calor:** O clima quente (>= 28°C) aumenta significativamente a taxa de vitória dos mandantes do Nordeste e do Sudeste (RJ)?
2. **O Fator Horário:** Jogar no turno da tarde (sob o sol das 16h) ou à noite altera o desempenho do mandante?
3. **Ritmo de Jogo:** O calor extremo diminui o número total de gols em uma partida devido ao cansaço?
4. **O Choque Regional:** Como os times (ex: da região Sul) se comportam quando viajam para jogar em outras regiões em condições de calor?
5. **A Força da Tabela (Zebras):** Qual a probabilidade real de um mandante vencer baseado apenas na diferença de posições? Quantas vezes no campeonato um time consegue vencer um adversário que está 5 ou mais posições acima dele?

## Resultados e Insights Obtidos

### 1. O Fator Calor por Região

> **Resultado:**

### 2. Impacto do Horário da Partida

> **Resultado:**

### 3. Gols vs. Cansaço

> **Resultado:**

### 4. O Choque Regional (Desempenho dos Visitantes)

> **Resultado:**

### 5. Probabilidade e Zebras

> **Resultado:**

## Metodologia e Pipeline de Dados

O projeto foi construído de forma modular em um Jupyter Notebook, seguindo as etapas:

1. **Web Scraping:** Extração dos dados brutos das 38 rodadas do Brasileirão via Transfermarkt, contornando bloqueios com `headers` personalizados e atrasos aleatórios.
2. **Data Cleaning & Feature Engineering:** Uso de Regex para limpar nomes de times misturados com posições na tabela, conversão de dados de tempo e mapeamento de dicionários regionais (Norte, Sul, Sudeste, etc.).
3. **Integração com API:** Consulta robusta (com barra de progresso `tqdm` e salvamento incremental) à API do Open-Meteo para buscar a temperatura máxima exata na latitude/longitude da capital do time mandante no dia do jogo.
4. **Análise Exploratória (EDA):** Criação de um Dashboard unificado e gráficos de probabilidade para responder às perguntas de pesquisa.

## Como Executar o Projeto Localmente

Este projeto utiliza Docker para garantir que rodará em qualquer computador sem problemas de dependências.

1. Clone o repositório:
   ```bash
   git clone [https://github.com/SEU_USUARIO/NOME_DO_SEU_REPOSITORIO.git](https://github.com/SEU_USUARIO/NOME_DO_SEU_REPOSITORIO.git)
   cd NOME_DO_SEU_REPOSITORIO
   ```
2. Suba o ambiente via Docker Compose:
   ```bash
    docker-compose up -d
   ```
3. Acesse o Jupyter Lab no seu navegador:
   ```bash
    http://localhost:8888/lab
   ```
4. Abra o arquivo main.ipynb e execute as células em ordem.

## Próximos Passos (Trabalhos Futuros)

- **Granularidade Geográfica e Mando de Campo:** Mapear o estádio e a cidade exata de cada partida. Atualmente, o modelo busca o clima baseado nas coordenadas da capital do estado do mandante. Obter a localização exata corrigiria distorções quando equipes "vendem" o mando de campo para outros estados (como jogos em Brasília ou Cariacica) ou jogam fora de seus estádios originais.
- **Modelagem Preditiva:** Implementar algoritmos de Machine Learning (como Regressão Logística, Random Forest ou XGBoost) para prever as probabilidades de vitória baseando-se no clima cruzado com o horário e a diferença na tabela.
- **Painel Interativo (BI):** Conectar a base de dados final enriquecida a uma ferramenta de Business Intelligence (como PowerBI, Metabase ou Tableau) para que os usuários possam filtrar interativamente os times, turnos e regiões.
- **Fator Desgaste (Calendário):** Adicionar uma variável que calcule o tempo de descanso (em dias) desde a última partida de cada equipe, cruzando o cansaço acumulado com as altas temperaturas.

## Limitações da Análise Atual (Pontos de Atenção)

Como em todo projeto de Ciência de Dados iterativo, a transparência sobre os métodos e limitações é fundamental. Esta primeira versão (MVP) possui os seguintes pontos de atenção, que servem como roteiro para as próximas atualizações:

- **A Ilusão da Temperatura Máxima:** A consulta atual à API do Open-Meteo busca a temperatura máxima do dia (`temperature_2m_max`). No entanto, muitos jogos no Brasil ocorrem no período noturno (ex: 21h30), quando a temperatura já caiu. Em atualizações futuras, consumiremos dados horários (`hourly`) para cruzar a temperatura exata com a hora do apito inicial.
- **A Miopia da Tabela Inicial:** O cálculo de probabilidade e "zebras" baseia-se puramente na diferença de posições. Nas primeiras 5 a 10 rodadas, a tabela do campeonato é altamente volátil e não reflete a força técnica real das equipes, o que pode gerar falsas classificações de favoritismo.
- **Tamanho da Amostra (N=380):** A análise de apenas uma temporada pode criar recortes estatísticos muito pequenos (ex: quantidade de jogos matutinos no Nordeste sob frio intenso), enviesando as taxas de vitória. Para validação estatística robusta, o _scraper_ será expandido para coletar as últimas 5 temporadas.
- **Dicionário Geográfico "Hardcoded":** O mapeamento de times para seus respectivos estados foi feito manualmente para os 20 clubes da edição atual. Ao raspar dados de anos anteriores, equipes rebaixadas/promovidas não listadas cairão na categoria "Outra". Isso será substituído por um mapeamento dinâmico ou consulta a uma API de clubes.
