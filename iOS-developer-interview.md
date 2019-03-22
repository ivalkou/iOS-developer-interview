# Swift

## Уровень Middle

<details>
<summary>Чем value type отличается от reference type?</summary>
    
* value type передаётся / присваивается по значению, т.е. каждая переменная получает свою копию объекта.
* reference type передаётся / присваивается ссылке, т.е. все переменные указывают на одну копию объекта.
</details>

<details>
<summary>Какие value type есть в Swift?</summary>

* `struct`
* `enumeration`
</details>

<details>
<summary>Как устроены типы `Int`, `Float`, `Bool`?</summary>
"Простые" типы: `Int`, `Float`, `Bool` и тд. являются структурами
</details>

<details>
<summary>Какие reference type есть в Swift?</summary>

* `class`
* `closure`
* `recursive enumeration`
</details>

<details>
<summary>Что такое ARC?</summary>

Система управления памятью на основе подсчёта количества сильных ссылок (счётчик ссылок) на объект.
</details>

<details>
<summary>Для каких типов используется ARC?</summary>

reference type
</details>

<details>
<summary>Что такое retain cycle (strong reference cycle)?</summary>

Ситуация, при которой 2+ объекта держат сильные ссылки друг на друга, не позволяя счётчикам ссылок занулиться и освободить память.
</details>

<details>
<summary>Как избежать retain cycle?</summary>

 * `weak` reference
 * `unowned` reference
</details>

<details>
<summary>Почему `weak` reference объявляется как `var` а не `let`?</summary>

`ARC` запишет в эту ссылку `nil` при удалении объекта.
</details>

<details>
<summary>Вызовется ли при этом property observer (`didSet`)?</summary>

Нет
</details>

<details>
<summary>В каком ещё случае не вызовется property observer (`didSet`)?</summary>

* `init`
* `didSet`

</details>

<details>
<summary>Чем отличается `weak` от `unowned`?</summary>

* `weak`: `ARC` запишет в эту ссылку `nil` при удалении объекта.
* `unowned`: `ARC` НЕ запишет в эту ссылку `nil` при удалении объекта.
</details>

<details>
<summary>Что произойдёт, если обратиться к объекту по `unowned` reference после его удаления?</summary>

Падение приложения / `runtime error`
</details>

<details>
<summary>Как решить что использовать: `weak` или `unowned`?</summary>

* `unowned` - более производительный вариант, нужно использовать в тех местах, где есть уверенность что объект не будет удалён.
* `weak` - во всех остальных случаях.
</details>

<details>
<summary>Что такое `autoreleasepool`?</summary>


* В `Objective-C`, `NSAutoreleasePool` - объект, в который помещаются ссылки на объекты. При `drain` делает `release` для каждого `autorelease` посланного объекту.
* В `Swift` - функция, принимающая на вход замыкание. После выполнения замыкания делает тоже самое для `Objective-C` объектов.
</details>

<details>
<summary>Что такое `enumeration`?</summary>

Value type, который может принимать одно из заданных значений (case).
</details>

<details>
<summary>Что такое `associated values`?</summary>

Значение `enumeration` может иметь кортеж ассоциированных с ним значений любого типа.
</details>

<details>
<summary>Что такое `indirect case`?</summary>

Кортеж ассоциированных значений может содержать другое значение из этого же `enumeration`.
</details>

<details>
<summary>Что такое `Optional`?</summary>

`enumeration` с 2мя значениями: `case none`, `case some(Wrapped)`
</details>

<details>
<summary>Чем `nil` отличается от `Optional.none`?</summary>

* `nil` - синтаксический сахар для `Optional.none`
* Возможно из-за того, что `Optional` реализует протокол `ExpressibleByNilLiteral`.
</details>

<details>
<summary>Как получить значение из Optional?</summary>

* `!`
* `if let`
* `guard let`
* `??`
* `switch`
* `map`
* `flatMap`
</details>

<details>
<summary>Какие есть базовые коллекции в Swift?</summary>

* `Array`
* `Dictionary`
* `Set`
</details>

<details>
<summary>Какого они типа: class, struct, enum?</summary>

* struct
</details>

## Уровень Senior

<details>
<summary>Почему NSArray типа class, а Array - struct?</summary>

Сделано специально, чтобы избежать ошибок из-за изменения массива из другого потока при работе с ним на текущем потоке.
</details>

<details>
<summary>Если Array - struct, значит при передаче в качестве параметра он копируется. Почему это работает быстро?</summary>

`Copy-on-Write`
</details>

<details>
<summary>Как реализован `Array` с `Copy-on-Write`?</summary>

Внутри `Array` хранится класс `ManagedBuffer`, который отвечает за фактическое хранение данных.
При каждой модификации `Array` делается проверка `isKnownUniquelyReferenced`. Если на объект более одной ссылки, делается копия.
</details>

<details>
<summary>Чем отличается `@objc` от `dynamic`?</summary>

* `@objc` - метод доступен из `Objective-C`. В `Objective-C` вызывается динамически, в `Swift` - статически.
* `dynamic` - метод вызывается в `Swift` динамически.
</details>


# Замыкания

## Уровень Middle

<details>
<summary>Что такое closure (замыкание)?</summary>

Отдельный блок кода, который можно передавать как параметр и вызывать в процессе выполнения программы.
</details>

<details>
<summary>Что будет напечатано?
	    
```Swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
	var runningTotal = 0
	func incrementer() -> Int {
	    runningTotal += amount
	    return runningTotal
	}
	return incrementer
 }

let incrementByTen = makeIncrementer(forIncrement: 10)
print(incrementByTen())
print(incrementByTen())
print(incrementByTen())

let incrementBySeven = makeIncrementer(forIncrement: 7)
print(incrementBySeven())

print(incrementByTen())

let alsoIncrementByTen = incrementByTen
print(alsoIncrementByTen())

print(incrementByTen())
```

</summary>
10 20 30 7 40 50 60
</details>

<details>
<summary>Что такое `escaping` замыкания?</summary>

Замыкания, помеченные как `escaping` параметр функции, могут быть вызваны после завершения функции.
</details>

## Уровень Senior

<details>
<summary>Что такое `autoclosure`?</summary>

* `autoclosure` - замыкание, которое автоматически создаётся для того, чтобы обернуть выражение, переданное в качестве аргумента при вызове функции.
* `autoclosure` позволяет отложить вычисление выражения с момента вызова до того момента, когда значение понадобится.
</details>

<details>
<summary>Что делает этот код?

```Swift
func allValues(in array: [Int], match predicate: (Int) -> Bool) -> Bool {
	return array.lazy.filter { !predicate($0) }.isEmpty
}
```
</summary>

Проверяет, что все элементы массива удовлетворяют предикату.
</details>

<details>
<summary>Какая ошибка есть в этом коде?</summary>

Ошибка компиляции: `error: closure use of non-escaping parameter 'predicate' may allow it to escape`

`non-escaping` замыкание используется как `escaping`
</details>

<details>
<summary>Как исправить?</summary>

```Swift
func allValues(in array: [Int], match predicate: (Int) -> Bool) -> Bool {
    return array.filter { !predicate($0) }.isEmpty
}
```
Минус: Создание промежуточного массива. Лишнее потребление памяти.

```Swift
func allValues(in array: [Int], match predicate: @escaping (Int) -> Bool) -> Bool {
	return array.lazy.filter { !predicate($0) }.isEmpty
}
```
Минус: Замыкание стало escaping, что противоречит сути (оно всегда вызывается до завершения функции `allValues`), и лишает компилятор возможности оптимизировать код.

Правильный вариант:
```Swift
func allValues(in array: [Int], match predicate: (Int) -> Bool) -> Bool {
	return withoutActuallyEscaping(predicate) { escapablePredicate in
		array.lazy.filter { !escapablePredicate($0) }.isEmpty
	}
}
```
</details>

# Многопоточность

## Уровень Middle

<details>
<summary>Какие есть способы реализации многопоточности?</summary>

* `OperationQueue`
* `DispatchQueue`
* `Thread`

</details>

<details>
<summary>Чем они отличаются?</summary>

* `DispatchQueue` - абстракция поверх потоков. Очередь, управляющая выполнением поставленных в неё задач на пуле потоков управляемых системой.
* `OperationQueue` - высокоуровневая надстройка поверх `DispatchQueue`.
	* Предоставляет следующие возможности поверх `DispatchQueue`:
		1. Установка зависимостей между задачами.
		2. Отмена всех операций.
		3. Получение количества операций в очереди.
		4. Динамическое управление количеством одновременно выполняемых операций.
		5. Приостановка выполнения.

</details>

<details>
<summary>Чем отличается `DispatchQueue.sync` от `DispatchQueue.async`?</summary>

* `DispatchQueue.async` - ставит задачу в очередь и продолжает выполнение.
* `DispatchQueue.sync` - ставит задачу в очередь и ждёт, пока эта операция будет выполнена.
</details>

## Уровень Senior

<details>
<summary>Как синхронизировать выполнение нескольких DispatchQueue операций?</summary>

* `DispatchGroup`
* `DispatchQueue.sync(flags: .barrier) {}`
</details>

<details>
<summary>Что будет напечатано?

```Swift
let queue = DispatchQueue(label: "Queue")            
print("1")            
queue.sync {
    print("2")            
    queue.sync { print("3") }
    queue.async { print("4") }            
    print("5")
}            
print("6")
```	    
</summary>

1 2 (Приложение зависнет / упадёт)
</details>

<details>
<summary>Что будет напечатано?

```Swift
let queue = DispatchQueue(label: "Queue", attributes: .concurrent)
print("1")
queue.sync {
    print("2")
    queue.sync { print("3") }
    queue.async { print("4") }
    print("5")
}
print("6")
```	    
</summary>

* 1 2 3 4 5 6
* 1 2 3 5 4 6
* 1 2 3 5 6 4
</details>

<details>
<summary>Что такое RunLoop?</summary>

Программный интерфейс обработки входных источников и операций, запускаемых на потоке, в цикле<br/>
Всегда есть на главном потоке.<br/>
Каждая операция на RunLoop оборачивается в `autoreleasepool`.<br/>
Нужен для работы таймеров.<br/>
Нет у `DispatchQueue`.
</details>

<details>
<summary>Когда чистится память для `DispatchQueue`?</summary>

Зависит от параметра `DispatchQueue.AutoreleaseFrequency` при создании очереди.
Стандартное поведение - когда у очереди нет активных задач.
</details>
 
# iOS

## Уровень Middle

<details>
<summary>Какие есть состояния жизненного цикла iOS приложения?</summary>

* Not running
* Inactive
* Active
* Background
* Suspend
</details>

<details>
<summary>Какие системные события приложение может/должно обрабатывать?</summary>

* **Memory warning**
* **Protected data becomes available/unavailable**
* **State restoration**
* **Open URLs**
* **Local/remote notifications**
* **Location changes**
* **Application shortcuts**
* AV sessions
* File download
* Handoff tasks
* Inter-app communication
</details>

<details>
<summary>Жизненный цикл `UIViewController`.</summary>

* `init`
* `viewDidLoad`
* `viewWillAppear`
* `viewDidAppear`
* `viewWillLayoutSubviews`
* `viewDidLayoutSubviews`
* `viewWillDisappear`
* `viewDidDisappear`
* `willMove(toParent:)`
* `didMove(toParent:)`
* `deinit`
</details>

<details>
<summary>Какая разница между использованием делегатов и нотификаций?</summary>

Общее:
* Получение уведомлений при каком-то изменении наблюдаемого объекта.

Различия:
* Делегирование позволяет передать часть логики другому объекту. Этот объект один и отвечает только на те вопросы, которые ему задают.
* Нотификации позволяют подписаться на любые изменения наблюдаемого объекта.
* Количество подписчиков на нотификации не ограничено.
* Подписчики не влияют на логику поведения наблюдаемого объекта.
</details>

<details>
<summary>Какие есть способы подписки на нотификации?</summary>

* `NotificationCenter`
* `NSKeyValueObserving`
</details>

<details>
<summary>Чем отличается `NotificationCenter` от `NSKeyValueObserving`?</summary>

* `NotificationCenter` - ручная посылка нотификаций всем подписчикам. Для подписки нужен доступ к наблюдаемому классу, чтобы посылать нотификации.
* `NSKeyValueObserving` - все наследники NSObject автоматически посылают сообщения всем своим подписчикам при изменении своих полей. Можно подписаться на изменение полей любого класса.
</details>

<details>
<summary>Нужно ли вызывать `NotificationCenter.removeObserver`?</summary>

Есть 2 способа подписки: через селектор и через замыкание (блок).
Если используются замыкания - отписываться обязательно нужно.
Если используется селектор, начиная с iOS 9.0 это не обязательно. До iOS 9.0, произойдёт падение, если объект удаляется, пока у него есть подписчики.
</details>

## Уровень Senior

<details>
<summary>Что будет напечатано?

```Swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey : Any]? = nil) -> Bool {
	print("1")
	DispatchQueue.main.sync { print("2") }
	print("3")
	return true
}
```
</summary>

1 (Приложение зависнет / упадёт)
</details>

<details>
<summary>Что такое `NSKeyValueCoding`?</summary>

Механизм непрямого доступа к полям любого наследника `NSObject` по имени или ключу.
</details>

# UI

## Уровень Middle

<details>
<summary>Чем `UIView` отличается от `CALayer`?</summary>

* `CALayer` отвечает за представление информации на экране.
* `UIView` содержит в себе `CALayer`, отвечает за взаимодействие с пользователем (`UIResponder`) и участвует в расчёте геометрии представления на экране (layout).
</details>

<details>
<summary>Чем `frame` отличается от `bounds`?</summary>

* `frame` - координаты UIView в родительской системе координат.
* `bounds` - координаты видимой области в собственной системе координат.
</details>

<details>
<summary>Как сделать закруглённые края у UIView?</summary>

1.
```Swift
clipsToBounds = true
layer.cornerRadius = *value*
```
Минус: часто пересчитывается.
   
2.
```Swift
mask = MaskView()
MaskView.layerClass = CAShapeLayer.self
shapeLayer.path = UIBezierPath
```
Минус: Обновление маски только при изменении frame

3.
```Swift
layer.cornerRadius = *value*
layer.maskedCorners = [.layerMinXMinYCorner, .layerMaxXMinYCorner, ...]
layer.masksToBounds = true
```
Минус: `layer.maskedCorners` доступен с iOS 11.0+.
</details>

<details>
<summary>Чем `CALayer.transform` отличается от `UIView.transform`?</summary>

* `CALayer.transform` - `CATransform3D`
* `UIView.transform` - `CGAffineTransform` (2D)
</details>

<details>
<summary>Что поменяется при изменении `CALayer.transform`, `UIView.transform` - `frame` или `bounds`?</summary>

frame
</details>

<details>
<summary>Что такое autolayout, как он работает?</summary>

Система динамического расчёта позиции и размера UIView.
Для определения позиции и размера используются заданные для UIView правила (констреинты).
</details>

<details>
<summary>Что такое hugging и resistance?</summary>
	   
Приоритеты, с которыми `UIView` противостоит попыткам растянуть / сжать её от `intrinsicContentSize`.
</details>

<details>
<summary>Что такое `intrinsicContentSize`?</summary>

Предпочтительный размер `UIView` для отображения всех внутренностей. Не учитывает внешние ограничения.
</details>

<details>
<summary>Как сообщить autolayout, что предпочтительный размер `UIView` изменился?</summary>

Вызвать метод `invalidateIntrinsicContentSize`.
</details>

<details>
<summary>Как анимировать позицию/размер `UIView` при помощи autolayout?</summary>

```Swift
constraint.constant = *value*
UIView.animate(withDuration: <duration>) { self.layoutIfNeeded() }
```
</details>

## Уровень Senior

<details>
<summary>Чем отличаются Layer tree, Presentation tree и Render tree?</summary>

* Layer tree - объекты в этом дереве хранят конечные значения анимаций. При изменении свойств слоя используется объект из этого дерева.
* Presentation tree - объекты в этом дереве хранят текущие значения анимаций.
* Render tree - используется для фактической отрисовки. Не доступно для разработчика.
</details>

<details>
<summary>Как работает `UIView.transform` и autolayout?</summary>

autolayout работает с `frame` `UIView` до трансформации.
Значение фрейма после трансформации не определено и должно игнорироваться.
</details>

<details>
<summary>Чем отличаются методы `layoutIfNeeded`, `layoutSubviews`, `setNeedsLayout`?</summary>

* `layoutIfNeeded` - немедленно обновляет layout, если это необходимо. Может начать выше по дереву.
* `layoutSubviews` - непосредственный layout, начиная с текущего `UIView` и ниже по дереву. В документации не рекомендуется вызывать этот метод напрямую.
* `setNeedsLayout` - помечает layout как требующий обновления. layout произойдёт на следующий цикл обновления UI.
</details>

<details>
<summary>Как бы ты написал `hitTest`?</summary>

```Swift
func hitTest(_ point: CGPoint, with event: UIEvent?) -> UIView? {
	guard point(inside: point, with: event) else { return nil }
	guard isUserInteractionEnabled && !isHidden && alpha > 0.01 else { return nil }
	for subview in subviews {
		if let hitView = subview.hitTest(convert(point, to: subview), with: event) {
			return hitView
		}
	}
	return self
}    
```
</details>
