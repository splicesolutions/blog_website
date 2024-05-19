---
title: "Shadowing de variavél não intencional"
meta_title: ""
description: "this is meta description"
date: 2022-04-04T05:00:00Z
image: "/images/gallery/07.png"
categories: ["Golang", "Code"]
author: "Felipe Cooper"
tags: ["golang"]
draft: false
---


Mesmo que o termo 'shadowing' possa parecer um tanto peculiar à primeira vista, é algo que talvez você ja pratique em seu codigo algumas vezes sem saber. Bom, vamos aprender um pouco sobre o termo para evitarmos no futuro.

O 'shadowing' ocorre quando uma variável em um escopo mais interno tem o mesmo nome que uma variável em um escopo mais externo. Um exemplo clássico disso é quando usamos uma variável com o nome err para verificar erros em funções. Muitas vezes, realizamos 'shadowing' nela, sobrescrevendo seu valor e, em seguida, validando o retorno do erro. Como neste exemplo:

<script src="https://gist.github.com/FelipeCooper/624351b41e76a5ac117b17b04186ca5e.js?file=shadowing-variable-example.go"></script>

Neste exemplo simples, podemos observar que criamos novamente a variável err ao usar a função setCookies, mesmo que ela tenha sido declarada ao chamar a função getFirstName. Isso é o que chamamos de 'shadowing' - ambas as variáveis têm o mesmo nome, mas estão em escopos diferentes.

Beleza, agora que entendemos o que é `shadowing` vamos ao assunto principal, o problema do `shadowing` não intencional, veja atentamente esse código:

<script src="https://gist.github.com/FelipeCooper/624351b41e76a5ac117b17b04186ca5e.js?file=shadowing-variable-problem.go"></script>

Ao executar o `go run main.go` nesse código, vamos ter o seguinte retorno:<br>
<img src="/images/gallery/shadowing-variable-01.png" width="500"/>

Estranho não é mesmo? o Final name está vazio, apenas o `log.Print(name)` de dentro do IF funcionou, bom esse problema está acontecendo porque estamos fazendo um `shadowing` não intencional nessa variavel `name` ao tentar criar a variavél `err` para validar o error, e como podemos solucionar esse problema?

## Solução 1

Uma maneira bem comum de resolver isso, é dando outro nome a variavél e depois atribuindo o valor para a variavel que desejamos, desse jeito:

<script src="https://gist.github.com/FelipeCooper/624351b41e76a5ac117b17b04186ca5e.js?file=shadowing-variable-first-solution.go"></script>

executando novamente o comando `go run main.go` o resultado já parece mais promissor:<br>
<img src="/images/gallery/shadowing-variable-02.png" width="500"/>

Perfeito, agora nosso `	fmt.Println("Final name: ", name)` está logando corretamente nossa variable name, em teoria está resolvido, porém vamos para uma segunda solução que é mais elegante,

## Solução 2

Uma segunda solução mais elegante é a de tirarmos o operador `:=` substituindo por um `=`, para fazer isso primeiro temos que tirar a variable `err` de dentro daquele escopo, com os ajustes necessarios o codigo ficaria assim:
<script src="https://gist.github.com/FelipeCooper/624351b41e76a5ac117b17b04186ca5e.js?file=shadowing-variable-second-solution.go"></script>

executando novamente o comando `go run main.go` o resultado continua correto: <br>
<img src="/images/gallery/shadowing-variable-02.png" width="500"/>

E bom, se reparmos bem, agora nossa variavel `err` está sendo testada no `if` e `else` entao nos conseguimos puxar ela pra fora do escopo també, simplificando ainda mais o código, agora que chegamos a um codigo mais funcional e mais elegante que a primeira solução e entendemos o conceito e como evitar o problema vamos olhar use cases reais.

## Use Case

<script src="https://gist.github.com/FelipeCooper/624351b41e76a5ac117b17b04186ca5e.js?file=shadowing-variable-use-case.go"></script>

Esse é um exemplo mais perto da realidade e que pode acontecer, também trago aqui um [PULL REQUEST](https://github.com/moby/moby/pull/3482) que encontrei no project da Mobi ou esse outro [PULL REQUEST](https://github.com/cli/cli/pull/1367/files) que encontrei no projeto do GitHub CLI.


Bom espero que esse conteudo tenha ajudado você a entender um pouco melhor sobre shadowing e como evitar, se foi util para você compartilhe com seus colegas :)