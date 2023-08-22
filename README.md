# Estudos sobre padrões de projetos

Este é apenas o resumo dos meus estudos iniciais sobre padrões de projeto. Os exemplos serão feitos em Python (estou aprendendo :)). Serão abordados todos os padrões descritos no livro; porém, haverá exemplos em Python apenas daqueles que julguei serem mais impactantes e mais comumente usados no meu dia a dia como programador em Java/JavaScript. As principais fontes de estudo foram [DESIGN PATTERNS -refactoring-guru](https://refactoring.guru/design-patterns) e 'Design Patterns: Elements of Reusable Object-Oriented Software'. Durante o meu estudo, pretendo visitar outros sites e vídeos.

## Introdução

### Padrões Criacionais
Os padrões criacionais se concentram em técnicas de criação de objetos, garantindo que os objetos sejam criados de maneira adequada para a situação.

1. **Singleton**
2. **[Factory Method](#factory-method)**
3. **[Abstract Factory](#abstract-factory)**
4. **[Builder](#builder)**
5. **Prototype**

### Padrões Estruturais
Os padrões estruturais se concentram em como os objetos e classes são combinados para formar estruturas maiores.

1. **[Adapter](#adapter)**
2. **[Bridge](#bridge)**
3. **[Composite](#composite)**
4. **[Decorator](#decorator)**
5. **[Facade](#facade)**
6. **[Flyweight](#flyweight)**
7. **[Proxy](#proxy)**

### Padrões Comportamentais
Os padrões comportamentais se concentram na comunicação entre objetos.

1. **[Chain of Responsibility](#chain-of-responsibility)**
2. **[Command](#command)**
3. **[Interpreter](#interpreter)**
4. **[Iterator](#iterator)**
5. **[Mediator](#mediator)**
6. **[Memento](#memento)**
7. **[Observer](#observer)**
8. **[State](#state)**
9. **[Strategy](#strategy)**
10. **[Template Method](#template-method)**
11. **[Visitor](#visitor)**


## Explorando mais detalhadamente os padrões de projetos.

### Padrões Criacionais


#### Factory Method

#### Definição
O Factory Method é um padrão de design criacional que fornece uma interface para criar objetos, mas permite que as subclasses alterem o tipo de objetos que serão criados.

#### Intenção
A intenção principal do padrão Factory Method é definir uma interface para criar um objeto, mas deixar as subclasses decidirem qual classe instanciar. O Factory Method permite adiar a instanciação para as subclasses.

#### Estrutura
O padrão Factory Method envolve as seguintes componentes principais:
- **Creator**: Classe abstrata que declara o método de fábrica.
- **ConcreteCreator**: Classe concreta que implementa o método de fábrica e retorna uma instância de ConcreteProduct.
- **Product**: Define a interface dos objetos que o método de fábrica cria.
- **ConcreteProduct**: Implementa a interface Product e é o objeto real que o método de fábrica cria.

#### Como Funciona
1. A classe Creator declara o método de fábrica que retorna um objeto do tipo Product.
2. As subclasses ConcreteCreator implementam esse método para produzir objetos que se conformam com a interface Product.
3. O código que usa o Creator nunca precisa saber a classe concreta do objeto que está sendo criado.

#### Vantagens
- **Flexibilidade**: Permite introduzir novas classes concretas sem alterar o código existente.
- **Desacoplamento**: O código cliente não depende das classes concretas, já que a criação de objetos é feita através de uma interface comum.

#### Exemplo
Imagine uma aplicação de desenho que pode criar diferentes tipos de formas, como círculos, quadrados, etc. Usando o Factory Method, você pode definir uma interface comum para criar uma forma e, em seguida, implementar subclasses específicas para cada tipo de forma. Isso permite adicionar novas formas à aplicação sem modificar o código existente.

#### Conclusão
O padrão Factory Method é uma ferramenta poderosa para promover a organização, flexibilidade e extensibilidade do código. É amplamente utilizado em bibliotecas e frameworks onde a implementação é esperada para ser estendida por aplicações cliente.

#### Exemplo

```python
# Exemplo do padrão Factory Method
# Descrição do problema - suponha que você tem varias entidades que podem ter dados persistidos em um banco de dados
# porém algum dados são persistidos com alguma inconsistência, por exemplo, o campo que deveria estar preenchido e não está
# ou com a mudança de um regra de negocio o campo que deveria ser preenchido com um valor passa a ser preenchido com outro algum similar
# um  health check é uma forma de verificar se os dados estão consistentes, porém cada entidade tem sua regra de negocio para verificar


from abc import ABC, abstractmethod

# metodo abstrato para que os serviços implementem o health check

class HealthCheck(ABC):
	@abstractmethod
	def health_check(self):
		pass

# classe abstrata que implementa a criação das mensagens de erro do health check

class CreatorMessage(ABC):
	@abstractmethod
	def create_message(self):# factory_method
		pass

# classe abstrata que implementa a criação dos serviços

class CreatorMessageServiceA(CreatorMessage):
	def create_message(self):
		print ("Service A message errors")

class CreatorMessageServiceB(CreatorMessage):
	def create_message(self):
		print ("Service B message errors")

class CreatorMessageServiceC(CreatorMessage):
	def create_message(self):
		print ("Service C message errors")

class ServiceA(HealthCheck):
	def health_check(self, creator_message: CreatorMessage):
		print("Service A health check")
		creator_message.create_message()

class ServiceB(HealthCheck):
	def health_check(self, creator_message: CreatorMessage):
		print("Service B health check")
		creator_message.create_message()

class ServiceC(HealthCheck):
	def health_check(self, creator_message: CreatorMessage):
		print("Service C health check")
		creator_message.create_message()


if __name__ == "__main__":
	service_a = ServiceA()
	service_b = ServiceB()
	service_c = ServiceC()

	creator_message_service_a = CreatorMessageServiceA()
	creator_message_service_b = CreatorMessageServiceB()
	creator_message_service_c = CreatorMessageServiceC()

	service_a.health_check(creator_message_service_a)
	service_b.health_check(creator_message_service_b)
	service_c.health_check(creator_message_service_c)

########################## Saida ###########################################
# Service A health check
# Service A message errors
# Service B health check
# Service B message errors
# Service C health check
# Service C message errors

```

#### Implementação do Factory Method
O padrão Factory Method é implementado através da interface **CreatorMessage** e suas subclasses concretas. A interface define um método abstrato **create_message**, que é o método de fábrica. As subclasses concretas implementam esse método para criar mensagens de erro específicas para cada serviço.

Os serviços (ServiceA/B/C) dependem da interface **CreatorMessage**, e não das classes concretas. Isso permite que os serviços sejam estendidos e modificados independentemente das regras de criação de mensagens de erro.
##### Referencia
[Factory Method](https://refactoring.guru/design-patterns/factory-method)
[Factory Method - python  ](https://refactoring.guru/design-patterns/factory-method/python/example)
[Um Programador Pleno já deveria saber usar esse Design Pattern (tutorial linha a linha)](https://youtu.be/arAz2Ff8s88)
[Combinação Extremamente Poderosa Para Qualquer Programador (Factory + Injeção de Dependência)](https://youtu.be/uyOJ2jjBtBs)

### Abstract Factory



##### Referencia
[Entenda DEFINITIVAMENTE o padrão Abstract Factory do GOF](https://youtu.be/_EcV-BcJ2-E)
[Abstract Factory Teoria - Padrões de Projeto - Parte 12/45](https://youtu.be/UPSuHqNsNs4)

### Padrões Estruturais

## Padrões Comportamentais

### strategy

#### O que é o Strategy Pattern?

O **Strategy Pattern** define uma família de algoritmos, encapsula cada um deles e os torna intercambiáveis. O padrão permite que o algoritmo varie independentemente dos clientes que o utilizam. Em outras palavras, ele permite que você separe um conjunto de algoritmos da classe que os utiliza, para que a classe e os algoritmos possam variar independentemente.

#### Componentes principais do Strategy Pattern:

1. **Strategy (Estratégia)**: Esta é uma interface ou uma classe abstrata usada para definir uma família de algoritmos.
2. **Concrete Strategies (Estratégias Concretas)**: Estas são as implementações específicas da estratégia. Cada uma implementa um algoritmo diferente.
3. **Context (Contexto)**: O Contexto mantém uma referência a uma estratégia e pode alternar entre diferentes estratégias. Ele usa a estratégia para executar uma operação.

#### Por que usar o Strategy Pattern?

1. **Flexibilidade**: O Strategy Pattern permite que você defina uma família de algoritmos e os torne intercambiáveis. Isso significa que você pode alterar o comportamento de um programa em tempo de execução, escolhendo diferentes estratégias.
2. **Separação de Responsabilidades**: O padrão separa o código que usa o algoritmo do código que implementa o algoritmo. Isso torna o código mais modular e fácil de estender.
3. **Evita Condições Múltiplas**: Em vez de usar várias condicionais para escolher qual algoritmo usar, você pode encapsular o algoritmo em uma estratégia e simplesmente mudar a estratégia.

#### Exemplo

### Exemplo: Calculadora de Descontos

```python
from abc import ABC, abstractmethod

# Estratégia abstrata
class DiscountStrategy(ABC):

    @abstractmethod
    def apply_discount(self, price: float) -> float:
        pass

# Estratégia concreta 1: Desconto fixo de $10
class FixedDiscount(DiscountStrategy):

    def apply_discount(self, price: float) -> float:
        return price - 10

# Estratégia concreta 2: Desconto de 10%
class PercentageDiscount(DiscountStrategy):

    def apply_discount(self, price: float) -> float:
        return price * 0.9

# Contexto
class Product:

    def __init__(self, name: str, price: float, discount_strategy: DiscountStrategy):
        self.name = name
        self.price = price
        self.discount_strategy = discount_strategy

    def get_discounted_price(self) -> float:
        return self.discount_strategy.apply_discount(self.price)

# Testando
product1 = Product("Camiseta", 50, FixedDiscount())
print(f"Preço com desconto para {product1.name}: ${product1.get_discounted_price()}")

product2 = Product("Tênis", 100, PercentageDiscount())
print(f"Preço com desconto para {product2.name}: ${product2.get_discounted_price()}")


```

Imagine que você está desenvolvendo um sistema de e-commerce e deseja aplicar diferentes estratégias de desconto aos produtos. Algumas estratégias podem oferecer um desconto fixo, outras um desconto percentual, e assim por diante.

1. **DiscountStrategy (Estratégia de Desconto)**: Esta é a estratégia abstrata que define o método `apply_discount`, que todas as estratégias concretas devem implementar.

2. **FixedDiscount e PercentageDiscount (Descontos Fixo e Percentual)**: Estas são as estratégias concretas que fornecem implementações específicas para o método `apply_discount`. `FixedDiscount` subtrai um valor fixo do preço, enquanto `PercentageDiscount` aplica um desconto percentual.

3. **Product (Produto)**: Esta é a classe contexto. Ela representa um produto que tem um nome, um preço e uma estratégia de desconto. A classe `Product` usa a estratégia de desconto para calcular o preço com desconto através do método `get_discounted_price`.

Ao usar o **Strategy Pattern** neste exemplo, você pode facilmente adicionar novas estratégias de desconto no futuro sem modificar o código existente. Por exemplo, se você quiser adicionar uma estratégia de "compre um, leve o segundo pela metade do preço", basta criar uma nova classe que implemente `DiscountStrategy` e aplicá-la ao produto desejado.


[Strategy](https://refactoring.guru/design-patterns/strategy)
[Identifique Quando e Como Usar o Design Pattern Strategy na Prática](https://youtu.be/WPdrnuSHAQs)
[Design Pattern Strategy: Entendendo na Prática](https://youtu.be/pxmqkzWPW6E)

### Chain of Responsibility

O padrão Chain of Responsibility permite que uma solicitação seja passada ao longo de uma cadeia de potenciais manipuladores até que um deles lide com a solicitação.

#### Componentes principais:
- **Handler (Manipulador):** Interface que define os métodos que todos os handlers devem implementar.
- **AbstractHandler:** Classe concreta que implementa a lógica padrão para encadear os handlers e passar a solicitação ao longo da cadeia.
- **ConcreteHandlers (Manipuladores Concretos):** Classes que implementam a lógica específica para lidar com as solicitações.

#### Funcionamento:
1. Uma solicitação é passada para o primeiro handler da cadeia.
2. Se o handler pode processar a solicitação, ele faz isso e a cadeia termina.
3. Se o handler não pode processar a solicitação, ele passa para o próximo handler na cadeia.
4. O processo se repete até que um handler processe a solicitação ou até que todos os handlers tenham sido tentados.

#### Exemplo
Imagine um sistema de suporte técnico onde diferentes níveis de suporte lidam com problemas de diferentes complexidades. Se um nível não pode resolver o problema, ele é passado para o próximo nível. No código fornecido, temos três níveis de suporte (`SuporteNivel1`, `SuporteNivel2` e `SuporteNivel3`) que tentam resolver problemas de diferentes complexidades.

```python
from abc import ABC, abstractmethod


# Interface Handler (AbstractHandler)
class Handler(ABC):
    @abstractmethod
    def set_proximo(self, handler):
        pass

    @abstractmethod
    def lidar_com_solicitacao(self, request):
        pass

# Classe concreta que implementa comportamentos padrões entre os handlers
class AbstractHandler(Handler):
    _proximo_handler = None

    def set_proximo(self, handler):
        self._proximo_handler = handler
        # Retornando handler para permitir encadeamento
        return handler

    def lidar_com_solicitacao(self, request):
        if self._proximo_handler:
            return self._proximo_handler.lidar_com_solicitacao(request)
        return "Não foi possível resolver o problema. :´("

# ConcreteHandlers
class SuporteNivel1(AbstractHandler):
    def lidar_com_solicitacao(self, request):
        if request == "Problema Nível 1":
            return f"Suporte Nível 1 resolveu o {request}"
        else:
            return super().lidar_com_solicitacao(request)

class SuporteNivel2(AbstractHandler):
    def lidar_com_solicitacao(self, request):
        if request == "Problema Nível 2":
            return f"Suporte Nível 2 resolveu o {request}"
        else:
            return super().lidar_com_solicitacao(request)

class SuporteNivel3(AbstractHandler):
    def lidar_com_solicitacao(self, request):
        if request == "Problema Nível 3":
            return f"Suporte Nível 3 resolveu o {request}"
        else:
            return super().lidar_com_solicitacao(request)

# Uso
suporte = SuporteNivel1()
suporte.set_proximo(SuporteNivel2()).set_proximo(SuporteNivel3())

print(suporte.lidar_com_solicitacao("Problema Nível 1"))
print(suporte.lidar_com_solicitacao("Problema Nível 2"))
print(suporte.lidar_com_solicitacao("Problema Nível 3"))
print(suporte.lidar_com_solicitacao("Problema Nível 4"))

```
