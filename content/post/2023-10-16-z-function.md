---
title: Z Function
date: '2023-10-16'
categories:
  - CP
tags:
  - Competitive Programming
  - String
  - Portuguese
---

## Definição
Dada ums string $s$ de tamanho $n$. A função Z para essa string é um vetor de tamanho $n$, no qual o índice $i$ é igual ao maior número de caracteres que coincidentes começando pela posição $i$ com o primeiro caractere de $s$. O algoritmo para calcular a função Z eficientemente é $\mathcal{O}(n)$.

Invariante: $z[i]$ é o tamanho da maior string que é, ao mesmo tempo, um prefixo de $s$ e o prefixo do sufixo de $s$ começando pela posição $i$.

### Exemplo 1:

- String: 'aaaaa'
- Vetor Resultante: [0, 4, 3, 2, 1]

|    | a | a | a | a | a |
|----|---|---|---|---|---|
|$i$ | 0 | 1 | 2 | 3 | 4 | 
|$Z$ | 0 | 4 | 3 | 2 | 1 |

##### 1. O primeiro elemento, geralmente, não é bem definido, então é sempre 0.

##### 2. Calculando o segundo elemento do vetor resultante $Z$    

Depois deve-se analisar o elemento da posição $1$ com o começo, ou seja, com a posição $0$, sempre seguindo a invariante da função $Z$. Caso seja elas sejam iguais, avança-se uma posição nos dois caracteres que estão sendo comparados e contabiliza 1 no vetor resultante na posição $1$. 

Na segunda iteração, iremos comparar o elemento da posição $2$ com o elemento da posição $1$. Eles são iguais, então adicionamos 1 na posição 2 do vetor resultante. 

Faça isso, até que a posição do $elem_{i+j}$ que está sendo comparado seja menor que o tamanho da string $s$, ou até que o segundo elemento $elem_{i+j}$ não seja igual ao primeiro elemento $elem_{i}$


#### 3. Calculando o terceiro elemento do vetor resultante $Z$

### Exemplo 2

- String: taktaktak
- Vetor Resultante: [0, 0, 0, 6, 0, 0, 3, 0, 0]

|    | t | a | k | t | a | k | t | a | k |
|----|---|---|---|---|---|---|---|---|---|
|$i$ | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 
|$Z$ | 0 | 0 | 0 | 6 | 0 | 0 | 3 | 0 | 0 |


### Algoritmo ineficiente

```cpp
vector<int> z_function_trivial(string s) {
    int n = s.size();
    vector<int> z(n);
    for (int i = 1; i < n; i++) {
        while (i + z[i] < n && s[z[i]] == s[i + z[i]]) {
            z[i]++;
        }
    }
    return z;
}
```

Note que esse algoritmo segue o que definimos no passo 2. do exemplo 1. Porém a gente vai precisar fazer no máximo, em um pior caso, $\mathcal{O}(n^2)$ operações, o que não é o ideal. Conseguimos melhorar esse algoritmo.

### Computando Função Z eficientemente
Devemos considerar o segmentos que fazem matching, nomeados também como substrings de $s$ que que são prefixos de $s$. Por exemplo, o valor da função Z $z[i]$ é o tamanho do segmento que faz match com o prefixo de $s$, começando na posição $i$ e terminando em $i+z[i]-1$.

Para isso, vamos guardar o segmento que termina mais a direita, $[l, r)$, sendo $l$ o índice do início do segmento e $r$ o índice que termina o segmento. Tudo que está à direita ainda não conhecemos.

Dado que estamos na posição $i$, temos duas opções:

- $i \geq r$ - a posição atual não está contido no segmento.

    Assim, devemos computar $z[i]$ utilizando o algoritmo ineficiente. Se $z[i]>0$, devemos atualizar o índice do segmento mais à direita, porque é garantido que $r=i+z[i]$ é com certeza melhor que o antigo $r$.

- $i<r$ - a posição atual está contida no segmento.

    Para isso podemos utilizar os valores que já calculamos anteriormente para calcular essa condição.

    Note que a substring $[l, \ldots, r)$ e a substring $[0, \ldots, r-l)$ dão match, seguindo a invariante da função Z. Isso possibilita o reuso do já calculamos para a substring $[0, \ldots, r-l)$, e isso é igual a $z[i-l]$.

    No entanto, $z[i-l]$ pode ser muito grande, ou seja, pode ultrapassar $r$. Isso não é permitido porque não sabemos nada além de $r$.

    Imagine o primeiro exemplo: 'aaaaaa'

    Temos que o segmento é igual a $[1, 5)$, a posição que queremos calcular é igual a $i=4$, então temos que a posição de match é igual a $4-1=3$. $z[3] = 2 $, o que está incorreto. Assim podemos considerar: $z_0[i] = min(r-i, z[i-l])$.

    Depois de inicializar $z[i]$ com o $z_0[i]$ devemos calcular o resto com o algoritmo trivial porque não sabemos nada além da borda $r$.

O algoritmo é simples, mas é tricky, porque devemos entender a propriedade dos segmentos que dão match.

```cpp
vector<int> z_function(string s) {
    int n = s.size();
    vector<int> z(n);
    int l = 0, r = 0;
    for(int i = 1; i < n; i++) {
        if(i < r) {
            z[i] = min(r - i, z[i - l]);
        }
        while(i + z[i] < n && s[z[i]] == s[i + z[i]]) {
            z[i]++;
        }
        if(i + z[i] > r) {
            l = i;
            r = i + z[i];
        }
    }
    return z;
}
```

### Aplicações

#### 1. Pattern Matching

Seja $t$ um texto e $p$ a palavra que você está procurando no texto $t$. O problema é encontrar todos os padrões $p$ dentro do texto $t$.

Para resolver criamos uma nova string $s=p+\#+t$, ou seja, concatenamos $p$ com $t$, adicionando qualquer caractere especial entre eles.

Calcula a função Z para a string $s$. Assim, para qualquer $i$ dentro do intervalo de $[0, length(t)-1]$, consideraremos o valor $k=z[i+length(p)+1]$. Se $k$ é igual ao tamanho de $p$, então sabemos que existe uma ocorrência de $p$ na iésima posição de $t$.

O tempo de execução é $\mathcal{O}(length(p)+length(t))$




