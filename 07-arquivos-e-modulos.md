# Arquivos

Uma funcionalidade que ainda nos atrapalha no jogo da forca é a palavra secreta, que atualmente está fixa. Se queremos que a palavra seja diferente, devemos modificá-la no código.

A nossa ideia é ler palavras de um arquivo de texto, e dentre elas escolhemos uma palavra aleatoriamente, que será a palavra secreta do jogo.

## Escrita de um arquivo

Para abrir um arquivo, o Python possui a função `open()`. Ela recebe dois parâmetros: o primeiro é o nome do arquivo a ser aberto, e o segundo parâmetro é o modo que queremos trabalhar com esse arquivo - se queremos ler ou escrever. O modo é passado através de uma _string_: "w" (abreviação para _write_) para escrita e "r" (abreviação para _read_) para leitura.

No jogo, faremos a leitura de um arquivo. Antes, vamos testar como funciona a escrita no terminal do Python 3:

```python
	>>> arquivo = open('palavras.txt', 'w')
```

O modo é opcional e o modo padrão é o "r" de leitura (_reading_) que veremos mais adiante. 

O arquivo criado se chama 'palavras.txt' e está no modo de escrita. É importante saber que o modo de escrita sobrescreve o arquivo, se o mesmo existir. Se a intenção é apenas adicionar conteúdo ao arquivo, utilizamos o modo "a" (abreviação para _append_).

Agora que temos o arquivo, vamos aprender a escrever algum conteúdo nele. Basta chamar a partir do arquivo a função `write()`, passando para ela o que se quer escrever no arquivo:

```python
	>>> arquivo.write('banana')
	6
	>>> arquivo.write('melancia')
	8
```

O retorno dessa função é o número de caracteres de cada texto adicionado no arquivo.

## Fechando um arquivo

Quando estamos trabalhando com arquivos, devemos nos preocupar em fechá-lo. Para fechá-lo usamos a função `close()`:

```python
	>>> arquivo.close()
```

Após isso, podemos verificar o conteúdo do arquivo. Repare que ele foi criado na mesma pasta em que o comando para abrir o console do Python 3 foi executado. Se você tentar fechar um arquivo que já está fechado, não vai surtir efeito algum, nem mesmo um erro. Abra o arquivo na pasta criada e verifique seu conteúdo:

```
	bananamelancia
```

As palavras foram escritas em uma mesma linha. Mas como escrever uma nova linha?

## Escrevendo palavras em novas linhas

A primeira coisa que devemos fazer é abrir o arquivo novamente, dessa vez utilizando o modo _'a'_, de `append`:

```python
	arquivo = open('palavras.txt', 'a')
```

Vamos escrever novamente no arquivo, mas dessa vez com a preocupação de criar uma nova linha após cada conteúdo escrito. Para representar uma nova linha em código, adicionamos o _\n_ ao final do que queremos escrever:

```python
	>>> arquivo.write('morango\n')
	8
	>>> arquivo.write('manga\n')
```

Ao fechar o arquivo e verificar novamente o seu conteúdo, vemos:

```python
	bananamelanciamorango
	manga
```

A palavra morango ainda ficou na mesma linha, mas como especificamos na sua adição que após a palavra deverá ter uma quebra de linha, a palavra manga foi adicionada abaixo, em uma nova linha.

Por fim, vamos mover este arquivo para nosso projeto e ajeitar suas palavras quebrando as linhas.

## Exercícios

1. Vamos navegar até nossa pasta _jogos_ dentro de _home_. Lembrando que nossa estrutura de arquivos está assim:
    ```
        |_ home
            |_ jogos
                |_ advinhacao.py
                |_ forca.py
                |_ menu.py
    ```

1. Crie um outro arquivo, chamado _arquivo.py_ e insira o seguinte código:
    ```python
       arquivo = open("palavras.txt", "w")
    ```
    Rode o arquivo e veja o resultado. O seu diretório deverá estar similar a:
    ```
        |_ home
            |_ jogos
                |_ advinhacao.py
                |_ forca.py
                |_ arquivo.py
                |_ menu.py
                |_ palavras.txt
    ```

1. Vamos começar a escrever no nosso arquivo utilizando a função `write()` as palavras que usaremos no nosso jogo da forca:
    ```python
        arquivo.write('banana\n')
        
        arquivo.write('melancia\n')
        
        arquivo.write('morango\n')
        
        arquivo.write('manga\n')
        
    ```
    Note que ao final de cada palavra temos que acrescentar o "\n" para a quebra de linha, que vai facilitar na hora da leitura.

1. É uma boa prática fechar o arquivo depois de utilizá-lo, assim outros programas ou processos podem ter acesso ao arquivo e ele não fica preso apenas ao nosso programa Python.
    ```python
       arquivo.close()
    ```

> **Para saber mais**
>
>Além do **r**, **w** e **a** existe o modificador **b** que é utilizado quando se deseja trabalhar no modo binário. Para abrir uma imagem no modo leitura, devemos usar:
    ```python
	    imagem = open('foto.jpg', 'rb')
    ```

## Lendo um arquivo

Ainda no terminal do Python 3, veremos o funcionamento da leitura de um arquivo. Como agora o arquivo _palavras.txt_ está na pasta do projeto `jogos`, devemos executar o comando que abre o terminal do Python 3 na pasta do projeto.
```bash
        $ cd jogos
        $ python3
```

Vamos então abrir o arquivo no modo de leitura, basta passar o nome do arquivo e a letra "r" para a função `open()`, como já visto anteriormente.

```python
	arquivo = open('palavras.txt', 'r')
```

Diferente do modo "w", abrir um arquivo que não existe no modo "r" não vai criar um arquivo. Se "palavras.txt" não existir, o Python vai lançar o erro `FileNotFoundError`:

```python
	arquivo.open('frutas.txt', 'r')

	Traceback (most recent call last):
  	  File "<stdin>", line 1, in <module>
	FileNotFoundError: [Errno 2] No such file or directory: 'frutas.txt'
```

Como o arquivo _frutas.txt_ não existe na pasta jogos, o Pyhton não consegue encontrar e acusa que não existe nenhum arquivo ou diretório com este nome na pasta raiz.

Como abrimos o arquivo no modo de leitura, a função `write()` não é suportada: 
```python
        arquivo.write("oi")

        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
        io.UnsupportedOperation: not writable
```
Para ler o arquivo inteiro, utilizamos a função `read()`:

```python
	arquivo.read()

	'banana\nmelancia\nmorango\nmanga\n'
```

Mas ao executar a função novamente, será retornado uma _string_ vazia:

```python       
	arquivo.read()
	''
```

Isso acontece porque o arquivo é como um fluxo de linhas, que começa no início do arquivo como se fosse um cursor. Ele vai descendo e lendo o arquivo. Após ler tudo, ele fica posicionado no final do arquivo. Quando chamamos a função `read()` novamente, não há mais conteúdo pois ele todo já foi lido.

Portanto, para ler o arquivo novamente, devemos fechá-lo e abrí-lo outra vez:
```python
    arquivo.close()
    arquivo = open('palavras.txt', 'r')
    arquivo.read()
```
## Lendo linha por linha do arquivo

Não queremos ler todo o conteúdo do arquivo mas ler linha por linha. Como já foi visto, um arquivo é um fluxo de linhas, ou seja, uma sequência de linhas. Sendo uma sequência, podemos utilizar um laço `for` para ler cada linha do arquivo:

```python
	arquivo = open('palavras.txt', 'r')
	for linha in arquivo:
        print(linha)
	...
	banana

	melancia

	morango

	manga
```

Repare que existe uma linha vazia entre cada fruta. Isso acontece porque estamos utilizando a função `print()` que também acrescenta, por padrão, um `\n`. Agora vamos utilizar outra função, a `readline()`, que lê apenas uma linha do arquivo:

```python
	arquivo = open('palavras.txt', 'r')
	linha = arquivo.readline()
	print(linha)

	'banana\n'
```

Há um `\n` ao final de cada linha, de cada palavra, mas queremos somente a palavra. Para tirar espaços em branco no início e no fim da _string_, basta utilizar a função `strip()`, que também remove caracteres especiais, como o `\n` - para mais informações consulte a documentação de _strings_. Sabendo disso tudo, já podemos implementar a funcionalidade de leitura de arquivo no nosso jogo:

```python
    arquivo = open('palavras.txt', 'r')
    palavras = []

    for linha in arquivo:
        linha = linha.strip()
        palavras.append(linha)

    arquivo.close()
```

Agora já temos todas as palavras na lista, mas como selecionar uma delas aleatoriamente?

## Gerando um número aleatório

Sabemos que cada elemento da lista possui uma posição e vimos no treinamento anterior como gerar um número aleatório. A biblioteca que sabe gerar um número aleatório é a random. Vamos testá-la no terminal do Python 3, primeiro importando-a:

```python
	>>> import random
```

Para gerar o número aleatório utilizamos a biblioteca e chamamos a função `randrange()`, que recebe o intervalo de valores que o número aleatório deve estar. Então vamos passar o valor 0 (equivalente à primeira posição da nossa lista) e 4 (lembrando que o número é exclusivo, ou seja, o número aleatório será entre 0 e 3, equivalente à última posição da nossa lista):


```python
	>>> import random
	>>> random.randrange(0, 4)
	0
	>>> random.randrange(0, 4)
	1
	>>> random.randrange(0, 4)
	3
	>>> random.randrange(0, 4)
	1
	>>> random.randrange(0, 4)
	3
```

Sabendo disso, vamos implementar esse código no nosso jogo.

## Exercícios - Leitura de arquivos

1.Vamos começar abrindo o arquivo "palavras.txt" no nosso script `forca.py` que criamos no exercício anterior. É importante já acrescentar o comando para fechá-lo para não esquecer no futuro:
```python
    def jogar():
        print('*********************************')
        print('***Bem vindo ao jogo da Forca!***')
        print('*********************************')

        arquivo = open('palavras.txt', 'r')
        arquivo.close()

        #restante do código
```

1. Agora vamos criar uma lista chamada `palavras` e fazer um laço `for` para acessar cada linha para guardar na lista:
```python
    arquivo = open('palavras.txt', 'r')
    palavras = []

    for linha in arquivo:
        palavras.append(linha)

    arquivo.close()
```

1. Como precisamos remover o **\n** ao final da linha, usaremos a função `strip()` em cada linha:
```python
    def jogar():

        arquivo = open('palavras.txt', 'r')
        palavras = []

        for linha in arquivo:
            linha = linha.strip()
            palavras.append(linha)

        arquivo.close()
```

1. Devemos importar a biblioteca _random_ para gerar um número que vai de 0 até a quantidade de palavras da nossa lista. Usaremos a função `len()` para saber o tamanho da lista e a `randrange()` para gerar um número randômico de um intervalo específico:
```python
	import random

	print('*********************************')
	print('***Bem vindo ao jogo da Forca!***')
	print('*********************************')

	arquivo = open('palavras.txt', 'r')
	palavras = []

	for linha in arquivo:
	    linha = linha.strip()
	    palavras.append(linha)

	arquivo.close()

	numero = random.randrange(0, len(palavras))
```

1. Agora que temos o número aleatório, vamos utilizá-lo como índice para acessar a lista e atribuir essa palavra à variável palavra_secreta:
```python
    import random

    print("*********************************")
    print("***Bem vindo ao jogo da Forca!***")
    print("*********************************")

    arquivo = open("palavras.txt", "r")
    palavras = []

    for linha in arquivo:
        linha = linha.strip()
        palavras.append(linha)

    arquivo.close()

    numero = random.randrange(0, len(palavras))

    palavra_secreta = palavras[numero].upper()
    letras_acertadas = ['_' for letra in palavra_secreta]
```

1. Por fim, temos que deixar nossa variável `letras_acertadas` dinâmica, com número de letras de acordo com nossa `palavra_secreta`. Vamos utilizar um `for` dentro da lista para gerar um '\_' para cada letra de acordo com o tamanho da palavra_secreta:
```python
    letras_acertadas = ['_' for letra in palavra_secreta]
```

1. Como já garantimos que a `palavra_secreta` está toda em letras maiúsculas com o código `palavras[numero].upper()`, modificaremos o `chute` para o primeiro **if** continuar funcionando
```python
    chute = input('Qual a letra? ')
    chute = chute.strip().upper()
```
Podemos executar o jogo e notar que a palavra é selecionada aleatoriamente! 

Mas agora a nossa função cresceu bastante, com várias funcionalidades e responsabilidades. No próximo capítulo organizaremos melhor o nosso código, separando-o em funções e deixando-o mais fácil de entender.


## Para saber mais - comando with

Já falamos da importância de fechar o arquivo, certo? Veja o código abaixo que justamente usa a função `close()` :
```python
	arquivo = open('palavras.txt', 'r')
	palavras = logo.read()
	arquivo.close()
```

Imagine que algum problema aconteça na hora da leitura quando a função read() é chamada. Será que o arquivo é fechado quando o erro ocorre?

Se for algum erro grave, o programa pode parar a execução sem ter fechado o arquivo e isto seria bastante ruim. Para evitar esse tipo de situação, existe no Python uma sintaxe especial para abertura de arquivo:
```python
	with open('palavras.txt') as arquivo:
	    for linha in arquivo:
	        print(linha)
```

Repare o comando `with` usa a função `open()`, mas não a função `close()`. Isso não será mais necessário, já que o comando `with` vai se encarregar de fechar o arquivo para nós, mesmo que aconteça algum erro no código dentro de seu escopo. Muito melhor, não?

## Melhorando nosso código

Nos capítulos anteriores criamos dois jogos, avançamos no jogo da Forca e implementamos leitura de palavras em um arquivo. Agora vamos utilizar o que aprendemos de funções para encapsular nosso código e deixá-lo mais organizado. Vamos começar, com os conhecimentos que temos até aqui, para estruturar nosso jogo da Forca.

A função `jogar()` possui um código muito complexo, com muitas funcionalidades e responsabilidades.

Entre as funcionalidades que o código possui, está a apresentação do jogo, a leitura do arquivo e inicialização da palavra secreta, entre outras. Vamos então separar as responsabilidades do código em funções, melhorando a sua legibilidade e organização.

Vamos começar com a mensagem de apresentação do nosso jogo e exportar o código para a função `imprime_mensagem_abertura()`. Não podemos nos esquecer de chamar essa função no início da função `jogar()`:

```python
    import random

    def jogar():
        imprime_mensagem_abertura()

        #código omitido

    def imprime_mensagem_abertura():
        print('*********************************')
        print('***Bem vindo ao jogo da Forca!***')
        print('*********************************')
```

Aqui não importa o local da função, ela pode ser declarada antes ou depois da função jogar().

O que fizemos foi **refatorar** nosso código. Refatoração é o processo de modificar um programa para melhorar a estrutura interna do código, sem alterar seu comportamento externo. Veja que se executarmos nosso jogo da Forca, tudo funciona como antes:

```python
    *********************************)
    ***Bem vindo ao jogo da Forca!***
    ********************************* 
    ['_', '_', '_', '_', '_', '_']
    Qual letra? 
```

No próximo exercício, vamos refatorar as demais partes do nosso código.

## Exercício - Refatorando o jogo da Forca

1. Crie a função imprime_mensagem_abertura que vai isolar a mensagem de abertura do jogo:
```python
    import random

    def jogar():
        imprime_mensagem_abertura()

        #código omitido

    def imprime_mensagem_abertura():
        print('*********************************')
        print('***Bem vindo ao jogo da Forca!***')
        print('*********************************')
```

1. Agora, vamos separar o código que realiza a leitura do arquivo e inicializa a palavra secreta na função `carrega_palavra_secreta()`:
```python
    def carrega_palavra_secreta():
        arquivo = open('palavras.txt', 'r')
        palavras = []

        for linha in arquivo:
            linha = linha.strip()
            palavras.append(linha)

        arquivo.close()

        numero = random.randrange(0, len(palavras))
        palavra_secreta = palavras[numero].upper()
```
Só que a função `jogar()` irá reclamar que a palavra_secreta não existe. O que queremos é que, ao executar a função `carrega_palavra_secreta()`, que ela retorne a palavra secreta para nós, assim poderemos guardá-la em uma variável:
```python
    import random

    def jogar():

        imprime_mensagem_abertura()

        palavra_secreta = carrega_palavra_secreta()

        letras_acertadas = ["_" for letra in palavra_secreta]

        # restante do código omitido
```
Só que como faremos a função `carrega_palavra_secreta()` retornar um valor, no caso a `palavra_secreta`? A `palavra_secreta` já existe, mas só dentro da função `carrega_palavra_secreta()`. Para que ela seja retornada, utilizamos a palavra-chave return:
```python
    def carrega_palavra_secreta():
        arquivo = open('palavras.txt', 'r')
        palavras = []

        for linha in arquivo:
            linha = linha.strip()
            palavras.append(linha)

        arquivo.close()

        numero = random.randrange(0, len(palavras))
        palavra_secreta = palavras[numero].upper()

        return palavra_secreta
```

1. Agora, vamos criar uma função que inicializa a lista de letras acertadas com o caractere '\_'. Criaremos a função `inicializa_letras_acertadas()`:
```python
    import random

    def jogar():

        imprime_mensagem_abertura()

        palavra_secreta = carrega_palavra_secreta()

        letras_acertadas = inicializa_letras_acertadas()

        # código omitido

    def inicializa_letras_acertadas():
        return ['_' for letra in palavra_secreta]
```

1. Mas a função `inicializa_letras_acertadas()` precisa ter acesso à palavra_secreta, pois ela não existe dentro da função, já que uma função define um escopo, e as variáveis declaradas dentro de uma função só estão disponíveis dentro dela. Então, ao chamar a função `inicializa_letras_acertadas()`, vamos passar `palavra_secreta` para ela por parâmetro:
```python
    import random

    def jogar():

        imprime_mensagem_abertura()

        palavra_secreta = carrega_palavra_secreta()

        letras_acertadas = inicializa_letras_acertadas(palavra_secreta)

        # restante do código omitido

    def inicializa_letras_acertadas(palavra):
        return ["_" for letra in palavra]
```

1. Vamos continuar refatorando nosso código. Criaremos a função `pede_chute()`, que ficará com o código que pede o chute do usuário, remove os espaços antes e depois, e o coloca em caixa alta. Não podemos nos esquecer de retornar o chute:
```python
    def jogar():
        # código omitido

        while (not acertou and not enforcou):
            chute = pede_chute()
            #código omitido

        #código omitido                

    def pede_chute():
        chute = input('Qual letra? ')
        chute = chute.strip().upper()
        return chute
```

1. Ainda temos o código que coloca o chute na posição correta, dentro da lista. Vamos colocá-lo dentro da função `marca_chute_correto()`:
```python
        while (not acertou and not enforcou):

            chute = pede_chute()

            if (chute in palavra_secreta):
                marca_chute_correto()
            else:
                erros += 1

            enforcou = erros == 6
            acertou = '_' not in letras_acertadas
            print(letras_acertadas)

        #código omitido
        
    def marca_chute_correto():
        posicao = 0
        for letra in palavra_secreta:
            if (chute == letra):
                letras_acertadas[posicao] = letra
            posicao += 1        
```

Mas a função `marca_chute_correto()` precisa ter acesso a três valores: `palavra_secreta`, `chute` e `letras_acertadas`. Assim, vamos passar esses valores por parâmetro:
```python
    if (chute in palavra_secreta):
        marca_chute_correto(chute, letras_acertadas, palavra_secreta)
```
E modificamos nossa função para receber esses parâmetros:
```python
    def marca_chute_correto(chute, letras_acertadas, palavra_secreta):
        posicao = 0
        for letra in palavra_secreta:
            if (chute == letra):
                letras_acertadas[posicao] = letra
            posicao += 1
```

1. Por fim, vamos remover a mensagem de fim de jogo e exportar os códigos que imprimem as mensagens de vencedor e perdedor do jogo:
```python
    if (acertou):
        imprime_mensagem_vencedor()
    else:
        imprime_mensagem_perdedor()
```
E criar as funções:
```python
    def imprime_mensagem_vencedor():
        print('Você ganhou!')

    def imprime_mensagem_perdedor():
        print('Você perdeu!')
```
Agora o nosso código está muito mais organizado e legível. Ao chamar todas as funções dentro da função `jogar()`, nosso código ficará assim:
```python
def jogar():
    imprime_mensagem_abertura()
    palavra_secreta = carrega_palavra_secreta()

    letras_acertadas = inicializa_letras_acertadas(palavra_secreta)
    print(letras_acertadas)

    enforcou = False
    acertou = False
    erros = 0

    while(not enforcou and not acertou):

        chute = pede_chute()

        if(chute in palavra_secreta):
            marca_chute_correto(chute, letras_acertadas, palavra_secreta)
        else:
            erros += 1

        enforcou = erros == 6
        acertou = "_" not in letras_acertadas

        print(letras_acertadas)

    if(acertou):
        imprime_mensagem_vencedor()
    else:
        imprime_mensagem_perdedor()
```
Por fim, podemos executar o nosso código, para verificar que o mesmo continua funcionando normalmente.
```python
    *********************************
    ***Bem vindo ao jogo da Forca!***
    ********************************* 
    ['_', '_', '_', '_', '_', '_']
    Qual letra? 
```

1. (opcional) Com a melhor organização do nosso código, vamos melhorar a exibição, a apresentação da forca, deixando o jogo mais amigável. Vamos começar com a mensagem de perdedor, alterando a função `imprime_mensagem_perdedor`. Ela ficará assim:
```python
def imprime_mensagem_perdedor(palavra_secreta):
    print('Puxa, você foi enforcado!')
    print('A palavra era {}'.format(palavra_secreta))
    print("    _______________         ")
    print("   /               \\        ")
    print("  /                 \\       ")
    print("//                   \\/\\    ")
    print("\\|   XXXX     XXXX   | /    ")
    print(" |   XXXX     XXXX   |/     ")
    print(" |   XXX       XXX   |      ")
    print(" |                   |      ")
    print(" \\__      XXX      __/      ")
    print("   |\\     XXX     /|        ")
    print("   | |           | |        ")
    print("   | I I I I I I I |        ")
    print("   |  I I I I I I  |        ")
    print("   \\_             _/        ")
    print("     \\_         _/          ")
    print("       \\_______/            ")
```
Precisamos passar a `palavra_secreta` para função `imprime_mensagem_perdedor()`:
```python
    if(acertou):
        imprime_mensagem_vencedor()
    else:
        imprime_mensagem_perdedor(palavra_secreta)
```
E modificamos o código de `imprime_mensagem_vencedor()` para:
``` python
def imprime_mensagem_vencedor():
    print('Parabéns, você ganhou!')
    print("       ___________      ")
    print("      '._==_==_=_.'     ")
    print("      .-\\:      /-.    ")
    print("     | (|:.     |) |    ")
    print("      '-|:.     |-'     ")
    print("        \\::.    /      ")
    print("         '::. .'        ")
    print("           ) (          ")
    print("         _.' '._        ")
    print("        '-------'       ")
```

Vá até a pasta do curso e copie o código destas funções que estão em um arquivo chamado _funcoes_forca.py_.

1. (opcional) Por fim, crie a função `desenha_forca()`, que irá desenhar uma parte da forca, baseado nos erros do usuário. Como ela precisa acessar os erros, passe-o por parâmetro para a função:
```python
    def desenha_forca(erros):
        pass
```
E copie o conteúdo do código da função `desenha_forca()` do arquivo funcoes _forca.py_ na pasta do curso para sua função.

1. Para finalizar, chame a função `desenha_forca` quando o jogador errar e aumente o limite de erros para 7.
```python
    if(chute in palavra_secreta):
        marca_chute_correto(chute, letras_acertadas, palavra_secreta)
    else:
        erros += 1
        desenha_forca(erros)

    enforcou = erros == 7
    acertou = "_" not in letras_acertadas
```

1. Tente fazer o mesmo com o jogo da adivinhação, refatore partes do código e isole em funções. Além disso, use sua criatividade para customizar mensagens para o usuário do seu jogo.

Neste exercício, praticamos bastante do que aprendemos no capítulo de função e finalizamos o jogo da forca.

Você pode estar se perguntando por que encapsulamos uma simples linha de código em uma função. Fizemos isso somente para deixar claro o que estamos fazendo, melhorando a **legibilidade do código**. Mas precisamos tomar cuidado com a criação de funções, pois criar funções desnecessariamente pode aumentar a complexidade do código.
