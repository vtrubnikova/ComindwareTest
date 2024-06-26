# Основы Git. Ветви и слияние ветвей 

## Оглавление
+ Введение
+ Создание нового репозитория
+ Понятие ветки в Git
+ Создание веток, работа с ними и их удаление
    + Создание веток и работа с ними 
    + Удаление веток 

# Введение
Здесь приводится краткое руководство по работе с ветками и их слиянию в Git. Оно дает базовое представление о пользовании системой Git.

# Создание нового репозитория

Рассмотрим работу системы на примере репозитория с проектом по автоматизации сети. Что содержат файлы, нам на данном этапе неважно. Для контроля версий возьмем текстовые файлы. 

Для начала создаем новую директорию с помощью команды mkdir, назовем её netauto. Чтобы зайти в неё, вводим команду cd. Здесь создаем репозиторий с помощью git init.

В нём будут находиться файлы, с которыми нам предстоит работать. Сначала откроем файл S1 в режиме редатора с помощью команды vi:

<img src="/images/01-31.png"/>

Впишем туда YAML-данные о сети и сохраним. Далее мы обозначаем, что в файле S1 есть изменения, которые мы хотим включить в ближайший коммит, с помощью команды git add. Далее коммитим изменения с помощью выражения git commit -m "create S1". Система вернёт сообщение о первом коммите:

<img src="/images/01-39.png"/>

Заводим второй файл S2 путем копирования первого (команда cp). Аналогичным образом применяем к нему git add и git commit -m. Вводим команду git log, чтобы проверить два последних коммита:

<img src="/images/02-14.png"/>

Запустим теперь команду git status. Система сообщает нам, что мы в ветке master. Мы не создавали её в репозитории: Git сделал это сам, а также дал этой первой ветке имя master:

<img src="/images/02-20.png"/>

# Понятие ветки в Git #

Ветка позволяет работать параллельно над разными версиями одного и того же файла. Правки по одной ветке могут происходить независимо от правок по другим веткам. Потом их можно объединить (например, провести слияние изменений из веток разработчиков и менеджеров):

<img src="/images/03-00.png"/>

Обратимся к схеме коммитов, чтобы наглядно увидеть созданные нами ветки:

<img src="/images/03-15.png"/>

Каждому коммиту присваивается хэш SHA-1 длиной 40 символов в шестнадцатеричном коде. На схеме показаны первые семь из них. Git создал первую ветку master по умолчанию. Сейчас ветка master указывает на второй коммит.

Поскольку мы сейчас находимся в ветке master, с каждым коммитом она будет двигаться вместе с ним вверх. Git определяет, на какой мы находимся ветке, по указателю HEAD.

Пока что у нас только одна ветка (master), и HEAD указывает на неё. Поскольку HEAD обычно указывает на ветку (но не на коммит), его иногда называют символическим указателем. В терминологии Git, указатель HEAD сообщает о том, что мы переключились — checked out. В данном примере переместились на ветку master.

Посмотрим на ветку и указатель HEAD из интерфейса командной строки:

<img src="/images/04-56.png"/>

С введёнными опциями git log возвращает весьма наглядную схему. Как видно по HEAD, мы находимся на втором коммите и на ветке master. То есть мы туда перемещены. Чтобы вызывать далее схему с такими же опциями по команде graph, закрепим ярлык alias. Так можно будет набирать только "graph", чтобы получать схему такой же верстки.

Итак, первая ветка master, созданная системой Git, готова.

# Создание веток, работа с ними и их удаление #

## Создание веток и работа с ними ##

Приступим к созданию других веток, которые понадобятся нам в работе.

Новые ветки создаются при помощи команды git branch (имя ветки). Появятся они там, где находится указатель HEAD. Создадим ветки SDN и auth:

<img src="/images/05-17.png"/>

Сама команда git branch без параметров выдает все наличествующие ветки. У нас их три. Звёздочка стоит у master, и она зелёная. Это означает, что мы в неё переместились, и HEAD также указывает на ветку master. Запустим команду graph, за которой мы ранее закрепили необходимые нам параметры:

<img src="/images/05-52.png"/>

Как видно, все ветки относятся к тому же коммиту. HEAD указывает на master, потому что туда мы перемещены.

Всего у нас три ветки: master, SDN и auth. С помощью команды git checkout SDN переместимся на ветку SDN. Теперь HEAD будет указывать на SDN.

<img src="/images/06-20.png"/>

Находясь на ветке SDN, отредактируем файл S1. Мы обозначим и закоммитим изменения. Поскольку мы находимся на SDN, только она сдвинется вверх в новый коммит:

<img src="/images/06-33.png"/>

Ветки же master и auth останутся на предыдущем коммите. Потом мы поработаем на ветке auth. После перемещения на неё (команда git checkout auth) указатель HEAD перейдёт с SDN на auth:

<img src="/images/06-46.png"/>

Находясь на ветке auth, мы внесём изменения, обозначим их и закоммитим. Так появится новый коммит, в который войдёт только auth:

<img src="/images/06-55.png"/>

В итоге получаем три ветки, которые указываются в разных коммитах. При перемещении по веткам нам будут видны разные версии файла S1.

Итак, мы находились на ветке master. Переместимся в SDN с помощью команды git checkout SDN:

<img src="/images/07-23.png"/>

По схеме видим, что HEAD перешёл на другую ветку и теперь указывает на SDN. Добавим в текст файла S1 IP SDN-контроллера:

<img src="/images/07-35.png"/>

Далее обозначим изменение (git add S1) и закоммитим (git commit) его. По схеме видим новый коммит по ветке SDN, на которую указывает HEAD:

<img src="/images/07-45.png"/>

Две другие ветки master и auth остались без изменений. Они находятся в предыдущем коммите. Если запустить cat S1, то мы увидим последние изменения (IP SDN-контроллера):

<img src="/images/08-02.png"/>

Перейдём теперь на ветку auth с помощью команды git checkout auth. Убедимся в этом с помощью команды git branch. 

<img src="/images/08-21.png"/>

По схеме видим, что указатель HEAD перешёл с SDN на auth.

С помощью команды cat S1 читаем находящуюся здесь версию файла: строчки с контроллером нет:

<img src="/images/08-32.png"/>

Чтобы убедиться в этом, перейдём опять на ветку SDN (git checkout SDN). Строчка с контроллером на месте:

<img src="/images/08-51.png"/>

Вернёмся в ветку auth. Впишем в файл S1 строчку с сервером аутентификации. Сохраним изменения и выйдем:

<img src="/images/09-08.png"/>

С поомщью git status видим, что в рабочем каталоге файл S1 изменился:

<img src="/images/09-17.png"/>

Есть быстрый способ обозначить изменение и закоммитить его. Параметр -a обозначает и коммитит все отслеживаемые файлы, которые изменялись. Graph возвращает такую же схему, как и раньше. Мы начинали в коммите *96f5b29. Master указывает так же на него. Из данного коммита расходятся две ветви:

<img src="/images/09-30.png"/>

Перейдя на SDN (*156b4bf), мы добавили IP контроллера. Затем мы перешли на auth и добавили IP сервера (*c435356).

Скажем, на ветках SDN и auth наша работа окончена. Мы хотим объединить эти изменения с веткой master. Иными словами, мы сольём с ней последние изменения.

Нам надо, чтобы конфигурация SDN-контроллера появилась в master. Контроллер был добавлен в коммите *156b4bf, это была ветка SDN. Родительский коммит это *96f5b29 (там находится ветка master). Чтобы произошло перенесение изменений из SDN в master, мы запускаем команду git merge (имя ветки). Поскольку путь от master к SDN прямой, Git произведёт так называемое быстрое слияние. То есть Git просто переместит ветку master туда, где находится ветка SDN:

<img src="/images/10-41.png"/>

Ветка master как бы догоняет ветку SDN. Даже если между двумя ветками производится много коммитов, достаточно быстрого слияния. Необходим только прямой путь между ними.

Итак, мы находимся на ветке auth. Перемещаемся на ветку master, и на неё переходит указатель HEAD. Git обновляет каталог и индекс:

<img src="/images/11-25.png"/>

Git diff master..SDN показывает, что изменится при слиянии SDN в master. В файле S1 ветки master появится строчка с sdn_controller из ветки SDN. Находясь на ветке master, мы хотим объединить её с SDN. Это производит команда git merge SDN. Система Git подтверждает, что закончилось быстрое слияние.

К содержимому файла S1 добавилась одна строчка. Запускаем cat S1 и убеждаемся с этом:

<img src="/images/11-30.png"/>

На схеме видно, что ветка master теперь соответствует ветке SDN. Git переместил указатель на тот коммит, в котором находится ветка SDN. Работа с SDN на этом закончена. Изменения из неё влиты в master.

<img src="/images/11-33.png"/>

## Удаление веток ##

Нам не нужны две ветки в одном коммите, поэтому ветку SDN теперь можно удалить.

<img src="/images/12-01.png"/>

Перед этим запустим команду git branch --merged, чтобы проверить коммиты, которые уже слились с текущей веткой. Мы видим, что master, на которой мы находимся, объединена с веткой SDN:

<img src="/images/12-19.png"/>

Мы удостоверились, что слияние веток произошло, и теперь можем спокойно удалить SDN.
Обратите внимание на то, что auth здесь нет, потому что её мы пока не объединили с master.

Запустим команду git branch -d SDN для удаления ветки. Попробуем удалить и auth. Git предотвращает это ("auth подвергнута слиянию не полностью"):

<img src="/images/12-48.png"/>

Система уведомляет, что несмотря на неполное слияние, удаление возможно. Для этого надо использовать прописную D вместо строчной d. Здесь надо дествовать осторожно, потому что можно потерять изменения из удаляемой ветки. Такое случается, когда Git проводит очистку коммитов удаляемой ветки.
