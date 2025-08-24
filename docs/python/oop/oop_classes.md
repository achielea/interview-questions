---
title: Класи
parent: ООП
nav_order: 2
layout: default
---

<!-- @formatter:off -->
# Класи
- TOC
{:toc}
<!-- @formatter:on -->

### Що таке конструктор?

<hr>

Конструктор — це спеціальний метод класу, який автоматично викликається під час створення
нового об’єкта. Він використовується для ініціалізації атрибутів (стану) об’єкта. У Python цим
методом є `__init__`.

Якщо конструктор не визначений у класі, Python створює порожній конструктор за замовчуванням, який
нічого не робить.

### Різниця між `__init__()` та `__new__()`

<hr>

* `__new__()` — це метод класу, який приймає аргумент `cls` (сам клас, а не об’єкт). Він
  викликається першим і відповідає за створення нового екземпляра класу. Метод `__new__()`
  повертає сам об’єкт (зазвичай — виклик `super().__new__(cls)`), або інший об’єкт (навіть іншого
  класу!).

* `__init__()` викликається після того, як об’єкт уже створений (після `__new__()`). Ві
  використовується для ініціалізації атрибутів об’єкта і не повертає значення (`None`). Це метод
  екземпляра, перший аргумент — `self` (посилання на створений об’єкт).

Якщо `__new__()` не повертає об’єкт (`None` або щось інше), то `__init__()` не викликається.
`__new__()` часто перевизначають, коли треба контролювати створення об’єктів (наприклад, у патерні
Singleton, або при створенні immutable класів — таких як `int`, `str`, `tuple`).

<!-- @formatter:off -->
```python
class Demo:
  def __new__(cls, *args, **kwargs):
    print("Викликано __new__")
    instance = super().__new__(cls)  # створюємо новий об’єкт
    return instance

  def __init__(self, value):
    print("Викликано __init__")
    self.value = value

d = Demo(10)
print(d.value)
```
<!-- @formatter:on -->

### Як визначити декілька конструкторів у класі?

<hr>

У Python не можна зробити кілька `__init__()` з різними сигнатурами, як у Java чи C++.
Але є кілька прийомів, щоб реалізувати "кілька конструкторів":

* Використати аргументи за замовчуванням

  <!-- @formatter:off -->
  ```python
  class Person:
      def __init__(self, name="Невідомо", age=0):
          self.name = name
          self.age = age
  
  
  p1 = Person("Андрій", 25)   # передаємо всі параметри
  p2 = Person("Ірина")        # тільки name
  p3 = Person()               # нічого не передаємо
  
  print(p1.name, p1.age)  # Андрій 25
  print(p2.name, p2.age)  # Ірина 0
  print(p3.name, p3.age)  # Невідомо 0
  ```
  <!-- @formatter:on -->

* Використати умовну логіку в `__init__()`

  <!-- @formatter:off -->
  ```python
  class Rectangle:
      def __init__(self, *args):
          if len(args) == 1:  # квадрат
              self.width = self.height = args[0]
          elif len(args) == 2:  # прямокутник
              self.width, self.height = args
          else:
              raise ValueError("Очікується 1 або 2 аргументи")
  
  
  r1 = Rectangle(5)  # квадрат 5x5
  r2 = Rectangle(4, 6)  # прямокутник 4x6
  
  print(r1.width, r1.height)  # 5 5
  print(r2.width, r2.height)  # 4 6
  ```
  <!-- @formatter:on -->

* Зробити альтернативні конструктори через `@classmethod`. Це більш Pythonic підхід.
  Замість кількох `__init__()`, створюють різні фабричні методи:

  <!-- @formatter:off -->
  ```python
  class Person:
      def __init__(self, name, age):
          self.name = name
          self.age = age
  
      @classmethod
      def from_string(cls, data: str):
          name, age = data.split("-")
          return cls(name, int(age))
  
      @classmethod
      def newborn(cls, name):
          return cls(name, 0)
  
  
  p1 = Person("Андрій", 25)
  p2 = Person.from_string("Ірина-30")
  p3 = Person.newborn("Малюк")
  
  print(p1.name, p1.age)  # Андрій 25
  print(p2.name, p2.age)  # Ірина 30
  print(p3.name, p3.age)  # Малюк 0
  ```
  <!-- @formatter:on -->

### Атрибути класу та атрибути об'єкта

<hr>

* **Атрибут класу (class attribute)** — cпільний для всіх об’єктів цього класу й оголошується
  всередині класу, але поза методами. Якщо його змінити через сам клас, зміни побачать усі об’єкти.

  <!-- @formatter:off -->
  ```python
  class Dog:
      species = "Canis familiaris"   # атрибут класу
  
      def __init__(self, name):
          self.name = name           # атрибут об’єкта
  
  d1 = Dog("Бобік")
  d2 = Dog("Шарік")
  
  print(d1.species)  # Canis familiaris
  print(d2.species)  # Canis familiaris
  
  Dog.species = "Canis lupus familiaris"  # зміна через клас
  
  print(d1.species)  # Canis lupus familiaris
  print(d2.species)  # Canis lupus familiaris
  ```
  <!-- @formatter:on -->

* **Атрибут об’єкта (object attribute)** — унікальний для кожного екземпляра. Створюється усередині
  `__init__()` або через `self`. Змінюється незалежно від інших об’єктів.

  <!-- @formatter:off -->
  ```python
  class Dog:
      def __init__(self, name):
          self.name = name   # атрибут об’єкта
  
  d1 = Dog("Бобік")
  d2 = Dog("Шарік")
  
  print(d1.name)  # Бобік
  print(d2.name)  # Шарік
  
  d1.name = "Рекс"   # зміна тільки для d1
  print(d1.name)     # Рекс
  print(d2.name)     # Шарік
  ```
  <!-- @formatter:on -->

Змінити атрибут класу через об’єкт у Python не можна. Замість цього створюється новий атрибут у
цьому конкретному об’єкті, який перевизначає (override) однойменний атрибут класу. Якщо хочемо
змінити атрибут класу саме у класі (а не створити новий у конкретному об’єкті), треба явно
звертатися до класу.

### Методи екземпляра, методи класу та статичні методи

<hr>

**Методи екземпляра (instance methods)**

* використовуються для роботи з конкретним об’єктом класу;
* першим параметром завжди приймають `self` (посилання на конкретний об’єкт);
* можуть звертатися до атрибутів об’єкта та атрибутів класу;
* викликаються через об’єкт `obj.method()` або клас `Class.method(obj)` (явно передаючи
  посилання на об'єкт).

  <!-- @formatter:off -->
  ```python
  class Employee:
      company = "ABC Corp"  # атрибут класу
  
      def __init__(self, name, salary):
          self.name = name    # атрибут об’єкта
          self.salary = salary
  
      def show_info(self):
          print(f"{self.name} працює в {self.company} із зарплатою {self.salary}")
  
  e1 = Employee("Іван", 5000)
  e1.show_info()  # Іван працює в ABC Corp із зарплатою 5000
  ```
  <!-- @formatter:on -->

**Методи класу (class methods)**

* використовуються для доступу та зміни атрибутів класу, а також створення альтернативних
  конструкторів;
* приймають першим параметром `cls` (посилання на клас);
* оголошуються через декоратор `@classmethod`;
* викликаються як через клас, так і через об’єкт: `Class.method()` або `obj.method()`.

  <!-- @formatter:off -->
  ```python
  class Employee:
      company = "ABC Corp"
      employee_count = 0
  
      def __init__(self, name, salary):
          self.name = name
          self.salary = salary
          Employee.employee_count += 1
  
      @classmethod
      def from_string(cls, data):
          name, salary = data.split("-")
          return cls(name, int(salary))
  
      @classmethod
      def get_employee_count(cls):
          return cls.employee_count
  
  e1 = Employee("Іван", 5000)
  e2 = Employee.from_string("Олена-6000")
  print(Employee.get_employee_count())  # 2
  ```
  <!-- @formatter:on -->

**Статичні методи (Static Methods)**

* не приймають параметр `self` чи `cls`;
* повністю незалежні від стану об’єкта та класу;
* використовуються для утилітарних функцій, логічно пов’язаних з класом;
* оголошуються через декоратор `@staticmethod`;
* викликається через клас або об’єкт: `Class.methods()` або `obj.method()`.

### Що таке перевизначення методів (overriding)?

<hr>

**Перевизначення (overriding)** — це створення в похідному класі методу з тією ж назвою, що й у
базовому класі, щоб змінити або розширити поведінку методу базового класу для об’єктів похідного
класу.

Приклад простого перевизначення:

<!-- @formatter:off -->
```python
class Animal:
    def speak(self):
        print("Вид звуку тварини")

class Dog(Animal):
    def speak(self):  # перевизначення методу
        print("Гав-гав!")

class Cat(Animal):
    def speak(self):  # перевизначення методу
        print("Мяу!")

a = Animal()
d = Dog()
c = Cat()

a.speak()  # Вид звуку тварини
d.speak()  # Гав-гав!
c.speak()  # Мяу!
```
<!-- @formatter:on -->

Щоб розширити, а не замінити поведінку, використовується `super()`.

Виклик методу базового класу у перевизначенні:

<!-- @formatter:off -->
```python
class Dog(Animal):
    def speak(self):
        super().speak()  # викликаємо базовий метод
        print("Гав-гав голосно!")

d = Dog()
d.speak()
# Вивід:
# Вид звуку тварини
# Гав-гав голосно!
```
<!-- @formatter:on -->

### Що таке MRO?

<hr>

MRO (Method Resolution Order) описує порядок, у якому інтерпретатор шукає методи та атрибути в
класах. Він визначає, який метод або атрибут використовувати у випадку конфлікту імен.

**Стандартний випадок (DFLR):**

* Відбувається пошук в глибину (depth-first) — від самого об'єкта, до його класу, потім у
  суперкласах.
* У випадку множинного успадкування після пошуку в глибину, здійснюється ще пошук зліва-направо
  відповідно до порядку, в якому суперкласи перелічені у заголовку.

<!-- @formatter:off -->
```python
class A:
    foo = 'A foo'

class B(A):
    bar = 'B bar'

class C:
    foo = 'C foo'

class D(B, C):
    pass

print(D.foo, D.bar)  # A foo B bar
print(D.__mro__) # (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.A'>, <class '__main__.C'>, <class 'object'>)
```
<!-- @formatter:on -->

У прикладі значення атрибуту `foo` береться з класу `А`, відповідно до підходу DFLR.

**Ромбоподібне успадкування (diamond inheritance)**

Diamond problem виникає у випадку множинного успадкування, коли декілька суперкласів мають
спільний базовий клас. У цій ситуації, значення атрибуту буде братися з класу, який є нижче в
ієрархій (спеціалізований), навіть якщо суперклас за порядком DFLR буде знайдено першим.

<!-- @formatter:off -->
```python
class A:
    foo = 'A foo'

class B(A):
    bar = 'B bar'

class C(A):
    foo = 'C foo'

class D(B, C):
    pass

print(D.foo, D.bar) # C foo B bar
print(D.__mro__) # (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.A'>, <class 'object'>)
```
<!-- @formatter:on -->

У цьому прикладі атрибут з класу C, який розміщений праворуч, "затьмарює" значення з базового
класу, розташованого ліворуч.

Пошук в ширину відбувається навіть серед класів, які не мають спільного суперкласа:

<!-- @formatter:off -->
```python
class A:
    foo = 'A foo'

class B(A):
    bar = 'B bar'

class C:
    foo = 'C foo'

class E(A):
    foo = 'E foo'

class D(B, C, E):
    pass

print(D.foo, D.bar) # C foo B bar
print(D.__mro__) # (<class '__main__.D'>, <class '__main__.B'>, <class '__main__.C'>, <class '__main__.E'>, <class '__main__.A'>, <class 'object'>)
```
<!-- @formatter:on -->

Підсумовуючи: у звичайному випадку пошук здійснюється аж догори, потім повертається назад і
починає пошук зліва-направо (DFLR); за робмоподібного успадкування пошук перевіряє класи праворуч,
перш ніж піднятися до спільного суперкласу (MRO).

### Які модифікатори доступу є у Python?

<hr>

У Python немає жорстких модифікаторів доступу, як у Java чи C++. Проте існують умовні правила для
позначення доступності атрибутів і методів.

1. Public:
    * доступні зовні класу, можуть використовуватися будь-яким кодом;
    * не мають спеціального префіксу.

2. Protected:
    * не рекомендується звертатися ззовні, але Python не забороняє;
    * використовується для вказівки розробникам, що це внутрішня реалізація;
    * префікс `_` перед ім’ям (`_attr`).

3. Private:
    * Python застосовує name mangling: атрибут стає доступним як `_ClassName__attr`.
    * Не можна безпосередньо звертатися ззовні, це створює захист від випадкового доступу.
    * Префікс `__` перед ім’ям (`__attr`).

### Що таке `@property` і як його використовувати?

<hr>

`@property` — це декоратор у Python, який дозволяє визначати метод класу як атрибут для читання.
Він використовується для:

* інкапсуляції (приховати внутрішнє представлення даних),
* додавання `getter`, `setter`, `deleter`,
* збереження чистого синтаксису (`obj.attr` замість `obj.get_attr()`).

`@property` дозволяє перетворити метод на атрибут, не змінюючи API класу. Це забезпечує backward
compatibility: можна почати з публічного атрибуту, а потім перетворити його в property без зміни
коду користувачів. По суті, `@property` в Python — це “пітонічна” альтернатива `get`/`set` методам з
Java чи C++.

Приклад використання:

<!-- @formatter:off -->
```python
class Person:
    def __init__(self, name):
        self._name = name   # захищений атрибут

    @property
    def name(self):
        """Getter"""
        return self._name

    @name.setter
    def name(self, value):
        """Setter"""
        if not isinstance(value, str):
            raise ValueError("Ім'я має бути рядком")
        self._name = value

    @name.deleter
    def name(self):
        """Deleter"""
        print("Видаляю ім'я")
        del self._name

p = Person("Іван")
print(p.name)   # викликає getter → "Іван"
p.name = "Олег" # викликає setter
del p.name      # викликає deleter
```
<!-- @formatter:on -->

### Як зробити атрибут лише для читання у Python?

<hr>

1. Використання `@property` (найпоширеніший спосіб)

    <!-- @formatter:off -->
    ```python
    class Person:
        def __init__(self, name):
            self._name = name   # внутрішній атрибут
    
        @property
        def name(self):
            """Атрибут лише для читання"""
            return self._name
    
    
    p = Person("Іван")
    print(p.name)   # ✅ "Іван"
    p.name = "Олег" # ❌ AttributeError: can't set attribute
    ```
    <!-- @formatter:on -->

   Атрибут доступний для читання, але без `@name.setter` він не змінюється.

2. Використання `__setattr__()` для блокування змін

    <!-- @formatter:off -->
    ```python
    class Config:
        def __init__(self, version):
            super().__setattr__("_version", version)
    
        def __setattr__(self, key, value):
            if key == "version":
                raise AttributeError("version є read-only")
            super().__setattr__(key, value)
    
        @property
        def version(self):
            return self._version
    ```
    <!-- @formatter:on -->

### Що таке магічні методи в Python?

<hr>

**Магічні методи** (або dunder methods — від double underscore) — це спеціальні методи класів, які
починаються і закінчуються двома підкресленнями: `__init__`, `__str__`, `__add__`, `__len__`,
`__getitem__` тощо. Вони дозволяють визначати поведінку об’єктів, щоб їх можна було
використовувати з вбудованими операторами, функціями та синтаксисом Python.

Основні категорії магічних методів

1. Ініціалізація та створення об’єктів
    * `__new__(cls, ...)` – створює об’єкт (рідко перевизначають).
    * `__init__(self, ...)` – конструктор, ініціалізує стан об’єкта.
    * `__del__(self)` – деструктор, викликається при знищенні об’єкта.

2. Представлення об’єктів

    * `__str__(self)` – “людяний” рядок (наприклад, для `print()`).
    * `__repr__(self)` – офіційне представлення (для розробників, `repr()`).
    * `__format__(self, spec)` – для форматування через `format()`.

3. Методи контейнерів

    * `__len__(self)` – `len(obj)`.
    * `__getitem__(self, key)` – `obj[key]`.
    * `__setitem__(self, key, value)` – `obj[key] = value`.
    * `__delitem__(self, key)` – `del obj[key]`.
    * `__iter__(self)` + `__next__(self)` – підтримка ітерації (`for x in obj`).
    * `__contains__(self, item)` – оператор `in`.

4. Арифметичні оператори
    * `__add__(self, other)` – +.
    * `__sub__(self, other)` – -.
    * `__mul__`, `__truediv__`, `__floordiv__`, `__mod__`, `__pow__` тощо.
    * Є також зворотні методи: `__radd__`, `__rsub__` (якщо об’єкт зліва не підтримує операцію).

5. Порівняння та хешування
    * `__eq__(self, other)` – ==.
    * `__ne__`, `__lt__`, `__le__`, `__gt__`, `__ge__`.
    * `__hash__(self)` – щоб об’єкт можна було використовувати в `set` або як ключ у `dict`.

6. Виклик та контекстні менеджери
    * `__call__(self, *args, **kwargs)` – робить об’єкт “функцією”.
    * `__enter__` / `__exit__` – для with (контекстні менеджери).

### Що таке `__setattr__()`?

<hr>

`__setattr__(self, name, value)` — це спеціальний (магічний) метод у Python, який викликається
автоматично, коли ви намагаєтесь встановити значення атрибуту в об’єкті:

```python
obj.attr = value  # викликає obj.__setattr__("attr", value)
```

Тобто він контролює процес присвоєння значень атрибутам.

Важливий момент: перевизначаючи метод, завжди викликайте `super().__setattr__(name, value)`.
Інакше значення просто не буде збережене у  `__dict__` об’єкта → отримаєте рекурсію або об’єкт без
атрибутів.

Приклад 1: логування всіх змін атрибутів

<!-- @formatter:off -->
```python
class Logger:
    def __setattr__(self, name, value):
        print(f"Зміна атрибуту: {name} = {value}")
        super().__setattr__(name, value)  # важливо викликати батьківський метод!

obj = Logger()
obj.x = 10
obj.y = 20
```
<!-- @formatter:on -->

Приклад 2: захист від зміни read-only атрибутів

<!-- @formatter:off -->
```python
class Config:
    def __init__(self):
        super().__setattr__("version", "1.0")  # встановлюємо початкове значення

    def __setattr__(self, name, value):
        if name == "version":
            raise AttributeError("Атрибут 'version' є лише для читання")
        super().__setattr__(name, value)

cfg = Config()
cfg.debug = True   # ✅ працює
cfg.version = "2.0"  # ❌ AttributeError
```
<!-- @formatter:on -->

### Методи `__str__()` і `__repr__()`

<hr>

`__repr__()` – точне представлення об'єкта, яке, за можливості, дозволяє відтворити об’єкт за
допомогою `eval()`. Викликається функцією `repr(obj)`. Використовується у консолі і при
відлагодженні. Має бути інформативним та детальним.

`__str__()` – зрозуміле представлення об'єкта для користувача. Викликається функцією `str(obj)`
або при `print(obj)`. Може бути коротким, без деталей для відтворення об’єкта.

<!-- @formatter:off -->
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def __repr__(self):
        return f"Point({self.x}, {self.y})"

    def __str__(self):
        return f"Точка на координатах ({self.x}, {self.y})"

p = Point(2, 3)
print(p)       # Точка на координатах (2, 3)
print(str(p))  # Точка на координатах (2, 3)
print(repr(p)) # Point(2, 3)
```
<!-- @formatter:on -->

### Як використовується функція `super()`?

<hr>

`super()` повертає проксі-об’єкт, через який можна викликати методи базового класу.
Застосовується для розширення або повторного використання поведінки базового класу без необхідності
явно називати клас.

1. Виклик у конструкторі:

   Без `super()` довелося б явно писати `Animal.__init__(self, name)`. Використання `super()` робить
   код більш гнучким і безпечним у разі зміни ієрархії класів.

    <!-- @formatter:off -->
    ```python
    class Animal:
        def __init__(self, name):
            self.name = name
    
    class Dog(Animal):
        def __init__(self, name, breed):
            super().__init__(name)  # виклик __init__ базового класу
            self.breed = breed
    
    d = Dog("Бобік", "Лабрадор")
    print(d.name)   # Бобік
    print(d.breed)  # Лабрадор
    ```
    <!-- @formatter:on -->

2. Виклик методів базового класу:

   Виклик базового методу дозволяє комбінувати поведінку базового і похідного класу.
    ```python
    class Animal:
        def speak(self):
            print("Вид звуку тварини")
    
    class Dog(Animal):
        def speak(self):
            super().speak()  # виклик методу базового класу
            print("Гав-гав!")
    
    d = Dog()
    d.speak()
    # Вид звуку тварини
    # Гав-гав!
    ```

3. Множинне наслідування і `super()`

   Python використовує MRO (Method Resolution Order), щоб визначити порядок виклику. `super()`
   автоматично йде по ланцюжку MRO, не потрібно вручну вказувати клас.

    ```python
    class A:
        def process(self):
            print("A")
    
    class B(A):
        def process(self):
            print("B")
            super().process()
    
    class C(A):
        def process(self):
            print("C")
            super().process()
    
    class D(B, C):
        def process(self):
            print("D")
            super().process()
    
    d = D()
    d.process()
    # Вивід:
    # D
    # B
    # C
    # A
    ```

### Як працює `__del__()`?

<hr>

Метод `__del__()` у Python — це деструктор класу. Він викликається, коли об’єкт збирається збирачем
сміття (GC, Garbage Collector), тобто коли для нього більше немає активних посилань.

`__del__()` виконує фіналізацію об’єкта перед його знищенням, застосовується для очищення
ресурсів (закриття файлів, з’єднань з БД, сокетів і т. д.).

<!-- @formatter:off -->
```python
class FileHandler:
    def __init__(self, filename):
        self.file = open(filename, "w")
        print("Файл відкрито")

    def __del__(self):
        print("Закриваю файл")
        self.file.close()

fh = FileHandler("test.txt")
del fh   # вручну видаляємо посилання → викличе __del__
```
<!-- @formatter:on -->

### Як зробити клас незмінним (immutable)?

<hr>

У Python immutable-класи (незмінні) — це такі, чиї атрибути не можна змінювати після створення
об’єкта (аналогічно до int, str, tuple). Є кілька способів це зробити:

* Використати `__setattr__()` і заборонити зміну.

  <!-- @formatter:off -->
  ```python
  class ImmutablePoint:
      def __init__(self, x, y):
          super().__setattr__("x", x)
          super().__setattr__("y", y)
  
      def __setattr__(self, key, value):
          raise AttributeError("Об'єкт є immutable")
  
  p = ImmutablePoint(2, 5)
  print(p.x, p.y)  # 2 5
  p.x = 10         # ❌ AttributeError
  ```
  <!-- @formatter:on -->

* Використати `__slots__()` і зробити захист.

  <!-- @formatter:off -->
  ```python
  class ImmutablePoint:
      __slots__ = ("_x", "_y")
  
      def __init__(self, x, y):
          object.__setattr__(self, "_x", x)
          object.__setattr__(self, "_y", y)
  
      def __setattr__(self, key, value):
          raise AttributeError("Об'єкт є immutable")
  
      @property
      def x(self):
          return self._x
  
      @property
      def y(self):
          return self._y
  
  p = ImmutablePoint(3, 4)
  print(p.x, p.y)  # 3 4
  p.y = 10         # ❌ AttributeError
  ```
  <!-- @formatter:on -->

### Що таке атрибут `__dict__`?

`__dict__` — це словник (`dict`), який зберігає атрибути об’єкта та їхні значення. Для
звичайного об’єкта це усі атрибути екземпляра, а для класу — атрибути класу.

Через `__dict__` можна:

* подивитися, що зберігає об’єкт;
* динамічно додавати, змінювати або видаляти атрибути.

Приклад для об’єкта:

<!-- @formatter:off -->
```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

p = Person("Іван", 25)

print(p.__dict__)
# {'name': 'Іван', 'age': 25}

# Динамічно додаємо атрибут
p.city = "Kyiv"
print(p.__dict__)
# {'name': 'Іван', 'age': 25, 'city': 'Kyiv'}
```
<!-- @formatter:on -->

Приклад для класу:

<!-- @formatter:off -->
```python
class MyClass:
    x = 10
    y = 20

print(MyClass.__dict__)
```
<!-- @formatter:on -->

Виведе словник усіх атрибутів класу, включно з методами та магічними методами (`__module__`,
`__doc__`
тощо). Це `mappingproxy`, тобто захищена версія словника, яку не можна змінити напряму.

Важливі моменти:

* `__dict__` не включає атрибути батьківських класів; тільки ті, що належать самому об’єкту/класу.
* Для екземплярів класів з `__slots__` атрибут `__dict__` може бути відсутній.
* Через `__dict__` можна реалізовувати динамічні атрибути або відображати стан об’єкта.

### Що таке `__slots__`?

`__slots__` — це спеціальний механізм Python, який використовується для обмеження набору атрибутів
екземпляра класу та економії пам’яті.

Зазвичай Python зберігає атрибути об’єкта у словнику `__dict__`. Це дуже гнучко, але потребує
додаткової пам’яті. Якщо клас визначає `__slots__`, Python не створює `__dict__` для кожного
екземпляра, а використовує фіксоване місце під атрибути, що економить пам’ять. Крім економії,
`__slots__` обмежує доступні атрибути — додати нові, не вказані у `__slots__`, не можна.

<!-- @formatter:off -->
```python
class Person:
    __slots__ = ("name", "age")  # лише ці атрибути можна створювати

    def __init__(self, name, age):
        self.name = name
        self.age = age

p = Person("Іван", 25)
print(p.name)  # Іван

p.city = "Kyiv"  # ❌ AttributeError: 'Person' object has no attribute 'city'
```
<!-- @formatter:on -->

Встановлюючи `__slots__`, ми забороняємо додавати атрибути поза визначеним списком.

> Хоча `__slots__` можна призначити список, варто завжди використовувати `tuple`. Зміна списку у
> `__slots__` після опрацювання тіла класу не матиме жодного ефекту, але використання змінної
> послідовності може бути оманою.

### Що таке data class?

<hr>

У Python **data класи (dataclasses)** — це спеціальний інструмент (модуль dataclasses,
з’явився у Python 3.7), який спрощує створення класів, призначених в основному для зберігання даних.

Проблема без dataclasses:
Якщо писати клас вручну, доводиться дублювати багато коду. Ми мусимо явно писати `__init__`,
`__repr__`,
іноді `__eq__`, `__hash__` тощо.

<!-- @formatter:off -->
```python
class Person:
    def __init__(self, name: str, age: int, city: str):
        self.name = name
        self.age = age
        self.city = city

    def __repr__(self):
        return f"Person(name={self.name!r}, age={self.age}, city={self.city!r})"

p = Person("Іван", 25, "Kyiv")
print(p)
```
<!-- @formatter:on -->


Використання `@dataclass`

<!-- @formatter:off -->
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    age: int
    city: str = "Kyiv"   # значення за замовчуванням

p = Person("Іван", 25)
print(p)          # Person(name='Іван', age=25, city='Kyiv')
print(p == Person("Іван", 25))  # True
```
<!-- @formatter:on -->

Python автоматично згенерує:

* `__init__()` — конструктор
* `__repr__()` — зручне представлення
* `__eq__()` — порівняння за значеннями полів
* (за бажанням) `__hash__()`, `__lt__()`, `__le__()` та інші

Можливості dataclasses:

1. Значення за замовчуванням
    ```python
    @dataclass
    class Point:
        x: int = 0
        y: int = 0
    ```

2. Не всі поля беруть участь у порівнянні/хешуванні
    ```python
    from dataclasses import field
    @dataclass
    class User:
        name: str
        password: str = field(repr=False, compare=False)
    ```
   Тепер `password` не показується в `repr` і не впливає на `==`.

3. Незмінність `frozen=True` (immutability)

    ```python
    @dataclass(frozen=True)
    class Point:
        x: int
        y: int
    
    p = Point(1, 2)
    p.x = 10  # ❌ помилка: dataclasses.FrozenInstanceError
    ```

### Що таке `enum` у Python?

<hr>

`enum` (скорочення від enumeration) — це спеціальний тип класів у Python (модуль `enum`), який
дозволяє створювати перелічувані константи. Використовується для представлення скінченного
набору іменованих значень. Дає більш читабельний, безпечний і контрольований спосіб роботи з
константами замість "магічних чисел" чи рядків.

```python
from enum import Enum


class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3


print(Color.RED)  # Color.RED
print(Color.RED.name)  # 'RED'
print(Color.RED.value)  # 1
```

Основні можливості `enum`:

1. Порівняння тільки за ідентичністю

    ```python
    Color.RED == Color.RED   # True
    Color.RED == Color.BLUE  # False
    Color.RED == 1           # False ❌
    ```

2. Ітерація по enum

    ```python
    for c in Color:
        print(c)
    # Color.RED
    # Color.GREEN
    # Color.BLUE
    ```

3. Доступ за ім’ям або значенням

    ```python
    Color["RED"]  # Color.RED
    Color(1)  # Color.RED
    ```

### Що таке міксини d Python?

<hr>

**Mixin-класи (mixins)** — це допоміжні класи, які надають додаткову поведінку чи
функціональність іншим класам через множинне наслідування, але самі по собі не призначені для
створення об’єктів. Інакше кажучи, mixin — це "домішка" функціоналу, яку додають до основного класу.

<!-- @formatter:off -->
```python
class ReprMixin:
    def __repr__(self):
        return f"{self.__class__.__name__}({self.__dict__})"

class Person(ReprMixin):  # додаємо mixin
    def __init__(self, name, age):
        self.name = name
        self.age = age

p = Person("Іван", 25)
print(p)  # Person({'name': 'Іван', 'age': 25})
```
<!-- @formatter:on -->

Клас `Person` отримав метод `__repr__` "з домішки".

Навіщо потрібні mixins?

* Повторне використання коду: один mixin можна підключати у багатьох класах.
* Модульність: можна комбінувати потрібні mixins замість величезного батьківського класу.
* Чиста ієрархія: розділяємо функціонал на "дрібні домішки".

### Чим відрізняється type від object?

<hr>

У Python все є об’єктами, включно з класами. Але клас — це теж "об’єкт спеціального виду".

* `object` — базовий клас у Python. Усі класи прямо чи опосередковано наслідують від `object`.
  Це мінімальний набір поведінки, спільний для всіх об’єктів (наприклад, `__str__`, `__eq__`,
  `__hash__`, `__class__`).

    <!-- @formatter:off -->
    ```python
    class A:
        pass
    
    print(issubclass(A, object))   # True
    print(isinstance(A(), object)) # True
    ```
    <!-- @formatter:on -->

  Тобто `object` — корінь усієї ієрархії об’єктів.

* `type` — метаклас у Python, який використовується для створення класів. Будь-який клас у
  Python є екземпляром `type`.

    <!-- @formatter:off -->
    ```python
    class A:
        pass
    
    print(type(A))   # <class 'type'>
    print(type(A())) # <class '__main__.A'>
    ```
    <!-- @formatter:on -->

  Тобто `type` — це "фабрика класів".

  Якщо `object` — це "корінь даних", то `type` — це "корінь метаданих".

Може здатися парадоксальним, але:

```python
print(isinstance(object, type))  # True  → object є класом, створеним type
print(isinstance(type, object))  # True  → type теж об'єкт
```

Виходить циклічна залежність:

* `object` є екземпляром `type`.
* `type` є нащадком `object`.

### Яка різниця між `is` та `==` для об’єктів?

<hr>

* `is` — перевіряє ідентичність об’єктів, тобто чи посилаються дві змінні на той самий об’єкт у
  пам’яті. Використовує функцію `id()` (адреса в пам’яті).

    <!-- @formatter:off -->
    ```python
    a = [1, 2, 3]
    b = a
    c = [1, 2, 3]
    
    print(a is b)  # True  (один і той самий об’єкт)
    print(a is c)  # False (різні об’єкти з однаковим вмістом)
    ```
    <!-- @formatter:on -->

* `==` — перевіряє рівність значень, викликаючи метод `__eq__()`. Може бути перевизначене в класах.

    ```python
    print(a == b)  # True (списки однакові)
    print(a == c)  # True (значення збігаються)
    ```

### Як реалізується Singleton у Python?

<hr>

**Singleton** (Одинак) — це патерн проєктування, який гарантує, що у програми є лише один екземпляр
класу, і він доступний глобально. У Python це можна реалізувати кількома способами.

1. Через перевизначення `__new__`

    <!-- @formatter:off -->
    ```python
    class Singleton:
        _instance = None
    
        def __new__(cls, *args, **kwargs):
            if cls._instance is None:
                cls._instance = super().__new__(cls)
            return cls._instance
    
    a = Singleton()
    b = Singleton()
    print(a is b)  # True
    ```
    <!-- @formatter:on -->

   ✅ Простий і зрозумілий варіант.
   ❌ Не захищає від створення екземплярів через pickle/copy.

2. Через декоратор

    <!-- @formatter:off -->
    ```python
    def singleton(cls):
        instances = {}
        def get_instance(*args, **kwargs):
            if cls not in instances:
                instances[cls] = cls(*args, **kwargs)
            return instances[cls]
        return get_instance
    
    @singleton
    class Database:
        pass
    
    db1 = Database()
    db2 = Database()
    print(db1 is db2)  # True
    ```
    <!-- @formatter:on -->

   Зручно "робити Singleton" для будь-якого класу.

3. Через метаклас

    <!-- @formatter:off -->
    ```python
    class SingletonMeta(type):
        _instances = {}
    
        def __call__(cls, *args, **kwargs):
            if cls not in cls._instances:
                cls._instances[cls] = super().__call__(*args, **kwargs)
            return cls._instances[cls]
    
    class Logger(metaclass=SingletonMeta):
        pass
    
    log1 = Logger()
    log2 = Logger()
    print(log1 is log2)  # True
    ```
   <!-- @formatter:on -->

   ✅ Найбільш "правильний" та гнучкий спосіб.

   ✅ Можна застосовувати до кількох класів одночасно.

4. Через модуль

   У Python модулі самі по собі — Singleton, бо вони завантажуються лише один раз.

    <!-- @formatter:off -->
    ```python
    # logger.py
    class Logger:
        pass
    
    logger = Logger()
    ```
    ```python
    # main.py
    from logger import logger
    
    print(logger is logger)  # завжди True
    ```
    <!-- @formatter:on -->

   ✅ Найпростіший і "пайтонічний" підхід.

   ❌ Але не завжди зручно, якщо потрібен контроль створення класу.

### Як працює копіювання об’єктів (`copy.copy()`, `copy.deepcopy()`)?

<hr>

У Python об’єкти копіюються через модуль `copy`.

```python
import copy
```

Є два основні методи:

* `copy.copy(obj)` — поверхнева копія. Створює новий об’єкт того ж типу, але його внутрішні
  елементи (атрибути, вкладені структури) залишаються посиланнями на ті самі об’єкти.
  Тобто копіюється лише "зовнішня оболонка".

    <!-- @formatter:off -->
    ```python
    import copy
    
    a = [[1, 2], [3, 4]]
    b = copy.copy(a)
    
    print(a is b)       # False (це новий список)
    print(a[0] is b[0]) # True  (внутрішні списки спільні!)
    ```
  <!-- @formatter:on -->

* `copy.deepcopy(obj)` — глибока копія. Створює повністю новий об’єкт, рекурсивно копіюючи всі
  вкладені структури. В результаті немає спільних об’єктів.

    <!-- @formatter:off -->
    ```python
    c = copy.deepcopy(a)
    
    print(a is c)       # False (новий список)
    print(a[0] is c[0]) # False (навіть внутрішні списки інші)
    ```
    <!-- @formatter:on -->

Як це працює під капотом

* `copy.copy()` викликає метод `__copy__`, якщо він визначений у класі.
* `copy.deepcopy()` викликає `__deepcopy__` (якщо є). Якщо ні — пробує рекурсивно копіювати
  атрибути.
* Щоб уникнути нескінченних циклів (наприклад, об’єкт, який містить посилання сам на себе),
  `deepcopy`
  використовує спеціальний словник `memo`, де зберігає вже скопійовані об’єкти.