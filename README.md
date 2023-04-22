# События (лекция)
[** источник](https://www.youtube.com/watch?v=VJEpE6DaOYo)
## Events
> • это действия, или случаи, возникающие в программируемой вами системе, о которых система сообза9ет вам для того, чтобы вы могли с ними взраимодействовать. (_MDN_)  
> Другими словами – это один из способов взаимодействия объектов, компонентов, или частей приложения.

_Другое определение, более абстрактное:_  
> • это объект, обладающий свойствами и методами. (то есть, перехватив какое-то событие, дальнейшее обращение с ним происходит, как с объектом).


## Interface
Рассматривая события, стоит понимать, что событие – это объект, который унаследован от интерфейса Event.
> • **Интерфейс** – это некая декларация действия или контракт того, что может делать объект, и какими признаками он обладает.  

• Одни и те же действия, описанные в интерфейсе, могут иметь различную реализацию у разных событий.  

• Они позволяют использовать объекты, не зная подробностей их реализации.  

### Интерфейсы, основанные на Событии:
- события MouseEvents  
- события клавиатуры KeyboardEvents  
- события анимации AnimationEvents  
- события отправки запросов FetchEvents  
- события таймеров TimeEvents  
- события ошибок ErrorEvents
- пользовательские события CustomEvents  
- множество иных, предопределённых событий  

## Функции обратного вызова Callback()
• это функция, переданная в другую функцию в качестве аргумента, которая затем вызывается по завершению какого-либо действия (_MDN_)  
• поскольку в JS функции – это объекты, то можно передавать их другим функциям в качестве аргументов.  
• чаще всего, callback() используются для продолжения выполнения кода после завершения асинхронных операций.  

```javascript
// --- Пример Callback()
function summ(numOne, numTwo) {
  console.log(`${numOne} + ${numTwo}`);
  return numOne + numTwo;
}

function substract(numOne, numTwo) {
  console.log(`${numOne} - ${numTwo}`);
  return numOne - numTwo;
}

function action(numbers, callbacks) {
  let result = numbers[0];
  for (let i = 0; i < numbers.length; i++) {
    const currentCallback = callbacks[i - 1];
    const number = numbers[i];
    result = currentCallback(result, number);
    console.log(`${result}`);
  }
  return result;
}
const numbers = [1, 2, 3, 4];
const callbacks = [summ, substract, summ];
const result = action(numbers, callbacks);
console.log(`Result = ${result}`);
```

## Асинхронное программирование  
> • **Асинхронность** – это концепция, заключающаяся в том, что результат выполнения функции доступен не сразу, а через некоторое время в виде некоторого асинхронного вызова, нарушаюшего обычный, последовательный порядок выполнения. (_MDN_)  
  
Другими словами этот процесс можно описать следующим образом:  
– это ситуация, при которой ваш алгоритм разветвляется, и одна его часть продлжает синхронно выполненяться, а другая ожидает результата завершения какого-либо действия, и начнёт своё выполнение только после этого.  
Механизм определения последовательности обработки этих "веток" регулируется циклом событий.  
Необходимость асинхронности обусловлена однопоточностью языка в браузерной среде исполнения.  
```javascript
// --- Пример Синхронного выполнения кода

function summ() {
  let counter = 0;
  for (let i = 0; i < 100000; i++) {
    // какие-то вычисления
    counter += 1;
  }
  console.log(`Завершение длительных вычислений. (2)`, counter);
}
console.log("Начало выполнения кода. (1)");
summ();
console.log("Завершение выполнения кода (3)");

```

### Где возникает асинхронный код
- в файлах таймеров и интервалов
- в функциях анимаций
- в обработчиках событий
- в объектах Promise
- в конструкциях asyn/await  

```javascript
// --- Пример Асинхронного выполнения кода

async function summ() {
  let result = await (function () {
    let counter = 0;
    for (let i = 0; i < 100000; i++) {
      // какие-то вычисления
      counter += 1;
    }
    return counter;
  })();
  console.log("Завершение длительных вычислений. (2)", result);
}
console.log("Начало выполнения кода. (1)");
summ();
console.log("Завершение выполнения кода. (3)");
```  
```javascript
// --- Пример Асинхронного Исполнения функции

function timer() {
  setTimeout(() => {
    console.log("Завершение длительных вычислений. (2)");
  }, 1000);
}
console.log("Начало выполнения кода. (1)");
timer();
console.log("Завершение выполнения кода. (3)");
```
# Обработчики событий
Способы указания обработчика событий:  
• Атрибутом в HTML разметке с указанием функции-обработчика
```html
<button onclick="counter+=1; if(counter>3) this.setAttribute('diabled')">
    Добавлен в разметке
</button>
```
- может быть добавлен единожды
- код обработки смешивается с разметкой
- трудно поддерживать в дальнейшем
- является устаревшим  

• Назначить определённому свойству DOM-элемента функцию-обработчик, которая сработает при вызове этого свойства  
```javascript
let button = document.getElementById('buttonProperty');
button.onclick = function propertyHandler() {
  // Обработка события
}
```
- этот вариант очень похож на предыдущий
- каждое новое присвоение перезаписывает предшествуещее
- сработает только последнее присвоение
- является устаревшим  

##### _Как отменить обработчик_
Сделать это достаточно просто (применимо к этим двум вариантам указаний обработчика событий):  
```javascript
let button = document.getElementById('buttonProperty');
button.onclick = null;
```  
• С использованием метода `addEventListener()` тремя способами:
##### **Способ 1 "Стрелочная функция"**
```javascript
let button = document.getElementById('buttonTargetFunc');
button.addEventListener('click', () => {
  // обработка события
});
```  
##### **Способ 2 "Callback()-функция"**
```javascript
let button = document.getElementById('buttonTargetFunc');
button.addEventListener('click', clickHandler);
function clickHandler() {
  // обработка события
};
```  
##### **Способ 3 "Объект, содержащий метод"**
```javascript
const clickObject = {
  handleEvent: function (event){
    // обработка события
  }
}
let button = document.getElementById('buttonTargetFunc');
button.addEventListener('click', clickObject);
```  
## Множество обработчиков
```javascript
let button = document.getElementById('buttonManyListener');
button.addEventListener('click', () => {
  console.log('Первый обработчик (1)')
});
button.addEventListener('click', () => {
  console.log('Второй обработчик (2)')
});
button.addEventListener('click', () => {
  console.log('Третий обработчик (3)')
});
```  
Обработка событий произойдёт в том порядке, в котором происходила регистрация слушателей.  
## Разные события элемента
```javascript
let button = document.getElementById('buttonDifferentEvents');
button.addEventListener('click', () => {
  console.log('Первый обработчик (1)')
});
button.addEventListener('mouseenter', () => {
  console.log('Второй обработчик (2)')
});
button.addEventListener('mouseout', () => {
  console.log('Третий обработчик (3)')
});
```  
## Отменить обработчик события
За удаление конкретного события отвечает метод `.removeEventListener()`
```javascript
let button = document.getElementById('buttonRemoveFunc');
button.addEventListener('mouseenter', mouseEnterHandler)
button.addEventListener('mouseout', mouseOutHandler)
function mouseEnterHandler() {
  console.log('Первый обработчик (1)');
}
function mouseOutHandler() {
  console.log('Второй обработчик (2)');
}
button.addEventListener('click', () => {
  button.removeEventListener('mouseenter', mouseEnterHandler);
});
```  
С удаление обработчика события есть некоторые нюансы:  
```javascript
let button = document.getElementById('buttonRemoveFunc');
button.addEventListener('mouseenter', () => {
  console.log('Первый обработчик (1)');
});
button.addEventListener('mouseout', mouseOutHandler);
function mouseOutHandler() {
  console.log('Второй обработчик (2)');
}
button.addEventListener('click', () => {
  button.removeEventListener('mouseenter', () => {
    console.log('Первый обработчик (1)');
  });
  button.removeEventListener('mouseout', mouseOutHandler);
});
```  
Здесь происхоит следующее:  
обработчик `mouseout` удаляется по `click`, однако `mouseenter` остаётся.  
Это происходит потому, что метод `removeEventListener` просит, чтобы мы передавали ему именнованную функцию. То есть, если был повешен обработчик событий с анонимной, или стрелочной функцией, то убрать обработчик события не получится.
## Слушатели события (резюме)
- Атрибут в HTML-разметке
- Свойство DOM-элемента
- Регистрация слушателя
- На одном элементе одно событие может обрабатываться несколько раз
- На одном элементе возникают разные события
- Обработчиком события может быть анонимная, стрелочная, или именнованная функция
- Обработчиком может быть Объект, который реализоваывает метод `handleEvent()`
- Обработчики исполняются в порядке их регистрации
- Удалить обработчик с анонимной или стрелочной функцией нельзя

# Всплытие и перехват событий
• Всплытие и перехват событий – это два механизма, описывающих то, что происходит, когда два обработчика одного и того же события активируются на одном элменте. (_MDN_)  

• **Событие** имеет три стадии:
- погружение
- целевая
- всплытие  

• Событие возникает на корневом элементе страницы  
• Погружается через все дочерние элементы до целевого элемента  
• Выполняется на целевом элементе  
• Всплывает через все родительские элементы до корневого элемента  
• Прекращает своё существование