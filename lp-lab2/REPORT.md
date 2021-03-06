#№ Отчет по лабораторной работе №2
## по курсу "Логическое программирование"

## Решение логических задач

### студент: Валов В.В

## Результат проверки

| Преподаватель     | Дата         |  Оценка       |
|-------------------|--------------|---------------|
| Сошников Д.В. |              |               |
| Левинская М.А.|              |               |

> *Комментарии проверяющих (обратите внимание, что более подробные комментарии возможны непосредственно в репозитории по тексту программы)*


## Введение

Решение логических задач имеет 2 основных подхода: метод порождения и проверок и метод ветвей и границ. Главное их свойство в том, что они перебирают набор решений, и проверяют, удовлетворяет ли он заданным условиям. Их различие сводится к методам проверки удовлетворения заданным условиям. Метод проверок можно разделить на две отдельные части: первая часть - генератор: она пытается угадать данные (генерирует множество исходных данных), и второй части - проверяющего. Проверяющий, соответсвенно проверяет пришедшее ему решение. Второй метод серьёзно отличается от первого. В методе ветвей и границ проверяющий и генератор тесно связаны: генерация всех условий происходит не моментально (сразу все), а постепенно, шагами. Значительные части возможных, но неверных, решений отсекаются на ранних шагах.

## Задание

В нашем городе обувной магазин закрывается каждый понедельник, хозяйственный каждый вторник, продовольственный каждый четверг, а парфюмерный магазин работает только по понедельникам, средам и пятницам. В воскресенье все магазины закрыты. Однажды подруги Ася, Ира, Клава и Женя отправились за покупками, причем каждая в свой магазин и притом в один. По дороге они обменивались такими замечаниями. Ася. Женя и я хотели пойти вместе еще раньше на этой неделе, но не было такого дня, чтобы мы обе могли сделать наши покупки. Ира. Я не хотела идти сегодня, но завтра я уже не смогу купить то, что мне нужно. Клава. А я могла бы пойти в магазин и вчера и позавчера. Женя. А я могла бы пойти и вчера и завтра. Скажите, кому какой магазин нужен?

## Принцип решения

Снчала прописываем все условия задачи:
 ```prolog
 open('обувной','вторник').
open('обувной','среда').
open('обувной','четверг').
open('обувной','пятница').
open('обувной','суббота').
close1('обувной','понедельник').

open('хозяйственный','понедельник').
open('хозяйственный','среда').
open('хозяйственный','четверг').
open('хозяйственный','пятница').
open('хозяйственный','суббота').
close1('хозяйственный','вторник').

open('продуктовый','понедельник').
open('продуктовый','вторник').
open('продуктовый','среда').
open('продуктовый','пятница').
open('продуктовый','суббота').
close1('продуктовый','четверг').

open('парфюмерный', 'понедельник').
open('парфюмерный', 'среда').
open('парфюмерный', 'пятница').
close1('парфюмерный', 'вторник').
close1('парфюмерный', 'четверг').
close1('парфюмерный', 'суббота').
 ```
 Находим день недели, когда девочки пошли в магазин.
```Prolog
num([],N,_):- N is 0.
num([L|_],N,Max) :-N is 1, L = Max.
num([_|Ls],N,Max):- num(Ls,N1,Max), N is N1+1.

day([],_,_,_):-!.
day([L|_],N,L,Num):-N=Num.
day([_|Ls],N,S,Num):-Num1 is Num + 1, day(Ls,N,S,Num1).

```
```Prolog
day(L,N,NowDay,1),
```
Предикат day() определяет, что искомый день - среда.

```Prolog
poisk(NextDay,PrevDay, Ob,'обувной',Hz,'хозяйственный',
Prd,'продуктовый', Prf, 'парфюмерный', Zhenya),
poisk(PrevprevDay,PrevDay, Ob,'обувной',Hz,'хозяйственный',
Prd,'продуктовый', Prf, 'парфюмерный', Klava),
findall(X,close1(X,NextDay) ,List1),
..
write('Ася : '), write(Asya), nl,
write('Женя : '), write(Zhenya), nl,
write('Клава : '), write(Klava), nl,
write('Ира : '), write(Ira), nl,
!.
```
Находим кто в каком магазине был.
Основной предикат, который выводит правильный ответ - `work.`

Результаты выполнения программы:
```Prolog
?- work.
Ася : хозяйственный
Женя : обувной
Клава : продуктовый
Ира : парфюмерный
true.
```

## Выводы

Пролог очень удобен для решения логических задач. Подобные задачи человеку решать таким методом, которым их решает Прoлог практически нереально: слишком много возможных вариантов. Вместо этого используются различные приёмы, например, метод исключения. Если говорить о кретивной части решения Прологом, в программе отсутствуют изящные логические выводы и хитрости, задача ломается под мощью перебора (который тем не менее оптимизирован), это хороший пример использования скорости недоступной человеческому мышлению: работа программиста - лишь объяснить задачу машине, и она сию же секунду выдаст правильное решение, а человек может просидеть с даннной задачей пару вечеров.

По поводу повторной используемости данного кода (и применения его в общем и целом), сложно придумать ситуацию, при которой бы возникала необходимость решать большое количество однотипных логических задач - ситуацию в которой нужно применять данную программу. Тем не менее поскольку условия задачи чётко разделяются на типы, можно представить себе генератор, который будет либо создавать задачи подобного плана, для применения, например, в учебных целях, либо решать такие типовые задачи, самостоятельно генерируя проверки из условий задач.





