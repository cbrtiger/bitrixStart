<a name="top"></a>
# Правила работы с Git

> На этой странице описаны лишь общие моменты работы с репозиториями, перед началом работы необходимо согласовать все особенности проекта с коллегами.
>
> **ВНИМАНИЕ!** 
> Для примера взята гипотетическая площадка DEMO XX.

## Настройка своего аккаунта на SSH

- Авторизуемся на SSH под своим логином и паролем и пишем в консоли:
```bash
cd
git config --global user.name "ЛОГИН_НА БИТБАКЕТЕ"
git config --global user.email "EMAIL_НА БИТБАКЕТЕ"
```
- Проверяем настройки
```bash
git config --list
```


## Создание репозитория

- Просим того, кто имеет право создавать новые репы, сделать это :smile:.


## Создание репозитория на демке

>Перед началом работы с репозиторием **ОБЯЗАТЕЛЬНО** создаём в корне файл `.gitignore` с вот таким содержимым:
```
# Исключаем всё
/*
# Кроме самого .gitignore (и как следствие всего, что в нём написано)
!.gitignore
# Кроме папки local
!/local/
```
Это необходимо для исключения из репозитория того, что нам там не нужно.

- Заходим на сервер через SSH
- Выполняем сдедующие команды для первоначальной настройки    
```bash 
cd /var/www/demoXX
git init 
```

- Настраиваем короткое имя репозитория и URL, куда будут уходить коммиты.
```bash
git remote add demoXX https://bitbucket.org/путь_до_репозитория/
```
`demoXX` - котроткое имя репозитория, может быть в принципе каким угодно.

- Отправляем данные в репозторий:
```bash
git add .
git commit -m "Initial Commit"
git push 
```



## Работа с репозиторием при разработке

#### Первым делом идём на SSH:
```bash 
cd /var/www/demoXX
```

#### Стягиваем изменения:
```bash
git pull demoXX master
```

#### Отправляем коммит:
```bash 
git status 
git add .
git commit -a
```

#### Вводим информацию о коммите в следующем формате:
```
[дата в формате ДД.ММ.ГГГГ][Заголовок коммита]
Список выполненной работы в коммите. 
Краткий changelog для того, что бы потом можно было сходу понять 
что сделано в репозитории конкретным разработчиком.
```

> **ВСЕ КОММИТЫ БЕЗ ОПИСАНИЯ БУДУТ БЕЗ ПРЕДУПРЕЖДЕНИЯ ОТКАТЫВАТЬСЯ**. <br>Это означает, что вся работа, которая проделана без описания будет в итоге похерена (и с большой вероятностью перетёрта другим разработчиком) и её придётся делать заново, но уже с описанием.


#### Пример коммита:
```md 
07.09.2015
- Добавлен новый компонент `custom.news.list`
- Исправлена ошибка в `80-main.less`
- Реализован шаблон главной
```

#### Отправляем данные в реп
```bash
git push demoXX master
```


## Работа с репозиторием после выгрузки сайта на бой (продакшн)

#### Первым делом идём на SSH
```bash 
cd /var/www/demoXX
git init
git clone https://bitbucket.org/путь_до_репозитория/
```

#### Стягиваем актуальные изменения
```bash 
git pull demoXX
```

#### Создаём ветку develop (если её не было)
```bash 
git branch develop
```

#### Переходим ветку dev и там уже будем работать
```bash 
git checkout develop
```

#### Коммит (правильный коммит см. выше)
```bash 
git add .
git commit -a
```

#### Отправка данных
```bash 
git push demoXX develop
```


## Полезные советы

### Алиасы
> Для удобства использования командной строки можно добавить алиасы для гита. Для этого открываем файл `/home/ИМЯ_ПОЛЬЗОВАТЕЛЯ/.bashrc` и добавляем алиасы для удобного вызова команд гита:
```bash
alias g='git'
alias cit='git'
alias gs='git status '
alias ga='git add '
alias gb='git branch '
alias gc='git commit'
alias gd='git diff'
alias go='git checkout '
alias gk='gitk --all&'
alias gx='gitx --all'
alias gp='git push'

alias got='git '
alias get='git '
```

> Так же можно настроить алиасы для конкретного проекта. 
Для этого идём в папку `.git` проекта, открываем файл `config`  и в конец дописываем:
```bash
[alias]
    # one-line log
    l = log --pretty=format:"%C(yellow)%h\\ %ad%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --date=short

    a = add
    ap = add -p
    c = commit --verbose
    ca = commit -a --verbose
    cm = commit -m
    cam = commit -a -m
    m = commit --amend --verbose

    d = diff
    ds = diff --stat
    dc = diff --cached

    s = status -s
    co = checkout
    cob = checkout -b
    # list branches sorted by last modified
    b = "!git for-each-ref --sort='-authordate' --format='%(authordate)%09%(objectname:short)%09%(refname)' refs/heads | sed -e 's-refs/heads/--'"

    # list aliases
    la = "!git config -l | grep alias | cut -c 7-"
```

### Глобальные настройки
>Чтобы опредилть настройки Git глобально их можно прописать в файл .gitconfig.

 - для win файл .gitconfig лежит в каталоге $HOME (C:\Documents and Settings\$USER или C:\Users\$USER для большинства пользователей)
 - для unix ~/.gitconfig для конкретного пользователя или /etc/gitconfig для всех пользователей

Пример файла:
```
[user]
# Логин и почта, использующиеся на github:
	name = username (User Name)
	email = email@mail.com
	
# Редактор, открывающий сообщения по умолчанию (должен быть прописан в PATH)
[core]
	editor = subl
	
# Цветовая схема для вывода консоли	
[color "branch"]
    current = yellow reverse
    local = yellow
    remote = green
[color "diff"]
    meta = yellow bold
    frag = magenta bold
    old = red bold
    new = green bold
[color "status"]
    added = yellow
    changed = green
    untracked = cyan

# Алиасы
[alias]
    st = status
    ci = commit
    br = branch
    co = checkout
    df = diff
    lg = log --oneline --graph --decorate

# Стратегия слияния при git push
[merge]
    conflictstyle = diff3
```
**[⬆ back to top](#top)**