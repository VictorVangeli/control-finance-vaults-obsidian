# Что делает это хранилище? 
Control Finance -  это готовое хранилище, которое позволит тебе вести учет своих доходов и расходов.

Для того, чтобы внести операцию достаточно нажать на соответствующую кнопку в меню и заполнить нужные поля. 

Система сохранит эту информацию, а затем выведет все в виде таблицы, которую можно фильтровать так, как вам нужно.

Мы используем `Modal Form` для того, чтобы получить возможность указывать подходящие нам категории трат, источников дохода, а также откуда списались или куда пришли деньги. 

>[!CAUTION]
>Особенности релиза: 
>- Работает на телефонах: Да
>- Работает на компьютерах: Да
>- Совместимо с `Self-hosted LiveSync`: Да
>- Рекомендуемый способ загрузки: клонирование текущего репозитория, загрузка из релиза текущего репозитория или внутреняя синхронизация

## Как установить готовое хранилище?
1. Скачайте этот проект на свое устройство (используя `git clone` или просто загрузив как архив)
2. Если скачали как архив - распакуйте его в нужной дирректории
3. Зайдите в `Obsidian` и откройте указанную дирректорию как хранилище
4. Доверьтесь сторонним плагинам, если спросят :)

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


1. Если у вас есть директория с шаблонами:
	 - поместите файлы `controlFinance_for_commands.md` и `controlFinance_to_refresh_filter.md` в нее
	- в файле `controlFinance_for_commands.md `замените содержимое `const path_to_templates = 'Templates/controlFinance_to_refresh_filter.md'` и укажите актуальный путь
2. Если у вас нет директории с шаблонами, то скопируйте из моего хранилища папку `Templates`
3. Проверьте настройки `Templater` на предмет верного пути к папке с шаблонами, при необходимости - укажите его
4. Зарегистрируйте шаблоны в настройках `Templater`, указав путь `controlFinance_for_commands.md` и `controlFinance_to_refresh_filter.md`
4. В настройках `QuickAdd` перейдите в настройки команды `addFinanceOperation` и укажите корректный путь к шаблону в поле `Template Path` (о том, что должно быть в команде найдете в соответствующем разделе)
5. Скопируйте `MCL Multi Column.css `в свою дирректорию внутри `.obsidian/snippets`
6. Если у вас установлен `Modal Form` - перейдите в дирректорию `.obsidian/plugins/modalforms` и обновите содержимое файла `data.json` скопировав содержимое из моего хранилища по аналогичному пути

>[!NOTE]
>Важно: нужно именно дополнить, т.к. сверху идет блок с настройками плагина:
```JSON
{
  "editorPosition": "mainView",
  "attachShortcutToGlobalWindow": true,
  "globalNamespace": "MF",
  "formDefinitions": [
```
А нужно добавить:
```JSON
    {
      "title": "Добавить учет",
      "name": "Добавить учет",
      "fields": [
        {
          "name": "Тип",
          "label": "Вид операции",
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
          "name": "Категория расхода",
          "label": "Категория расхода",
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
          },
          "condition": {
            "dependencyName": "Тип",
            "type": "contains",
            "value": "Расход"
          }
        },
        {
          "name": "Категория дохода",
          "label": "Категория дохода",
          "description": "",
          "input": {
            "type": "select",
            "allowUnknownValues": false,
            "hidden": false,
            "options": [
              {
                "value": "Зарплата",
                "label": "Зарплата"
              },
              {
                "value": "Выплата по договору",
                "label": "Выплата по договору"
              },
              {
                "value": "Кредит",
                "label": "Кредит"
              },
              {
                "value": "Долг",
                "label": "Долг"
              },
              {
                "value": "Подарок",
                "label": "Подарок"
              },
              {
                "value": "Другое",
                "label": "Другое"
              }
            ],
            "source": "fixed"
          },
          "condition": {
            "dependencyName": "Тип",
            "type": "contains",
            "value": "Доход"
          },
          "isRequired": false
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
```

7. Если у вас установлен `Commander` - перейдите в настройки плагина и добавьте вызовы в `Панель вкладок` (подробнее об этом дальше):
	- `QuickAdd: addFinanceOpertation`
	- `Modal forms: Manage Forms`
8. При первом вызове настроенных шаблонов автоматически создадутся все необходимые дирректории. Вызвать шаблон можно или через `Палитру команд` или через настроенную кнопку на `Панели вкладок`

---

### 3. Как устроена система?
1. При открытии хранилища по умолчанию открывается файл `Учет финансов`, в котором отражается текущая информация по доходам/расходам

![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109140753.png)

2. Логически структура файла разделена на 2 блока:
	- Фильтрации по критериям;
	- Таблицы, в которых отображаются все операции;
3. Структура дирректорий (дерево) устроена таким образом:
```markdown
Учет финансов (корневая директория)
├── Учет финансов
│   ├── Доходы
│   ├── Расходы
│   └── Utils
│       └── Temporary
├── Templates
└── README.md
```

Выбор такой структуры обусловлен тем, чтобы упростить процесс интеграции хранилища в вашу экосистему. 
 
В дирректории `Templates` находятся файлы, отвечающие за добавление финансовой операции и применение выбранных пользователем фильтров.

---

### 4. Как внести операцию в систему?

Для того, чтобы внести новую операцию в систему необходимо:
1. Вызвать модальное окно, находящееся в правом верхнем углу (иконка `Accesability`)
   
![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109140907.png)
2. В появившемся окне доступны следующие поля:
   
>[!TIP]
>Чтобx добавить свое поле вам необходимо перейти в `Manage forms` , выбрать редактирование нужной формы и внести изменения в соответствующие поля, добавив или удалив необходимые значения

	- Тип: Доход, Расход
	- Счет: (в рассматриваемом примере: Бизнес-счет, Тинькоф, РНКБ, СБЕР, Наличные)
	- Категория операции:
		- Если выбран тип "Доход", то к выбору будут доступны: Зарплата, Оплата по договору, Кредит, Долг, Подарок, Другое
		- Если выбран тип "Расход", то к выбору будут доступны: Еда, Одежда, Гигиена, Машина, Транспорт, Медицина, Дом, Быт, Техника, Животные, Развлечения и другие. 
	- Сумма: необходимо ввести число
	- Дата: вам предлагается указать дату, когда была совершена операция, но если вы не взаимодействуете с этим полем, то по умолчанию система проставит текущую дату
	- Комментарий: в этом поле вы можете оставить примечание об особенностях этой операции

![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109140924.png)

3. После того как вы заполнили шаблон необходимой информацией будет создан соответствующий файл (заметка) в дирректории `Учет финансов/Доходы/Расходы(в зависимости от того, что выбрали)/название категории`, о чем система отправит соответствующее уведомление
   
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

![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109141036.png)

---

### 5. Как отфильтровать операции?
Существуют следующие возможности для фильтрации данных:
- В зависимости от типа: Доходы или Расходы
- В зависимости от категории: (включаются все категории, по которым у вас были платежи)
- В зависимости от счета: (включаются все счета, по которым у вас были платежи)
- В зависимости от даты: (можно выбрать начальную, конечную или срез)
- В зависимости от суммы: (можно выбрать больше, меньше или равно сумме)

![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109141212.png)

Предположим, что у нас имеется (пока что) небольшой список операций, но нам нужно получить только данные, которые прошли по бизнес-счету. Для этого нам необходимо выбрать в блоке `Счет` - `Бизнес-счет` и применить фильтрацию нажатием кнопки `Обновить фильтрацию`

После чего система проанализирует все записанные операции и выведет только те, которые соответствуют условиям: 

![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109142503.png)

Также есть возможность ограничить количество записей, которое будет отображено, по умолчанию, если значения там нет - будут выводиться все доступные записи

---

### 6. За что отвечают используемые плагины и их настройки?


#### Templater
Templater необходим для того, чтобы обрабатывать логику создания файлов в результате работы модального окна, а также проводить фильтрацию, заменяя в `Учет финансов` `dataviewjs` запросы для таблицы доходов и расходов

В настройках плагина нас интересуют следующие поля:
- `Template folder location` , в котором мы указываем путь к нашим шаблонам. В нашем случае это `Templates` в корневой дирректории
- `Automatic jump to cursor`
- Регистрация конкретных шаблонов в `Template Hotkeys`, чтобы иметь возможность вызывать шаблоны через `Meta Bind Button` и `Quick Add`

 ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109142756.png)

---

#### Meta Bind
C помощью этого плагина у нас появляется возможность добавлять удобные поля для ввода информации разных типов и автоматически обновлять эту информацию в Frontmatter файла `Учет финансов` c помощью полей \`INPUT[тип_поля:какой Frontmatter записываем]\`

Как-то особенно настраивать этот плагин здесь не имеет смысла, для удобства пользователя мы не регистрируем `BUTTON` в плагине, а определяем и вызываем непосредственно в коде. 

---

#### Modal Form
Этот плагин позволяет нам вызвать удобное модальное окно, в котором пользователь определяет поля и какие значения доступны для выбора в этих полях. 

В настройках плагина нам нужно поставить галочку напротив `Attach Modal-Form Shortcut to Global Window`

 ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109142826.png)

А в режиме редактирования форм `Manage forms`, нам необходимо определить форму, которую будем вызывать в дальнейшем при обработке нашего `Templater` шаблона

Получить доступ к этому разделу можно следующими способами: 
1. Вызвать палитру команд через `Ctrl + P` или `Cmd + P` 

 ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109142924.png)

2. Вызвать контекстное меню через кнопку на боковой панели

 ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109142942.png)

3. Перейти по специально созданной с помощью `Commander` кнопки на `Панели вкладок`
Перейдя в этот раздел нам доступны текущие формы, в данном хранилище реализована только одна форма. 

 ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109143046.png)

Если нажать на кнопку `Edit`, то мы попадаем в меню редактирования этой формы, где нам доступно редактирование полей. 

В этом релизе я решил отойти от использования `Словаря ключей`, и заменил атрибуты `Категории трат` и `Счета` на режим `Select` и расставив в нужном порядке (приоритете) интересующие значения. 

Дело в том, что использование текущей реализации через указание Notes как дирректории не позволяет управлять порядком доступных полей, кроме как указание принудительного индекса в начале файла, например `1. Еда` для поля `Категории трат`

Также такой подход позволяет инициалиировать хранилище по нажатию кнопки, следовательно, нужно чтобы у пользователя были созданы соответствующие преднастройки и нет необходимости дублирования всей дирректории "Учет финансов"

 ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109143300.png)

---

#### Quick Add

Данный плагин в текущей реализации отвечает за возможность вызова `Templater` шаблона без необходимости находиться в режиме редактирования, поскольку мы используем команду типа `Insert templates`, а не `Create Templates`. 

Альтернативную реализацию также мог предложить `Meta Bind`, но для текущего проекта был выбран именно `Quick Add` как некий "роутер". 

В меню настроек плагина нас интересует создание команды `addFinanceOperation`. 

 ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109143349.png)

Чтобы все корректно работало, необходимо активировать иконку "молнии", а также перейти в непосредственные настройки комманды, нажав на "шестеренку".

В меню настроек комманды нас интересует:
- путь к шаблону, который мы выполняем;
- возможность задать имя временному файлу;
- указать место его создания;
- условие перезаписи временного файла при каждом вызове команды;

 ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109143420.png)

---

#### Commander

Этот плагин мы используем для создания трех кнопок на главной панели: 
1. Кнопки `Home`, которая вернет нас в файл `Учет финансов`
2. Кнопки `QuickAdd:addFinanceOperation` , которая после создания стала доступна для вызова как команда в режиме `Палитры команд`
3. Кнопки `Modal forms: Manage forms`, которая позволит перейти в меню изменения модальной формы

 ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109143046.png)

  Для этого в настройках плагина в разделе `Панель вкладок` нам необходимо зарегистрировать соответствующие кнопки

 ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109143503.png)

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

 ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109143655.png)
  ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109143705.png)

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

  ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109143737.png)
  ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109143815.png)

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

  ![Image](https://raw.githubusercontent.com/VictorVangeli/control-finance-vaults-obsidian/fast_alternative/.github/images/Pasted%20image%2020241109143844.png)

