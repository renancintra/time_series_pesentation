Narrowest-Over-Threshold (NOT) Detection of Multiple Change-Points and Change-point-like Features
========================================================
author: Bruno Grillo e Renan Cintra
date: Porto Alegre, abril de 2021.
Séries Temporais (PPGEst - UFRGS)

Organização do Artigo
========================================================
 <font size="4">
**1.Introduction**: Motivação, Contexto, Objetivos

**2. The framework of NOT: (Setup, Main Idea, Log-Like ratios and Cont.functions, NOT algorithm, Theoretical properties)**
- Descrição matemática do NOT
- Considerações do NOT em quatro cenários distintos, com diferentes formas de quebra estrutural (média e/ou variância)
- Função contraste tailor-made obtida a partir da razão de verossimilhança generalizada (GLR - general likelihood ratio)
- Propriedades teóricas do NOT, como consistência e taxas de convergência

**3. NOT with the strengthened Schwarz Information Criterion (sSIC) (Motivation,Algorihtm2, Choice Contrast Function sSIC, Theoretical properties of NOT with the sSIC, Computational complexity, Other practical considerations)**

 - NOT com critério de informação fortificado, sSIC (strengthened Scharwz Information Criterion) 
 - Discussão de aspectos computacionais e propriedades teóricas
 
**4. NOT under different noise types (NOT under dependent noise, Extension of NOT heavy-tailed noise)**

**5.Simulation study**

**6.Real data analysis**

</font>

Algoritmo - NOT - Objetivo
========================================================
<font size="4">
- Parte de um modelo simples para detectar “pontos de mudança” (ou mudanças na descrição paramétrica de uma $f_t$ ) que ocorrem em locais desconhecidos

\begin{equation}
 Y_t = f_t + \varepsilon_t, \qquad  t = 1, \dots, T
\end{equation}

- A abordagem proposta também permite detectar "features" a partir de aspectos distributivos de $\varepsilon_t$  (ex: variância do erro)

- Os tipos de mudança considerados a partir de uma descrição paramétrica de $f_t$.

- Num nível mais amplo, propõe-se a ideia de utilizar modelos simples nos subconjuntos da amostra (*local*) e depois agregar os resultados para obter um ajuste geral (*global*)


![Resultados Ilustrativos.](imagens/imagem_ilustracao.JPG) 

</font>
Algoritmo - NOT - Método
========================================================
<font size="5">
- **Global**: tira-se várias subamostras $Y_{s+1}, \dots, Y_{e}^{'}$, onde $0 \leqslant s < e \leqslant T$ 
  - Em cada subamostra, assume-se que apenas uma *feature* está presente e emprega-se uma função contraste feita *sob medida* obtida para encontrar o ponto/localização mais provável dessa feature.
  - Mantém-se as subamostras para qual a função contraste excede um limite (*threshold*) definido pelo usuário e descarta-se as demais.

- **Local**: das subamostras retidas, busca-se a que tem o intervalo mais curto (narrowest interval), ou seja, a que tem $e - s$ com menor valor. 
  - Ao detectar uma *feature*, o algoritmo avança recursivamente para “esquerda” e “direita” do ponto e para no intervalo onde não encontra-se nenhum outro intervalo cujo contraste exceda o threshold.

</font>



Algoritmo - NOT - Framework
========================================================
<font size="5">
Considerando $\boldsymbol{Y} = (Y_1, \dots ,Y_T)'$, dado por

\begin{equation}
Y_t = f_t +\sigma_t \varepsilon_t,  \qquad  t = 1, \dots, T
\end{equation}

Onde $f_t$ é o sinal, $\sigma_t$ é o desvio padrão do ruído, $\varepsilon_t$ é o resíduo (nesta etapa considera-se que segue uma distribuição Normal com média 0 e desvio-padrão unitário).

- Assume-se que o par $f_t$ e $\sigma_t$ pode ser particionado em $q+1$ segmentos, onde $q$ é o número de pontos de mudança desconhecidos e está entre o início e fim do vetor de observação da série temporal ($0 = \tau_0 < \tau_1 < \dots < \tau_q < \tau_{q+1} = T$). 

- Para cada intervalo $j = 1, …, q+1$ gerado pelos segmentos, modela-se parametricamente $\boldsymbol{\Theta_j}$ a subamostra por um vetor de parâmetros *d*-dimensional com valores pertencentes aos reais, onde d é conhecido e geralmente pequeno.

- Requer-se que a distância mínima entre dois pontos de mudança seja $\geqslant d$ por questões de identificação (identifiability). Ex: ao considerar uma função com mais de 3 parâmetros, não será possível estimá-la com um ou dois pontos.

</font>
<!-- ![Resultado Algoritmo WBS.](imagens/imagem_001.JPG) -->
<!-- ```{r, echo=FALSE} -->
<!-- plot(cars) -->
<!-- ``` -->


Algoritmo - NOT - Framework
========================================================
<font size="4">
Ou seja, o par $f_t$ e $\sigma_t$ pode ser dividido em $q+1$ segmentos  cada um deles da mesma família paramétrica de estrutura muito mais simples. Exemplos de estruturas (Cenários):

- **(S1) Variância constante, média constante por partes**:
    $$\sigma_t = \sigma_0 \qquad e \qquad f_t = \theta_j \qquad para \qquad t = \tau_{j-1} +1, \dots, \tau_j$$

-  **(S2) Variância constante, média contínua e linear por partes**:
    $$ \sigma_t = \sigma_0 \qquad e \qquad f_t = \theta_{j,1} +\theta_{j,2} \qquad  para \qquad t = \tau_{j-1} +1, \dots \tau_j$$
    com restrições de
    $$\theta_{j,1}+\theta_{j,2}\tau_j = \theta_{j+1,1}+\theta_{j+1,2} \tau_j \qquad para \qquad j =1, \dots, q$$
    
- **(S3) Variância constante, média linear por partes (mas não necessariamente contínua)**:
$$ \sigma_t = \sigma_0 \qquad e \qquad f_t = \theta_{j,1} +\theta_{j,2} \qquad  para \qquad t = \tau_{j-1} +1, \dots \tau_j$$ e
$$f_{tau_j} + \theta_{j,2} \neq f_{tau_j+1}$ para $j = 1, \dots, q$$

    
- **(S4) Variância constante por partes, média constante por partes**:
$$f_t = \theta_{j,1} \qquad e \qquad \sigma_t = \theta_{j,2} >0 \qquad  para \qquad t = \tau_{j-1} +1, \dots \tau_j$$ 

Obs. Considera-se $\sigma$ conhecido, contudo, caso seja desconhecido, é possível estimá-lo a partir do método MAD( *median absolute deviation*): robusto a pontos de mudanças.


</font>


Algoritmo - NOT - Main Idea
========================================================
<font size="4.5">

 - Formalmente, primeiro, extrai-se aleatoriamente subamostras com índices [s,e] tal que $0 \leqslant s < e \leqslant T$, ou seja, [s,e] não podem ter valores fora dos índices existentes, o início tem que ser inferior ao fim, e, pela condição de identificação, $e - s \geqslant 2d$.
 
- Dada a verossimilhança $\ell(Y_{s+1}, …, Y_e ; \Theta)$, calcula-se a estatística da razão generalizada da log-verossimilhança para todos pontos únicos candidatos dentro do intervalo (s,e) e utiliza o máximo.

- São gerados vários candidatos de "b", que segmentam a subamostra em duas, com a condição de que os pares [s,b] e [e,b] sejam maiores que *d* para poder estimar a verossimilhança.

$$
R^{b}_{(s,e)}(\boldsymbol{Y}) = 2 log \biggl[ \frac{sup_{\Theta^1, \Theta^2} \{ \ell( Y_{s+1}, \dots ,Y_b; \Theta^1) \ell( Y_{b+1}, \dots ,Y_e; \Theta^2) \}}{sup_{\Theta} \ell( Y_{s+1}, \dots ,Y_b; \Theta)} \biggr]
$$


$$
R_{(s,e)}(\boldsymbol{Y}) = max_{b \in \{ s+d,\dots,e-d\} } R^{b}_{(s,e)}(\boldsymbol{Y})
$$

Esse procedimento acima é repetido para todos $M$ pares retirados aleatoriamente $(s_1, e_1), ... , (s_M, e_M)$. 
Num segundo momento, testa-se todos $R_{ (s_m, e_m] }(\boldsymbol{Y})$ para $m = 1,\dots, M$ contra um determinado *threshold*  $\zeta_{T}$. Dos intervalos significativos, seleciona-se o que possui menor comprimento. 


</font>


Algoritmo - NOT - Log-likelihood ratios and contrast functions
========================================================

<font size="5">
Em muitas aplicações, o GLR (apresentado anteriormente) pode ser simplificado usando funções contraste sob a hipótese de ruído gaussiano.

- Essas funções contraste envolvem principalmente o produto interno entre os dados e outros vetores determinísticos. Elas facilitam tanto a parte teórica quanto computacional, especialmente se os vetores determinísticos são mutuamente ortonormais. 

Mais precisamente, para cada $(s, e, b)$ com $0 \leq s < e \leq T$, o objetivo é achar $C^{b}_{(s,e]} (\boldsymbol{Y})$ tal que:
 - $argmax_b C^{b}_{(s,e]} (\boldsymbol{Y}) =  argmax_b R^{b}_{(s,e)}(\boldsymbol{Y})$
 - O valor assumido pela função contraste $C^{b}_{(s,e]} (\boldsymbol{Y})$ precisa ser pequeno se não houver ponto de mudança no intervalo 
 - A função contraste $C^{b}_{(s,e]} (\boldsymbol{Y})$ consiste principalmente no produto interno entre os dados e os vetores de contraste 

</font>


Algoritmo - NOT - Pseudocódigo
========================================================
<center>
![Pseudocódigo NOT.](imagens/pseudcodigo.JPG) 
</center>



Algoritmo - NOT - Solution Path Algorithm
========================================================

<font size="5">
Denote-se $\tau(\zeta_T) = \hat{\tau_1} (\zeta_T), \dots, \hat{\tau_1}_{\hat{q} \zeta_T} (\zeta_T)$ os pontos de mudança estimados no algoritmo anterior, com *treshold* $\zeta_T$, e define-se ${\tau(\zeta_T)}_{\zeta_T \leq 0}$ como a  família dos *tresholds* indexiados das soluções. 

Dado que os *tresholds* $\zeta_{T}^{(i)}$ são desconhecidos e variam de acordo com os dados utilizadaos, apenas usando o algoritmo anterior num intervalo pré-determinado não recupera, tipicamente, todo caminho de solução. Ademais, do ponto de vista computacional é ineficiente, visto que as soluções para $\zeta^{(i)}_T$ e $\zeta^{(i + 1)}_T$ são muito parecidas para muitos *i's*.

Dessa forma, os autores, num artigo suplementar, desenvolvem um Algortimo 2, para  usar a informação de $\tau(\zeta^{(i)}_T)$ para calcular tanto $\zeta^{i+1}_T$ e $\tau(\zeta^{i+1}_T)$ iterativamente para todo $i =, \dots, N-1$.

</font>



Algoritmo - NOT - Choice via sSIC
========================================================

Uma vez construída a coleção de candidatos de modelos a ser usados, os autores propõem a minimização da função Schwarz Information Criterion(sSIC) como critério de escolha do modelo utilizado. Defindo por:

$$
sSIC(k) = - 2 \sum^{\hat{q}_k+1}_{j=1} log \ell (Y_{\hat{\tau}_{j-1}+1},\dots,Y_{\hat{\tau}_j}; \hat{\Theta}_j)+n_klog^{\alpha}(T)
$$


onde $k = 1,\dots, N$, $\hat{q}_k= |\tau(\zeta^k_T)|$;
$\hat{\Theta}_1, \dots, \hat{\Theta}_{\hat{q}_k+1}$ os estimadores de máxima verossimilhança; $n_k$ o número total de parâmetros estimados; $\alpha \leq 1$; e $\hat{\tau}_{\hat{q}_k+1}=T$.


Exemplo -  NOT - Cenário 1
========================================================


<font size="5">


```r
library(not)
pcws.const.sig <- c(rep(0, 100), rep(1,100))
# Cenario 1 . Variancia constante e Media constante nas partes
set.seed(123456)
x <- pcws.const.sig + rnorm(100)
w <- not(x, contrast = "pcwsConstMean") 
# onde foi detectado o pulo
features(w)$cpt
```

```
[1] 91
```

```r
plot(w)
```

<img src="Series Temporais-figure/unnamed-chunk-1-1.png" title="plot of chunk unnamed-chunk-1" alt="plot of chunk unnamed-chunk-1" style="display: block; margin: auto;" />


</font>


Exemplo -  NOT - Aplicação
========================================================


<font size="5">

Wrangling



```r
library(covid19br)
library(tidyverse)

db_rs_covid <- downloadCovid19(level = "states") %>% 
  filter(state =='RS')


db_lag<-
  db_rs_covid %>% 
  dplyr::select(
  date,
  newDeaths) %>% 
  # media movel para tirar a sazonalidade dos registros
  dplyr::mutate( 
    death_14da = zoo::rollmean(newDeaths, k = 14, fill =NA, align = "right")
    ) %>% 
  dplyr::mutate(death_lag_14 = lag(death_14da, 14),
                date_lag_14  = lag(death_14da, 14)
                ) %>% 
  # calculo da proporcao referente a  media de 14 dias antes
  dplyr::mutate(
    death_prop = (death_14da/death_lag_14)-1
  ) %>% 
  filter(!is.infinite(death_prop)) %>% 
  filter(!is.nan(death_prop)) %>% 
  filter(!is.na(death_prop)) %>% 
  # soh no ultimo ano
  filter(date >= '2020-04-07')
```

</font>


Exemplo -  NOT - Aplicado
========================================================
  
  
<font size="3">
  

```r
set.seed(123456)
w<-not(db_lag$death_prop,
       method = 'not',
       contrast = "pcwsConstMeanVar",
       M= 1000)

db_lag[features(w)$cpt,]
```

```
# A tibble: 13 x 6
   date       newDeaths death_14da death_lag_14 date_lag_14 death_prop
   <date>         <int>      <dbl>        <dbl>       <dbl>      <dbl>
 1 2020-04-13         0      0.929        0.214       0.214     3.33  
 2 2020-05-17        10      5.5          2.93        2.93      0.878 
 3 2020-06-21         4     10.6          7.57        7.57      0.396 
 4 2020-07-22        48     40.9         23.2        23.2       0.760 
 5 2020-08-14        47     53.9         50.7        50.7       0.0634
 6 2020-09-24        29     43.9         46.7        46.7      -0.0596
 7 2020-10-25         0     31.4         36.7        36.7      -0.144 
 8 2020-11-16        24     31.3         29.8        29.8       0.0504
 9 2020-12-25        31     65.8         56.3        56.3       0.169 
10 2021-01-22        45     63           66.1        66.1      -0.0465
11 2021-02-17        72     46.6         50.2        50.2      -0.0711
12 2021-03-01        78     77.4         48          48         0.612 
13 2021-03-23       342    262.         136.        136.        0.922 
```

```r
plot(w)
```

<img src="Series Temporais-figure/unnamed-chunk-3-1.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" style="display: block; margin: auto;" />

```
Press [enter] to continue
```

<img src="Series Temporais-figure/unnamed-chunk-3-2.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" style="display: block; margin: auto;" />

<!-- ```{r, echo=FALSE, fig.align='center',fig.height = 3, fig.width = 3} -->
<!-- include_graphics("imagens/plot_gerado.JPG") -->
<!-- ``` -->


</font>
