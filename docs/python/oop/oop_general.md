---
title: Загальне
parent: ООП
nav_order: 1
layout: default
---

<!-- @formatter:off -->
# Загальне
- TOC
{:toc}
<!-- @formatter:on -->

### Що таке ООП?

<hr>

**Об'єктно-орієнтоване програмування (ООП)** — це парадигма програмування, в якій програма
розглядається як сукупність об'єктів, що взаємодіють між собою. Основна ідея ООП — моделювати
реальні або абстрактні сутності у вигляді об'єктів, які взаємодіють між собою.

**Об'єкт** — це базовий елемент програми, який має деякий тип і поєднує в собі стан (атрибути) та
поведінку (методи). Об'єкти є екземплярами класу.

**Клас** — це шаблон для визначення типу, який визначає, які атрибути та методи матимуть об'єкти.

### Основні принципи ООП

<hr>

Основними принципами ООП є абстракція, інкапсуляція, успадкування та поліморфізм.

* **Абстракція** полягає у відокремлення суттєвого від несуттєвого, тобто приховуванні складних
  деталей реалізації та показ лише необхідного інтерфейсу.

  У Python реалізується через абстрактні класи (`ABC` + `@abstractmethod`). Можна також
  використовувати “duck typing”: головне — щоб об’єкт мав потрібний метод.

    <!-- @formatter:off -->
    ```python
    from abc import ABC, abstractmethod
    
    class Shape(ABC):
        @abstractmethod
        def area(self):
            pass
    
    class Circle(Shape):
        def __init__(self, r): self.r = r
        def area(self): return 3.14 * self.r * self.r
    
    s = Circle(5)
    print(s.area())  # 78.5
    ```
    <!-- @formatter:on -->

* **Інкапсуляція** — об'єднання логічно пов'язаних даних та операцій (атрибутів і методів) в
  одному класі, а також контроль доступу до них за допомогою модифікаторів доступу. Основна
  перевага інкапсуляції — можливість змінювати внутрішню реалізацію без впливу на зовнішній код.

  В класичних мовах (Java, C#) є модифікатори доступу: `private`, `protected`, `public`. У
  Python їх немає жорстко, але є домовленості:
    * `name` → публічний атрибут (доступний всім);
    * `_name` → “protected”, домовленість: не використовувати напряму;
    * `__name` → “private”, Python застосовує name mangling (`_ClassName__name`).

    <!-- @formatter:off -->
    ```python
    class BankAccount:
        def __init__(self, balance):
            self.__balance = balance  # приватний атрибут
    
        def deposit(self, amount):
            self.__balance += amount
    
        def get_balance(self):
            return self.__balance
    
    acc = BankAccount(100)
    acc.deposit(50)
    print(acc.get_balance())   # 150
    # print(acc.__balance) ❌ AttributeError
    ```
    <!-- @formatter:on -->

* **Успадкування** — механізм створення нових класів на основі вже наявних. Успадкування сприяє
  зменшенню повторення коду та його логічній організації у вигляді ієрархій класів.

  У Python підтримується множинне успадкування. Є функція `super()`, щоб викликати методи
  батьківського класу.

    <!-- @formatter:off -->
    ```python
    class Animal:
        def speak(self):
            print("Якась тварина говорить")
    
    class Dog(Animal):
        def speak(self):
            super().speak()
            print("Гав 🐶")
    
    dog = Dog()
    dog.speak()
    # Вивід:
    # Якась тварина говорить
    # Гав 🐶
    ```
    <!-- @formatter:on -->

* **Поліморфізм** — використання єдиного інтерфейсу для об'єктів різних типів.

  У Python це проявляється завдяки динамічній типізації. Є два типи поліморфізму в Python:
    * Перевизначення методів (overriding) – підклас перевизначає метод батьківського класу.
    * Перевантаження методів (overloading) – у Python його класичного варіанта немає (як у Java),
      але можна реалізувати через аргументи за замовчуванням або *args, **kwargs.

    <!-- @formatter:off -->
    ```python
    class Cat:
        def speak(self): print("Мяу 🐱")
    
    class Dog:
        def speak(self): print("Гав 🐶")
    
    def animal_speak(animal):
        animal.speak()
    
    animal_speak(Cat())  # Мяу
    animal_speak(Dog())  # Гав
    ```
    <!-- @formatter:on -->

{: .important }
> Поліморфізм у Python має особливість, яка відрізняє його від багатьох класичних мов
> програмування. У строготипізованих мовах, таких як Java чи C++, поліморфізм зазвичай
> пов’язаний з інтерфейсами та ієрархіями класів: щоб об’єкти могли вважатися взаємозамінними,
> вони повинні реалізовувати спільний інтерфейс або успадковувати базовий клас. У Python підхід
> набагато гнучкіший завдяки динамічній типізації та принципу duck typing: якщо об’єкт має метод
> із потрібною назвою, його можна викликати незалежно від того, чи є цей об’єкт нащадком певного
> класу. Це дозволяє писати код, який орієнтований на поведінку об’єктів, а не на їх формальну
> приналежність до певної ієрархії.

### Типи зв'язків між класами

<hr>

* **Асоціація**: найслабкіший тип зв'язку, який полягає в тому, що клас `А` знає про клас `B` і
  може з ним взаємодіяти.

  Приклад: студент має посилання на університет.

    <!-- @formatter:off -->
    ```python
    class University:
        def __init__(self, name):
            self.name = name
    
    class Student:
        def __init__(self, name, university):
            self.name = name
            self.university = university  # асоціація
    
    uni = University("КНУ")
    student = Student("Іван", uni)
    print(f"{student.name} навчається в {student.university.name}")
    ```
<!-- @formatter:on -->

* **Агрегація**: клас `B` є частиною класу `A` (`A` has-a `B`), але може існувати незалежно від
  нього.

  Приклад: кафедра має викладачів, але викладач може існувати і без кафедри.

  <!-- @formatter:off -->
  ```python
  class Teacher:
    def __init__(self, name):
      self.name = name
  
  class Department:
    def __init__(self, name):
      self.name = name
      self.teachers = []  # список викладачів
  
    def add_teacher(self, teacher):
      self.teachers.append(teacher)
  
  dep = Department("Математика")
  t1 = Teacher("Петренко")
  dep.add_teacher(t1)
  
  print(f"Кафедра {dep.name} має викладача {dep.teachers[0].name}")
  ```
  <!-- @formatter:on -->

* **Композиція**: посилений варіант агрегації, клас `B` є частиною класу `A` (`A` has-a `B`) і
  не може існувати окремо.

  Приклад: автомобіль складається з двигуна. Двигун не існує окремо від машини.

  <!-- @formatter:off -->
  ```python
  class Engine:
      def __init__(self, horsepower):
          self.horsepower = horsepower
  
  class Car:
      def __init__(self, brand):
          self.brand = brand
          self.engine = Engine(150)  # створюється всередині, живе разом із Car
  
  car = Car("Toyota")
  print(f"{car.brand} має двигун на {car.engine.horsepower} к.с.")
  ```
<!-- @formatter:on -->

* **Успадкування**: зв'язок "is-a", один клас успадковує атрибути та методи іншого.

  Приклад: Кіт є твариною.

  <!-- @formatter:off -->
  ```python
  class Animal:
      def speak(self):
          print("Якась тварина говорить")
  
  class Cat(Animal):  # успадкування
      def speak(self):
          print("Мяу")
  
  cat = Cat()
  cat.speak()  # Мяу
  ```
<!-- @formatter:on -->

### Що таке інтерфейс?

<hr>

**Інтерфейс** — це контракт (набір методів), який клас повинен реалізувати. Він визначає що клас
має вміти робити, але не визначає як саме.

У Python немає окремого ключового слова `interface`, як у Java чи C#. Проте є кілька способів
визначати інтерфейси:

1. **Абстрактні базові класи** (`ABC` — Abstract Base Classes)

   Створюються за допомогою модуля `abc`. Методи з `@abstractmethod` повинні бути реалізовані у
   дочірніх класах.

    <!-- @formatter:off -->
    ```python
    from abc import ABC, abstractmethod
    
    class Flyable(ABC):  # інтерфейс
        @abstractmethod
        def fly(self):
            pass
    
    class Bird(Flyable):
        def fly(self):
            print("Птах летить")
    
    class Airplane(Flyable):
        def fly(self):
            print("Літак злітає")
    
    # bird = Flyable()  # ❌ не можна створити інтерфейс напряму
    bird = Bird()
    plane = Airplane()
    
    bird.fly()   # Птах летить
    plane.fly()  # Літак злітає
    ```
    <!-- @formatter:on -->

2. **Duck typing**
   У Python інтерфейси часто не формалізують. Прийнято правило: “Якщо об’єкт виглядає як качка,
   плаває як качка і крякає як качка — значить, це качка”. Тобто, якщо об’єкт має метод `fly()`,
   Python вважатиме його сумісним із “інтерфейсом” `Flyable`, навіть якщо він не успадковує базовий
   клас.

    <!-- @formatter:off -->
    ```python
    class Drone:
        def fly(self):
            print("Дрон летить")
    
    def make_it_fly(obj):
        obj.fly()  # працює з будь-яким об’єктом, що має метод fly()
    
    make_it_fly(Drone())   # Дрон летить
    make_it_fly(Bird())    # Птах летить
    ```
    <!-- @formatter:on -->

### Що таке абстрактний клас?

**Абстрактний клас** — це клас, який не можна створити напряму, а можна тільки успадкувати.
Він служить як шаблон для інших класів: визначає спільну поведінку (реалізовані методи, атрибути) і
одночасно може містити абстрактні методи, які зобов’язані реалізувати всі підкласи.

У Python абстрактні класи реалізуються через модуль abc (Abstract Base Classes), і вони мають кілька
специфічних особливостей у порівнянні з класами в Java чи C#.

1. Не можна створювати об’єкти напряму

    <!-- @formatter:off -->
    ```python
    from abc import ABC, abstractmethod
    
    class Animal(ABC):
        @abstractmethod
        def make_sound(self):
            pass
    
    # a = Animal()  # ❌ Помилка: не можна створити абстрактний клас
    ```
    <!-- @formatter:on -->

2. Можуть містити як абстрактні, так і звичайні методи

    <!-- @formatter:off -->
    ```python
    class Animal(ABC):
        @abstractmethod
        def make_sound(self):
            pass
        
        def sleep(self):
            print("Я сплю 💤")
    ```
   <!-- @formatter:on -->

3. Підкласи повинні реалізувати всі абстрактні методи, інакше вони теж залишаться абстрактними.

    <!-- @formatter:off -->
    ```python
    class Dog(Animal):
        def make_sound(self):
            print("Гав 🐶")
    
    dog = Dog()   # ✅ працює, бо метод реалізовано
    ```
    <!-- @formatter:on -->

4. Можна наслідувати кілька абстрактних класів (Python підтримує множинне наслідування).

    <!-- @formatter:off -->
    ```python
    class Runnable(ABC):
        @abstractmethod
        def run(self): pass
    
    class Flyable(ABC):
        @abstractmethod
        def fly(self): pass
    
    class SuperHero(Runnable, Flyable):
        def run(self): print("Біжу")
        def fly(self): print("Лечу")
    ```
    <!-- @formatter:on -->

5. Можна оголошувати абстрактні властивості та статичні методи

    <!-- @formatter:off -->
    ```python
    from abc import ABC, abstractmethod
    
    class Shape(ABC):
        @property
        @abstractmethod
        def area(self):  # абстрактна властивість
            pass
    ```
    <!-- @formatter:on -->

6. Абстрактні класи можна використовувати як інтерфейси.
    * Якщо в абстрактному класі залишати лише абстрактні методи → він фактично виконує роль
      інтерфейсу.
    * Якщо додати спільну реалізацію → він стає “напівготовим” батьківським класом.

7. Duck typing не зникає. Навіть із абстрактними класами Python все одно дозволяє працювати за
   принципом “якщо виглядає як качка — то це качка”. Тобто іноді можна обійтися і без формального
   ABC.

### Інтерфейс vs. абстрактний клас

<hr>

Концептуально, інтерфейс — це чистий контракт: він описує що клас має робити, але не каже як. Методи
в інтерфейсі (у класичних ООП-мовах) не мають реалізації. Абстрактний клас — це “напівготовий” клас:
він може містити і абстрактні методи (без реалізації), і звичайні методи/атрибути (з реалізацією).

* Використовуй інтерфейс, якщо різні класи повинні дотримуватись однакового контракту, але вони
  можуть мати зовсім різну природу.

* Використовуй абстрактний клас, якщо хочеш дати спільну базову поведінку для групи класів +
  змусити їх реалізувати деякі методи.

У Python інтерфейси реалізуються через ABC (абстрактні класи), тому відмінність не така жорстка, як
у Java чи C#. Але концептуально різниця зберігається:

Приклад абстрактного класу:

<!-- @formatter:off -->
```python
from abc import ABC, abstractmethod

class Animal(ABC):
    def sleep(self):  # спільна реалізація
        print("Сплю")

    @abstractmethod
    def make_sound(self):  # абстрактний метод
        pass

class Dog(Animal):
    def make_sound(self):
        print("Гав")

dog = Dog()
dog.sleep()       # реалізація з батьківського класу
dog.make_sound()  # власна реалізація
```
<!-- @formatter:on -->

Приклад інтерфейс (чистий контракт):

<!-- @formatter:off -->
```python
from abc import ABC, abstractmethod

class Flyable(ABC):
    @abstractmethod
    def fly(self):  # немає жодної реалізації
        pass

class Bird(Flyable):
    def fly(self):
        print("Птах летить")
```
<!-- @formatter:on -->