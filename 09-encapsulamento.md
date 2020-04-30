# Modificadores de acesso e métodos de classe

Um dos problemas mais simples que temos no nosso sistema de contas é que o método `saca()` permite sacar mesmo que o saldo seja insuficiente. A seguir, você pode lembrar como está a classe `Conta`:

```python
class Conta:

    def __init__(self, numero, titular, saldo, limite=1000.0):
        self.numero = numero
        self.titular = titular	
        self.saldo = saldo
        self.limite = limite

    # outros métodos
		
    def saca(self, valor):
        self.saldo -= valor	
```

Vamos testar nosso código:

```python
    minha_conta = Conta('123-4', 'João', 1000.0, 2000.0)
    minha_conta.saca(500000)
```

O limite de saque é ultrapassado. Podemos incluir um `if` dentro do método `saca()` para evitar a situação que resultaria em uma conta em estado inconsistente, com seu saldo menor do que zero. Fizemos isso no capítulo de orientação a objetos básica.
 
Apesar de melhorar bastante, ainda temos um problema mais grave: ninguém garante que o usuário da classe vai sempre utilizar o método para alterar o saldo da conta. O código a seguir altera o saldo diretamente:

```python
    minha_conta = Conta('123-4', 'João', 1000.0)
    minha_conta.saldo = -200
```

Como evitar isso? Uma ideia simples seria testar se não estamos sacando um valor maior que o saldo toda vez que formos alterá-lo.

```python
    minha_conta = Conta('123-4', 'joão', 1000.0)
    novo_saldo = -200

if (novo_saldo < 0):
    print("saldo inválido")
else:
    minha_conta.saldo = novo_saldo
```

Esse código iria se repetir ao longo de toda nossa aplicação, ou pior, alguém pode esquecer de fazer essa comparação em algum momento, deixando a conta em uma situação inconsistente. A melhor forma de resolver isso seria forçar quem usa a classe `Conta` a invocar o método `saca()`, para assim não permitir o acesso direto ao atributo.

Em linguagens como Java e C#, basta declarar que os atributos não possam ser acessados de fora da classe utilizando a palavra chave **private**. Em orientação a objetos, é prática quase que obrigatória proteger seus atributos com **private**. Cada classe é responsável por controlar seus atributos, portanto ela deve julgar se aquele novo valor é válido ou não. E esta validação não deve ser controlada por quem está usando a classe, e sim por ela mesma, centralizando essa responsabilidade e facilitando futuras mudanças no sistema. 

O Python não utiliza o termo **private**, que é um **modificador de acesso** e também chamado de **modificador de visibilidade**. No Python, inserimos dois _underscores_ ('\_\_') ao atributo para adicionarmos esta característica:

```python
class Pessoa:

    def __init__(self, idade):
        self.__idade = idade
```

Dessa maneira, não conseguimos acessar o atributo `idade` de um objeto do tipo `Pessoa` fora da classe:

```python
    pessoa = Pessoa(20)
    pessoa.idade
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Pessoa' object has no attribute 'idade'
```


O interpretador acusa que o atributo `idade` não existe na classe `Pessoa`. Mas isso não garante que ninguém possa acessá-lo. No Python, não existem atributos realmente privados, ele apenas alerta que você não deveria tentar acessar este atributo, ou modificá-lo. Para acessá-lo, fazemos:

```python
    p._Pessoa__idade
```

Ao colocar o prefixo \_\_ no atributo da classe, o Python apenas renomeia '**\_\_nome_do_atributo**' para '**\_nome_da_Classe\_\_nome_do_atributo**', como fez em **\_\_idade** para **\_Pessoa\_\_idade**. Qualquer pessoa que saiba que os atributos privados não são realmente privados, mas "desconfigurados", pode ler e atribuir um valor ao atributo "privado" diretamente. Mas fazer `pessoa._Pessoa__idade = 20` é considerado má prática e pode acarretar em erros.

Podemos utilizar a função `dir` para ver que o atributo `_Pessoa__idade` pertence ao objeto:

```python
    dir(pessoa)
['_Pessoa__idade', '__class__', '__delattr__', '__dict__', '__dir__',
'__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__',
'__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', 
'__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', 
'__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__',
'__weakref__']
```

  

Repare que não existe nenhum atributo `__idade` no objeto pessoa. Agora vamos tentar atribuir um valor para `__idade`:

``` python
    pessoa.__idade = 25
```

Epa, será que o Python deveria deixar isso ocorrer? Vamos acessar a variável novamente e ver se a modificação realmente aconteceu:

``` python
    print(pessoa._Pessoa__idade)
    #20	
```

O que aconteceu aqui é que o Python criou um novo atributo **\_\_idade** para o objeto `pessoa` já que é uma linguagem dinâmica. Vamos utilizar a função `dir` novamente para ver isso:

```python
    dir(pessoa)
['_Pessoa__idade', '__class__', '__delattr__', '__dict__', '__dir__',
'__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__',
'__hash__', '__idade', '__init__', '__init_subclass__', '__le__', '__lt_'
'__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__',
'__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__',
'__weakref__']
```


Note que um novo atributo `__idade` apareceu, já que foi inicializado em tempo de execução e é diferente do `__idade` da classe. Isso pode gerar muita confusão e erros! O Python também tem uma maneira de lidar com este problema através da variável **\_\_slots\_\_**, onde definimos um número limitado de atributos que veremos a seguir.

Nenhum atributo é realmente privado em Python, já que podemos acessá-lo pelo seu nome 'desfigurado'. Muitos programadores Python não gostam dessa sintaxe e preferem usar apenas um _underscore_ '\_' para indicar quando um atributo deve ser protegido. Ou seja, deve ser explícita essa desconfiguração do nome - feita pelo programador e não pelo interpretador - já que oferece o mesmo resultado. E argumentam que '\_\_' são obscuros.

O prefixo com apenas um _underscore_ não tem significado para o interpretador quando usado em nome de atributos, mas entre programadores Python é uma convenção que deve ser respeitada. O programador alerta que esse atributo não deve ser acessado diretamente:

```python
def __init__(self, idade):
    self._idade = idade
```

Um atributo com apenas um underscore é chamado de protegido, mas quando usado **sinaliza** que deve ser tratado como um atributo "privado", fazendo com que acessá-lo diretamente possa ser perigoso. 

As mesmas regras de acesso aos atributos valem para os métodos. É muito comum, e faz todo sentido, que seus atributos sejam privados e quase todos seus métodos sejam públicos (não é uma regra!). Desta forma, toda conversa de um objeto com outro é feita por troca de mensagens, isto é, acessando seus métodos. Algo muito mais educado que mexer diretamente em um atributo que não é seu.

Melhor ainda! O dia que precisarmos mudar como é realizado um saque na nossa classe `Conta`, adivinhe onde precisaríamos modificar? Apenas no método `saca()`, o que faz pleno sentido.

## Encapsulamento

O que começamos a ver nesse capítulo é a ideia de encapsular, isto é, 'esconder' todos os membros de uma classe (como vimos acima), além de esconder como funcionam as rotinas (no caso métodos) do nosso sistema.

Encapsular é fundamental para que seu sistema seja suscetível a mudanças: não precisamos mudar uma regra de negócio em vários lugares, mas sim em apenas um único lugar, já que essa regra está encapsulada. O conjunto de métodos públicos de uma classe é também chamado de **interface da classe**, pois esta é a única maneira a qual você se comunica com objetos dessa classe.

O _underscore_ \_ alerta que ninguém deve modificar, nem mesmo ler, o atributo em questão. Com isso, temos um problema: como fazer para mostrar o saldo de uma `Conta`, já que não devemos acessá-lo para leitura diretamente?

Precisamos então arranjar uma maneira de fazer esse acesso. Sempre que precisamos arrumar uma maneira de fazer alguma coisa com um objeto, utilizamos métodos! Vamos então criar um método, digamos `pega_saldo()`, para realizar essa simples tarefa:

```python
class Conta:

    # outros métodos
    
    def pega_saldo(self):
        return self._saldo
```

Para acessarmos o saldo de uma conta, podemos fazer:

```python
    minha_conta = Conta('123-4', 'joão', 1000.0)
    minha_conta.deposita(100)
    minha_conta.pega_saldo()
1100
```

Para permitir o acesso aos atributos (já que eles são 'protegidos') de uma maneira controlada, a prática mais comum é criar dois métodos, um que retorna o valor e outro que muda o valor. A convenção para esses métodos em muitas linguagens orientadas a objetos é colocar a palavra **get** ou **set** antes do nome do atributo. Por exemplo, uma conta com saldo e titular fica assim, no caso de desejarmos dar acesso a leitura e escrita a todos os atributos:

```python
class Conta:

    def __init__(self, titular, saldo):
        self._titular = titular
        self._saldo = saldo

    def get_saldo(self):
        return self._saldo

    def set_saldo(self, saldo):
        self._saldo = saldo		

    def get_titular(self):
        return self._titular

    def set_titular(self, titular):
        self._titular = titular	
```

*Getters* e *setters* são usados ​​em muitas linguagens de programação orientada a objetos para garantir o princípio do encapsulamento de dados. O encapsulamento de dados é visto como o agrupamento de dados com os métodos que operam nesses dados. Esses métodos são, obviamente, o *getter* para recuperar os dados e o *setter* para alterar os dados. De acordo com esse princípio, os atributos de uma classe são tornados privados para ocultá-los e protegê-los de outro código.

Infelizmente, é uma crença generalizada que uma classe Python adequada deve encapsular atributos privados usando *getters* e *setters*. Assim que um desses programadores introduzirem um novo atributo, ele fará com que seja uma variável privada e criará "automaticamente" um *getter* e um *setter* para esses atributos. 

Os programadores de Java irão torcer o nariz quando lerem o seguinte: A maneira *pythônica* de introduzir atributos é torná-los públicos. Vamos explicar isso mais tarde. Primeiro, demonstramos no exemplo a seguir, como podemos projetar uma classe, da mesma maneira usada no Java, com *getters* e *setters* para encapsular um atributo protegido: 

```python
class Conta:

    def __init__(self, saldo):
        self._saldo = saldo

    def get_saldo(self):
        return self._saldo

    def set_saldo(self, saldo):
        self._saldo = saldo
```

E podemos ver como trabalhar com essa classe e os métodos:

```python
    conta1 = Conta(200.0)
    conta2 = Conta(300.0)
    conta3 = Conta(-100.0)
    conta1.get_saldo()
    #200.0
    conta2.get_saldo()
    #300.0
    conta3.set_saldo(conta1.get_saldo() + conta2.get_saldo())
        conta3.get_saldo()
    #500.0
```

O que você acha da expressão "conta3.set_saldo(conta1.get_saldo() + conta2.get_saldo())"? É feio, não é? É muito mais fácil escrever uma expressão como a seguinte:

```python
conta3.saldo = conta1.saldo + conta2.saldo
```

Tal atribuição é mais fácil de escrever e, acima de tudo, mais fácil de ler do que a expressão com _getters_ e _setters_. Vamos reescrever a classe `Conta` de um modo Pythônico, sem _getter_ e sem _setter_:

```python
class Conta:

    def __init __(self, saldo):
        self.saldo = saldo
```

Neste caso, não há encapsulamento e não seria um problema. Mas o que acontece se quisermos mudar a implementação no futuro? O leitor atento deve ter reparado que no exemplo anterior declaramos uma variável do tipo `Conta` com saldo negativo, e isso não deveria acontecer. Para evitarmos essa situação, o *setter* é justificado para acrescentar a seguinte validação:

```python
class Conta:

    def __init __(self, saldo):
        self.saldo = saldo

    def set_saldo(self, saldo):
        if(saldo < 0):
            print("saldo não pode ser negativo")
        else:
	        self.saldo = saldo
```

Podemos testar isso com:

```python
    conta1 = Conta(200.0)
    print(conta1.saldo)
    #200.0
    conta2 = Conta(300.0)
    print(conta2.saldo)
    #300.0
    conta3 = Conta(100.0)
    conta3.set_saldo(-100.0)
    "saldo não pode ser negativo"
    print(conta3.saldo)
    #100.0
```

Mas há um problema, pois caso projetemos nossa classe com atributo público e sem métodos, acabamos quebrando a interface:

```python
conta1 = Conta(100.0)
conta1.saldo = -100.0
```

É por isso que em Java recomenda-se que as pessoas usem somente atributos privados com _getters_ e _setters_, para que seja possível alterar a implementação sem precisar alterar a interface. O Python oferece uma solução bastante parecida para este problema. A solução é chamada de **properties**. Mantemos nossos atributos protegidos e decoramos nossos métodos com um *decorator* chamado _property_.

A classe com uma propriedade fica assim:

```python
class Conta:

    def __init__(self, saldo=0.0):
        self._saldo = saldo

    @property
    def saldo(self):
        return self._saldo	

    @saldo.setter
    def saldo(self, saldo):
        if(saldo < 0):
            print("saldo não pode ser negativo")
        else:
            self._saldo = saldo
```

Um método que é usado para obter um valor (o _getter_) é decorado com `@property`, isto é, colocamos essa linha diretamente acima da declaração do método que recebe o nome do próprio atributo. O método que tem que funcionar como _setter_ é decorado com `@saldo.setter`. Se a função tivesse sido chamada de "func", teríamos que anotá-la com `@func.setter`.

> **para saber mais: decorator**
> 
> Um decorador, ou **decorator** é um padrão de projeto de software que permite adicionar um comportamento a um objeto já existente em tempo de execução, ou seja, agrega dinamicamente responsabilidades adicionais a um objeto. Esta solução traz uma flexibilidade maior, em que podemos adicionar ou remover responsabilidades sem que seja necessário editar o código-fonte. 
>
>Um decorador é um objeto invocável, uma função que aceita outra função como parâmetro (a função decorada). O decorador pode realizar algum processamento com a função decorada e devolvê-la ou substituí-la por outra função. O *property* é um decorador que possui métodos extras, como um *getter* e um *setter*, e ao ser aplicado a um objeto retorna uma cópia dele com essas funcionalidades:

``` python
@property
def foo(self):
    return self._foo
```

é equivalente a:

``` python
def foo(self)
    return self._foo
foo = property(foo)
```

Portanto, a função `foo()` é substituída pela propriedade `property(foo)`. Então, se você usa `@foo.setter()`, o que você está fazendo é chamar o método `property().setter`

Desta maneira, podemos chamar esses métodos sem os parênteses, como se fossem atributos públicos. É uma forma mais elegante de encapsular nossos atributos. Vamos testar criar uma conta e depois atribuir um valor negativo ao saldo:

```python
    conta = Conta(1000.0)
    conta.saldo = -300.0
"saldo não pode ser negativo"
```

Veja que temos um resultado muito melhor do que usar _getters_ and _setters_ diretamente. Chamamos o atributo pela suas propriedades, que podem conter validações, e nossos atributos estão sinalizados como 'protegidos' através do '\_'.

Ainda podemos modificar o saldo, e isto deveria ser feito através dos métodos públicos `saca()` e `deposita()`. Então, a necessidade de um `@saldo.setter` é questionável. Devemos apenas manipular o saldo através dos métodos `saca()` e `deposita()`, não precisamos da *property* `saldo.setter`. Isso é uma decisão de negócio específico. O programador deve ficar alerta quanto as propriedades _setters_ de seus atributos, nem sempre elas são necessárias.

É uma má prática criar uma classe e, logo em seguida, criar as propriedades para todos seus atributos. Você só deve criar _properties_ se tiver real necessidade. Repare que nesse exemplo, a propriedade _setter_ do saldo não deveria ter sido criada, já que queremos que todos usem os métodos `deposita()` e `saca()`.

## Atributos de classe

Nosso banco também quer controlar a quantidade de contas existentes no sistema. Como poderíamos fazer isso? Bom, a cada instância criada, deveríamos incrementar esse total:

``` python
total_contas = 0
conta = Conta(300.0)
total_contas = total_contas + 1
conta2 = Conta(100.0)
total_contas = total_contas + 1
```

Aqui volta o problema de repetir um mesmo código para toda aplicação, além de ter que lembrar de incrementar a variável `total_contas` toda vez após instanciar uma `Conta`. Como `total_contas` tem vínculo com a classe `Conta`, ele deve ser um atributo controlado pela **classe**, que deve incrementá-lo toda vez que instanciamos um objeto, ou seja, quando chamamos o método `__init__()`:

``` python
class Conta:

    def __init__(self, saldo):
        self._saldo = saldo
        self._total_contas = self._total_contas + 1
```

Mas onde inicializamos a variável `_total_contas`? Não faz sentido recebermos por parâmetro no `__init__()`, já que é a classe que deve controlar esse número e não o objeto. Seria interessante que essa variável fosse própria da classe, sendo única e compartilhada por todos os objetos da mesma. Dessa maneira, quando a mesma fosse mudada através de um objeto, o outro enxergaria o mesmo valor. Para fazer isso, vamos inicializar a variável na classe, portanto, fora do método `__init__()`:

```python
class Conta:

    total_contas = 0

    def __init__(self, saldo):
        self._saldo = saldo
        self.total_contas += 1
```

Veja que `saldo` é um **atributo de instância**, e `total_contas` um **atributo de classe**. Vamos fazer um teste para ver se nosso `total_contas` funciona como esperado:

``` python
    c1 = Conta(100.0)
    print(c1.total_contas)
    #1
    c2 = Conta(200.0)
    print(c2.total_contas)
    #1
```

Criamos duas instâncias, e mesmo assim o `total_contas` não mudou. Isso acontece por conta do `self.total_contas` ser diferente de `total_contas` da classe. Como `total_contas` é uma variável da classe, devemos chamá-la pela classe:

``` python
class Conta:

    total_contas = 0

    def __init__(self, saldo):
        self._saldo = saldo
        Conta.total_contas += 1
```

E testamos:

```python
	    c1 = Conta(100.0)
	    print(c1.total_contas)
	    #1
	    c2 = Conta(200.0)
	    print(c2.total_contas)
	    #2

```

  

Agora obtemos o resultado esperado. Também é possível acessar este atributo direto da classe:

``` python
    Conta.total_contas
2
```

Mas não queremos que ninguém venha a acessar nosso atributo `total_contas` e modificá-lo. Portanto vamos torná-lo 'protegido' acrescentando um '\_':

``` python
class Conta:

    _total_contas = 0
```

Dessa maneira avisamos os usuários de nossa classe que esse atributo deve ser considerado 'privado' e não modificado. Mas como acessá-lo então? Veja que agora, ao acessar pela classe obtemos um erro:

``` python
    Conta.total_contas
Traceback (most recent call last):
  File <stdin>, line 23, in <module>
	Conta.total_contas
AttributeError: 'Conta' object has no attribute 'total_contas'
```

Precisamos criar um método para acessar o atributo. Vamos criar o `get_total_contas`:

``` python
class Conta:

    _total_contas = 0
	
    # __init__ e outros métodos

    def get_total_contas(self):
        return Conta._total_contas
```

Funciona quando chamamos este método por um instância, mas quando fazemos `Conta.get_total_contas()` o interpretador reclama, pois não passamos a instância:

``` python
    c1 = Conta(100.0)
    print(c1.get_total_contas())
    #1
    c2 = Conta(200.0)
    print(c2.get_total_contas())
    #2
    Conta.get_total_contas()
Traceback (most recent call last):
  File <stdin>, line 17, in <module>
    Conta.get_total_contas()
TypeError: get_total_contas() missing 1 required positional argument: 'self'
```

Veja que o erro avisa que falta passar o argumento `self`. Não podemos chamá-lo, pois ele não está vinculado a qualquer instância de `Conta`. Além disso, um método requer uma instância como seu primeiro argumento:

```python
    c1 = Conta(100.0)
    c2 = Conta(200.0)
    Conta.get_total_contas(c1)
    #2
```

Passamos a instância `c1` de `Conta` e funcionou. Mas essa não é a melhor maneira de se chamar um método. A chamada não é clara e leva um tempo para ler e entender o que a terceira linha desse código realmente faz. Vamos então deixar de passar o 'self' como argumento de `get_total_contas`:

```python
def get_total_contas():
    return Conta._total_contas
```

Mas dessa maneira não conseguimos acessar o método, já que todo método exige o argumento `self`:

``` python
    c1 = Conta(100.0)
    c1.get_total_contas()
Traceback (most recent call last):
  File <stdin> in <module>
    c1.get_total_contas()
TypeError: get_total_contas() takes 0 positional arguments but 1 was given
```

  

E agora, o que fazer? Queremos um método que seja chamado via classe e via instância sem a necessidade de passar a referência deste objeto. O Python resolve isso usando **métodos estáticos**.

Métodos estáticos não precisam de uma referência, não recebem um primeiro argumento especial (self). É como uma função simples que, por acaso, reside no corpo de uma classe em vez de ser definida no nível do módulo. 

Para que um método seja considerado estático, basta adicionarmos um decorador, assim como fizemos com as propriedades no capítulo anterior. O decorador se chama _@staticmethod_:

``` python
@staticmethod
def get_total_contas():
    return Conta._total_contas
```

Testando, vemos que funciona tanto chamado por um instância quanto pela classe:

``` python
    c1 = Conta(100.0)
    c1.get_total_contas()
    #1
    c2 = Conta(200.0)
    c2.get_total_contas()
    #2
    Conta.get_total_contas()
    #2
```

## Métodos de classe

Métodos estáticos não devem ser confundidos com métodos de classe. Como os métodos estáticos, métodos de classe não são ligados às instâncias, mas sim a classe. O primeiro parâmetro de um método de classe é uma referência para a classe, isto é, um objeto do tipo _class_, que por convenção nomeamos como 'cls'. Eles podem ser chamados via instância ou pela classe e utilizam um outro decorador, o _@classmethod_:

``` python
class Conta:

    _total_contas = 0

    def __init__(self):
	    type(self)._total_contas += 1

    @classmethod
    def get_total_contas(cls):
	    return cls._total_contas
```

E podemos testar:

``` python
    c1 = Conta(100.0)
    c1.get_total_contas()
    #1
    c2 = Conta(200.0)
    c2.get_total_contas()
    #2
    Conta.get_total_contas()
    #2
```

No início pode parecer confuso qual usar: `@staticmethod` ou `@classmethod`? Isso não é trivial. Métodos de classe servem para definir um método que opera na classe, e não em instâncias. Já os métodos estáticos utilizamos quando não precisamos receber a referência de um objeto especial (seja da classe ou de uma instância) e funciona como uma função comum, sem relação.

Isso ficará mais claro quando avançarmos no aprendizado. No próximo capítulo discutiremos Herança, um conceito fundamental em Orientação a Objetos. Veremos que classes podem ter filhas e aproveitar o código das classes mães. Um método de classe pode mudar a implementação, ou seja, pode ser reescrito pela classe filha. Já os métodos estáticos não podem ser reescritos pelas filhas, já que são imutáveis e não dependem de um referência especial.

> **@classmethod x @staticmethod**
> 
>Alguns programadores não veem muito sentido em usar métodos estáticos, já que se você escrever uma função que não vai interagir com a classe, basta defini-la no módulo. Outros já contra argumentam em outra via, considerando herança de classes que veremos em outro capítulo. Indicamos a leitura do artigo '_The Definitive Guide on How to Use Static, Class and Abstract Methods in Python_' de Julien Danjou que pode ser acessado pelo link: https://julien.danjou.info/guide-python-static-class-abstract-methods/ .
>

## Para saber mais - Slots

Aprendemos sobre encapsulamento, e vimos que é uma boa prática proteger nossos atributos incluindo o prefixo _underscore_ em seus nomes, seguindo a convenção utilizada pelos programadores. Além disso, utilizamos _properties_ para acessar e modificar nossos atributos. Mas como Python é uma linguagem dinâmica, nada impede que usuários de nossa classe `Conta` criem atributos em tempo de execução, fazendo, por exemplo:

``` python
>>> conta.nome = "minha conta"
```

Esse código não acusa erro e nossa conta fica aberta a modificações ferindo a segurança da classe. Para evitar isso, podemos utilizar uma variável embutida no Python chamada `__slots__`, que pode guardar uma lista de atributos da classe definidos por nós:

```python
class Conta:

    __slots__ = ['_numero', '_titular', '_saldo', '_limite']

    def __init__(self, numero, titular, saldo, limite=1000.0):
        # inicialização dos atributos

    # código omitido	
```

Agora, quando tentamos adicionar um atributo na classe, recebemos um erro:

``` python
>>> conta.nome = "minha_conta"
Traceback (most recent call last):
	File <stdin>, line 1, in <module>
AttributeError: 'Conta' object has no attribute '__dict__'   
```

```python
class Conta:

    __slots__ = ['_numero', '_titular', '_saldo', '_limite']

    def __init__(self, numero, titular, saldo, limite=1000.0):
        self._numero = numero
        self._titular = titular
        self._saldo = saldo
        self._limite = limite

    # restante do código

conta.nome = "minha_conta"
```

Repare que o erro acusa que a classe `Conta` não possui o atributo `__dict__`. Ao atribuir um valor para `__slots__`, o interpretador do Python vai entender que queremos excluir o `__dict__` da classe `Conta` não sendo possível criar atributos, ou seja, impossibilitando adicionar atributos ao dicionário da classe que é responsável por armazenar atributos de instância. Portanto, tentar chamar `vars(conta)` também vai gerar um erro:

``` python
>>> vars(conta)
Traceback (most recent call last):
  File <stdin>, line 1, in <module>
TypeError: vars() argument must have __dict__ attribute
```

Embora `__slots__` seja muito utilizado para não permitir que usuários de nossas classes criem outros atributos, essa não é sua principal função nem o motivo de sua existência. O que acontece é que o `__dict__` desperdiça muita memória. Imagine um sistema grande, com milhões de instâncias de `Conta` - teríamos, consequentemente, milhões de dicionários de classe armazenando seus atributos de instância. O Python não pode simplesmente alocar uma quantidade estática de memória na criação de objetos para armazenar todos os atributos. Por isso, consome muita memória RAM se você criar muitos objetos.

Para contornar este problema é que se usa o `__slots__`, e este é seu principal propósito. O `__slots__` avisa o Python para não usar um dicionário e apenas alocar espaço para um conjunto fixo de atributos.  

Programadores viram uma redução de quase 40 a 50% no uso de RAM usando essa técnica.

## Exercícios:

1. Adicione o modificador de visibilidade privado (dois _underscores_: `__`) para cada atributo e método da sua classe `Conta`.

    ```python
        class Conta:

            def __init__(self, numero, titular, saldo, limite=1000.0):
                self.__numero = numero
                self.__titular = titular
                self.__saldo = saldo
                self.__limite = limite
    ```

    Tente criar uma `Conta` e modificar ou ler um de seus atributos "privados". O que acontece?

1. Sabendo que no Python não existem atributos privados, como podemos modificar e ler esses atributos? É uma boa prática fazer isso?

    Dica: teste os comandos ```print(conta.__numero)``` e ```print(conta._Conta__numero```. O que ocorre?

1. Modifique o acesso para 'protegido' seguindo a convenção do Python e modifique o prefixo `__` por apenas um _underscore_ `_`. Crie métodos de acesso em sua classe `Conta` através do _decorator_ `@property`.

    ```python
        class Conta:

            def __init__(self, numero, titular, saldo, limite=1000.0):
                self._numero = numero
                self._titular = titular
                self._saldo = saldo
                self._limite = limite

            @property
            def saldo(self):
                return self._saldo	

            @saldo.setter
            def saldo(self, saldo):
                if (saldo < 0):
                    print("saldo não pode ser negativo")
                else:
                    self._saldo = saldo

            #restante dos métodos escritos no exercício anterior
    ```

1. Crie novamente uma conta e acesse e modifique seus atributos. O que mudou?

    Dica: tente os comandos na seguinte ordem: ```print(conta._numero)``` , ```conta._numero= '50'``` e ```print(conta._numero)```. O que ocorre?

1. Modifique sua classe `Conta` de modo que não seja permitido criar outros atributos além dos definidos anteriormente utilizando `__slots__`.

    ```python
        class Conta:

            __slots__ = ['_numero', '_titular', '_saldo', '_limite']

            def __init__(self, numero, titular, saldo, limite=1000.0):
                self._numero = numero
                self._titular = titular
                self._saldo = saldo
                self._limite = limite
    ```

1. (Opcional) Adicione um atributo `identificador` na classe `Conta`. Esse identificador deve ter um valor único para cada instância do tipo `Conta`. A primeira Conta instanciada tem identificador 1, a segunda 2, e assim por diante.

    ```python
        class Conta:

            identificador = 1

            def __init__(self, numero, titular, saldo, limite=1000.0):
                # código omitido
                self.identificador = Conta.identificador
                Conta.identificador += 1
    ```
