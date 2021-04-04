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

Algoritmo - NOT
========================================================

- Parte de um modelo simples para detectar “pontos de mudança” que ocorrem em locais desconhecidos

$$ Y_t = f_t + \epsilon_t,  $$
$$t = 1, \dots, T$$

- A abordagem proposta também permite detectar "features" a partir de aspectos distributivos de $\epsilon_t$  (ex: variância do erro)



Slide With Plot
========================================================

![plot of chunk unnamed-chunk-1](Series Temporais-figure/unnamed-chunk-1-1.png)