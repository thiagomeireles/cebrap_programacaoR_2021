# Tutorial 4: Visualização de Dados com *ggplot2*

Para este tutorial vamos usar novamente dados de Fakeland, como no Tutorial 1. No entanto, utilizaremos um conjunto maior de cidadãos, 200, e não apenas 30 como anteriormente. Além disso, utilizaremos um número reduzido de variáveis para facilitar nosso trabalho.

Antes de abrir os dados, carregaremos o *tidyverse* que contém as bibliotecas como para abertura de dados (*readr*), manipulação de dados (*dplyr*) e produção de gráficos (*ggplot2*):

```{r}
library(tidyverse)
```

Em sequência, carregaremos os dados que estão no repositório do curso:

```{r}
url_fake_data <- "https://raw.githubusercontent.com/thiagomeireles/cebrap_programacaoR_2021/main/data/fake_data_2.csv"
fake <- read_delim(url_fake_data, delim = ';', col_names = T)
```

## ggplot2: uma Gramática de Dados

Vimos no Tutorial 2 como a *dplyr* é um dos destaques do *tidyverse* voltando para a manipulação de dados. Outro ponto que merece bastante atenção dentro do universo *tidy* é o *ggplot2*, uma nova gramática para visualização de dados. Entre suas vantagens, a flexibilidade e a aplicabilidade a diversas classes de objetos (como *dataframes*, mapas, redes, etc.), produz gráficos com uma qualidade muito superior ao pacote gráfico do *base*.

A integração entre a *dplyr* e o *ggplot2* é bastante ampla. Primeiramente, temos que entender que as informações para produção do gráfico estão em um *dataframe*. Assim, cada linha (observação) é uma "unidade" exibida no gráfico e cada coluna (vetor) é uma variável que auxilia na construção de aspectos específicos desse gráfico, como posição, cor, tamanho ou forma.

Este tutorial trata da compreensão da estrutura de código para produção de gráficos com *ggplot2* utilizando exemplos simples e, de forma proposital, não trataremos de todas as possibilidades de visualização pelo grande número de opções.

De forma geral, a estrutura muda muito pouco entre diferentes gráficos e você perceberá isso ao final do tutorial. Cada novo tipo de gráfico, chamada de "geometria", possui apenas pequenas diferenças entre si. Assim, para produção de gráficos que não trataremos no tutorial, bastará acrescentar uma linha de código ao que produziremos por aqui.

Já falamos muito sem nenhuma ação, não? Vamos para um primeiro exemplo para ilustrar tudo isso.

## Um primeiro exemplo de gráficos com uma variável discreta

Em nossa primeira tentativa, vamos conhecer a distribuição da preferência de candidato à presidência pelos cidadãos de Fakeland a partir da nossa amostra. Apresentaremos a informação em um gráfico de barras a partir do *ggplot2*:

```{r}
fake %>% 
  ggplot() +
  geom_bar(aes(x = candidato))
```

Temos coisas diferentes por aqui, assim é importante entender cada parte do nosso código.

Primeiramente, vemos a integração do *dplyr* com o *ggplot2* quando indicamos qual o nossso *dataframe* e utilizamos o *pipe*. Como ressaltado desde o começo do Tutorial, é uma das vantagens de se trabalhar com pacotes do universo *tidyverse*. Perceba que aqui não realizamos nenhuma atribuição do resultado a um objeto, assim, somente queremos "imprimir" esse gráfico e não "guardá-lo".

Agora para os comandos relacionados diretamente à produção gráfica. Aqui, na primeira linha, utilizamos a principal função do código, a *ggplot* (sem o 2 mesmo). Bastante intuitivo a função base do nosso gráfico seguir a base do nome do pacote, não? 

O principal parâmetro da função *ggplot* é o "data", ou seja, o objeto que contém os dados que queremos visualizar que, no nosso caso, é o *dataframe* "fake". Mas, como já vimos, podemos integrar nosso *dataframe* à função *ggplot* como *pipe* e colocá-lo no início do código. "Traduzindo" para o português, "chamamos" o objeto "fake" e realizamos determinada operação que, nesse caso, é a produção de um gráfico com a função *ggplot*. Aqui é bem parecido com o que vimos ao trabalhar com o *dplyr*, não? Esse tipo de abordagem pode ser bem útil e ficará mais claro em breve.

No entanto, com a função *ggplot* apenas iniciamos a construção do gráfico, mas ainda não indicamos qual o conteúdo que queremos.

Para isso, adicionaremos uma geometria colocando um símbolo de "+" após fecharmos o último parêntese da função *ggplot*. Aqui estamos adicionando "camadas" ao nosso gráfico, por isso é bastante intuitivo pensar na adição de novas geometrias. E, a cada "+", é uma nova camada no nosso gráfico.

Mas quais camadas adicionamos? É aí que a definição de uma geometria importa pois ela indica qual o tipo de visualização que queremos. Ao utilizar a *geom\_bar* indicamos uma geometria de barras como em um "bar chart" em outros softwares de análise de dados. 

É sempre importante pensar no tipo de dados que queremos visualizar para definir qual a geometria adequada. Aqui, por exemplo, analisamos a distribuição de preferência dos cidadãos sobre os candidatos à presidência que é uma variável discreta (*character* ou *factor*). Assim, precisamos de uma geometria que se comunique bem com dados discretos. Um gráfico de barras representa a contagem de frequência de cada categoria discreta então, assim, faz sentido utilizar a geometria *geom\_bar*. Ainda trabalharemos com geometrias que correspondam a dados de outros tipos. 

Ainda falta um parâmetro estranho e que, normalmente, não estabelecemos uma relação direta. É o *aes*, abreviação para "aesthetics". Aqui definimos quais variáveis do nosso *dataframe* serão utilizdas para a produção do gráfico. Por enquanto, utilizamos apenas uma variável, representada no eixo horizontal (ou eixo "x). Por isso apenas indicamos a variável para o parâmetro "x" da "aesthetics" e nada mais.

## Gráficos com uma variável contínua

### Gráficos de histogramas

Vamos fazer um teste trocando, rapidamente, o valor de "x" dentro de "aesthetics" para uma variável contínua como renda.

```{r}
fake %>% 
  ggplot() + 
  geom_bar(aes(x = renda))
```

Acabamos gerando um gráfico em branco, mas por quê? Tentamos representar uma variável contínua com uma geometria construída para variáveis discretas. Como os valores de renda são únicos, existe uma barra para cada indíviduo que é tão minuscula que torna o gráfico sem sentido. Assim, precisamos de uma outra geometria que equivala a um "gráfico de barras para variáveis contínuas", ou seja, um histograma. Para isso, utilizamos a *geom_\histogram*:

```{r}
fake %>% 
  ggplot() + 
  geom_histogram(aes(x = renda))
```

Faz mais sentido, não? Espero que sim. Agora compare os códigos para gerar os dois gráficos acima e tente entender as diferenças. percabva que o tipo de variável é o que determina a geometria escolhida e não o contrário.

#### Exercício para casa

A partir dos dados de Fakeland, crie um gráfico que mostre o nível de apoio para cada partido (variável discreta) e um outro que mostre a distribuição da idade (trate a idade como variável contínua).

#### Parâmetros fixos 

Dentro de cada geometria, com sua utilidade própria, existem outros parâmetros que tambmém podemos alterar. Por exemplo, produzimos um histogramas em que as barras (intervalos) são muito "fininhas". Podemos aumentar esse intervalo (largura), ou seja, representar "mais valores" do eixo "x" em cada barra do histograma:

```{r}
fake %>% 
  ggplot() + 
  geom_histogram(aes(x = renda), 
                 binwidth = 4500)
```

Perceba um ponto importante: o parâmetro *binwidth* (a largura da barra) é especificado **FORA** do *aes*. Mas por que isso? Existe uma regra muito importante na gramática do *ggplot2*: parâmetros que **DEPENDEM** dos nossos dados devem ficar dentro do *aes*, enquanto parâmetros fixos, aqueles que **NÃO DEPENDEM** dos nossos dados, devem ficar de fora do *aes*. Assim, no nosso código temos uma variável dentro do *aes*, a renda, e um número que independe dos dados fora do *aes*, 4500.

No início do tutorial falamos da produção de gráficos bastante bonitos, mas, por exemplo, só produzimos gráficos cinzas até agora. Poderíamos mudar as cores, não? Pensando nessa discussão de parâmetros fixos, se quisermos aplicar uma cor ou um preenchimento às nossas barras, onde esses parâmetros são especificados? Como queremos determinar essas cores de forma fica para todo o gráfico, isso não depende dos nossos dados e, portanto, são determinados fora do *aes*.

```{r}
fake %>% 
  ggplot() + 
  geom_histogram(aes(x = renda), 
                 binwidth = 4500, 
                 color = "red", 
                 fill = "green")
```

A escolha de cores não foi muito boa, né? Ficou bastante estranho. Mas esse exemplo é útil para perceber que podemos trocar as cores do contorno (*color*) e do preenchimento (*fill*) das barras. De forma feral, ambos parâmetros são utilizados por boa parte das geometrias do *ggplot2*.

Dica: R aceita duas grafias distintas em inglês para a palavra cor, "colour" (britânico) e "color" (norte-americano).

### Gráficos de densidade

Histogamas são, em regra, bastante adequados para variáveis numéricas com valores mais epaçados. É o caso das variáveis numéricas discretas que são representrados por números inteiros, como variáveis de idade ou número de filhos.

No entanto, para variáveis verdadeiramente contúnuas, como a renda, temos uma opção mais elegante com os gráficos de densidade.

Assim, precisamos apenas alterar a geometria para a mesma variável, renda, e observar novamente sua distribuição. O importante é que, mesmo que a geometria corresponda ao tipo do dado, existem diversas geometrias que funcionam para um tipo específico de dados, como o histograma e o gráfico de densidade.

```{r}
fake %>% 
  ggplot() + 
  geom_density(aes(x = renda))
```

Ainda cinza demais, não? Vamos mudar a cor da borda:

```{r}
fake %>% 
  ggplot() + 
  geom_density(aes(x = renda), 
               color="blue")
```

Um pouco melhor, mas vamos adicionar cor ao interior da curva:

```{r}
fake %>% 
  ggplot() + 
  geom_density(aes(x = renda), 
               color="blue", 
                 fill="blue")
```

Agora ficou estranho. Podemos deixar o interior da curva mais transparente:

```{r}
fake %>% 
  ggplot() + 
  geom_density(aes(x = renda), 
               color="blue", 
               fill="blue",
               alpha=0.2)
```

Bem melhor, não? Mas não temos nenhuma referência para auxiliar a leitura do gráfico. Podemos, por exemplo, adicionar uma linha vertical que indique onde fica a média da distribuição. Vamos, em um primeiro momento, calcular a média da renda:

```{r}
media_renda <- mean(fake$renda)
```

Voltando para nosso gráfico, é uma curva de densidade. Nessa geometria não há um parâmetro para representar uma linha vertical. No entanto, podemos adicionar uma nova geometria com uma nova "aesthetics" para novos dados (aqui, o dado único da média da renda) ao gráfico que construímos anteriormente:

```{r}
fake %>% 
  ggplot() + 
  geom_density(aes(x = renda), 
               color="blue", 
               fill="blue",
               alpha=0.2) +
  geom_vline(aes(xintercept = media_renda))
```

Bacana, não? Com *ggplot2* podemos adicionar novas geometrias utilizando os mesmos ou outros dados sempre que precisar. Lembram das diferentes camadas dentro do mesmo objeto *ggplot*? Aqui possuímos duas camadas com duas geometrias. Essa é uma das razões que torna a estrutura do código deste pacote tão diferente da estrutura do pacote *base*. O pacote nos dá uma flexibilidade muito grande para adicionar diferentes geometrias, mesmo com dados que não fazem parte da estrutura inicial do nosso gráfico. É uma das principais vantagens da *ggplot2*.

Vamos deixar o gráfico mais interesante, com uma aparência um pouco mais atrativa. Vamos alterar a forma e a cor da linha adicionada ao gráfico anterior:

```{r}
fake %>% 
  ggplot() + 
  geom_density(aes(x = renda), 
               color="blue", 
               fill="blue",
               alpha=0.2) +
  geom_vline(aes(xintercept = media_renda),
             linetype="dashed",
             color="red")
```

Você já deve ter percebido que "linetype" é um novo parâmetro (e bastante intuitivo). Ele é bastante comum e pode ser aplicado a diversas geometrias - aquelas com linhas, claro.

#### Exercício para casa

Crie um gráfico de densidade de idade e adicione uma linha vertical que indica as pessoas com mais de 21 anos de idade. Ajuste a formatação para uma combinação de cores que seja atrativa.

## Gráficos com uma variável contínua e uma variável discreta

### Histogramas e densidade divividos em grupos

Vamos voltar ao histograma para verificar alguns passos que não realizamos. Por exemplo, o que precisamos fazer para comparar as distribuição de renda por sexo? É necessário filtrar os dados e fazer um gráfico para cada categoria de sexo?

Olha, até poderíamos fazer asssim. No entanto, é muito mais simples realizr a comparação de distribuições em um mesmo gráfico. Para isso, devemos entender como visualizar duas variáveis do nosso *dataframe* ao mesmo tempo. Isso é realitvamente simples. Como queremos dividir a distribuição de uma variável contínua (renda) em dois grupos a partir de uma variável discreta (sexo), apenas incluímos essa segunda variável à "aesthetics":

```{r}
fake %>% 
  ggplot() + 
  geom_histogram(aes(x = renda,
                     fill = sexo), 
                 binwidth = 4500)
```

Perceba que agora trouxemos o parâmetro "fill" para a "aesthetics" (dentro do *aes*) porque ele depende de nossos dados, no caso, a variável sexo. Basicamente, estamos indicando que a variável sexo separará as distribuições de renda em cores de preenchimento diferentes. Perceba que, por isso, agora temos uma legenda. Conseguimos ver as distribuições assim, uma atrás da outra? Note que é um tanto quanto difícil.

A sobreposição dos dois histogramas dificulta a visualização de todos os dados. Existem opções para ajustar a forma de exibição dos dois conjuntos de dados no parâmetro "position". Por exemplo, podemos organizar os dados lado a lado com *position="dodge"*:

```{r}
fake %>% 
  ggplot() + 
  geom_histogram(aes(x = renda, 
                     fill = sexo), 
                 binwidth = 4500, 
                 position = "dodge")
```

Melhor, mas ainda ainda podemos trabalhar um pouco mais nisso. Vamos tentar alguns dos passos que fizemos com as curvas de densidade. Vamos substituir o "fill" por "color" para aplicar a variável sexo dentro da "aesthetics" (lembre-se que agora é dentro da *aes*) e separar as distribuições por cores de borda:

```{r}
fake %>% ggplot() + 
  geom_density(aes(x = renda, 
                   color = sexo))
```

Agora melhorou bastante, não? Muito mais informativo. Vamos realizar o mesmo com "fill":

```{r}
fake %>% 
  ggplot() + 
  geom_density(aes(x = renda,
                   fill = sexo))
```

Um pouco estranho, mas podemos melhorar. Lembram do parâmetro "alpha" usado na prórpia distribuição de densidade para deixar mais "transparente"? Vamos observar como atuam aqui e seus efeitos sobre as áreas sobrepostas.

```{r}
fake %>% 
  ggplot() + 
  geom_density(aes(x = renda, 
                   fill = sexo), 
               alpha=0.5)
```

Agora sim! Conseguimos ver bem como as duas curvas se comportam e em que pontos existe sobreposição.

Vamos dar mais um passo e usar "fill" e "color" ao mesmo tempo na "aesthetics".

```{r}
fake %>% 
  ggplot() + 
  geom_density(aes(x = renda, 
                   fill = sexo, 
                   color = sexo), 
               alpha = 0.5)
```

Apesar de precisar de alguns ajustes, veja como o gráfico fica bonito! 

Esse tipo de comparação, entre distribuições de uma variável contínua por uma variável discreta (aqui binária - duas categorias) é uma das mais úteis na ciência por ser a forma gráfica de testes de hipóteses cássicos. 

Qual grupo tem maior renda, na média, em Fakeland? Os gráficos tornam a resposta fácil, não?

#### Exercício para casa

As pessoas mais ricas do Fakeland estão mais propensas a serem membros do partido "Conservative Party"? Crie um gráfico claro para mostrar a relação entre essas variáveis.

### Gráficos de boxplot

Vamos testar a aplicação do mesmo gráfico acima, mas substituindo as distribuições por sexo por uma variável com mais categorias. Com "educ" temos representado o nível educacional mais alto obtido pelos indivíduos de Fakeland em quatro categorias.

```{r}
fake %>% ggplot() + 
  geom_density(aes(x = renda, 
                   fill = educ, 
                   color = educ), 
               alpha = 0.5)
```

Conseguimos distinguir as distribuições da renda entre os grupos? Muito difícil, não? Poderíamos ter uma primeira impressão de que não existe muita diferença, mas o gráfico é poluído demais.

Uma alternativa para reprentar distribuições de variáveis numéricas entre grupos são os "boxplots". Vamos aplicar um exemplo que é uma alternativa ao gráfico anterior. Perceba, que aqui, nossa "aesthetics" tem um elemento em "x", eixo horizontal, e um em "y", eixo vertical.

```{r}
fake %>% 
  ggplot() + 
  geom_boxplot(aes(x = educ, 
                   y = renda))
```

**Importante**: caso não tenha familiaridade com boxplots, peça uma explicação ao professor.

Mesmo que tenhamos alguma perda de informação, agora é possível comparar as distribuições de renda por nível educacional de forma muito rápida. A renda média das pessoas com "college degree" é maior que dos outros grupos, ainda que a mediana seja muito próxima de "college incomplete". Além disso, aqueles com "High school degree" apresentam uma grande variação nos rendimentos. 

Podemos trazer um pouco de cor aos boxplots utilizando "fill" novamente.

```{r}
fake %>% 
  ggplot() + 
  geom_boxplot(aes(x = educ, 
                   y = renda, 
                   fill = educ))
```

Quando queremos conhecer a distribuição dos dados que coletamos ou utilizamos de fontes secundárias, existe uma diferença entre as melhores opções a depender do tipo de variável. Para variáveis categóricas, gráficos de barras; para variáveis contínuas, histogramas, curvas de densidade e boxplot são mais indicados.

Um novo gráfico que tem se popularizado como alternativa ou complemento ao boxplot são os gráficos de violino. Basicamente, eles apresentam as informações da distribuição de forma semelhante aos boxplots, mas incorporam elementos da densidade ao longo da distribuição. Vamos a um exemplo:

```{r}
fake %>% 
  ggplot() + 
  geom_violin(aes(x = educ, 
                   y = renda, 
                   fill = educ))
```

Parece estranho, não? Vamos ajustar esse gráfico em dois passos. No primeiro, criaremos um boxplot como acima, mas mudaremos a cor do seu contorno e removeremos o preenchimento. Em seguida, acrescentaremos uma camada com o "violino", também mais transparente e retirando as cores do contorno. Para tirar cores e preenchimento, indicamos o argumento "NA" fora do *aes*. Vejamos o resultado

```{r}
fake %>% 
  ggplot() + 
    geom_boxplot(aes(x = educ, 
                   y = renda,
                   color = educ), fill = NA) +
  geom_violin(aes(x = educ,
                  y = renda, 
                  fill = educ), alpha = 0.5, color = NA) 
```

Agora temos uma informação um pouco mais completa, não? Além da indicação dos quartis que o boxplot oferece, sabemos em que pontos a distribuição se concentra para cada um dos grupos.

## Gráficos de duas variáveis contínuas

Por enquanto vimos apenas a distribuição de uma única variável, ainda que na última seção a distribuição fosse da variável contínua em grupos definidos por variáveis discretas - ou a separação da dsitribuição de uma variável em várias a partir de uma variável categórica.

Agora vamos ver como relacionar graficamente duas variáveis contínuas. O padrão e utilizar a gemoetria de um gráfico de dispersão. Nele, cada ponto representa o par de informações de uma observação como uma coordenada em um espaço bidimensional. Como exemplo, vamos observar a distribuição da renda (eixo vertical, o "y") pela idada (eixo vertical, o "x") usando a geometria *geom\_point*:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade,
                 y = renda))
```

Consegue ler o gráfico? Cada ponto representa um indivíduo, ou seja, o posiciona no espaço a partir da idade e da renda daquele indivíduo.

Percebemos que existe uma certa tendência nos dados, não? Quanto mais velha a pessoa, maior a sua renda. Esse tipo de relação pode ser representada por modelos lineares e não lineares. A geometria *geom\_smooth* é específica para a representação gráfica desses modelos.

Sua utilização é condicionada ao método (parâmetro "method") para modelagem dos dados. O mais comum é a representação da relação entre as variáveis por uma reta. Este é o "linear model", representado por "lm". Vamos ao exemplo e ignore o parâmetro "se" por enquanto:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda)) +
  geom_smooth(aes(x = idade, 
                  y = renda), 
              method = "lm", 
              se = FALSE)
```

Bacana, não? Mas agora voltaremos ao parâmetro "se". Se o retirarmos, ele volta para seu padrão, "TRUE". Ele indica o intervalo de confiança (95\% no padrão) da relação que inserimos.

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda)) +
  geom_smooth(aes(x = idade, 
                  y = renda), 
              method = "lm")
```

Modelos de regressão estão bastante distantes do foco do curso, seja com modelos lineares ou não. Vamos apenas tentar interpretar o resultado gráfico.

A principal alternativa não linear para representar a relação dos dados a partir dessa geometria é o método "loess" (local weighted regression). Vamos ao resultado:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda)) +
  geom_smooth(aes(x = idade, 
                  y = renda), 
              method = "loess")
```

Um pouco mais estranho, não? Parte disso é por utilizarmos uma variável com valores discretos (ainda que muitos) como variável contínua (idade).

## Gráficos de três ou mais variáveis

De forma geral, estamos limitados ao papel e telas bidimensionais para exibição apenas de geometrias de duas variáveis. No entanto, existem uma saída para traze mais informações: a inclusão de outros parâmetros de uma geometria. Já vimos as cores, tamanhos e também temos as formas como opções a incluir dentro do *aes* como uma terceira variável do nosso *dataframe*.

Podemos, por exemplo, representar uma terceira variável numérica. Para isso, podemos ajustar o tamanho dos pontos (raio dos círculos) a essa variável. Usando nosso *dataframe* sobre Fakeland, poderíamos adicionar o número de filhos, variável que vai de 1 a 10, da seguinte forma:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda, 
                 size = filhos))
```

Outra possível ação seria a utilização de uma terceira variável discreta. Para isso, poderíamos alterar a cor ou a forma dos pontos baseados em uma variável categórica. Vamos aplicar com sexo, como fizemos anteriormente trabalhando somente com uma variável contínua:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda, 
                 color = sexo))
```

Ou aplicando a diferenciação do formato dos pontos:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda, 
                 shape = sexo))
```

Obs: Cada símbolo utilizado nas geometrias do *ggplot2* possui correspondência com um número e é facilmente encontrada no [Cheat Sheet do ggplot2](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf). 

Podemos, também, alterar simultaneamente cor e forma:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda, 
                 color = sexo, 
                 shape = sexo))
```

O método também é aplicado para outras geometrias, como criarmos uma reta de regressão para cada categoria de sexo com a *geom\_smooth*

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda, 
                 color = sexo, 
                 shape = sexo)) +
  geom_smooth(aes(x = idade, 
                  y = renda, 
                  color = sexo, 
                  shape = sexo), 
              method = "lm", 
              se = F)
```

Diga a verdade: é fácil se apaixonar pelos gráficos *ggplot2*.

Existe uma outra forma de apresentar mais de duas variáveis. Podemos criar múltiplos gráficos organizados em uma única grade sem necessidade de repetir o código para cada categoria. Isso é possível incluindo uma nova camada ao noso objeto *ggplot2* com a função *facet\_wrap*. Vamos a um exemplo:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda)) +
  facet_wrap(~sexo)
```

Poderíamos aplicar a função, também, para variáveis com mais categorias, como a "educ":

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda)) +
  facet_wrap(~educ, nrow = 2)
```

Veja que incluímos um novo parâmetro na função *facet\_wrap*, a *nrow*. Ela indica, como inuitivamente já deve ter pensado, o número de linhas que queremos em nossa grade de gráficos. Poderíamos ter definido a partir do número de colunas com o parâmetro *ncol*. 

### Exercício para casa

Vamos usar o banco de dados menor de Fakeland para investigar a relação entre renda, poupança e número de crianças. Começando com o código abaixo, crie um gráfico de dispersão entre renda (income) e poupança (savings), e ajuste o tamanho de cada ponto dependendo do número de crianças (kids). 

```{r}
fake_menor <- read_delim("https://raw.githubusercontent.com/thiagomeireles/cebrap_programacaoR_2021/main/data/fake_data.csv", delim = ";", col_names = T)
```

## Aspectos não relacionados à geometria

Uma coisa que deve ter incomodado é a falta de atribuição de rótulos a nossos eixos e legendas ou mesmo títulos. Estes são aspectos não relacionados aos dados, geometria ou "aesthetics". Os procedimentos para adicionar alterações em títulos, eixos, legendas, etc. é o mesmo utilizado para adicionar novas geometrias/camadas.

Em primeiro lugar, vamos adicionar um título ao gráfico:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda, 
                 color = sexo)) +
  ggtitle("Renda por idade, separado por sexo")
```

Em seguida, vamos modificar os nomes dos rótulos dos eixos:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda, 
                 color = sexo)) +
  ggtitle("Renda por idade, separado por sexo") +
  xlab("Idade (em anos inteiros)") +
  ylab("FM$ (Fake Money)")
```

E agora, o título da legenda:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda, 
                 color = sexo)) +
  ggtitle("Renda por idade, separado por sexo") +
  xlab("Idade (em anos inteiros)") +
  ylab("FM$ (Fake Money)") +
  guides(color=guide_legend(title="Sexo"))
```

O *ggplot2* permite a customização de praticamente todos os elementos do estilo do nosso gráfico, mas isso pode levar a algumas confusões pelo excesso de detalhes. Para alterar o estilo do nosso gráfico é muito mais fácil trabalhar com algum tema (*theme*) pré-definido. Vamos, por exemplo, utilizar o tema preto e branco (*theme\_bw*) para tirar o preenchimento cinza:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda, 
                 color = sexo)) +
  ggtitle("Renda por idade, separado por sexo") +
  xlab("Idade (em anos inteiros)") +
  ylab("FM$ (Fake Money)") +
  guides(color=guide_legend(title="Sexo"))+
  theme_bw()
```

No entanto, não existe apenas uma opção para estabelecer essas mudanças ou atribuição de rótulos aos eixos ou título. Particularmente, prefio a utilização de uma camada específica para os labels, a *labs*, onde podemos indicar todos os rótulos que queremos. Aqui, além da cor, também diferenciaremos a forma dos pontos para observarem a aplicação da legenda:

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda, 
                 color = sexo,
                 shape = sexo)) +
  labs(title = "Renda por idade, separado por sexo",
       x     = "Idade (em anos inteiros)",
       y     = "FM$ (Fake Money)",
       color = "Sexo",
       shape = "Sexo") +
  theme_bw()
```

Perceba que para que o *ggplot2* entendesse que "color" e "shape" representavam a mesma coisa, foi preciso indicar ambos os argumentos na construção dos rótulos.

Um ponto crescente de preocupação na visualização de gráficos é a acessibilidade a pessoas daltônicas. Existem alguns pacotes que oferecem paletas de cores para lidar com isso, como a *viridis*. Vamos instalá-la:

```{r, eval=FALSE}
install.packages('viridis')
```

Vamos refazer o último gráfico aplicando as cores do pacote. Para isso, utilizamos a função *scale_color_viridis*, que possui especificações próprias para cada tipo de variável. Aqui, como a cor é aplicada a partir de uma variável discreta, utilizamos o *scale_color_viridis_d*. Existem algumas combinações em paletas que são definidas por letras maiúsculas. Assim, indicamos o parâmetro *option* que possui a combinação "D" como padrão. Vamos com a opção "C", "plasma":

```{r}
fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda, 
                 color = sexo,
                 shape = sexo)) +
  scale_color_viridis_d(option = "C") +
  labs(title = "Renda por idade, separado por sexo",
       x     = "Idade (em anos inteiros)",
       y     = "FM$ (Fake Money)",
       color = "Sexo",
       shape = "Sexo") +
  theme_bw()
```

Você percebeu que existem outras "scale_color_etc". Para as opções de cor, preenchimento e forma, é possível aplicar outras opções pré-definidas pelo *ggplot2* ou fazer combinações manuais com as funções de camadas terminadas em "manual".

Além disso, existem temas que replicam estilos de outras fontes profissionais, por exemplo usando o pacote *ggthemes*.

```{r, eval=FALSE}
install.packages("ggthemes")
```

Vamos criar um gráfico usando o estilo da revista "The Economist" em uma linha só de código?

```{r}
library(ggthemes)

fake %>% 
  ggplot() + 
  geom_point(aes(x = idade, 
                 y = renda, 
                 color = sexo)) +
  labs(title = "Renda por idade, separado por sexo",
       x     = "Idade (em anos inteiros)",
       y     = "FM$ (Fake Money)",
       color = "Sexo") +
  theme_economist()
```

Muito bacana, não?

### Exercício para casa

Melhore seu gráfico do Exercício para casa anterior especificando um título, títulos de eixos e um tema de sua preferência.