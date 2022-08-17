# crp-optimization
a little bit of info about CRP (critical rendering path) and code optimization


CRP (critical rendering path) has five phases. 
1. the first one is DOM (Document Object Model);
it works after HTML has been parsed, HTML can request JavaScript, which can modify the DOM.  HTML can also request styles that are involved in creating the CSS Object Model.
The DOM (Document Object Model) Tree is an Object representation of the fully parsed HTML page. Starting with the root element, HTML, nodes are created for each element/text on the page. Elements nested within other elements are represented as child nodes and each node contains the full attributes of that element.
2. the second one is CSSOM (CSS Object Model)
The CSSOM (CSS Object Model) is an Object representation of the styles associated with the DOM. It is represented in a similar way to the DOM, but with the associated styles for each node, whether they are explicitly declared or implicitly inherited, included.
3. rendering phase (when HTML and CSS are combined)
in this phase, Document Object Model and Cascading Styles Sheets are combined. CSS is looking at all the styles that have to represent HTML elements
4. The Layouting phase. determines the size and position of each element on the page. Once the layout is defined
5. the last phase is painting (the pixels are drawn on the screen.)


// Example first
in the first example you can see four nested tags (section, div, ul, li)
it is required to try not to have more than four nested tags in body HTML. to not break one of the rules of the critical rendering path 




 // second example
 what happens in the second example where we wrote div > p...
 CSS interpretation is working from right to left. so at first, it looks all p tags we have in HTML, and only after that does it look for all tags p which is a child of elements that have a glass container. 
 so it is faster to find an element like this
 .item




 // third example
 now let's talk a little bit about colors, which is a better way to style Document Object Model, HEX, RGB, HSL, color name...

This would not be a factor that I'd focus on for improving load time.
CSS can impact performance, but, when you consider all of the other things that can impact web performance, CSS should be at the bottom of the list. And in terms of things to prioritize in CSS performance, hex or RGB values would have the lowest priority.

With that said:

#c0ffee --> 7 characters
RGB(192,255,238) -->16 characters
hsl(164,100%,88%)--17 characters

Another example:

#fff --> 4 characters
white --> 5 characters
#ffffff --> 7 characters
HSL(0,0%,100%) --> 14 characters
rgb(255,255,255) --> 16 characters

All of these color options, except HSL, have a direct RGB equivalent. It is the only one that would require an extra amount of computation. So, you might see a performance issue if you had 1,000 different rulesets assigning colors written in HSL.

No matter what value you give, the browser will compute it to an RGB value. It doesn't matter if it's hex, a keyword, or hsl - it's all RGB at the end of the day.


Hex vs RGB wouldn't matter outside of maybe a few bits in your file size. HSL might cause a performance problem if you used it a lot.

I would instead focus on the length of selectors, their specificity, and even the order of the properties within rulesets.




// notice that
1. tags with display none are off from rendering phase
2. The layout phase is looking at the device which must be open on the web page to understand pixels and regenerate everything into pixels.
example. 70% is being transformed to the device pixels in which the web page must be open, if the device width is 1600px then 70% is equal to 1600 * 70 / 100 which is equal to 1120px. and only after this pixelization of the whole page does the painting phase starts




# crp-оптимизация
Немного информации о CRP (критическом пути рендеринга) и оптимизации кода


CRP (critical rendering path) состоит из пяти фаз. 
1. первая - DOM (Document Object Model);
она работает после разбора HTML, HTML может запрашивать JavaScript, который может изменять DOM.  HTML также может запрашивать стили, которые участвуют в создании объектной модели CSS.
Дерево DOM (Document Object Model) - это объектное представление полностью разобранной HTML-страницы. Начиная с корневого элемента HTML, создаются узлы для каждого элемента/текста на странице. Элементы, вложенные в другие элементы, представлены как дочерние узлы, и каждый узел содержит полные атрибуты этого элемента.
2. второй - CSSOM (CSS Object Model).
CSSOM (CSS Object Model) - это объектное представление стилей, связанных с DOM. Она представлена аналогично DOM, но с включением связанных стилей для каждого узла, независимо от того, объявлены ли они явно или неявно унаследованы.
3. фаза рендеринга (когда HTML и CSS объединяются)
на этом этапе объединяются объектная модель документа и каскадные таблицы стилей. CSS рассматривает все стили, которые должны представлять элементы HTML.
4. Фаза макетирования. определяет размер и положение каждого элемента на странице. После определения макета
5. последняя фаза - рисование (пиксели рисуются на экране).


// Пример первый
в первом примере вы можете видеть четыре вложенных тега (section, div, ul, li)
необходимо стараться не иметь более четырех вложенных тегов в теле HTML. чтобы не нарушить одно из правил критического пути рендеринга 




 // второй пример
 что происходит во втором примере, где мы написали div > p...
 CSS интерпретация работает справа налево. поэтому сначала она ищет все теги p, которые есть в HTML, и только после этого ищет все теги p, которые являются дочерними для элементов, имеющих контейнер class. 
 Таким образом, быстрее найти элемент, подобный этому
 .item




 // третий пример
 теперь давайте немного поговорим о цветах, что лучше стилизовать Document Object Model, HEX, RGB, HSL, название цвета...

Это не тот фактор, на котором я бы сосредоточился для улучшения времени загрузки.
CSS может повлиять на производительность, но, если учесть все остальные вещи, которые могут повлиять на производительность сайта, CSS должен быть в конце списка. И с точки зрения того, что должно быть приоритетным в производительности CSS, шестнадцатеричные или RGB-значения будут иметь самый низкий приоритет.

С учетом сказанного:

#c0ffee --> 7 символов
RGB(192,255,238) --> 16 символов
HSL(164,100%,88%)--17 символов

Другой пример:

#fff --> 4 символа
белый --> 5 символов
#ffffff --> 7 символов
HSL(0,0%,100%) --> 14 символов
RGB(255,255,255) --> 16 символов

Все эти варианты цветов, кроме HSL, имеют прямой эквивалент в RGB. Это единственный вариант, который требует дополнительных вычислений. Таким образом, вы можете столкнуться с проблемой производительности, если у вас есть 1 000 различных наборов правил, назначающих цвета, написанные в HSL.

Неважно, какое значение вы зададите, браузер пересчитает его в значение RGB. Неважно, будет ли это hex, ключевое слово или HSL- в конечном счете, это все RGB.


Hex vs RGB не имеет значения, кроме, может быть, нескольких бит в размере файла. HSL может вызвать проблемы с производительностью, если вы часто его используете.

Вместо этого я бы сосредоточился на длине селекторов, их специфичности и даже на порядке следования свойств в наборе правил.




// замечено
1. теги с display none выключены из фазы рендеринга
2. Фаза верстки смотрит на устройство, на котором должна быть открыта веб-страница, чтобы понять пиксели и регенерировать все в пиксели.
пример. 70% преобразуется в пиксели устройства, на котором должна быть открыта веб-страница, если ширина устройства 1600px, то 70% равно 1600 * 70 / 100, что равно 1120px. и только после этой пикселизации всей страницы начинается фаза рисования.