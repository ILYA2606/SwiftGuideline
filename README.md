# Гайдлайн по разработке iOS-приложения на Swift

Русифицированный и немного измененный гайдлайн от raywenderlich.com

## Оглавление

* [Начнем с перфекционизма](#Начнем-с-перфекционизма)
* [Ширина страницы](#Ширина-страницы)
* [Именование](#Именование)
  * [Методы](#Методы)
  * [Булевы](#Булевы)
  * [Префиксы класса](#Префиксы-класса)
  * [Делегаты](#Делегаты)
  * [Зависимый от типа контекст](#Зависимый-от-типа-контекст)
  * [Обобщения](#Обобщения)
  * [Язык](#Язык)
* [Организация кода](#Организация-кода)
  * [Соответствие протокола](#Соответствие-протокола)
  * [Неиспользованный код](#Неиспользованный-код)
  * [Минимальный импорт](#Минимальный-импорт)
* [Отступы](#Отступы)
* [Комментарии](#Комментарии)
* [Классы и структуры](#Классы-и-структуры)
  * [Использование self](#Использование-self)
  * [Вычисляемые свойства](#Вычисляемые-свойства)
  * [Использование final](#Использование-final)
* [Объявление функций](#Объявление-функций)
* [Выражения замыканий](#Выражения-замыканий)
* [Типы](#Типы)
  * [Константы](#Константы)
  * [Статические методы и свойства типа переменных](#Статические-методы-и-свойства-типа-переменных)
  * [Необязательные значения](#Необязательные-значения)
  * [Ленивая инициализация](#Ленивая-инициализация)
  * [Вывод типа](#Вывод-типа)
  * [Синтаксический сахар](#Синтаксический-сахар)
  * [Массивы и словари](#Массивы-и-словари)
* [Функции против Методов](#Функции-против-методов)
* [Управление памятью](#Управление-памятью)
  * [Продление срока жизни объекта](#Продление-срока-жизни-объекта)
* [Контроль доступа](#Контроль-доступа)
* [Поток управления](#Поток-управления)
* [Золотой путь](#Золотой-путь)
* [Использование guard](#Использование-guard)
  * [Обработка guard](#Обработка-guard)
* [Точки с запятой](#Точки-с-запятой)
* [Круглые скобки](#Круглые-скобки)
* [Формат организации и Bundle Identifier](#Формат-организации-и-bundle-identifier)
* [Ссылки](#Ссылки)

## Начнем с перфекционизма

Как бы это ни звучало банально – всегда старайтесь компилировать свой код без предупреждений (warnings).

## Ширина страницы

Установите ширину страницы в 120 символов (Xcode / Preferences / Text Editing / Page guide at column). Это значение рекомендуется включить в соответствующем правиле в SwiftLint. Таким образом, весь код будет умещаться в заданную ширину страницы и будет более читабельным.

## Именование

Правильное именование упрощает чтение и понимание кода. Используйте соглашения именования в Swift, описанные в [API Design Guidelines](https://swift.org/documentation/api-design-guidelines/). Некоторые ключевые особенности включают:

- Стремитесь к ясности при вызовах
- Выбирайте приоритет ясности перед крайностью
- Используйте camelCase, а не snake_case
- Используйте верхний регистр для типов и протоколов, нижний регистр для всего остального
- Включайте только необходимые слова, при этом избегая лишних
- Используйте именование на основе роли, а не типа
- Иногда давайте расширенную информацию для типов со слабой ссылкой
- Стремитесь к свободному использованию
- Начинайте фабричные методы через “make” (Например, `makeProduct`)
- Методы именования:
  - методы в форме глагола следуют правилу -ed, -ing для неизменяемого объекта (Например, `sorted`)
  - методы в форме глагола следуют правилу первой формы для изменяемого объекта (Например, `sort`)
  - булевы типы должны читаться, как утверждения (Например, `isEnabled`)
  - протоколы должны описывать существительные (Например, Sphere -> `Spherable`)
  - протоколы, описывающие возможность, должны заканчиваться в -able или -ible (Например, `Fuckable`)
- Используйте термины, которые не удивляют экспертов или путают начинающих разработчиков
- Избегайте сокращений
- Именуйте веские причины для выбора имени
- Отдавайте приоритет методам и свойствам, вместо свободных функций
- Используйте аббревиатуры только в верхнем или нижнем регистре (Например, `userID`, `isRepresentableAsASCII`, `utf8Bytes`)
- Давайте одинаковое базовое имя для методов, которые имеют одинаковое значение (Например, не использовать где-то `configurate`, а где-то `setup`)
- Избегайте перегрузок в возвращаемом типе
- Выбирайте само-документируемые имена для параметров
- Маркируйте параметры, если они являются замыканием или кортежем
- Используйте параметры по умолчанию

### Методы

Очень важно, когда речь идет о наименовании методов. Чтобы обратиться к имени метода, желательно использовать как можно более простейшую форму:

Пишите имя метода без параметров (Например, `addTarget`)
Пишите имя метода с метками аргументов (Например, `addTarget(:action:)`)
Пишите полное имя метода с метками и типами аргументов (Например, `addTarget(_: Any?, action: Selector?)`)
В приведенном выше примере с использованием _UIGestureRecognizer_ вариант 1 более однозначен и предпочтителен.

**Совет**: Можно использовать панель перехода Xcode для поиска методов с метками аргументов.

![Methods in Xcode jump bar](screens/xcode-jump-bar.png)

### Булевы

Для булевых переменных принято использовать вспомогательный глагол в начале.

**Рекомендуется:**

```swift
var isFavoriteCategoryFilled: Bool
var hasRoaming: Bool
var isBlocked: Bool
```

**Не рекомендуется:**

```swift
var favoriteCategoryFilled: Bool
var roaming: Bool
var blocked: Bool
```

### Префиксы класса

Типы Swift автоматически называются модулем, в котором они содержатся, и вы не должны добавлять префикс класса, например DP. Если сталкиваются два имени из разных модулей, вы можете сделать различие, предварительно указав имя типа с именем модуля. Вводите имя модуля только в том случае, если существует вероятность путаницы, которая должна быть крайне редкой.

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```

### Делегаты

При создании пользовательских методов делегата первый параметр должен быть параметром делегата (UIKit содержит многочисленные примеры подобного).

**Рекомендуется:**
```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```

**Не рекомендуется:**
```swift
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```

### Зависимый от типа контекст

Используйте возможности компилятора для вывода более короткого и понятного кода.

**Рекомендуется:**
```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

**Не рекомендуется:**
```swift
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```

### Обобщения

Имена обобщений должны быть понятными и в формате UpperCamelCase. Если имя типа не имеет значимой роли, используйте традиционную одиночную прописную букву, такую как `T`, `U` или `V`.

**Рекомендуется:**
```swift
struct Stack<Element> { ... }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
```

**Не рекомендуется:**
```swift
struct Stack<T> { ... }
func write<target: OutputStream>(to target: inout target)
func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```

### Язык

Возьмите за основу американско-английское правописание в соответствии с API Apple.

**Рекомендуется:**
```swift
let color = "red"
```

**Не рекомендуется:**
```swift
let colour = "red"
```

## Организация кода

Используйте расширения, чтобы разложить свой код на логические функциональные блоки. Каждое расширение должно быть подписано с помощью `// MARK: -` Комментарий, чтобы все было хорошо организовано.

Лучше соблюдать следующую последовательность:

```swift
// MARK: - Types

// MARK: - Constants

// MARK: - IBOutlet

// MARK: - Public Properties

// MARK: - Private Properties

// MARK: - Initializers

// MARK: - UIViewController(*)

// MARK: - Public methods

// MARK: - IBAction

// MARK: - Private Methods

```
> (*)Вместо `UIViewController` вы должны написать другой суперкласс, от которого вы наследуетесь.

### Соответствие протокола

В частности, при добавлении соответствия протокола желательно добавить отдельное расширение для реализации его методов. Это удерживает связанные методы вместе с протоколом и может упростить инструкции для добавления протокола к классу с помощью связанных с ним методов.

**Рекомендуется:**
```swift
class MyViewController: UIViewController {
  // код класса
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
  // методы датасорса
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
  // методы делегата
}
```

**Не рекомендуется:**
```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
  // все методы
}
```

> Это правило не работает в двух случаях:
> - Когда вам нужен метод переопределения, который реализован в другом расширении
> - Когда вы используете класс с обобщениями и Objective-C протоколами с дополнительными методами (@objc не поддерживается в расширениях Generic-классов или классов, которые наследуются от Generic-классов)

Поскольку компилятор не позволяет повторно объявить соответствие протокола в дочернем классе, не всегда требуется повторять группы расширений базового класса. Это совершенно справедливо, если дочерний класс является финальным и переопределяется лишь небольшое число методов. Сохранение группы расширений оставляют на усмотрение автора.

Для наследников UIViewController можно рассмотреть разложение в отдельные расширения: жизненный цикл, пользовательские аксессоры и IBActions.

### Неиспользованный код

Неиспользованный (мертвый) код, включая код шаблона Xcode и комментарии к заполнению, должен быть удален. За исключением того, что вы пишете учебник, где даете указание читателю использовать прокомментированный код.

Методы, непосредственно не связанные с учебником, реализация которых просто вызывает суперкласс, также должны быть удалены. Сюда входят любые пустые/неиспользуемые методы UIApplicationDelegate.

**Рекомендуется:**
```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  return Database.contacts.count
}
```

**Не рекомендуется:**
```swift
override func didReceiveMemoryWarning() {
  super.didReceiveMemoryWarning()
  // Dispose of any resources that can be recreated.
}

override func numberOfSections(in tableView: UITableView) -> Int {
  // #warning Incomplete implementation, return the number of sections
  return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
  // #warning Incomplete implementation, return the number of rows
  return Database.contacts.count
}

```
### Минимальный импорт

Соблюдайте минимальное количество импортов. Например, опустите импорт `Foundation`, если импортируете `UIKit`.

## Отступы

* Отступ использует 4 пробела, а не табуляцию, чтобы сохранить пространство и помочь предотвратить обертку строк. Для этого надо убедиться, что выбрана соответствующая опция в Xcode (Preferences / Text Editing / Indentation / Prefer Indent using: Spaces, и Tab width: 4)

* Обычные и фигурные скобки (в `if`/`else`/`switch`/`while` и т.д.) всегда открываются в той же строки, что и оператор, но закрываются на новой строке
* Совет: Можно автоматически применить корректный отступ, выделив нужный код, а затем нажав Control-I (или Editor/Structure/Re-Indent)

**Рекомендуется:**
```swift
if user.isHappy {
    // что-то сделаем
} else {
    // сделаем что-то другое
}
```

**Не рекомендуется:**
```swift
if user.isHappy
{
    // что-то сделаем
}
else {
    // сделаем что-то другое
}
```

* Должна быть ровно одна пустая строка между методами, помогающими визуальной ясности и организации. Пропуски внутри методов должны разделять функциональность, но наличие слишком большого их количества в методе часто означает, что вы должны отрефакторить этот метод

* Двоеточия всегда не имеют пробела слева, но имеют один справа. Исключением является тройной оператор `? :`, пустой словарь `[:]` и синтаксис `#selector` для неназванных параметров `(_:)`.

**Рекомендуется:**
```swift
class TestDatabase: Database {
    var data: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

**Не рекомендуется:**
```swift
class TestDatabase : Database {
    var data :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```

* В идеале, длинные строки должны быть в пределах 70 символов. Тем не менее, жесткого ограничения нет

* Пробелов на концах строк быть не должно

* В конце каждого файла должна быть одна пустая строка

## Комментарии

При необходимости, используйте комментарии, чтобы объяснить, почему какой-то фрагмент кода что-то делает. Комментарии должны быть обновлены до текущего состояния кода или удалены совсем.

Избегайте комментариев внутри блока кода, так как код должен быть как можно более самодокументированным. Исключение: это не относится к тем комментариям, которые используются для создания документации.


## Классы и структуры

### Что использовать?

Структуры имеют [семантику значений](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144). Используйте структуры для вещей, которые не имеют личности. Массив, содержащий [a, b, c], действительно тот же, что и другой массив, содержащий [a, b, c], и они полностью взаимозаменяемы. Неважно, используете ли вы первый массив или второй, потому что они представляют собой то же самое. Вот почему массивы – это структуры.

Классы имеют [ссылочную семантику](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). Используйте классы для вещей, которые имеют личность или определенный жизненный цикл. Можно моделировать человека как класс, потому что объекты двух людей – две разные вещи. Все потому, что если у двух людей одно и то же имя и дата рождения, то это не значит, что они одни и те же. Но дата рождения человека была бы структурой, потому что дата 3 марта 1950 года такая же, как и любой другой объект даты 3 марта 1950 года. Сама дата не имеет личности.

Иногда вещи должны быть структурами, но при этом должны соответствовать `AnyObject` или исторически моделироваться уже как классы (`NSDate`, `NSSet`). Старайтесь как можно ближе следовать этим рекомендациям.

### Пример определения

Вот пример хорошо продуманного определения класса:

```swift
class Circle: Shape {
    var x: Int, y: Int
    var radius: Double
    var diameter: Double {
        get {
            return radius * 2
        }
        set {
            radius = newValue / 2
        }
    }

    init(x: Int, y: Int, radius: Double) {
        self.x = x
        self.y = y
        self.radius = radius
    }

    convenience init(x: Int, y: Int, diameter: Double) {
        self.init(x: x, y: y, radius: diameter / 2)
    }

    override func area() -> Double {
        return Double.pi * radius * radius
    }
}

extension Circle: CustomStringConvertible {
    var description: String {
        return "center = \(centerString) area = \(area())"
    }
    private var centerString: String {
        return "(\(x),\(y))"
    }
}
```

Когда сигнатура метода слишком длинная, параметры должны быть перенесены в новую строку:
```swift 
override func register(
    withСardNumber cardNumber: String,
    completion completionBlock: @escaping AuthService.RegisterWithCardNumberCompletionBlock,
    failure failureBlock: @escaping Service.ServiceFailureBlock) -> WebTransportOperation {
    // ...
}
```

Пример выше демонстрирует следующие стили:

 + Указывайте типы для свойств, переменных, констант, объявлений аргументов и других операторов с пробелом после двоеточия, например. `x: Int` и `Circle: Shape`.
 + Определяйте несколько переменных и структур в одной строке, если они имеют общую цель/контекст
 + Определение геттера и сеттера, а также наблюдателя свойств
 + Не добавляйте модификаторы доступа, такие как `internal`, если они уже установлены по умолчанию. Аналогичным образом не следует повторять модификатор доступа при переопределении метода
 + Организовывайте дополнительную функциональность (например, печать) в расширениях
 + Скрывайте данные реализации, не связанные с общим доступом, такие как `centerString` внутри расширения с помощью частного контроля доступа

### Использование self

Для краткости кода избегайте использования `self`, так как Swift не требует доступа к свойствам объекта или вызова его методов.

Используйте `self` только тогда, когда это требуется компилятором (в сбегающих `@escaping` замыканиях или в инициализаторах для устранения неоднозначности свойств из аргументов). Другими словами, если код компилируется без `self`, опустите его.


### Вычисляемые свойства

Если вычисляемое свойство доступно только для чтения, опустите `get`. Предложение `get` требуется только тогда, когда предоставляется предложение `set`.

**Рекомендуется:**
```swift
var diameter: Double {
    return radius * 2
}
```

**Не рекомендуется:**
```swift
var diameter: Double {
    get {
        return radius * 2
    }
}
```

Всегда используйте вычисляемое свойство вместо метода без аргументов и побочных эффектов.

**Рекомендуется:**

```swift
var homeFolderPath: Path {
    //...
    return path
}
```

**Не рекомендуется:**

```swift
func homeFolderPath() -> Path {
    //...
    return path
}
```

### Использование Final

Маркировка классов как `final` в примерах может отвлекать от основной темы и не всегда требуется. Тем не менее, использование `final` может иногда уточнять ваши намерения и стоит затрат. В приведенном ниже примере `Box` имеет особую цель, и расширение в виде производного класса не предназначено. Пометка `final` делает это более понятным.

```swift
// Turn any generic type into a reference type using this Box class.
final class Box<T> {
    let value: T
    init(_ value: T) {
        self.value = value
    }
}
```

Также это улучшает чтение кода и ускоряет компиляцию.

## Объявление функций

Сохраняйте короткие объявления функций или типов на одной строке, включая открывающую фигурную скобку:

```swift
func reticulateSplines(spline: [Double]) -> Bool {
    // ...
}
```

Для функций надо добавить переносы строк для каждого аргумента, если они превышают руководство по ширине страницы:

```swift
func reticulateSplines(spline: [Double], 
    		       adjustmentFactor: Double,
    		       translateConstant: Int, 
    		       comment: String) -> Bool {
    // ...
}
```

или если параметры выходят за пределы ширины страницы:
	
```swift
func reticulateSplines(
    veryLoooooooooooooooooooooooooooooooooooongParameter: [Double], 
    adjustmentFactor: Double,
    translateConstant: Int, 
    comment: String) -> Bool {
    //...
}
```

Это же правило применяется и для вызовов функций.

Оставьте одну или две пустые строки между функциями и одну после `MARK`

```swift
func colorView() {
    //...
}

func reticulate() {
    //...
}


// MARK: - Private Methods

private func retry() {
    //...
}
```

## Выражения замыканий

Используйте синтаксис закрывающего замыкания только в том случае, если в конце списка аргументов имеется одно замыкание. При этом, укажи понятное имя для замыкания. Тип и скобки замыкания должны быть опущены.

**Рекомендуется:**
```swift
UIView.animate(withDuration: 1.0) {
    // ...
}

UIView.animate(
    withDuration: 1.0,
    animations: {
        // ...
},
    completion: { finished in
        // ...
})
```

**Не рекомендуется:**
```swift
UIView.animate(withDuration: 1.0, animations: {
    // ...
})

UIView.animate(withDuration: 1.0, animations: {
    // ...
}) { (f: Bool) in
    // ...
}
```

Для замыканий с одним выражением, где контекст понятен, используйте неявный возврат:

```swift
attendeeList.sort { a, b in
    a > b
}
```

Связанные методы с использованием закрывающих замыканий должны быть понятными и легко читаемыми. Примеры:

```swift
let value = numbers
    .map {$0 * 2}
    .filter {$0 > 50}
    .map {$0 + 10}
```

## Типы

Всегда используйте родные типы Swift, когда они доступны. Swift предлагает ассоциацию к типам Objective-C, поэтому вы можете использовать полный набор методов по мере необходимости.

**Рекомендуется:**
```swift
let width: Double = 120 // Double
let widthString = (width as NSNumber).stringValue // String
```

**Не рекомендуется:**
```swift
let width: NSNumber = 120.0 // NSNumber
let widthString: NSString = width.stringValue // NSString
```

В коде SpriteKit используйте `CGFloat`, т.к. он сделает код более кратким, избегая слишком большого количества преобразований.

### Константы

Константы определяются с помощью ключевого слова `let`, а переменные – `var`. Всегда используйте `let` вместо `var`, если значение переменной не изменится.

**Совет**: Есть неплохая техника, когда определяете объекты, используя `let`, и изменяете его на `var`, если компилятор начнет ругаться.

Вы можете определить константы для типа, а не для экземпляра этого типа, используя свойства типа. Чтобы объявить свойство типа как константу, просто используйте `static let`. Свойства типа, объявленные таким образом, намного лучше, чем глобальные константами, потому что их легче отличить от свойств экземпляра. Пример:

**Рекомендуется:**
```swift
enum Math {
    static let e = 2.718281828459045235360287
    static let root2 = 1.41421356237309504880168872
}

let hypotenuse = side * Math.root2
```

**Не рекомендуется:**
```swift
let e = 2.718281828459045235360287  // портит глобальное пространство имен
let root2 = 1.41421356237309504880168872

let hypotenuse = side * root2 // что за root2?
```

Если уж приспичило использовать глобальное пространство имен, то советуем именовать в следующем формате:

**Рекомендуется:**

```swift
let TopMargin: CGFloat = 10
```

**Не рекомендуется:**

```swift
let kTopMargin: CGFloat = 10
let bottomMargin: CGFloat = 100 
```

### Статические методы и свойства типа переменных

Статические методы и свойства типа работают аналогично глобальным функциям и глобальным переменным, также должны использоваться экономно. Они полезны, когда функциональность привязана к определенному типу или когда требуется совместимость с Objective-C.

### Необязательные значения

Можно объявлять переменные и возвращаемые типы функций как необязательные `?`, где допустимо значение `nil`.

Используйте неявно развернутые типы, объявленные с помощью `!` только тогда, когда заранее известно, что они будут инициализированы раньше, чем будут использованы. Например, `subviews`, которые будут установлены во `viewDidLoad`.

При доступе к необязательному значению используйте необязательную цепочку:

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

Используйте также дополнительную связку `if let`, когда удобнее разворачивать значение и выполнять несколько операций:

```swift
if let textContainer = self.textContainer {
    // ...
}
```

При наименовании необязательных переменных и свойств избегайте указывать их необязательность, например, `optionalString` или `maybeView`, поскольку их необязательность уже указана в объявлении типа.

Для объектов развернутых типов не используй такие имена, как `unwrappedView` или `actualLabel`. Также лучше добавить новую строку после if, если в разделе разворачивания есть несколько переменных.

**Рекомендуется:**
```swift
var subview: UIView?
var volume: Double?

// позднее...
if let subview = subview, 
    let volume = volume {
    // что-то сделать с развернутыми subview и volume
}
```

**Не рекомендуется:**
```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
    if let realVolume = volume {
        // что-то сделать с развернутыми unwrappedSubview и realVolume
    }
}
```

### Ленивая инициализация

Рассмотрите возможность использования ленивой инициализации для более тонкого контроля за временем жизни объекта. Это особенно актуально для `UIViewController`, который лениво загружает вьюшки. Можно использовать замыкание, которое немедленно вызовится `{ }()` или вызвать фабричный метод. Пример:

```swift
lazy var locationManager: CLLocationManager = self.makeLocationManager()

private func makeLocationManager() -> CLLocationManager {
    let manager = CLLocationManager()
    manager.desiredAccuracy = kCLLocationAccuracyBest
    manager.delegate = self
    manager.requestAlwaysAuthorization()
    return manager
}
```

**Примечания:**
  - `[unowned self]`здесь не требуется. Retain-цикл не создается
  - У менеджера местоположений есть побочный эффект для появления пользовательского интерфейса, чтобы спросить у пользователя разрешение, поэтому подобный тонкий контроль здесь имеет смысл


### Вывод типа

Всегда предпочитайте компактный код и пусть компилятор сам выводит тип для констант или переменных отдельных экземпляров. Вывод типа также подходит для небольших массивов и словарей. При необходимости можно указать конкретный тип, например `CGFloat` и `Int16`.

**Рекомендуется:**
```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5
```

**Не рекомендуется:**
```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
let names = [String]()
```

#### Тип аннотации для пустых массивов и словарей

Для пустых массивов и словарей не надо использовать аннотацию типа.

**Рекомендуется:**
```swift
var names = [String]()
var lookup = [String: Int]()
```

**Не рекомендуется:**
```swift
var names: [String] = []
var lookup: [String: Int] = [:]
```

**Примечание**: Следуя этому руководству, выбор понятных имен еще более важен, чем раньше. Массивы, желательно, именовать существительным во множественном числе (Например, `names`).


### Синтаксический сахар

Используйте краткие версии объявлений типа, используя синтаксис обобщений.

**Рекомендуется:**
```swift
var deviceModels: [String]
var employees: [Int: String]
var faxNumber: Int?
```

**Не рекомендуется:**
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

### Массивы и словари

Используйте следующий перенос, когда массив или словарь не умещаются по ширине страницы:

**Рекомендуется:**

```swift
let deviceModels = ["BMW": 100, "Mercedes": 120]
let wheels = [
    "Continental", 
    "Vitoria", 
    ...
]
```

**Не рекомендуется:**

```swift
let wheels = ["Continental", "Vitoria", // параметры, превышающие ширину страницы]
```

## Функции против методов

Свободные функции, которые не привязаны к классу или типу, должны использоваться крайне осторожно. Когда это возможно, используйте метод вместо свободной функции. Это помогает в удобочитаемости и открытости.

Свободные функции наиболее подходят, если они не связаны с каким-либо конкретным типом или экземпляром.

**Рекомендуется**
```swift
let sorted = items.mergeSorted()  // легко найти
rocket.launch()  // воздействует только на модель
```

**Не рекомендуется**
```swift
let sorted = mergeSort(items)  // сложно найти
launch(&rocket)
```

**Исключения для свободных функций**
```swift
let tuples = zip(a, b)
let value = max(x, y, z)
```

## Управление памятью

Код не должен создавать retain-циклы. Нужно проанализировать график объектов (Xcode / Debug navigator / View Memory Graph Hierarchy) и предотвратить strong-циклы с помощью weak или unowned ссылками. Кроме того, можно использовать типы значений  (`struct`, `enum`) для предотвращения циклов.

### Продление срока жизни объекта

Продлите срок жизни объекта, используя `[weak self]` и ```guard let `self` = self else { return }``` (если это необходимо). Лучше использовать `[weak self]`, нежели `[unowned self]`, т.к. не всегда понятно, что `self` к этому моменту будет жить. Продление срока жизни объекта – это рекомендуемый вариант для необязательного разворачивания.

**Рекомендуется**
```swift
resource.request().onComplete { [weak self] response in
    guard let `self` = self else { return }
    let model = strongSelf.updateModel(response)
    strongSelf.updateUI(model)
}
```

**Не рекомендуется**
```swift
// будет сбой, если self будет освобожден (выгружен из памяти) в момент выполнения замыкания
resource.request().onComplete { [unowned self] response in
    let model = self.updateModel(response)
    self.updateUI(model)
}
```

**Не рекомендуется**
```swift
// освобождение self может произойти между обновлением модели и обновлением пользовательского интерфейса, если обновление модели происходит не в главном потоке
resource.request().onComplete { [weak self] response in
    let model = self?.updateModel(response)
    self?.updateUI(model)
}
```

## Контроль доступа

Аннотации полного контроля доступа в учебниках могут отвлекать от основной темы и в общем-то не требуются. Однако использование `private` и `fileprivate`, таким образом, добавляет ясности и способствует инкапсуляции. Лучше использовать `private`, нежели `fileprivate`, если это возможно. Использование расширений может потребовать использования `fileprivate`.

Можно явно использовать `open`, `public` и `internal`, когда вам требуется полная спецификация контроля доступа.

Используйте контроль доступа в качестве первичного спецификатора свойств. Исключения составляют лишь `static` или атрибуты, такие как `@IBAction`, `@IBOutlet`, `@discardableResult` и `@IBInspectable`.

**Рекомендуется:**
```swift
private let message = "Great Scott!"

class TimeMachine {
    fileprivate dynamic lazy var fluxCapacitor = FluxCapacitor()
}
```

**Не рекомендуется:**
```swift
fileprivate let message = "Great Scott!"

class TimeMachine {
    lazy dynamic fileprivate var fluxCapacitor = FluxCapacitor()
}
```

## Поток управления

Лучше использовать стиль `for-in` для цикла `for`, нежели стиль `while-condition-increment`.

**Рекомендуется:**
```swift
for _ in 0..<3 {
    print("Hello three times")
}

for (index, person) in attendeeList.enumerated() {
    print("\(person) is at position #\(index)")
}

for index in stride(from: 0, to: items.count, by: 2) {
     print(index)
}

for index in (0...3).reversed() {
     print(index)
}
```

**Не рекомендуется:**
```swift
var i = 0
while i < 3 {
    print("Hello three times")
    i += 1
}


var i = 0
while i < attendeeList.count {
    let person = attendeeList[i]
    print("\(person) is at position #\(i)")
    i += 1
}
```

## Золотой путь

При написании кода с условными обозначениями левый край кода должен быть, как говорят, «золотым». То есть, не надо добавлять лишние операторы `if`, заменив их на операторы множественного возврата. Для этого был создан оператор `guard`.

**Рекомендуется:**
```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {
    guard let context = context else { throw FFTError.noContext }
    guard let inputData = inputData else { throw FFTError.noInputData }
    // используем context и inputData для подсчета frequencies
    return frequencies
}
```

**Не рекомендуется:**
```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {
    if let context = context {
        if let inputData = inputData {
        	// используем context и inputData для подсчета frequencies
       		return frequencies
        } else {
        	throw FFTError.noInputData
        }
    } else {
        throw FFTError.noContext
    }
}
```

## Использование guard

Когда в оператор входят несколько условных выражений, надо добавить перенос строки перед новым операндом. Сложные условия лучше перенести в отдельные функции.

**Рекомендуется:**

```swift
if isTrue(),
    isFalse(),
    tax > MinTax {
    // ...
}
```

**Не рекомендуется:**

```swift
if a > b && b > c && tax > MinTax && currentValue > MaxValue {
    // ...
}
```

Когда несколько опций раскрываются через `guard` или `if let`, то вложенность заметно сокращается. Пример:

**Рекомендуется:**
```swift
guard
    let number1 = number1,
    let number2 = number2,
    let number3 = number3
    else { 
	clean()
	fatalError("impossible") 
}
// что-то делаем
```

или можно использовать вариант `else` в одну строку, если внутри всего один оператор

```swift
guard 
    let number1 = number1, 
    let number2 = number2
    else { return }
// что-то делаем
```

или однострочный вариант, если условие всего одно

or one-liner with one `let` operation

```swift
guard let number1 = number1 else { return }
```

**Не рекомендуется:**
```swift
if let number1 = number1 {
    if let number2 = number2 {
        if let number3 = number3 {
            // что-то делаем
        } else {
            fatalError("impossible")
        }
    } else {
        fatalError("impossible")
    }
} else {
    fatalError("impossible")
}
```

### Обработка guard

Операторы guard обязаны каким-то образом обозначить выход. Как правило, это должен быть простой однострочный оператор, такой как `return`, `throw`, `break`, `continue`, или `fatalError()`. Следует избегать больших блоков кода. Если для нескольких точек выхода требуется код очистки, стоит использовать `defer`, чтобы избежать дублирования кода очистки.

## Точки с запятой

Swift не требует точки с запятой после каждого оператора в вашем коде. Они необходимы только в том случае, если вы хотите объединить несколько операторов в одной строке. Но это делать крайне нежелательно.

**Рекомендуется:**
```swift
let swift = "not a scripting language"
```

**Не рекомендуется:**
```swift
let swift = "not a scripting language";
```

**Примечание**: Swift is very different from JavaScript, where omitting semicolons is [считается опасным занятием](http://stackoverflow.com/questions/444080/do-you-recommend-using-semicolons-after-every-statement-in-javascript)

## Круглые скобки

Скобки вокруг выражений не требуются и их следует опустить.

**Рекомендуется:**
```swift
if name == "Hello" {
    print("World")
}
```

**Не рекомендуется:**
```swift
if (name == "Hello") {
    print("World")
}
```

В больших выражениях необязательные круглые скобки иногда могут сделать код более понятным.

**Рекомендуется:**
```swift
let playerMark = (player == current ? "X" : "O") && player.active
```

## Формат организации и Bundle Identifier

В случае использования проекта Xcode организация должна быть настроена на вашу компанию/имя, также Bundle Identifier должен быть в формате `domain.companyName.appName`,
где
domain – доменная зона проекта, обычно соотствует доменной зоне сайта компании, например, com,
companyname – наименование компании (обычно в нижнем регистре),
appName – название вашего проекта (обычно в формате UpperCamelCase)

![Xcode Project settings](screens/project_settings.png)

## Ссылки

* [The Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
* [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)
* [Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html)
* [Swift Standard Library Reference](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/SwiftStandardLibraryReference/index.html)
