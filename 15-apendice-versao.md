# Apêndice - Python2 ou Python3?

Caso você esteja iniciando seus estudos na linguagem Python ou começando um projeto novo, aconselhamos fortemente que você utilize o Python3.

O Python2 vem sendo chamado de Python legado ou Python antigo por boa parte da comunidade que está em constante atividade para fazer a migração da base de código existente (bem grande, por sinal) para Python3.

Aconselhamos a leitura deste artigo para maiores detalhes: https://wiki.python.org/moin/Python2orPython3.

A pergunta correta aqui é: Quando devo usar o Python antigo? E a resposta mais comum que você vai encontrar é: use Python antigo quando você não tiver escolha.

Por exemplo, quando você trabalhar em um projeto antigo e migrar para a nova versão não for uma alternativa no momento. Ou quando você precisa utilizar uma biblioteca que ainda não funciona no Python3 ou não está em processo de mudança. Outro caso é quando seu servidor de hospedagem só permite usar Python2 - aqui o aconselhável é procurar por outro serviço que atenda sua demanda.

No mais, você encontrará muito material sobre o Python2 na internet e aos poucos vai conhecendo melhor as diferenças entre uma versão e outra.

## Quais as diferenças?

Neste artigo você vai encontrar a resposta https://docs.python.org/3/whatsnew/3.0.html. Mas neste capítulo mostramos as diferenças mais básicas e importantes para você iniciar seus estudos.

## A função print()

No Python2 o comando `print` funciona de maneira diferente já que não é uma função. Para imprimir algo fazemos:

 ```python
	# no python2
	>>> print "Hello World!"
	Hello World!
 ```

No Python3 `print` é uma função e utilizamos os parênteses como delimitadores:

 ```python
	# no python3
	>>> print("Hello World!")
	Hello World!
 ```

## A função input()

A função `raw_input` do Python2 foi renomeada para `input()` no Python3:

 ```python
	# no python2
	>>> nome = raw_input("Digite seu nome: ")
 ```

No Python3:

 ```python
	# no python3
	>>> nome = input("Digite seu nome: ")
 ```

## Divisão decimal

No Python2 a divisão entre números decimais é diferente entre um número decimal e um inteiro:

 ```python
	# no python2
	>>> 5 / 2
	2
	>>> 5 / 2.0
	2.5
 ```

No Python3 a divisão tem o mesmo comportamento da matemática. E se quisermos o resultado inteiro da divisão utilizamos `//`:

 ```python
	# no python3
	>>> 5 / 2
	2.5
	>>> 5 // 2
	2
 ```

## Herança

No Python2 suas classes devem herdar de `object`:

 ```python
	# no python2
	>>> class MinhaClasse(object):
		def metodo(self, attr1, attr2):
			return attr1 + attr2
 ```

No Python3 essa herança é implícita, não precisando herdar explicitamente de `object`:

 ```python
	# no python3
	>>> class MinhaClasse():
		def metodo(self, attr1, attr2):
			return attr1 + attr2
 ```
