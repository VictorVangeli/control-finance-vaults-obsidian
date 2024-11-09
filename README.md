# Альтернативная ветка хранилища. В чем отличие? 
Control Finance -  это готовое хранилище, которое позволит тебе вести учет своих доходов и расходов.
Отличие этой ветки в том, что для того, чтобы создать и редактировать поля - не нужно создавать новые заметки в папке `Словарь ключей`, вместо этого мы используем `Manage Forms` внутри `Modal Form`

>[!CAUTION]
>Особенности релиза: 
>- Работает на телефонах: Да
>- Работает на компьютерах: Да
>- Совместимо с `Self-hosted LiveSync`: Да
>- Рекомендуемый способ загрузки: клонирование текущего репозитория или внутреняя синхронизация

---

### Навигация:
- [Что делает это хранилище?](#что-делает-это-хранилище)
- [1. Используемый стек](#1-используемый-стек)
- [2. Как интегрировать в своё хранилище?](#2-как-интегрировать-в-своё-хранилище)
- [3. Как устроена система?](#3-как-устроена-система)
- [4. Как внести операцию в систему?](#4-как-внести-операцию-в-систему)
- [5. Как отфильтровать операции?](#5-как-отфильтровать-операции)
- [6. За что отвечают используемые плагины и их настройки?](#6-за-что-отвечают-используемые-плагины-и-их-настройки)
  - [Templater](#templater)
  - [Meta Bind](#meta-bind)
  - [Modal Form](#modal-form)
  - [Quick Add](#quick-add)
  - [Commander](#commander)
  - [Homepage](#homepage)
  - [Dataview](#dataview)
  - [Trash Explorer](#trash-explorer)
  - [snippets: MCL Multi Column.css](#snippets-mcl-multi-columncss)

---

### 1. Используемый стек: 
- [Templater](https://github.com/SilentVoid13/Templater)
- [Meta Bind](https://github.com/mProjectsCode/obsidian-meta-bind-plugin)
- [Modal Form](https://github.com/danielo515/obsidian-modal-form)
- [Commander](https://github.com/phibr0/obsidian-commander)
- [Homepage](https://github.com/mirnovov/obsidian-homepage)
- [Dataview](https://github.com/blacksmithgu/obsidian-dataview)
- [Quick Add](https://github.com/chhoumann/quickadd)
- [Trash Explorer](https://github.com/proog/obsidian-trash-explorer)
- [snippets: MCL Multi Column.css](https://github.com/efemkay/obsidian-modular-css-layout)

---

### 2. Как интегрировать в своё хранилище?

>[!TIP]
>Это можно было бы упростить до 3-х шагов, добившись автоматической инициаилизации библиотеки, но это бы немного усложнило на мой взгляд процесс взаимодействия пользователя с добавлением полей внутри модальной формы. Но в случае если вам интересно - я могу выпустить альтернативный релиз

1. Если у вас есть директория с шаблонами:
	 - поместите файлы `controlFinance_for_commands.md` и `controlFinance_to_refresh_filter.md` в нее
	- в файле `controlFinance_for_commands.md `замените содержимое `const path_to_templates = 'Templates/controlFinance_to_refresh_filter.md'` и укажите актуальный путь
2. Если у вас нет директории с шаблонами, то скопируйте из моего хранилища папку `Templates`
3. Проверьте настройки `Templater` на предмет верного пути к папке с шаблонами, при необходимости - укажите его
4. Зарегистрируйте шаблоны в настройках `Templater`, указав путь `controlFinance_for_commands.md` и `controlFinance_to_refresh_filter.md`
4. В настройках `QuickAdd` перейдите в настройки команды `addFinanceOperation` и укажите корректный путь к шаблону в поле `Template Path` (о том, что должно быть в команде найдете в соответствующем разделе)
5. Скопируйте `MCL Multi Column.css `в свою дирректорию внутри `.obsidian/snippets`
6. Если у вас установлен `Modal Form` - перейдите в дирректорию `.obsidian/plugins/modalforms` и обновите содержимое файла `data.json` скопировав содержимое из моего хранилища по аналогичному пути:
   ```
       {
      "title": "Добавить учет",
      "name": "Добавить учет",
      "fields": [
        {
          "name": "Тип",
          "label": "Добавить доход",
          "description": "",
          "isRequired": false,
          "input": {
            "type": "select",
            "source": "fixed",
            "options": [
              {
                "value": "Расход",
                "label": "Расход"
              },
              {
                "value": "Доход",
                "label": "Доход"
              }
            ]
          }
        },
        {
          "name": "Счет",
          "label": "Счет",
          "description": "",
          "isRequired": false,
          "input": {
            "type": "select",
            "source": "fixed",
            "options": [
              {
                "value": "Тинькофф",
                "label": "Тинькофф"
              },
              {
                "value": "РНКБ",
                "label": "РНКБ"
              },
              {
                "value": "Наличные",
                "label": "Наличные"
              },
              {
                "value": "Бизнес-счет",
                "label": "Бизнес-счет"
              },
              {
                "value": "СБЕР",
                "label": "СБЕР"
              }
            ]
          }
        },
        {
          "name": "Категория трат",
          "label": "Категория трат",
          "description": "",
          "isRequired": false,
          "input": {
            "type": "select",
            "source": "fixed",
            "options": [
              {
                "value": "Еда",
                "label": "Еда"
              },
              {
                "value": "Машина",
                "label": "Машина"
              },
              {
                "value": "Дом",
                "label": "Дом"
              },
              {
                "value": "Быт",
                "label": "Быт"
              },
              {
                "value": "Гигиена",
                "label": "Гигиена"
              },
              {
                "value": "Одежда",
                "label": "Одежда"
              },
              {
                "value": "Развлечения",
                "label": "Развлечения"
              },
              {
                "value": "Учеба",
                "label": "Учеба"
              },
              {
                "value": "Транспорт",
                "label": "Транспорт"
              },
              {
                "value": "Подарки",
                "label": "Подарки"
              },
              {
                "value": "Другое",
                "label": "Другое"
              }
            ]
          }
        },
        {
          "name": "Сумма",
          "label": "Сумма",
          "description": "",
          "isRequired": false,
          "input": {
            "type": "text",
            "hidden": false
          }
        },
        {
          "name": "Дата",
          "label": "Дата",
          "description": "",
          "isRequired": false,
          "input": {
            "type": "date",
            "hidden": false
          }
        },
        {
          "name": "Комментарий",
          "label": "Комментарий",
          "description": "",
          "isRequired": false,
          "input": {
            "type": "textarea",
            "hidden": false
          }
        }
      ],
      "version": "1",
      "template": {
        "createCommand": true,
        "parsedTemplate": []
      }
    }
  ]
}
}```
7. Если у вас установлен `Commander` - перейдите в настройки плагина и добавьте вызов `QuickAdd: addFinanceOpertation` в `Панель вкладок` (подробнее об этом дальше)
8. При первом вызове настроенных шаблонов автоматически создадутся все необходимые дирректории. Вызвать шаблон можно или через `Палитру команд` или через настроенную кнопку на `Панели вкладок`

---

### 3. Как устроена система?
1. При открытии хранилища по умолчанию открывается файл [[Учет финансов]], в котором отражается текущая информация по доходам/расходам

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108150810.png" height="700px">

2. Логически структура файла разделена на 2 блока:
	- Фильтрации по критериям;
	- Таблицы, в которых отображаются все операции;
3. Структура дирректорий (дерево) устроена таким образом:
```markdown
	- Учет финансов (корневая дирректория)
		- Учет финансов
			- Доходы
			- Расходы
			- Utils
				- Temporary
		- Templates
		- README.md
```

Выбор такой структуры обусловлен тем, чтобы упростить процесс интеграции хранилища в вашу экосистему. 

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108153125.png" height="600px"> 
 
В дирректории `Templates` находятся файлы, отвечающие за добавление финансовой операции и применение выбранных пользователем фильтров.

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108153150.png" height="78px">

---

### 4. Как внести операцию в систему?

Для того, чтобы внести новую операцию в систему необходимо:
1. Вызвать модальное окно, находящееся в правом верхнем углу (иконка `Accesability`)
   
<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108155317.png" height="80px">

2. В появившемся окне доступны следующие поля:
   
>[!TIP]
>Чтобx добавить свое поле не обращаясь к управлению формами в Modal Forms вам необходимо перейти в дирректорию: `Учет финансов/Utils/Словарь ключей` и отредактировать содержимое этих папок, добавив или удалив необходимые значения

	- Тип: Доход, Расход
	- Счет: (в рассматриваемом примере: Бизнес-счет, Тинькоф, РНКБ, СБЕР, Наличные)
	- Категория трат (в расмматриваемом примере Еда, Одежда, Гигиена, Машина, Транспорт, Медицина, Дом, Быт, Техника, Животные, Развлечения и другие)
	- Сумма: необходимо ввести число
	- Дата: вам предлагается указать дату, когда была совершена операция, но если вы не взаимодействуете с этим полем, то по умолчанию система проставит текущую дату
	- Комментарий: в этом поле вы можете оставить примечание об особенностях этой операции

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108205635.png" height="550px">

3. После того как вы заполнили шаблон необходимой информацией будет создан соответствующий файл (заметка) в дирректории `Учет финансов/Доходы/Расходы(в зависимости от того, что выбрали)/название категории`, о чем система отправит соответствующее уведомление
   
<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108160204.png" height="60px">
<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108163448.png" height="135px">

В метаданных файла будут записаны соответствующие данные, которые в последующем послужат основаниями для фильтрации: 
```markdown
---
Тип: Расход
Счет: Наличные
Категория: Еда
Сумма: 100
Дата: 08.11.2024
Комментарий: Овсяное печенье
---

Счет расхода: Наличные
Категория расхода: Еда
Сумма: 100
Дата расхода: 08.11.2024
Комментарий: Овсяное печенье
```

Также вы можете увидеть изменения в файле `Учет финансов`

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108163811.png" height="350px">

---

### 5. Как отфильтровать операции?
Существуют следующие возможности для фильтрации данных:
- В зависимости от типа: Доходы или Расходы
- В зависимости от категории: (включаются все категории, по которым у вас были платежи)
- В зависимости от счета: (включаются все счета, по которым у вас были платежи)
- В зависимости от даты: (можно выбрать начальную, конечную или срез)
- В зависимости от суммы: (можно выбрать больше, меньше или равно сумме)


<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108164344.png" height="655px">


Предположим, что у нас имеется (пока что) небольшой список операций, но нам нужно получить только данные, которые прошли по бизнес-счету. Для этого нам необходимо выбрать в блоке `Счет` - `Бизнес-счет` и применить фильтрацию нажатием кнопки `Обновить фильтрацию`

После чего система проанализирует все записанные операции и выведет только те, которые соответствуют условиям: 

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108181520.png" height="560px">

Также есть возможность ограничить количество записей, которое будет отображено, по умолчанию, если значения там нет - будут выводиться все доступные записи

---

### 6. За что отвечают используемые плагины и их настройки?


#### Templater
Templater необходим для того, чтобы обрабатывать логику создания файлов в результате работы модального окна, а также проводить фильтрацию, заменяя в `Учет финансов` `dataviewjs` запросы для таблицы доходов и расходов

В настройках плагина нас интересуют следующие поля:
- `Template folder location` , в котором мы указываем путь к нашим шаблонам. В нашем случае это `Templates` в корневой дирректории
- `Automatic jump to cursor`
- Регистрация конкретных шаблонов в `Template Hotkeys`, чтобы иметь возможность вызывать шаблоны через `Meta Bind Button` и `Quick Add`
 
<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108195521.png" height="465px">

---

#### Meta Bind
C помощью этого плагина у нас появляется возможность добавлять удобные поля для ввода информации разных типов и автоматически обновлять эту информацию в Frontmatter файла `Учет финансов` c помощью полей \`INPUT[тип_поля:какой Frontmatter записываем]\`

Как-то особенно настраивать этот плагин здесь не имеет смысла, для удобства пользователя мы не регистрируем `BUTTON` в плагине, а определяем и вызываем непосредственно в коде. 

---

#### Modal Form
Этот плагин позволяет нам вызвать удобное модальное окно, в котором пользователь определяет поля и какие значения доступны для выбора в этих полях. 

В настройках плагина все остается по умолчанию, но в режиме редактирования форм `Manage forms`, нам необходимо определить форму, которую будем вызывать в дальнейшем при обработке нашего `Templater` шаблона

Получить доступ к этому разделу можно следующими способами: 
1. Вызвать палитру команд через `Ctrl + P` или `Cmd + P` 

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108200504.png" height="117px">


2. Вызвать контекстное меню через кнопку на боковой панели

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108200539.png" height="400px">

Перейдя в этот раздел нам доступны текущие формы, в данном хранилище реализована только одна форма. 

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108200801.png" height="335px">

Если нажать на кнопку `Edit`, то мы попадаем в меню редактирования этой формы, где нам доступно редактирование полей. 

В этом релизе я решил отойти от использования `Словаря ключей`, и заменил атрибуты `Категории трат` и `Счета` на режим `Select` и расставив в нужном порядке (приоритете) интересующие значения. 

Дело в том, что использование текущей реализации через указание Notes как дирректории не позволяет управлять порядком доступных полей, кроме как указание принудительного индекса в начале файла, например `1. Еда` для поля `Категории трат`

Также такой подход позволяет инициалиировать хранилище по нажатию кнопки, следовательно, нужно чтобы у пользователя были созданы соответствующие преднастройки и нет необходимости дублирования всей дирректории "Учет финансов"

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108200957.png" height="419px">

---

#### Quick Add

Данный плагин в текущей реализации отвечает за возможность вызова `Templater` шаблона без необходимости находиться в режиме редактирования, поскольку мы используем команду типа `Insert templates`, а не `Create Templates`. 

Альтернативную реализацию также мог предложить `Meta Bind`, но для текущего проекта был выбран именно `Quick Add` как некий "роутер". 

В меню настроек плагина нас интересует создание команды addFinanceOperation. 

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108202308.png" height="440px">

Чтобы все корректно работало, необходимо активировать иконку "молнии", а также перейти в непосредственные настройки комманды, нажав на "шестеренку".

В меню настроек комманды нас интересует:
- путь к шаблону, который мы выполняем;
- возможность задать имя временному файлу;
- указать место его создания;
- условие перезаписи временного файла при каждом вызове команды;

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108202526.png" height="697px">
<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108202733.png" height="697px">

---

#### Commander

Этот плагин мы используем для создания двух кнопок на главной панели: 
1. Кнопки `Home`, которая вернет нас в файл `Учет финансов`
2. Кнопки `QuickAdd:addFinanceOperation` , которая после создания стала доступна для вызова как команда в режиме `Палитры команд`


<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108202949.png" height="30.7px">


Для этого в настройках плагина в разделе `Панель вкладок` нам необходимо зарегистрировать соответствующие кнопки

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108203101.png" height="366px">



--- 
#### Homepage

Мы используем этот плагин для:
- открытия файла `Учет финансов` при открытии `Obsidian`;
- открытия файла `Учет финансов` по нажатию на соответствующую кнопку из `Панели вкладок`;
- открытия файла `Учет финансов` если вы зашли в заметку, а затем закрыли и у вас больше не осталось доступных вкладок;

Для этого нам необходимо соответствующим образом настроить плагин, а именно:
- указать путь к файлу `Учет финансов`;
- активировать опцию `Open on startup`;
- активировать опцию `Open when empty`;
- активировать опцию `Use when opening normaly`;
- в поле `Opened View` необходимо выбрать Reading view;
- в самом низу выбрать `Refresh Dataview`

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108203528.png" height="605px">
<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108203720.png" height="79.8px">

---
#### Dataview

Этот плагин нам необходим для выполнения Dataview запросов, которые выводят в файле `Учет финансов` таблицы:
- Таблица доходов;
- Таблица расходов; 

Но проблема в том, что я не смог получить возможность подобного управления над файлами как представлено в текущем хранилище с помощью обычного `Dataview`, поэтому пришлось воспользоваться `DataviewJS` - стандартной надстройкой плагина, которая активируется в настройках. 

В данном случае нас интересует много настроек внутри плагина, но многие из них активированы по умолчанию. На всякий случай оставлю здесь список нужных нам:
- `Enable inline queries`
- `Enable JavaScript queries`
- `Enable inline JavaScript queries`
- `Enable inline field higlighting in reading view`
- `Enable inline field highlighting in Live Preview`
- `Code block inline queries`
- `Display result count`
- `Warn on empty result`
- `Automatic view refreshing`

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108204330.png" height="602.5px">
<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108204351.png" height="595px">

---

#### Trash Explorer

Используем этот плагин на тот случай, если нечаянно удалили какой-то из нужных файлов. 

Доступ к корзине в Obsidian, к сожалению не встроен по умолчанию. 

Чтобы попасть в корзину можно:
- воспользоваться `Палитрой команд` и ввести: `Trash Explorer: Show trash explorer`
- или нажать на одну из двух кнопок на боковых панелях

У плагина нет собственного меню настроек, поэтому настраивать ничего не нужно.  Единственное, что верхняя кнопка корзины обычно появляется после первого перезапуска Obsidian

---

#### snippets: MCL Multi Column.css

Этот снипет нам нужен для того, чтобы получить красивое оформление внутри `Callout`

<img src="https://github.com/VictorVangeli/control-finance-vaults-obsidian/blob/main/Учет%20финансов/Utils/Media/Pasted%20image%2020241108205401.png" height="652px">
