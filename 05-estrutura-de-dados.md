# Estrutura de dados

No capítulo passado criamos o jogo da adivinhação, agora vamos criar o jogo da Forca. Vamos começar com os conhecimentos que temos até aqui para estruturarmos o nosso jogo. Vamos criar um arquivo chamado _forca.py_ na pasta _jogos_:

```
    |_ home
    |_ jogos
        |_ adivinhacao.py
        |_ forca.py
```

Como no jogo da adivinhação, devemos adivinhar uma palavra secreta, então nada mais justo que defini-la em uma variável. Por enquanto, a palavra será fixa, com o valor **banana**:

```python
    print('*********************************')
    print('***Bem vindo ao jogo da Forca!***')
    print('*********************************')

    palavra_secreta = 'banana'

    print('Fim do jogo')
```

Mais à frente deixaremos essa palavra secreta mais dinâmica.

Como estamos tratando de um jogo da forca, o usuário deve acertar uma palavra e chutar letras. Além disso, precisa saber se o usuário acertou ou errou e saber se foi enforcado ou não. Então precisaremos de 2 variáveis booleanas `enforcou` e `acertou` para guardar esta informação e o jogo continuará até uma delas for `True`; para tal acrescentamos um laço `while`:

```python
    acertou = False
    enforcou= False

    while(not acertou and not enforcou):
        print('Jogando...')
```

O usuário do jogo também vai chutar letras. Além disso, se o chute for igual a uma letra contida na `palavra_secreta`, quer dizer que o usuário encontrou uma letra. Podemos utilizar um laço `for` para tratar isso, assim como fizemos no jogo da adivinhação para apresentar as tentativas e rodadas:

```python
    while(not acertou and not errou):
        chute = input('Qual letra?')

        posicao = 0
        for letra in palavra_secreta:
            if (chute == letra):
                print('Encontrei a letra {} na posição {}'.format(letra, posicao))
            posicao = posicao + 1  

            print('Jogando...')
```

Como uma _string_ é uma sequência de letras, o _loop_ `for` vai iterar por cada letra. 

Nossa palavra_secreta é 'banana'. E se o usuário chutar a letra 'A' ao invés de 'a'? Vamos testar:

```bash
    >>> $python3 jogos/forca.py
    *********************************
    ***Bem vindo ao jogo da Forca!***
    *********************************
    Qual letra? A
    Jogando...
    Qual letra? 
```

O Python é _case-sensitive_, ou seja 'a' e 'A' são distintos para o interpretador. Será preciso acrescentar este tratamento e fazer o jogo aceitar como acerto tanto 'a' como 'A'. _String_ (`str`) é um tipo embutido no Python que possui algumas funções prontas e uma delas vai nos ajudar neste problema. 

Existe uma função chamada `upper()` que devolve uma _string_ com todas as letras maiúsculas. Também possui a `lower()` que devolve uma _string_ com todas as letras minúsculas:

```python
    >>> texto = 'python'
    >>> texto.upper()
    'PYTHON'
    >>> 
    >>> texto = 'PYTHON'
    >>> texto.lower()
    'python' 
```

Agora precisamos modificar a condição `chute == letra` do `if` para `chute.upper() == letra.upper()`:

```python
    if (chute.upper() == letra.upper()):
        print('Encontrei a letra {} na posição {}'.format(letra, posicao))
```

Dessa maneira, o jogador pode chutar tanto 'a' como 'A' que o tratamento do `if` vai considerar todas como maiúsculas.

Agora podemos testar novamente com chute igual a 'A' (ou 'a') e vemos o programa funcionar como o esperado:

```bash
    *********************************
    ***Bem vindo ao jogo da Forca!***
    *********************************
    Qual letra? A
    Encontrei a letra a na posição 1
    Encontrei a letra a na posição 3
    Encontrei a letra a na posição 5
    Jogando...
    Qual letra? 
```

Atualmente, já dizemos ao jogador em que posição a letra que ele chutou está na palavra secreta, caso a letra exista na palavra. Mas em um jogo real de forca, o jogador vê quantas letras há na palavra secreta. Algo como:

```
    Qual letra? _ _ _ _ _ _
```

E se ele encontrar alguma letra, a mesma tem a sua lacuna preenchida. Ao digitar a letra "a", ficaria:

```
    _ a _ a _ a
```

Muito mais intuitivo, não? Vamos implementar essa funcionalidade. Para exibir as letras dessa forma, precisamos guardar os chutes certos do usuário, mas como fazer isso?

Para tal, o Python nos oferece um tipo de estrutura de dados que nos permite guardar mais de um valor. Essa estrutura é a `list` (lista). Para criar uma lista, utilizamos colchetes ([]):

```python
    >>> valores = []
    >>> type(valores)
    <class 'list'>
```

  

Assim como a _string_, `list` também é uma sequência de dados. Podemos ver sua documentação através da função `help()`:

```python
>>> help(list)
    Help on class list in module builtins:
    class list(object)
     |  list() -> new empty list
     |  list(iterable) -> new list initialized from iterable's items
     |  
     |  Methods defined here:
     |  
     |  __add__(self, value, /)
     |      Return self+value.
     |  
     |  __contains__(self, key, /)
     |      Return key in self.
     |  
     |  __delitem__(self, key, /)
     |      Delete self[key].
     |  
     |  __eq__(self, value, /)
     |      Return self==value.
     |  
     |  __ge__(self, value, /)
     |      Return self>=value.

     #código omitido
```

Vemos o que podemos fazer com uma lista. Podemos, por exemplo, verificar o seu valor mínimo com `min` e o seu máximo com `max`. Nossa lista ainda está vazia, mas já podemos iniciá-la com alguns valores e utilizarmos essas funções para verificarmos seus valores máximo e mínimo:

```python
    >>> valores = [0, 1, 2, 3]
    >>> min(valores)
    0
    >>> max(valores)
    3
```

  

Para acessar um valor específico, podemos acessá-lo através do seu índice (posição). O primeiro elemento da lista possui índice 0, o segundo possui índice 1 e assim por diante:

```python
    >>> valores = [0, 1, 2, 3]
    >>> valores[2]
    2
    >>> valores[0]
    0
```

  

Para modificar um valor, basta usar o operador de atribuição em uma determinada posição:

```python
    >>> valores[0] = 4
    >>> valores
    [4, 1, 2, 3]
```

  

É possível saber o tamanho da lista com a função `len` e verificar se determinado valor está guardado nela com o comando `in`:

```python
    >>> valores = [0, 1, 2, 3]
    >>> len(valores)
    4
    >>> 0 in valores
    True
    >>> 6 in valores
    False
```

  

Além disso, existem funções específicas da lista, que podem ser acessadas na documentação: https://docs.python.org/3.6/library/stdtypes.html#mutable-sequence-types. 

Podemos adicionar elementos ao final da lista com a função `append()`, exibir e remover um elemento de determinada posição com a função `pop()`, entre diversas outras funcionalidades.

Agora que sabemos como guardar valores em uma lista, podemos voltar ao nosso jogo e guardar os acertos do usuário. Como queremos exibir os espaços vazios primeiro, criaremos uma lista com eles, na mesma quantidade de letras da palavra secreta:

```python
    palavra_secreta = 'banana'
    letras_acertadas = ['_', '_', '_', '_', '_', '_']
```

Já temos a posição da letra (também chamado de índice). Logo, caso o chute seja correto, basta guardar a letra dentro da lista, na sua posição correta e imprimir a lista após o laço `for`:

```python
    posicao = 0

    for letra in palavra_secreta:
        if (chute.upper() == letra.upper()):
            letras_acertadas[posicao] = letra
        posicao = posicao + 1

    print(letras_acertadas) 
```

Ou seja, para cada letra na palavra secreta, o programa vai verificar se `chute` é igual a `letra`. Em caso afirmativo, adiciona a letra na posição correta e incrementa a `posicao` após o bloco do `if`.

Ao executar o jogo e chutar algumas letras, temos:

```bash
    $ python3 jogos/forca.py
    *********************************
    ***Bem vindo ao jogo da Forca!***
    *********************************
    Qual letra? b
    ['b', '_', '_', '_', '_', '_']
    Jogando...
    Qual letra? a
    ['b', 'a', '_', 'a', '_', 'a']
    Jogando...
    Qual letra? 
```

A saída ainda não está visualmente agradável. Para ficar ainda melhor, vamos exibir a lista no início do jogo também e excluir o print('Jogando...') para o código ficar mais limpo.

```python
    print(letras_acertadas)

    while(not acertou and not errou):       
        chute = input('Qual letra?')

        #código omitido
```

## Exercícios: Jogo da Forca

Neste exercício, vamos aproveitar nosso novo conhecimento em listas para fazer com que o jogo da forca se lembre das letras acertadas pelo jogador.

1. Crie um arquivo chamado _forca.py_ na pasta _jogos_:
    ```
        |_ home
        |_ jogos
            |_ adivinhacao.py
            |_ forca.py
    ```

1. Primeiro, precisamos mostrar para o jogador a mensagem de abertura, similar ao que foi feito no jogo da adivinhação:
    ```python
        print('*********************************')
        print('***Bem vindo ao jogo da Forca!***')
        print('*********************************') 
    ```

1. Crie a variável _palavra_secreta_, que será iniciada com o valor 'banana', e uma lista para representar as letras acertadas. 
    ```python
        palavra_secreta = 'banana'
        letras_acertadas = ['_', '_', '_', '_', '_', '_']
    ```

1. Crie as variáveis booleanas `acertou` e `errou`, que vamos utilizar no laço `while`. Além disso, crie a variável `erros`, que armazenará o numero de erros do usuário:
    ```python
        acertou = False
        enforcou = False
        erros = 0

        while(not acertou and not enforcou):
            # código
    ```

1. Crie a variável `chute`, que vai guardar entrada do usuário.
    ```python
        while(not acertou and not enforcou):
            chute = input('Qual letra? ')
    ```

1. Dentro do `while`, crie um _loop_ `for` para checar se a letra existe na palavra secreta. Se o chute for igual a letra digitada pelo jogador, adicione a letra na lista `letras_acertadas` na posição correta
    ```python
        while(not acertou and not enforcou):
            chute = input('Qual letra? ')

            posicao = 0
            for letra in palavra_secreta:
                if (chute == letra):
                    letras_acertadas[posicao] = letra
                posicao += 1 
    ```

1. Modifique a condição do `if` para considerarmos apenas letras maiúsculas utilizando a função `upper` de _strings_. Atualize também a variável `chute`.
    ```python
        if (chute.upper() == letra.upper()):     
    ```

1. Agora precisamos incrementar a variável _erros_ caso o jogador não acerte a letra. Se o chute é uma letra dentro de _palavra_secreta_, quer dizer que o jogador acertou, e caso contrário, incrementamos a variável _erros_. Para isso, teremos mais um bloco `if/else`:
    ```python
        if(chute in palavra_secreta):
            posicao = 0
            for letra in palavra_secreta:
                if(chute.upper() == letra.upper()):
                    letras_acertadas[posicao] = letra
                posicao += 1
        else:
            erros += 1

    ```

1. Atualize a variável `acertou`. Se todas as letras ainda não foram acertadas, quer dizer que o usuário ainda não finalizou o jogo, ou seja, letras_acertas ainda contém espaços vazios ('_'). Dentro do _loop_ `while`, após o comando `for`, vamos atualizar a variável _acertou_ e utilizar o operador `not in`: 
    ```python
        acertou = '_' not in letras_acertadas.
    ```
    Podemos ler o código acima como "'_' não está contido em letras_acertadas".

1. Vamos também atualizar a variável `enforcou`. Vamos considerar que se o jogador errar 7 vezes ele perde o jogo. Como a variável inicia com 0 (zero), quer dizer que quando ela for igual a 6, ele se enforca. Atualize a variável dentro do _loop_ `while` após a variável `acertou`:
    ```python
        acertou = "_" not in letras_acertadas
        enforcou = erros == 6
    ```

1. Para que o jogador acompanhe o resultado a cada chute que ele der, ao final do laço `while`, imprima também a lista `letras_acertadas` para que ele veja como ele está indo no jogo:
    ```python
        while(not enforcou and not acertou):
            
            #código omitido
            
            print(letras_acertadas)
    ```

1. E claro, para dar uma dica ao nosso jogador de quantas letras a palavra tem, vamos colocar acima do while um print inicial para que ele veja de início qual o tamanho da palavra:
    ```python
        print(letras_acertadas)

        while (not acertou and not enforcou):
            ...
    ```

1. Por fim, vamos imprimir uma mensagem de "Você ganhou!" se o usuário adivinhar a letra e "Você perdeu" caso tenha cometido 6 erros. Após o laço `while`, fora dele, acrescente:
    ```python
        if(acertou):
            print('Você ganhou!!')
        else:
            print('Você perdeu!!')

        print('Fim do jogo')

    ```

1. Faça o teste e veja na resposta se seu código funcionando. 
    ```python
    $ python3 jogos/forca.py
    ```

    No final, seu código deve estar parecido com este:
    ```python
        def jogar():
            print('*********************************')
            print('***Bem vindo ao jogo da Forca!***')
            print('*********************************') 

            palavra_secreta = 'banana'
            letras_acertadas = ['_', '_', '_', '_', '_', '_']

            enforcou = False
            acertou = False
            erros = 0

            print(letras_acertadas)

            while(not enforcou and not acertou):

                chute = input("Qual letra? ")

                if(chute in palavra_secreta):
                    posicao = 0
                    for letra in palavra_secreta:
                        if(chute.upper() == letra.upper()):
                            letras_acertadas[posicao] = letra
                        posicao = posicao + 1
                else:
                    erros += 1

                enforcou = erros == 6
                acertou = '_' not in letras_acertadas            
                print(letras_acertadas)

            if(acertou):
                print('Você ganhou!!')
            else:
                print('Você perdeu!!')
            print('Fim do jogo')    
    ```

## Sequências

Desenvolvemos um novo jogo e conhecemos uma nova estrutura, as listas. Uma lista é denominada por uma sequência de valores, e o Python possui outros tipos de dados que também são sequências. Neste momento, conheceremos um pouco de cada uma delas. 

Sequências são _containers_, um tipo de dado que contém outros dados. Existem três tipos básicos de sequência: `list` (lista) , `tuple` (tupla) e _`range`_ (objeto de intervalo). Outro tipo de sequência famoso que já vimos são as _strings_ que são sequências de texto.

Sequências podem ser mutáveis ou imutáveis. Sequências imutáveis não podem ter seus valores modificados. Tuplas, _strings_ e _ranges_ são sequências imutáveis, enquanto listas são sequências mutáveis.

As operações na tabela a seguir são suportadas pela maioria dos tipos de sequência, mutáveis ​​e imutáveis. Na tabela abaixo, s e t são sequências do mesmo tipo, n , i , j e k são inteiros e x é um objeto arbitrário que atende a qualquer tipo e restrições de valor impostas por s .

| Operação       | Resultado                                     |
|----------------|-----------------------------------------------|
| x in s         | True se um item de s é igual a *x*            |
| x not in s     | False se um item de s é igual a *x*           |
| s + t          | Concatenação de s e s                         |
| s * n ou n * s | Equivalente a adicionar s a si mesmo n vezes  |
| s[i]           | Elemento na posição i de s                    |
| s[i:j]         | Fatia s de i para j                           |
| s[i:j:k]       | Fatia s de i para j com o passo k             |
| len(s)         | Comprimento de s                              |
| min(s)         | Menor item de s                               |
| max(s)         | Maior item de s                               |
| s.count(x)     | Número total de ocorrências de x em s         |


* **Listas**

Uma lista é uma sequência de valores onde cada valor é identificado por um índice iniciado por 0. São similares a _strings_ (coleção de caracteres) exceto pelo fato de que os elementos de uma lista podem ser de qualquer tipo. A sintaxe é simples, listas são delimitadas por colchetes e seus elementos separados por vírgula:

```python
    >>> lista1 = [1, 2, 3, 4]
    >>> lista1
    [1, 2, 3, 4]
    >>>
    >>>lista2 = ['python', 'java', 'c#']
    >>> lista2
    ['python', 'java', 'c#']
```

  

O primeiro exemplo é uma lista com 4 inteiros, o segundo é uma lista contendo três _strings_. Contudo, listas não precisam conter elementos de mesmo tipo. Podemos ter listas heterogêneas:

```python
    >>> lista = [1, 2, 'python', 3.5, 'java']
    >>> lista
    [1, 2, 'python', 3.5, 'java']
```

  

Nossa _lista_ possui elementos do tipo `int`, `float` e `str`. Se queremos selecionar um elemento específico, utilizamos o operador `[ ]` passando a posição:

```python
    >>> lista = [1, 2, 3, 4]
    >>> lista[0]
    1
    >>> lista[1]
    2
    >>> lista[2]
    3
    >>> lista[3]
    4
    >>> lista[4]
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    IndexError: list index out of range
```

  

Quando tentamos acessar a posição 4 fazendo `lista[4]`, aparece o erro `IndexError`, dizendo que a lista excedeu seu limite já que não há um quinto elemento (índice 4).

O Python permite passar valores negativos como índice que vai devolver o valor naquela posição de forma reversa:

```python
    >>> lista = [1, 2, 3, 4]
    >>> lista[-1]
    4
    >>> lista[-2]
    3
```

  

Também é possível usar a função `list()` para criar uma lista passando um tipo que pode ser iterável como uma _string_:

```python
    >>> lista = list('python')
    >>> lista
    ['p', 'y', 't', 'h', 'o', 'n']
```

  

Listas são muito úteis, por exemplo:

```python
    meses = ['Janeiro', 'Fevereiro', 'Março', 'Abril', 'Maio', 'Junho', 'Julho', 'Agosto', 'Setembro', 'Outubro', 'Novembro', 'Dezembro']    
    n = 1

    while(n < 4):
        mes = int(input("Escolha um mês (1-12): "))
        if 1 <= mes < 13:
            print('O mês é {}'.format(meses[mes-1]))
        n += 1  
```

E testamos:

```python
    >>> Escolha um mês (1-12): 4
    O mês é Abril
    >>> Escolha um mês (1-12): 11
    O mês é Novembro
    >>> Escolha um mês (1-12): 6
    O mês é Junho
```

Primeiro criamos um lista, relacionamos seus índices com os meses do ano, recuperamos seus valores através da entrada do usuário e imprimimos na tela o mês escolhido (mes[mes-1]) indexado a lista a partir do zero. Usamos um laço `while` que faz nosso programa entrar em _loop_ e ser rodado 3 vezes.

Além de acessar um valor específico utilizando o índice, podemos acessar múltiplos valores através do fatiamento. Também utilizamos colchetes para o fatiamento. Suponha que queremos acessar os dois primeiros elementos de uma lista:

```python
    >>> lista = [2, 3, 5, 7, 11]
    >>> lista[0:2]
    [2, 3]
```

  

Podemos ter o mesmo comportamento fazendo:

```python
    >>> lista[:2]
    [2, 3]
```

  

Se queremos todos os valores excluindo os dois primeiros, fazemos:

```python
    >>> lista[2:]
    [5, 7, 11]
```

  

Ou utilizamos índices negativos:

```python
    >>> lista[-3:]
    [5, 7, 11]
```

  

Também podemos fatiar uma lista de modo a pegar elementos em um intervalo específico:

```python
    >>> lista[2:4]
    [5, 7]
```

  

As listas também possuem funcionalidades prontas, e podemos manipulá-las através de funções embutidas. As listas podem ser utilizadas em conjunto com uma função chamada `append()`, que adiciona um dado na lista:

```python
    >>> lista = []
    >>> lista.append('zero')
    >>> lista.append('um')
    >>> lista
    ['zero', 'um']

```

  


A função `append()` só consegue inserir um elemento por vez. Se quisermos inserir mais elementos, podemos somar ou multiplicar listas, ou então utilizar a função `extend()`:

```python
    >>> lista = ['zero', 'um']
    >>> lista.extend(['dois', 'três',])
    >>> lista += ['quatro', 'cinco']
    >>> lista + ['seis']
    ['zero', 'um', 'dois', 'três', 'quatro', 'cinco', 'seis']
    >>> lista * 2
    ['zero', 'um', 'dois', 'três', 'quatro', 'cinco', 'zero', 'um', 'dois', 'três', 'quatro', 'cinco']
```

  

Isso é possível pois listas são sequências **mutáveis**, ou seja, conseguimos adicionar, remover e modificar seus elementos. Para imprimir o conteúdo de uma lista, podemos utilizar o comando `for`:

```python
    for valor in lista:
    ... print(valor)
    ...
    zero
    um
    dois
    três
    quatro
    cinco
```

  

* **Tuplas**

Uma tupla é uma lista **imutável**, ou seja, uma tupla é uma sequência que não pode ser alterada depois de criada. Uma tupla é definida de forma parecida com uma lista com a diferença do delimitador. Enquanto listas utilizam colchetes como delimitadores, as tuplas usam parênteses:

```python
    >>> dias = ('domingo', 'segunda', 'terça', 'quarta', 'quinta', 'sexta', 'sabado')
    >>> type(dias)
    <class 'tuple'>
```

  

Podemos omitir os parênteses e inserir os elementos separados por vírgula:

```python
    >>> dias = 'domingo', 'segunda', 'terça', 'quarta', 'quinta', 'sexta', 'sabado'
    >>> type(dias)
    >>> <class 'tuple'>
```

  

Note que, na verdade, é a vírgula que faz uma tupla, não os parênteses. Os parênteses são opcionais, exceto no caso da tupla vazia, ou quando são necessários para evitar ambiguidade sintática.

Assim como as listas, também podemos usar uma função para criar uma tupla passando um tipo que pode ser iterável como uma _string_ ou uma lista. Essa função é a `tuple()`:

```python
    >>> texto = 'python'
    >>> tuple(texto)
    ('p', 'y', 't', 'h', 'o', 'n')
    >>> lista = [1, 2, 3, 4]
    >>> tuple(lista)
    (1, 2, 3, 4)
```

  

As regras para os índices são as mesmas das listas, exceto para elementos também imutáveis. Como são imutáveis, uma vez criadas não podemos adicionar nem remover elementos de uma tupla. O método `append()` da lista não existe na tupla:

```python
    >>> dias.append('sabado2')
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    AttributeError: 'tuple' object has no attribute 'append'
    >>> 
    >>> dias[0]
    'domingo'
    >>>
    >>> dias[0] = 'dom'
      File "<stdin>", line 1, in <module>
    TypeError: 'tuple' object does not support item assignment

```

  

Não é possível atribuir valores aos itens individuais de uma tupla, no entanto, é possível criar tuplas que contenham objetos mutáveis, como listas.

```python
    >>> lista = [3, 4]
    >>> tupla = (1, 2, lista)
    >>> tupla
    (1, 2, [3, 4])
    >>> lista = [4, 4]
    >>> tupla
    (1, 2, [4, 4])
    >>> tupla[2] = [3, 4]
    Traceback (most recent call last):
        File "<stdin>", line 1, in <module>
    TypeError: 'tuple' object does not support item assignment
```

  

As tuplas são imutáveis e geralmente contêm uma sequência heterogênea de elementos. Já as listas são mutáveis, e seus elementos geralmente são homogêneos, sendo acessados ​​pela iteração da lista, embora não seja uma regra. 

Quando é necessário armazenar uma coleção de dados que não possa ser alterada, prefira usar tuplas a listas. Outra vantagem é que tuplas podem ser usadas como chaves de dicionários, que discutiremos nas seções adiante.

Tuplas são frequentemente usadas em programas Python. Um uso bastante comum são em funções que recebem múltiplos valores. As tuplas implementam todas as operações de sequência comuns.

* **Range**

O range é um tipo de sequência imutável de números, sendo comumente usado para _looping_ de um número específico de vezes em um comando `for` já que representam um intervalo. O comando **range** gera um valor contendo números inteiros sequenciais, obedecendo a sintaxe:

```python
    range(inicio, fim)
```

O número finalizador, o _fim_, não é incluído na sequência. Vejamos um exemplo:

```python
    >>> sequencia = range(1, 3)
    >>> print(sequencia)
    range(1, 3)
```

  

O range não imprime os elementos da sequência, ele apenas armazena seu início e seu final. Para imprimir seus elementos precisamos de um laço `for`:

```python
    >>> for valor in range(1, 3):
    ...     print(valor)
    ...
    1
    2
```

  

Observe que ele não inclui o segundo parâmetro da função range na sequência. Outra característica deste comando é a de poder controlar o passo da sequência adicionando um terceiro parâmetro, isto é, a variação entre um número e o seu sucessor:

```python
    >>> for valor in range(1, 10, 2):
    ...     print(valor)
    ...
    1
    3
    5
    7
    9
```

  

Os intervalos implementam todas as operações de seqüência comuns, exceto concatenação e repetição (devido ao fato de que objetos de intervalo só podem representar sequências que seguem um padrão estrito e a repetição e a concatenação geralmente violam esse padrão).

## Conjuntos

O Python também inclui um tipo de dados para conjuntos. Um conjunto, diferente de uma sequência, é uma coleção **não ordenada** e  que **não admite elementos duplicados**.

Chaves ou a função `set()` podem ser usados para criar conjuntos. 

```python
    >>> frutas = {'laranja', 'banana', 'uva', 'pera', 'laranja', 'uva', 'abacate'}
    >>> frutas
    >>> {'uva', 'abacate', 'pera', 'banana', 'laranja'}
    >>> type(frutas)
    <class 'set'>
```

  

Usos básicos incluem testes de associação e eliminação de entradas duplicadas. Os objetos de conjunto também suportam operações matemáticas como união, interseção, diferença e diferença simétrica. Podemos transformar um texto em um conjunto com a frunção `set()` e testar os operações:

```python
    >>> a =  set('abacate')
    >>> b = set('abacaxi')
    >>> a
    {'a', 'e', 'c', 't', 'b'}
    >>> b
    {'a', 'x', 'i', 'c', 'b'}
    >>> a - b                             # diferença
    {'e', 't'}
    >>> a | b                             # união 
    {'c', 'b', 'i', 't', 'x', 'e', 'a'}
    >>> a & b                             # interseção
    {'a', 'c', 'b'}
    >>> a ^ b                             # diferença simétrica
    {'i', 't', 'x', 'e'}
```

  

Note que para criar um conjunto vazio você tem que usar `set()`, não `{}`; o segundo cria um dicionário vazio, uma estrutura de dados que discutiremos na próxima seção.

```python
    >>> a = set()
    >>> a
    set()
    >>> b = {}
    >>> b
    {}
    >>> type(a)
    <class 'set'>
    >>> type(b)
    <class 'dict'>
```

  

## Dicionários

Vimos que `list`, `tuple`, `range` e `str` são sequências ordenadas de objetos, e _sets_ são coleções de elementos não ordenados. Dicionário é outra estrutura de dados em Python e seus elementos, sendo estruturadas de forma não ordenada assim como os conjuntos. Ainda assim, essa não é a principal diferença com as listas. Os dicionários são estruturas poderosas e muito utilizadas, já que podemos acessar seus elementos através de chaves e não por sua posição. Em outras linguagens, este tipo é conhecido como "matrizes associativas". 

Qualquer chave de um dicionário é associada (ou mapeada) a um valor. Os valores podem ser qualquer tipo de dado do Python. Portanto, os dicionários são pares de chave-valor não ordenados. 

Os dicionários pertencem ao tipo de mapeamento integrado e não sequenciais como as listas, tuplas e _strings_. Vamos ver como isso funciona no código e criar um dicionário com dados de uma pessoa:

```python
    >>> pessoa = {'nome': 'João', 'idade': 25, 'cidade': 'São Paulo'}
    >>> pessoa
    'nome': 'João', 'idade': 25, 'cidade': 'São Paulo'
```

  

Os dicionários são delimitados por chaves ({}) e suas chaves ('nome', 'idade' e 'cidade') por aspas. Já os valores podem ser de qualquer tipo. No exemplo acima, temos duas _strings_ e um `int`. 

O que será que acontece se tentarmos acessar seu primeiro elemento?

```python
    >>> pessoa[0]
    Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    KeyError: 0
```

  

Não é possível acessar um elemento de um dicionário por um índice como na lista. Devemos acessá-los por sua chave:

```python
    >>> pessoa['nome']
    'João'
    >>> pessoa['idade']
    25

```

  

Se precisarmos adicionar algum elemento, como por exemplo, o país, basta fazermos:

```python
    >>> pessoa1['país'] = 'Brasil'
    >>> pessoa1
    {'nome': 'João', 'idade': 25, 'cidade': 'São Paulo', 'país': 'Brasil'}
```

  

Como sempre acessamos seus elementos através de chaves, o dicionário possui um método chamado `keys()` que devolve o conjunto de suas chaves:

```python
    >>> pessoa1.keys()
    dict_keys(['nome', 'idade', 'cidade', 'pais'])
```

  

Assim como um método chamado `values()` que retorna seus valores:

```python
    >>> pessoa1.values()
    dict_values(['João', 25, 'São Paulo', 'Brasil'])
```

  

Note que as chaves de um dicionário não podem ser iguais para não causar conflito. Além disso, somente tipos de dados imutáveis podem ser usados como chaves, ou seja, nenhuma lista ou dicionário pode ser usado. Caso isso aconteça, recebemos um erro:

```python
    >>> dic = {[1, 2, 3]: 'valor'}
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
    TypeError: unhashable type: 'list'

```

  

Já as tuplas, como chaves, são permitidas:

```python
    >>> dic = {(1, 2, 3): 'valor'}
    >>> dic
    {(1, 2, 3): 'valor'}
```

  

Também podemos criar dicionários utilizando a função **dict()**:

```python
    >>> a = dict(um=1, dois=2, três=3)
    >>> a
    {'três': 3, 'dois': 2, 'um': 1}
```

  

## Exercícios: Estrutura de dados

Vamos tentar resolver alguns desafios. Dada a lista = [12, -2, 4, 8, 29, 45, 78, 36, -17, 2, 12, 8, 3, 3, -52] faça um programa que:

    a) imprima o maior elemento

    b) imprima o menor elemento

    c) imprima os números pares

    d) imprima o número de ocorrências do primeiro elemento da lista

    e) imprima a média dos elementos

    f) imprima a soma dos elementos de valor negativo

1. Primeiramente, vamos gerar um novo arquivo para este código, chamado `lista.py`. Crie o arquivo no Pycharm dentro do mesmo projeto 'jogos' criado no capítulo anterior.

1. Vamos começar gerando um loop para percorrer a lista. Utilizamos um `for` junto com um `range` para percorrer cada índice da nossa lista:

    ```python
        lista = [12, -2, 4, 8, 29, 45, 78, 36, -17, 2, 12, 12, 3, 3, -52]

        for index in range(0, len(lista)):
    ```

1. Agora, vamos resolver o item a. Defina uma variável fora do seu for chamada `maiorValor` e a iguale ao primeiro elemento na lista. Dentro do seu `for`, percorra os elementos dentro de um `if` para substituir o valor encontrado caso seja maior do que o mesmo:

    ```python
        lista = [12, -2, 4, 8, 29, 45, 78, 36, -17, 2, 12, 12, 3, 3, -52]

        maiorValor = lista[0]

        for index in range(0, len(lista)):
            #Maior valor
            if(maiorValor < lista[index]):
                maiorValor = lista[index]
        
        print(maiorValor)
    ```

1. Para resolver o item `b`, basta seguir a mesma ideia do exemplo anterior. Crie um outro `if` abaixo do que você criou no passo anterior, apenas mudando o operador da condição:

    ```python
        menorValor = lista[0]

        for index in range(0, len(lista)):
        #... seu código aqui
        #Menor valor
        if(menorValor > lista[index]):
            menorValor = lista[index]
        
        print(menorValor)

    ```

1. Para resolver o item `c`, basta definir uma lista, e caso o valor atual da lista com módulo 2 retorne 0, ele é adicionado na mesma:

    ```python
        listaPares = []

        for index in range(0, len(lista)):
            #... seu código aqui
            #Numeros pares
            if( lista[index] % 2 == 0):
                listaPares.append(lista[index])

        print(listaPares)
    ```

1. Para resolver o item `d`, é preciso verificar se o item atual da lista a ser percorrida coincide com o elemento em seu primeiro índice:

    ```python
        ocorrenciasItem1 = 0

        for index in range(0, len(lista)):
            #... seu código aqui
            #Numero de ocorrencias
            if(lista[index] == lista[0]):
                ocorrenciasItem1 = ocorrenciasItem1 + 1
        
        print(ocorrenciasItem1)
    ```

1. A resolução do item `e` requer a implementação dentro e fora do `for`. Dentro do `for`, some cada um dos elementos em uma variável única. Após o `loop`, divida o valor obtido pelo total de elementos na sua lista:

    ```python
        mediaElementos = 0

        for index in range(0, len(lista)):
            #... seu código aqui
            #Media de elementos
            mediaElementos = mediaElementos + lista[index]
        mediaElementos = mediaElementos / len(lista)
        
        print(mediaElementos)
    ```
1. Por fim, para resolver o item `f`, faça a soma de todos os números negativos somando todos os valores que são menores que 0:

    ```python
        somaNegativos = 0

        for index in range(0, len(lista)):
            #... seu código aqui
            #Soma dos números negativos
            if(lista[index] < 0):
                somaNegativos = somaNegativos + lista[index]
        
        print(somaNegativos)
    ```

1. Tente imprimir todas estas condições no seu `loop` e veja o resultado. O seu resultado deverá estar similar a:

    ```python
        lista = [12, -2, 4, 8, 29, 45, 78, 36, -17, 2, 12, 12, 3, 3, -52]

        #declarando nossas variáveis
        maiorValor = lista[0]
        menorValor = lista[0]
        listaPares = []
        ocorrenciasItem1 = 0
        mediaElementos = 0
        somaNegativos = 0


        #iniciando o nosso loop:
        for index in range(0, len(lista)):

            #Maior valor
            if(maiorValor < lista[index]):
                maiorValor = lista[index]

            #Menor valor
            if(menorValor > lista[index]):
                menorValor = lista[index]

            #Numeros pares
            if(lista[index] % 2 == 0):
                listaPares.append(lista[index])
                
            #Numero de ocorrências
            if(lista[index] == lista[0]):
                ocorrenciasItem1 = ocorrenciasItem1 + 1

            #Soma dos números negativos
            if(lista[index] < 0):
                somaNegativos = somaNegativos + lista[index]

            #Media do somatório dos elementos
            mediaElementos = mediaElementos + lista[index]


        mediaElementos = mediaElementos / len(lista)

        print("Maior valor: " + str(maiorValor))
        print("Menor valor: " + str(menorValor))
        print("Lista de elementos pares: " + str(listaPares))
        print("Número de ocorrências do primeiro item: " + str(ocorrenciasItem1))
        print("Média dos elementos: " + str(mediaElementos))
        print("Somatório dos valores negativos:" + str(somaNegativos))
    ```
