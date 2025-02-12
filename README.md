# Cheatsheet Extendido de Programación Orientada a Objetos en Python

Este cheatsheet integral abarca desde la definición básica de clases y objetos hasta conceptos avanzados como encapsulación, herencia, polimorfismo y abstracción, permitiéndote resolver cualquier problema relacionado con la programación orientada a objetos en Python.

---

## 1. Clases y Objetos

### Definición de Clase
- **Clase:** Una plantilla o "blueprint" que define atributos (datos) y métodos (comportamientos) comunes a sus instancias.
- **Constructor (__init__):** Método especial que se ejecuta al crear una instancia, inicializando sus atributos.
- **self:** Referencia a la instancia actual, siempre el primer parámetro en métodos de instancia.

```python
class SuperHero:
    def __init__(self, name, power, health, speed):
        self.name = name      # Atributo público
        self.power = power
        self.health = health
        self.speed = speed

    def use_power(self):
        print(f"{self.name} uses {self.power}!")
```

### Creación de Objetos
Se crea un objeto invocando la clase como si fuera una función, pasando los argumentos necesarios.

```python
iron_man = SuperHero("Iron Man", "repulsor beams", 100, 80)
spider_man = SuperHero("Spider Man", "web slinging", 90, 95)
iron_man.use_power()  # Salida: Iron Man uses repulsor beams!
```

### Acceso y Modificación de Atributos
- Acceso: Utiliza la notación de punto (objeto.atributo).
- Modificación: Asigna un nuevo valor con objeto.atributo = nuevo_valor.

```python
print(iron_man.name)    # Iron Man
iron_man.health = 90
print(iron_man.health)  # 90
```

## 2. Encapsulación

### Niveles de Acceso a Atributos y Métodos
- Públicos: Sin prefijo; accesibles desde cualquier parte.
```python
self.name = name
```
- Protegidos: Con un guión bajo (_atributo); convención para indicar que no deben modificarse fuera de la clase o subclases.
```python
self._name = name
```
- Privados: Con doble guión bajo (__atributo); limita el acceso externo mediante name mangling.
```python
self.__power = power
```

### Getters y Setters
Propósito: Controlar el acceso y la modificación de atributos privados, permitiendo validaciones y encapsulación.

```python
class Hero:
    def __init__(self, name):
        self.__name = name  # Atributo privado

    @property
    def name(self):
        """Getter para el nombre."""
        return self.__name

    @name.setter
    def name(self, new_name):
        """Setter con validación: el nombre no puede estar vacío."""
        if new_name:
            self.__name = new_name
        else:
            print("El nombre no puede estar vacío.")

# Uso
hero = Hero("Batman")
print(hero.name)  # Batman
hero.name = "Superman"
print(hero.name)  # Superman
hero.name = ""    # Imprime: El nombre no puede estar vacío.
```

## 3. Atributos y Métodos de Clase

### Atributos de Clase
Definición: Variables compartidas por todas las instancias, definidas en el cuerpo de la clase, fuera de __init__.

```python
class Superhero:
    hero_count = 0  # Atributo de clase

    def __init__(self, name, power):
        self.name = name    # Atributo de instancia
        self.power = power
        Superhero.hero_count += 1

iron_man = Superhero("Iron Man", "repulsor beams")
thor = Superhero("Thor", "lightning")
print(f"Héroes disponibles: {Superhero.hero_count}")  # Salida: Héroes disponibles: 2
```

### Métodos de Clase
Decorador @classmethod: El método recibe la clase (cls) como primer argumento, permitiendo modificar o acceder a atributos de clase.

```python
class Superhero:
    training_level = 1  # Atributo de clase

    def __init__(self, name, power):
        self.name = name
        self.power = power

    @classmethod
    def upgrade_training(cls):
        cls.training_level += 1
        print(f"Nuevo nivel de entrenamiento: {cls.training_level}")

Superhero.upgrade_training()  # Salida: Nuevo nivel de entrenamiento: 2
```

### Métodos Estáticos
Decorador @staticmethod: Métodos que no requieren acceso a la instancia (self) ni a la clase (cls). Funcionan como funciones lógicas relacionadas con la clase.

```python
class Superhero:
    @staticmethod
    def is_valid_power(power):
        valid_powers = ["Flying", "Strength", "Speed", "Intelligence"]
        return power in valid_powers

print(Superhero.is_valid_power("Flying"))       # True
print(Superhero.is_valid_power("Mind Reading")) # False
```

## 4. Herencia

### Herencia Simple
Concepto: Permite que una clase hija herede atributos y métodos de una clase padre.

```python
class SuperHero:
    def __init__(self, name, power):
        self.name = name
        self.power = power

    def use_power(self):
        print(f"{self.name} uses {self.power}!")

class Avenger(SuperHero):
    def fly(self):
        print(f"{self.name} can fly using {self.power}!")

iron_man = Avenger("Iron Man", "repulsor beams")
iron_man.use_power()  # Hereda el método de la clase padre
iron_man.fly()        # Método específico de Avenger
```

### Sobrescritura de Métodos (Method Overriding)
Objetivo: Modificar el comportamiento de un método heredado.

```python
class SuperHero:
    def attack(self):
        print("Ataca con habilidades básicas.")

class IronMan(SuperHero):
    def attack(self):
        # Opcional: Llama al método de la superclase para mantener parte del comportamiento original
        super().attack()
        print("Ataca con repulsores de alta potencia!")

iron_man = IronMan("Iron Man", "repulsor beams")
iron_man.attack()
# Salida:
# Ataca con habilidades básicas.
# Ataca con repulsores de alta potencia!
```

### Uso de super()
Función: Facilita la llamada a métodos de la clase padre, permitiendo extender su funcionalidad.

```python
class Parent:
    def __init__(self, value):
        self.value = value

class Child(Parent):
    def __init__(self, value, extra):
        super().__init__(value)  # Llama al constructor de Parent
        self.extra = extra
```

### Herencia Múltiple y el Problema del Diamante
- Herencia Múltiple: Una clase puede heredar de varias clases base.
- Problema del Diamante: Ocurre cuando dos clases base comparten una clase padre común. Python resuelve la ambigüedad mediante el Method Resolution Order (MRO).

```python
class A:
    def print_method(self):
        print("A")

class B(A):
    def print_method(self):
        print("B")

class C(A):
    def print_method(self):
        print("C")

class D(B, C):
    pass

d = D()
d.print_method()  # Salida: B (B es el primer padre en el MRO)
print(D.__mro__)  # Muestra el orden de resolución: (D, B, C, A, object)
```

## 5. Polimorfismo

### Concepto de Polimorfismo
- Definición: La capacidad de distintos objetos de responder al mismo método, a pesar de pertenecer a clases diferentes.
- Duck Typing: Se basa en la presencia de métodos y atributos en lugar de la pertenencia a una jerarquía de clases.

```python
class IronMan:
    def attack(self):
        return "Repulsor Blast!"

class Thor:
    def attack(self):
        return "Lightning Strike!"

def hero_attack(hero):
    # No importa la clase, siempre y cuando el objeto tenga el método attack
    print(hero.attack())

hero_attack(IronMan())  # Salida: Repulsor Blast!
hero_attack(Thor())     # Salida: Lightning Strike!
```

### Simulación de Sobrecarga de Métodos
Nota: Python no soporta la sobrecarga de métodos de forma nativa. Se simula mediante argumentos por defecto o *args.

```python
class Calculator:
    # Usando argumentos por defecto
    def add(self, a, b, c=0):
        return a + b + c

    # Usando argumentos variables
    def add_multiple(self, *args):
        return sum(args)

calc = Calculator()
print(calc.add(5, 10))         # Salida: 15
print(calc.add(5, 10, 15))     # Salida: 30
print(calc.add_multiple(5, 10, 15, 20))  # Salida: 50
```

## 6. Abstracción

### Objetivo de la Abstracción
- Definición: Ocultar detalles complejos de implementación y exponer solo la funcionalidad necesaria.
- Tipos:
  - Abstracción de Datos: Oculta la estructura interna usando atributos privados.
  - Abstracción de Comportamiento: Oculta la implementación de métodos, mostrando solo la interfaz.

### Clases y Métodos Abstractos
Uso del módulo abc: Permite definir clases y métodos abstractos que obligan a las subclases a implementar ciertos comportamientos.

```python
from abc import ABC, abstractmethod

class FlyingHero(ABC):
    def __init__(self, name):
        self._name = name  # Atributo protegido

    def get_name(self):
        return self._name

    @abstractmethod
    def fly(self):
        """Método abstracto: las subclases deben implementarlo."""
        pass

class Superman(FlyingHero):
    def fly(self):
        print(f"{self.get_name()} vuela: ¡Up, up and away!")

# No se puede instanciar FlyingHero directamente:
# hero = FlyingHero("Generic Hero")  -> Generará TypeError
superman = Superman("Superman")
superman.fly()  # Salida: Superman vuela: ¡Up, up and away!
```

### Interfaces en Python
Concepto: Una interfaz es una clase abstracta pura que define únicamente métodos abstractos sin implementación.

```python
from abc import ABC, abstractmethod

class DatabaseInterface(ABC):
    @abstractmethod
    def connect(self):
        pass

    @abstractmethod
    def query(self, sql):
        pass

class MySQLDatabase(DatabaseInterface):
    def connect(self):
        print("Conectando a MySQL...")

    def query(self, sql):
        print(f"Ejecutando: {sql}")

db = MySQLDatabase()
db.connect()                      # Salida: Conectando a MySQL...
db.query("SELECT * FROM users")   # Salida: Ejecutando: SELECT * FROM users
```

## 7. Buenas Prácticas y Consideraciones Adicionales

### Documentación y Docstrings
Uso de Docstrings: Agrega documentación a clases, métodos y funciones para facilitar el mantenimiento y la legibilidad.

```python
class SuperHero:
    """
    Clase para representar un superhéroe.

    Atributos:
        name (str): Nombre del superhéroe.
        power (str): Habilidad o poder del superhéroe.
        health (int): Puntos de vida.
        speed (int): Velocidad de acción.
    """

    def __init__(self, name, power, health, speed):
        """Inicializa una instancia de SuperHero."""
        self.name = name
        self.power = power
        self.health = health
        self.speed = speed
```

### Principios SOLID (Breve Mención)
- S: Responsabilidad Única
- O: Abierto/Cerrado
- L: Sustitución de Liskov
- I: Segregación de Interfaces
- D: Inversión de Dependencias

Estos principios promueven un código modular, reutilizable y mantenible.

### Composición vs. Herencia
- Herencia: Utilízala cuando exista una relación "es un" entre clases.
- Composición: Prefiere la composición (una clase "tiene un" objeto) para lograr un acoplamiento más flexible y evitar jerarquías rígidas.

## Conclusión

Este cheatsheet extendido cubre los fundamentos y técnicas avanzadas de la programación orientada a objetos en Python. Al comprender y aplicar estos conceptos —desde la definición de clases y manejo de atributos hasta la implementación de herencia, polimorfismo y abstracción— estarás preparado para diseñar soluciones robustas, escalables y de fácil mantenimiento.

Utiliza este documento como referencia para abordar cualquier problema relacionado con la programación orientada a objetos en Python.