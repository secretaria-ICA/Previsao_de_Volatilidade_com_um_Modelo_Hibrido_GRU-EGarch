<!-- antes de enviar a versão final, solicitamos que todos os comentários, colocados para orientação ao aluno, sejam removidos do arquivo -->
# [Previsão de Volatilidade com um modelo híbrido GRU-EGarch](https://github.com/DanielRZO/volatility_forecast)

#### Aluno: [Daniel Ryba Zanardini de Oliveira](https://github.com/DanielRZO)
#### Orientadores: [Drª Manoela Kohler](https://github.com/manoelakohler) e [Dr Leonardo Alfredo Forero Mendoza](https://github.com/leofome8)

---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".  

---

### Resumo

Podemos considerar que a volatilidade é de suma importância no mercado financeiro e pode ser utilizado para gerenciamento de risco de portfólio e precificação de derivativos, por exemplo. Considerando o cálculo da volatilidade como um ponto crítico e buscando precisão, propomos um modelo híbrido, combinando parâmetros de um modelo heterocedástico EGarch com uma Rede Neural "Gated Recorrent Unit" (GRU), para a previsão da volatilidade em uma série de preços de barril de petróleo. Utilizaremos uma séria de preços OPEC Crude Oil Price - Nasdaq, para fazer a previsão da volatilidade. Cabe destacar que o trabalho não contempla uma análise comparativa entre modelos clássicos da família GARCH com modelos os novos modelos híbridos, apenas apresenta resultados de uma metodologia híbrida de previsão de volatilidade para os próximos três dias.

### Abstract 

We can consider volatility of great importance in the financial market and can be used for portfolio risk management and derivatives pricing, for example. Therefore the volatility calculation as a critical point and seeking accuracy, we propose a hybrid model, combining parameters from an EGarch heteroscedastic model with a "Gated Recurrent Unit" (GRU) Neural Network, for predicting volatility in an oil price series. We will use a series of OPEC Crude Oil Price - Nasdaq, to forecast volatility. The paper does not include a comparative analysis between classical GARCH family models and the new hybrid models, but presents results of a hybrid volatility forecasting methodology for the next three days.

### 1. Introdução

Na teoria de finanças a incerteza ocupa um espaço preponderante. Em geral a noção de risco está associada à variância dos retornos ou ao seu desvio padrão, que se define como sendo a volatilidade histórica da série de retornos. A volatilidade é uma variável não observável diretamente.  Previsões precisas da volatilidade são importantes para determinar, com eficácia, o valor dos derivativos, opções de determinar, da melhor forma, os mecanismos de proteção. Além disso, a volatilidade desempenha papel importante na gestão de risco de portifólio e estratégias de “hedge”.  Devido a sua importância, a volatilidade se tornou um tópico de extensas pesquisas durante os últimos anos, onde modelos econométricos vêm sendo largamente utilizados. Com o avanço da tecnologia, a literatura começa a apresentar novos modelos, como o modelo híbrido, que aqui apresentaremos.

Considerando a importância da previsão da volatilidade, tanto para as empresas de investimento como para os investidores privados, apresentaremos uma metodologia híbrida de previsão da volatilidade para os próximos 3 dias. O objetivo aqui é apenas apresentar um novo método de cálculo, não comparar resultados entre modelos, o que, também, seria bastante interessante. 

### 2. Modelagem

A primeira etapa envolveu a obtenção da série histórica dos preços de barril de óleo, que é composta por 3800 amostras de preços diários compreendidas entre 09/01/2007 à 01/10/2021, do OPEC Crude Oil Price – Nasdaq, fornecida pela biblioteca Quantdl. 

Para formulação do modelo da família GARCH (generalized autoregressive conditional heterocedasticity), foi calculada a série de retorno através da série de preços. Para escolher o melhor dos modelos de volatilidade condicional, dos modelos testados, GARCH e EGARCH (exponential GARCH), foram utilizados os critérios AIC (Akaike Information Criteria) e BIC (Bayesian Information Criteria). O modelo selecionado foi um EGarch (1,1) com distribuição skew student. Cabe destacar que foram testados alguns modelos, não sendo os testes levados a exaustão, pois não é o objetivo do presente trabalho.

Após o modelo EGarch construído, através da biblioteca ARCH, selecionamos as os atributos de volatilidade condicional e resíduos do modelo acrescentando à um data frame, que compõe os preços diários, os retornos diários, a volatilidade condicional diária, os resíduos da volatilidade condicional diária e a volatidade semanal (que será o objetivo da previsão do modelo). 

Para a preparação da base para a Rede Neural Gated Recorrent Unit (GRU), transformamos a base em uma base supervisionada, utilizando uma janela deslizante de 10 passos para trás (t-10) e 3 para frente (t+3) para todos os atributos da série temporal em cada intervalo de tempo (t). A ideia aqui foi utilizar um output de t+3 para fazer uma breve análise de persistência do modelo, não olhando apenas para t+1. 

A preparação da Rede GRU foi feita com duas camadas de GRU e uma saída. A primeira camada é composta por 60 neurônios e um dropout de 35%, a segunda camada foi composta por 25 neurônios sem dropout e a camada de saída possui três unidades (t+3). Foi utilizado um otimizados Adam com Mean Squared Erros (MSE) no loss e acurácia na métrica.

A base utilizada para treino e teste foi dividida na proporção de 75% para o treino do algoritmo e 25% para o teste. Toda a preparação da base, análises, as modelagens foram feitas em Python e apresentam-se no notebook.
### 3. Resultados

O modelo GRU-EGarch, utilizado para a previsão de volatilidade semanal para os próximos três dias, apresentou resultados interessantes. Os valores de RMSE foram: 0.004 (t+1), 0.005 (t+2) e 0.006 (t+3). Para o MSE temos: 0.00009 (t+1), 0.00014 (t+2) e 0.00646 (t+3). No MAPE observamos 25.2 (t+1), 31.6 (t+2) e 36.5 (t+3).

Graficamente podemos verificar que o resultado foi bastante satisfatório, observando que os valores previstos ficam muito próximos dos reais. Cabe destacar o diferencial para a visualização gráfica com todos os “time-steps”, ou seja, com a previsão em t+1, t+2 e t+3 em cada espaço de tempo, ou seja, podemos ver que o modelo acompanha os picos e vales, podendo ser um bom preditor de volatilidade.

### 4. Conclusões

Dada importância da mensuração da volatilidade, uma vez que pode ser considerada uma medida de risco, o modelo poderá gerar valor para qualquer instituição financeira, agente ou pessoa que necessite de elevado gerenciamento de risco, cálculo de opções, derivativos, entre outros.

Podemos observar que, no presente trabalho, não houve comparação a fim de verificar se houve, ou não, uma melhor performance com a utilização de uma rede neural frente à modelos econométricos. Cabe destacar que o modelo clássico utilizado pode ser melhor ajustado, efetuando mais testes e utilizando outros modelos da família GARCH. Também podem ser testadas utilização de outros parâmetros, de um ou mais modelos “clássicos” como imput na rede neural, a fim de utilizar o poder da rede com vários modelos e parâmetros juntos. 

O principal objetivo foi utilizar a metodologia aprendida em sala de aula para solucionar um problema de série temporal. Aqui utilizamos um modelo híbrido que, recentemente, vem sendo testado e apresentado bons resultados na literatura. 

---

Matrícula: 192.110.207

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
