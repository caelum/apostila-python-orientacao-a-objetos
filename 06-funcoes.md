# Funções

Objetivos:

* entender o conceito de função
* saber e usar algumas funções embutidas da linguagem
* criar uma função

## O que é uma função?

O conceito de função é um dos mais importantes na matemática. Em computação, uma função é uma sequência de instruções que computa um ou mais resultados que chamamos de parâmetros. No capítulo anterior, utilizamos algumas funções já prontas do Python, como o **print()**, **input()**, **format()** e **type()**. 

Também podemos criar nossas próprias funções. Por exemplo, quando queremos calcular a razão do espaço pelo tempo, podemos definir uma função recebendo estes parâmetros:

```
	f(espaco, tempo) = espaco/tempo
```

Essa razão do espaço pelo tempo	é o que chamamos de velocidade média na física. Podemos então dar este nome a nossa função:

```
	velocidade(espaco, tempo) = espaco/tempo
```

Se um carro percorreu uma distância de 100 metros em 20 segundos, podemos calcular sua velocidade média:

```
	velocidade(100, 20) = 100/20 = 	5 m/s
```

O Python permite definirmos funções como essa da velocidade média. A sintaxe é muito parecida com a da matemática. Para definirmos uma função no Python, utilizamos o comando **def**:

```python
	def velocidade(espaco, tempo):
	    pass
```

Logo após o `def` vem o nome da função, e entre parênteses vêm os seus parâmetros. Uma função também tem um escopo e um bloco de instruções onde colocamos os cálculos, onde estes devem seguir a identação padrão do Python (4 espaços a direita). 

Como nossa função ainda não faz nada, utilizamos a palavra chave **pass** para dizer ao interpretador que definiremos os cálculos depois. A palavra `pass` não é usada apenas em funções, podemos usar em qualquer bloco de comandos como nas instruções `if`, `while` e `for`, por exemplo.

Vamos substituir a palavra `pass` pelos cálculos que nossa função deve executar:

```python
	def velocidade(espaco, tempo):
	    v = espaco/tempo
	    print('velocidade: {} m/s'.format(v))
```

Nossa função faz o cálculo da velocidade média e utiliza a função `print()` do Python para imprimi-lo na tela. Vamos testar nossa função:

```python
	velocidade(100, 20)

	#velocidade: 5 m/s
```

  

De maneira geral, uma função é um estrutura para agrupar um conjunto de instruções que podem ser reutilizadas.	Agora qualquer parte do nosso programa pode chamar a função velocidade quando precisar calcular a velocidade média de um veículo, por exemplo. E podemos chamá-la mais de uma vez, o que significa que não precisamos escrever o mesmo código novamente.

Funções são conhecidas por diversos nomes em linguagens de programação como subrotinas, rotinas, procedimentos, métodos e subprogramas.

Podemos ter funções sem parâmetros. Por exemplo, podemos ter uma função que diz 'oi' na tela:

```python
		def diz_oi():
		   print("oi")
		diz_oi()

		#oi
```

  

## Parâmetros de Função

Um conjunto de parâmetros consiste em uma lista com nenhum ou mais elementos que podem ser obrigatórios ou opcionais. Para um parâmetro ser opcional, o mesmo é atribuído a um valor padrão (default) - o mais comum é utilizar **None**. Por exemplo:

```python
	def dados(nome, idade=None):
	    print('nome: {}'.format(nome))
	    if(idade is not None):
	        print('idade: {}'.format(idade))
	    else:
	        print('idade: não informada')
```

O código da função acima recebe uma idade como parâmetro e faz uma verificação com uma instrução **if**: se a idade for diferente de _None_ ela vai imprimir a idade, caso contrário vai imprimir _idade não informada_. Vamos testar passando os dois parâmetros e depois apenas o nome:

```python	
	dados('joão', 20)

	#nome: joão
	#idade: 20
```

Agora passando apenas o nome:

```python
	dados('joão')

	#nome: joão
	#idade: não informada
```

  

E o que acontece se passarmos apenas a idade?

```python
	dados(20)

	#nome: 20
	#idade: não informada
```

  

Veja que o Python obedece a ordem dos parâmetros. Nossa intenção era passar o número 20 como `idade`, mas o interpretador vai entender que estamos passando o `nome` porque não avisamos isso à ele. Caso queiramos passar apenas a `idade`, devemos especificar o parâmetro:

```python
	dados(idade=20)
	 File "<stdin>", line 1, in <module>
	TypeError: dados() missing 1 required positional argument: 'nome'
```

  

O interpretador irá acusar um erro, já que não passamos o atributo obrigatório `nome`.

## Função com retorno

E se ao invés de apenas mostrar o resultado, quisermos utilizar a velocidade média para fazer outro cálculo, como calcular a aceleração? Da maneira como está, nossa função `velocidade()` não consegue utilizar seu resultado final para cálculos.

```
	Exemplo: 
	aceleracao = velocidade(parametros) / tempo
```

Para conseguirmos este comportamento, precisamos que nossa função **retorne** o valor calculado por ela. Em Python e em outras linguagens de programação, isto é alcançado através do comando **return**:

```python
	def velocidade(espaco, tempo):
	    v = espaco/tempo
	    return v 
```

Testando:

```python
	print(velocidade(100, 20))
	#5.0
```

  

Ou ainda, podemos atribuí-la a uma variável:

```python
	resultado = velocidade(100, 20)
	print(resultado)
	#5.0
```

  

E conseguimos utilizá-la no cálculo da aceleração:

```python
	aceleracao = velocidade(100, 20)/20
	print(aceleracao)
	#0.25
```

Uma função pode conter mais de um comando **return**. Por exemplo, nossa função `dados()` que imprime o nome e a idade, pode agora retornar uma _string_. Repare que, neste caso, temos duas situações possíveis: a que a idade é passada por parâmetro e a que ela não é passada. Aqui, teremos dois comandos **return**:

```python
	def dados(nome, idade=None):
	    if(idade is not None):
	        return ('nome: {} \nidade: {}'.format(nome, idade))
	    else:
	        return ('nome: {} \nidade: não informada'.format(nome))
```

Apesar da função possuir dois comandos **return**, ela tem apenas um retorno -- vai retornar um ou o outro. Quando a função encontra um comando **return**, ela não executa mais nada que vier depois dele dentro de seu escopo.

## Retornando múltiplos valores

Apesar de uma função executar apenas um retorno, em Python podemos retornar mais de um valor. Vamos fazer uma função calculadora que vai retornar os resultados de operações básicas entre dois números: adição(+) e subtração(-), nesta ordem.

Para retornar múltiplos valores, retornamos os resultados separados por vírgula:

```python
	def calculadora(x, y):
	    return x+y, x-y

	print(calculadora(1, 2))

	#(3, -1)
```

  

Qual será o tipo de retorno desta função? Vamos perguntar ao interpretador através da função type:

```python
	print(type(calculadora(1,2)))
	#<class 'tuple'>
```

  

Da maneira que definimos o retorno, a função devolve uma tupla. Neste caso específico, poderíamos retornar um dicionário e usar um laço `for` para imprimir os resultados:

```python
		def calculadora(x, y):
		    return {'soma':x+y, 'subtração':x-y}
	
	    resultados = calculadora(1, 2)
		for key in resultados:
	       print('{}: {}'.format(key, resultados[key]))

		#soma: 3
		#subtração: -1
```

  

## Exercícios: Funções

1. Defina uma função chamada velocidade_media() em um script chamado `funcoes.py` que recebe dois parâmetros: a distância percorrida (em metros) e o tempo (em segundos) gasto.
	```python
		def velocidade_media(distancia, tempo):
		    pass
	```

1. Agora vamos inserir as instruções, ou seja, o que a função deve fazer. Vamos inserir os comandos para calcular a velocidade média e guardar o resultado em uma variável `velocidade`:
	```python
		def velocidade_media(distancia, tempo):
		    velocidade = distancia/tempo
	```

1. Vamos fazer a função imprimir o valor da velocidade média calculada:
	```python
		def velocidade_media(distancia, tempo):
		    velocidade = distancia/tempo
		    print(velocidade)
	```

1. Teste o seu código chamando a função para os valores abaixo e compare os resultados com seus colegas:
	* distância: 100, tempo = 20
	* distância: 150, tempo = 22
	* distância: 200, tempo = 30
	* distância: 50, tempo = 3

1. Modifique a função `velocidade_media()` de modo que ela retorne o resultado calculado.

1. Defina uma função `soma()` que recebe dois números como parâmetros e calcula a soma entre eles.

1. Defina uma função `subtracao()`que recebe dois números como parâmetros e calcula a diferença entre eles.

1. Agora faça uma função `calculadora()` que recebe dois números como parâmetros e retorna o resultado da soma e da subtração entre eles.

1. Modifique a função `calculadora()` do exercício anterior e faça ela retornar também o resultado da multiplicação e divisão dos parâmetros.

1. Chame a função `calculadora()` com alguns valores.

1. (opcional) Defina uma função `divisao()` que recebe dois números como parâmetros, calcula e retorna o resultado da divisão do primeiro pelo segundo. Modifique a função `velocidade_media()` utilizando a função `divisao()` para calcular a velocidade. Teste o seu código chamando a função `velocidade_media()` com o valores abaixo:
    a. distância: 100, tempo = 20
    b. distância: -20, tempo = 10
    c. distância: 150, tempo = 0

## Número arbitrário de parâmetros (*args)

Podemos passar um número arbitrário de parâmetros em uma função. Utilizamos as chamadas variáveis mágicas do Python: **\*args** e **\*\*kwargs**. Muitos programadores tem dificuldades em entender essas variáveis. Vamos entender o que elas são. 

Não é necessário utilizar exatamente estes nomes: `*args` e `**kwargs`. Apenas o asterisco(\*), ou dois deles(\*\*), serão necessários. Podemos optar, por exemplo, em escrever `*var` e `**vars`. Mas `*args` e `**kwargs` é uma convenção entre a comunidade que também seguiremos.

Primeiro, aprenderemos a usar o `*args`. É usado, assim como o `**kwargs`, em definições de funções. `*args` e `**kwargs` permitem passar um número variável de argumentos de uma função. O que a variável significa é que o programador ainda não sabe de antemão quantos argumentos serão passados para sua função, apenas que são muitos. Então, neste caso usamos a palavra chave `*args`.

Veja um exemplo:

```python
	def teste(arg, *args):
	    print('primeiro argumento normal: {}'.format(arg))
	    for arg in args:
	        print('outro argumento: {}'.format(arg))

	teste('python', 'é', 'muito', 'legal')        
```

Que vai gerar a saída:

```python
	primeiro argumento normal:  python
	outro argumento:  é
	outro argumento:  muito
	outro argumento:  legal
```

  

O parâmetro `arg` é como qualquer outro parâmetro de função, já o `*args` recebe múltiplos parâmetros. Viu como é fácil? Também poderíamos conseguir o mesmo resultado passando um `list` ou `tuple` de argumentos, acrescido do asterisco:

```python
	lista = ["é", "muito", "legal"]
	teste('python', *lista)
```

  

Ou ainda:

```python
	tupla = ("é", "muito", "legal")
	teste('python', *tupla)
```

  

O `*args`  então é utilizado quando não sabemos de antemão quantos argumentos queremos passar para uma função. O asterisco (`*`) executa um empacotamento dos dados para facilitar a passagem de parâmetros, e a função que recebe este tipo de parâmetro é capaz de fazer o desempacotamento.

## Número arbitrário de chaves (**kwargs) 

O `**kwargs` permite que passemos o tamanho variável da palavra-chave dos argumentos para uma função. Você deve usar o `**kwargs` se quiser manipular argumentos nomeados em uma função. Veja um exemplo:

```python
	def minha_funcao(**kwargs):
	    for key, value in kwargs.items():
	        print('{0} = {1}'.format(key, value))

	>>> minha_funcao(nome='caelum')
	nome = caelum
```

  

Também podemos passar um dicionário acrescido de dois asteriscos, já que se trata de chave e valor:

```python
	dicionario = {'nome': 'joao', 'idade': 25}
	minha_funcao(**dicionario)
	idade = 25
	nome = joao
```

  

A diferença é que o `*args` espera uma tupla de argumentos posicionais, enquanto o `**kwargs` um dicionário com argumentos nomeados.

## Exercício - \*args e \*\*kwargs

1. Crie um arquivo com uma função chamada `teste_args_kwargs()` que recebe três argumentos e imprime cada um deles:
	```python
		def teste_args_kwargs(arg1, arg2, arg3):
		    print("arg1: ", arg1)
		    print("arg2: ", arg2)
		    print("arg3: ", arg3)
	```

1. Agora vamos chamar a função utilizando o **\*args**:
	```python
		args = ('um', 2, 3)
		teste_args_kwargs(*args)
	```

	Que gera a saída:
	```	
		arg1: um
		arg2: 2
		arg3: 3
	```

1. Teste a mesma função usando o **\*\*kwargs**. Para isso, crie um dicionário com três argumentos:
	```python
		kwargs = {'arg3': 3, 'arg2': 'dois', 'arg1': 'um'}
		teste_args_kwargs(**kwargs)
	```
	Que deve gerar a saída:
	```	
		arg1: um
		arg2: dois
		arg3: 3
	```

1. **(Opcional)** Tente chamar a mesma função, mas adicionando um quarto argumento na variável args e kwargs dos exercícios anteriores. O que acontece se a função recebe mais do que 3 argumentos?

1. De que maneira você resolveria o problema do exercício anterior?

	Discuta com o instrutor e seus colegas quando usar **\*args** e **\*\*kwargs**.

## Exercício - Função jogar()

1. Vamos começar definindo uma função jogar que conterá toda lógica do jogo da forca. Abra o arquivo forca.py e coloque o código do jogo em uma função `jogar()`:
	```python
		def jogar():
		    #código do jogo aqui
	```

	Em seguida, chame a função `jogar()` logo abaixo da definição da função no arquivo forca.py:
	```python
		>>> jogar()
		"*********************************"
		"***Bem vindo ao jogo da Forca!***"
		"*********************************"
		['_', '_', '_', '_', '_', '_']
		Qual letra? 
	```
	Agora nosso jogo funciona como esperado.

1. Faça o mesmo com o jogo da adivinhação e execute o jogo.

1. Agora vamos criar um menu para que o usuário possa escolher um jogo (adivinhação ou forca). Crie o arquivo menu.py de modo que a nossa estrutura de arquivos fique assim:
	```
        |_ home
            |_ jogos
                |_ advinhacao.py
                |_ forca.py
				|_ menu.py
	```

	Importe os arquivos de cada jogo dentro do menu.py:
	``` python
		import adivinhacao
		import forca
	```

	Obs: Certifique-se de remover a chamada da função jogar() logo abaixo da definição das mesmas nos arquivos forca.py e adivinhação.py, pois caso estas permaneçam nos arquivos, as chamadas aos jogos serão realizadas automaticamente ao gerar os imports no arquivo menu.py!

	Peça para o usuário escolher uma das opções:
	``` python
		# Importa módulos (código omitido)

		print('*********************************')
		print('**********MENU DE JOGOS**********')
		print('*********************************')
		print("1. Adivinhação")
		print("2. Forca")
		escolha = int(input("Qual jogo quer jogar? Digite o número: "))

		if escolha == 1:
			# Jogar adivinhação
		elif escolha == 2:
			# Jogar forca
	```

	Chame a função `jogar()` do módulo do jogo escolhido:
	``` python
		# Importa módulos e recebe a escolha do usuário (código omitido)

		if escolha == 1:
			adivinhacao.jogar()
		elif escolha == 2:
			forca.jogar()
	```
	Agora toda vez que o usuário quiser jogar um de nossos jogos, ele poderá escolher por meio do menu criado.

## Módulos e o comando import

Ao importar o arquivo forca.py, estamos importando um **módulo** de nosso programa, que nada mais é do que um arquivo. Vamos verificar o tipo de `forca`:

```python
	print(type(forca))
	<class 'module'>

`