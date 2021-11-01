# Data Science Aplicada à Análise dos Microdados do ENEM

A análise foi dividida em 4 partes principais:

 [1. Coleta e Pré-processamento dos Dados](#1ª-etapa:-coleta-e-pré-processamento-dos-dados)
 [2. Análise Exploratória dos Dados](#2ª-etapa:-análise-exploratória-dos-dados)
 [3. Aplicação de Machine Learning](#3ª-etapa:-aplicação-de-machine-learning)
 [4. Deploy de Resultados](#4ª-etapa:-deploy-de-resultados)

## 1ª Etapa: Coleta e Pré-processamento dos Dados

A base de dados utilizada é fornecida pelo Inep [aqui](https://www.gov.br/inep/pt-br/acesso-a-informacao/dados-abertos/microdados/enem "Página do Inep contendo as bases de dados utiliizadas"). Foram escolhidos os bancos correspondentes aos anos de 2017, 2018 e 2019, totalizando, aproximadamente, 16 milhões de linhas (inscritos).

Os dados compactados obtidos do Inep estão armazenados no Google Drive e são descompactados por meio de código para uma pasta temporária chamada "DADOS". O arquivo _csv_ será convertido para _parquet_ a fim de ocupar menos espaço e melhorar o desempenho do processo.

Os dados foram, então, processados em duas partes. A primeira, realizada individualmente com os dados de cada ano utilizado, consiste em manipulações básicas e prévias dos dados de forma a facilitar a segunda etapa do processo, a Análise Exploratória, deixando, assim, diversas colunas categóricas e nominais para serem agrupadas e promoverem uma visualização interessante. 

Já na segunda parte, adiantou-se um processamento dos dados visando sua utilização nos algoritmos de _Machine Learning_ (terceira parte do projeto), agrupando todos os arquivos individuais gerados pelo pré-processamento anterior em um só arquivo _parquet_. Portanto, contem apenas colunas com valores numéricos com dados faltantes já tratados e todas as colunas alinhadas e estruturadas. Apesar desta etapa ter sido realizada com base nas colunas de interesse, este arquivo poderá ser modificado ou até este passo pode ser refeito a depender da Análise Exploratória.

Nesta etapa, utilizou-se a biblioteca _Dask_ para a manipulação dos arquivos, devido a familiaridade com _Pandas_ e a necessidade de lidar com arquivos de tamanho considerável.

## 2ª Etapa: Análise Exploratória dos Dados

Da mesma forma que a seção anterior, esta foi dividida em duas partes, mas que foram majoritariamente realizadas e apresentadas concomitantemente.

A primeira parte consiste em testes estaísticos de hipóteses para se determinar a relevância estatística de cada variável em relação à variável de nota. Foi utilizado somente os dados referentes a um ano afim de garantir a real independência das amostras, visto que é comum aqueles que nao passaram num ano anterior tentarem novamente no ano seguinte, assim como os treineiros. Neste trabalho, escolheu-se o ano mais recente disponível: 2019.

Para isso, foi estabelecido um tamanho de amostragem equivalente a 10% da população, suficiente para mitigar chances de erros α e ß (falso positivo e falso negativo, respectivamente). Não foi necessária nenhuma reamostragem e os dados apresentaram características assimétricas à curva normal, rejeitaram hipótese de normalidade de acordo com 3 testes (Shapiro-Wilk, D'Agostino e Anderson-Darling) e apresentaram variâncias não equivalentes entre si pelo teste de Brown-Forsythe, como esperado para variáveis que refletem uma característica social. Por conta disso, os testes de diferença de médias ou medianas precisaram ser todos não-paramétricos (Mann-Whitney e Kruskal-Wallis).

Já a segunda parte consistiu na visualização dos dados. O intuito foi deixar o mais explícito possível os impactos de cada variável na nota ou desistência e que fossem além de tabelas e números. Foram gráficos e mapas analisando diversas características dos inscritos e, também, da base de dados, já que foi plotado um gráfico demonstrando a ausência de dados acerca das escolas.

Nesta segunda parte, utilizou-se, algumas vezes, os dados dos três anos para fins de comparação. Para isso, já que se trata de um grande volume de dados - 16 milhões de linhas -, fez-se uso da API do Apache Spark (_PySpark_) para um armazenamento um pouco mais eficiente, além de uma melhor performance para se manipular os dados. Devido ao formato colunar, algumas funcionalidades são mais "engessadas" que no _Pandas_, por exemplo, mas atende à demanda computacional significativa dos dados.

Roadmap:

 - Realizar Testes de Nemenyi quando conveniente;

## 3ª Etapa: Aplicação de Machine Learning

Em construção: a implementar.

Roadmap:

 - Regressão linear/polinomial para verificar possibilidade de se prever a média das notas;
 - Árvores de Decisão para Regressão para as notas e Classificação para a desistência;
 - ANN para ambas situações;
 - SVC para tentar prever a situação de desistência dos candidatos;
 - Verificação de possível predição enviesada;
 - Validação acerca de _overfitting_ devido a um grande número de colunas ou até de _underfitting_ devido à quantidade de linhas e/ou não  correlação das variáveis.

## 4ª Etapa: Deploy de Resultados

Em construção: não implementado.

Roadmap:

 - Comparação análises "manuais" e com ML
 - Aplicação de explicabilidade dos resultados obtidos através do(s) modelo(s) de _Machine Learning_;
 - Discussão a respeito dos resultados explicados;
 - Proposta de políticas públicas.
