## В чем суть игры?

Надо написать своего бота для героя, который обыграет других ботов по очкам. Все играют в разных комнатах (смотрите раздел про раунды и комнаты). Герой может передвигаться по свободным ячейкам во все четыре стороны.

## Подключение к серверу и описание поля

Регистрируйтесь на сервер, указав ваш emai.

Далее необходимо подключиться [из кода](http://codingdojo.kz/codenjoy-contest/resources/user/bomberman-servers.zip) к серверу через вебсокеты. Это Maven проект и подойдет он для игры на JVM языках. Также в архиве ты найдешь исходники для других языков. Как его запустить смотри в корне проекта в файле README.txt. Если ты не можешь найти свой язык - придется написать свой клиент (а после пошарить с командой Coding DoJo KZ)

Адрес для подключения к игре на сервере [http://codingdojo.kz](http://codingdojo.kz/)

ws://codingdojo.kz:80/codenjoy-contest/ws?user=3edq63tw0bq4w4iem7nb&amp;code=12345678901234567890

Тут &#39;user&#39; - id игрока, a &#39;code&#39; - твой security token, его ты можешь получить из адресной строки браузера после регистрации/логина

После подключения клиент будет регулярно (каждую секунду) получать строку символов — с закодированным состоянием поля. Формат таков

^board=(.\*)$

с помощью этого regexp можно выкусить строку доски. Вот пример строки от сервера (размер строки и поля может отличаться в настройках игры):

board=☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼ # # # #♥# # # &amp; #☼☼♥☼♥☼♥☼#☼ ☼ ☼ ☼ ☼♥☼ ☼ ☼#☼#☼♥☼#☼#☼☼#♥♥ ♥# # #♥ # ♥# ☼☼ ☼ ☼#☼ ☼♥☼ ☼ ☼#☼ ☼ ☼ ☼ ☼&amp;☼ ☼ ☼ ☼☼ ♥ # # ☼☼ ☼ ☼ ☼ ☼♥☼ ☼ ☼♥☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼☼# # # ☺&amp; 2 # # #☼☼#☼♥☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼ ☼ ☼☼# # ♥# # ♥ # ☼☼ ☼ ☼#☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼☼ #♥ # # ☼☼ ☼ ☼ ☼ ☼♥☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼☼ ## # # # # ♥ ☼☼ ☼ ☼♥☼ ☼ ☼#☼ ☼#☼ ☼ ☼♥☼ ☼ ☼ ☼ ☼ ☼☼ #♥ # ## # ###☼☼ ☼ ☼ ☼#☼ ☼ ☼#☼ ☼ ☼#☼#☼&amp;☼ ☼ ☼ ☼ ☼☼ # # ♣# # ♥ ☼☼ ☼ ☼ ☼♥☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼☼ ## ## ♥ # #☼☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼☼ &amp; ### ##☼☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼☼ ♥ ## ☼☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼♥☼#☼ ☼ ☼ ☼☼ ## &amp;# # ☼☼ ☼ ☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼ ☼ ☼ ☼☼ # # # # &amp; ☼☼♥☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼#☼ ☼☼ # ## &amp; ☼☼ ☼ ☼ ☼ ☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼#☼ ☼☼ # # &amp; # # ☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼

Длина строки равна площади поля. Если вставить символ переноса строки каждые sqrt(length(string)) символов, то получится читабельное изображение поля.
```
☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼
☼ # # # #♥# # # & #☼
☼♥☼♥☼♥☼#☼ ☼ ☼ ☼ ☼♥☼ ☼ ☼#☼#☼♥☼#☼#☼
☼#♥♥ ♥# # #♥ # ♥# ☼
☼ ☼ ☼#☼ ☼♥☼ ☼ ☼#☼ ☼ ☼ ☼ ☼&☼ ☼ ☼ ☼
☼ ♥ # # ☼
☼ ☼ ☼ ☼ ☼♥☼ ☼ ☼♥☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼
☼# # # ☺& 2 # # #☼
☼#☼♥☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼ ☼ ☼
☼# # ♥# # ♥ # ☼
☼ ☼ ☼#☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼
☼ #♥ # # ☼
☼ ☼ ☼ ☼ ☼♥☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼
☼ ## # # # # ♥ ☼
☼ ☼ ☼♥☼ ☼ ☼#☼ ☼#☼ ☼ ☼♥☼ ☼ ☼ ☼ ☼ ☼
☼ #♥ # ## # ###☼
☼ ☼ ☼ ☼#☼ ☼ ☼#☼ ☼ ☼#☼#☼&☼ ☼ ☼ ☼ ☼
☼ # # ♣# # ♥ ☼
☼ ☼ ☼ ☼♥☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼
☼ ## ## ♥ # #☼
☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼
☼ & ### ##☼
☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼
☼ ♥ ## ☼
☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼♥☼#☼ ☼ ☼ ☼
☼ ## &# # ☼
☼ ☼ ☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼ ☼ ☼ ☼
☼ # # # # & ☼
☼♥☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼ ☼#☼#☼ ☼
☼ # ## & ☼
☼ ☼ ☼ ☼ ☼ ☼#☼ ☼ ☼ ☼ ☼ ☼ ☼#☼ ☼#☼ ☼
☼ # # & # # ☼
☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼☼
```

Первый символ строки соответствует ячейке расположенной в левом верхнем углу и имеет координату [0, 32]. В этом примере — позиция бомбермена (символ ☺) — [19, 25]. Левый нижний угол имеет координату [0, 0].

Расшифровка символов на рисунке ниже
```
public enum Element {

/// This is your Bomberman

BOMBERMAN(&#39;☺&#39;), // так выглядит мой бомбер
BOMB\_BOMBERMAN(&#39;☻&#39;), // так выглядит мой бомбер, если он сидит на бомбе
DEAD\_BOMBERMAN(&#39;Ѡ&#39;), // ойкс! твой бомбер умер. Не волнуйся, он появится через секунду где-нибудь на поле, но вполне вероятно за это ты получишь штрафные очки.

/// this is other players Bombermans

OTHER\_BOMBERMAN(&#39;♥&#39;), // а так выглядит бомбер противника
OTHER\_BOMB\_BOMBERMAN(&#39;♠&#39;), // так, если бомбер противника сидит на бомбе
OTHER\_DEAD\_BOMBERMAN(&#39;♣&#39;), // так, если бомбер противника подорвался. если это ты его подорвал - ты получишь бонусные очки.

/// the bombs

BOMB\_TIMER\_5(&#39;5&#39;), // после того как бомбер поставит бомбу таймер вкючится (всего 5 тиков)
BOMB\_TIMER\_4(&#39;4&#39;), // эта бомба взорвется через 4 тика
BOMB\_TIMER\_3(&#39;3&#39;), // эта - через 3
BOMB\_TIMER\_2(&#39;2&#39;), // два
BOMB\_TIMER\_1(&#39;1&#39;), // один
BOOM(&#39;҉&#39;), // Бам! Это то, как бомба взрывается. При этом все, что может быть разрушено - разрушится

/// walls

WALL(&#39;☼&#39;), // неразрушаемые стены - им взрывы бомб не страшны
DESTROYABLE\_WALL(&#39;#&#39;), // а эта стенка может быть разрушена
DESTROYED\_WALL(&#39;H&#39;), // это как разрушенная стенка выглядит, она пропадет в следующую секунду. если это ты сделал - ты получишь бонусные очки

/// meatchoppers

MEAT\_CHOPPER(&#39;&amp;&#39;), // этот малый бегает по полю в произвольном порядке. если он дотроентся до бомбера - тот умрет. лучше бы тебе учничтожить этот кусок.... мяса, за это ты получишь бонусные очки
DEAD\_MEAT\_CHOPPER(&#39;x&#39;), // это взровравшийся митчопер

/// perks

/// Значения, таймауты, вероятность выпадения перков могут быть изменены администратором игры.
/// Указаны значения по умолчанию.
/// Действие перка истекает по таймауту (10 тиков) если не указано иначе в описании перка.

BOMB\_BLAST\_RADIUS\_INCREASE(&#39;+&#39;), // увеличивает радиус взрыва бомбы (смотрите информацию о Перках). Действует только для новых бомб.
BOMB\_COUNT\_INCREASE(&#39;c&#39;), // Увеличивает количество доступных игроку бомб (смотрите информацию о Перках).
BOMB\_IMMUNE(&#39;i&#39;), // Дает иммунитет от взрыва бомб.
BOMB\_REMOTE\_CONTROL(&#39;r&#39;), // Дистанционный взрыватель (смотрите информацию о Перках). Срабатывает при повторном действии. 

/// a void

NONE(&#39; &#39;); // свободная ячейка, куда ты можешь направить бомбера
```

## Управление ботом

Игра пошаговая, каждую секунду сервер отправляет вашему клиенту (ваш бот) состояние обновленного поля на текущий момент и ждет ответа в виде команды от героя (ваш игрок на поле). За следующую секунду игрок должен успеть дать ответ. Если же не успел - ваш герой стоит на месте.

Команд несколько:

1. UP, DOWN, LEFT, RIGHT - приводят в движение героя в заданном направлении на 1 клетку;
2. ACT - оставить бомбу на месте героя. Команды движения можно комбинировать с командой ACT, разделяя их через запятую. Порядок (LEFT, ACT) или (ACT, LEFT) - имеет значение, или движемся влево и там ставим бомбу, или ставим бомбу, а потом бежим влево. Если игрок будет использовать только одну команду ACT, то бомба будет установлена ​​под героем без его перемещения на поле.

## Ваша задача в игре

Ваша задача - написать клиента с использованием вебсокетов (для некоторых языков есть уже готовые клиенты) и подключиться к серверу. Затем заставить героя выполнять команды по передвижению героя. Главная цель - вести осмысленную игру и победить, набрав наибольшее количество баллов за указанное время (до суботы мы пишем алгоритм бота и проверяем, а в субботу запускаем финальный забег).

## Раунды/Комнаты

Для того, чтобы на поле не было хаоса, все игроки будут разделены по комнатам или раундам. В каждой комнате играет ровно 5 человек. В том случае, если в какой-то комнате не будет хватать игроков, они будут ждать проигравших игроков из других комнат: участник, который проиграл в текущем раунде, сразу попадает в новую комнату, где после того, как соберется достаточное количество участников, игра начнется в новом составе.

Т.к количество раундов очень большое, то в конечном счете количество игр у каждого игрока будет выравнено.

Раунд продолжается 200 тиков (секунд) и заканчивается победой бомбермена:

- Если до конца раунда остались живыми более 1 бомбермена - побеждает тот, который сделал больше разрушений (при этом подсчитывается общее количество баллов полученных от начала раунда)
- Если же на поле живым остался только один Бомбермен - он и побеждает.
- Победитель раунда получает 30 бонусных баллов дополнительно к тем баллов, полученных во время раунда за разрушение объектов на поле (другие бомбермены, митчоперы и стены, которые разрушаются)
- Бомбермен, который принудительно покинул раунд (погиб от бомбы или митчопера) получает штрафные баллы: -30

## Особенности игры

Поле имеет форму квадрата с определенной длиной 23 символа

### Бомбы:

- В бомбермена по умолчанию есть определенное количество бомб: 3 бомбы для наших настроек
- Бомба оставленая ​​на поле подорвется через 5 тиков (секунд).
- Бомба разрывается во все стороны на определенное количество клеток (3 клетки для наших настроек) пока не встретит препятствие - любой элемент
- Бомбермен, который подорвался на своей или чужой бомбе погибает и получает штрафные баллы: -30 баллов.
- Бомбермен, бомба которого подорвала другой бомбермена получает бонусные баллы: 20

### Митчоперы:

- На поле обычно находится определенное количество Митчоперив: 5 для наших настроек
- Бомбермен, бомба которого подорвала Митчопера, получает бонусные баллы: 10
- Подорванный Митчопер сразу же появляется на поле в новом месте.
- Бомбермен, который встретился с Митчопером, погибает и получает штрафные баллы -30 баллов.

### Стены:

- На поле обычно находится определенное количество стен, которые могут быть взорваны: 52
- Бомбермен, бомба которого подорвала разрушамему стенку, получает бонусные баллы: 1
- Подорваная стена может появиться на поле в новом месте
- При разрушении стенки может появиться Перк (Модификатор)

### Перки (Модификаторы):

- Перки выпадают на месте уничтожения стены с 25% вероятностью
- Если перк никто не подобрал, он исчезает из поля после спустя 30 секунд
- Если же участник подобрал перк, он получает бонусные баллы - 5
- Перки не влияют друг на друга, только дополняют
- ~~Действие перка не бесконечно и исчезает через некоторое время (поэтому воспользоваться им надо до окончания действия):~~
- Каждый перк имеет свои собственные огранчения: таймер и / или счетчик количеств (зависит от типа)
  - BOMB\_BLAST\_RADIUS\_INCREASE работает следующее количество тиков (секунд) - 30. Но если было взято несколько перков подряд - общее время работы суммируется
  - BOMB\_COUNT\_INCREASE работает следующее количество тиков (секунд) - 30
  - BOMB\_IMMUNE работает следующее количество тиков (секунд) - 30
  - BOMB\_REMOTE\_CONTROL не имеет срока истечения

#### Перк BOMB\_BLAST\_RADIUS\_INCREASE:
- Увеличивает радиус взрыва бомбы на определенное количество клеток во все стороны - в наших настройках это +2
- Действует только для новых бомб (те, что вы оставили после получения Перка)
- Действие перка не вечно и исчезает через 30 тиков
- При подборе нескольких перков этого типа одновременно, радиус взрыва бомбы увеличивается пропорционально количеству полученных перков, а общее время их действия суммируется
  
#### Перк BOMB\_COUNT\_INCREASE:
- Временно увеличивает количество доступных игроку бомб на определенное количество (+ 4 бомбы) дополнительно к имеющимся по умолчанию бомбам (2 бомбы) бомбермена.
- Бомбермен может класть не более чем одну бомбу за тик (секунду)
- Действие перка исчезает через 30 тиков
- Данные перки не суммируются. При получении нескольких перков этого типа одновременно, таймер действия устанавливается в исходное положение.

#### Перк BOMB\_IMMUNE:
- Дает иммунитет к взрывам бомб (даже чужих).
- Действие перка исчезает через 30 секунд
- Перки не суммируются. При получении нескольких перков этого типа одновременно, таймер действия устанавливается в исходное положение.

#### Перк BOMB\_REMOTE\_CONTROL:
- Дает возможность дистанционного управления детонатором
  - Бомба устанавливается на поле первой командой ACT
  - Установленная бомба взрывается при повторении ACT команды. При этом используется как бомба так взрыватель (отдельно).
- В наличии есть определенное количество удаленных бомб: 3
- Данный перк не имеет таймаута
- Перки не суммируются. При получении нескольких перков этого типа одновременно, общее количество детонаторов возобновляется до исходного значения

## Подсказки для написания алгоритма (можно реализовать в заданном порядке):

- Перемещайтесь в ту сторону, где есть свободная клетка
- Старайтесь двигаться на свободную клетку, которая находится ближе к стенке, которую можно разрушить
- Оставляйте бомбы рядом с разрушаемыми стенками
- Уклоняйтесь от бомб и берите во внимание взрывную волну
- Избегайте встреч с Митчоперами
- Подрывайте Митчоперов и других бомберменов
- Начните собирать перки и реализуйте стратегию на базе их возможностей - это усилит ваш алгоритм и безусловно приведет к победе
