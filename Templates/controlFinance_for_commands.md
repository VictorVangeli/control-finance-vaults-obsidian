<%*
const modalForm = app.plugins.plugins['modalforms'].api;
const vault = app.vault;

const path_to_templates = 'Templates/controlFinance_to_refresh_filter.md'

async function processData() {
    // Вызов модального окна и получение данных
    const result = await modalForm.openForm("Добавить учет");
    if (result.status !== 'ok') {
        new Notice("Форма была закрыта без заполнения.");
        return;
    }

    // Извлечение данных напрямую из result.data
    const data = result.data;

    // Проверка типа операции
    const operationType = data["Тип"] === "Доход" ? "Доход" : "Расход";

    // Инициализация переменных для хранения данных из формы
    const account = data["Счет"] || "Не указан";
    const category = operationType === "Доход" 
        ? data["Категория дохода"] || "Не указана" 
        : data["Категория расхода"] || "Не указана";
    const amount = data["Сумма"] || "0";
    const date = data["Дата"] && !isNaN(Date.parse(data["Дата"]))
        ? new Date(data["Дата"]).toLocaleDateString('ru-RU', { day: '2-digit', month: '2-digit', year: 'numeric' })
        : new Date().toLocaleDateString('ru-RU', { day: '2-digit', month: '2-digit', year: 'numeric' });
    const comment = data["Комментарий"] || "Нет комментария";


    // Создание директории для учета финансов
    const baseDirectory = "Учет финансов";
    const subDirectory = operationType === "Доход" ? "Доходы" : "Расходы";
    const categoryDirectory = `${category.replace(/[\\/:"*?<>|]+/g, "")}`;
    const fullDirectory = `${baseDirectory}/${subDirectory}/${categoryDirectory}`;

    if (!vault.getAbstractFileByPath(fullDirectory)) {
        await vault.createFolder(fullDirectory);
    }

    // Формирование базового имени файла
    const sanitizedAmount = amount.replace(/[^\d]/g, "");
    const sanitizedDate = date.replace(/[^\d]/g, "-");
    const baseFileName = `${sanitizedAmount}_${sanitizedDate}`;
    let fileName = `${baseFileName}.md`;
    let filePath = `${fullDirectory}/${fileName}`;

    // Проверка наличия файла и добавление суффикса, если файл существует
    let counter = 1;
    while (vault.getAbstractFileByPath(filePath)) {
        fileName = `${baseFileName}(${counter}).md`;
        filePath = `${fullDirectory}/${fileName}`;
        counter++;
    }

    // Формирование содержимого файла
    let content = `---
Тип: ${operationType}
Счет: ${account}
Категория: ${category}
Сумма: ${amount}
Дата: ${date}
Комментарий: ${comment}
---

Счет ${operationType === "Доход" ? "дохода" : "расхода"}: ${account}
Категория ${operationType === "Доход" ? "дохода" : "расхода"}: ${category}
Сумма: ${amount}
Дата ${operationType === "Доход" ? "дохода" : "расхода"}: ${date}
Комментарий: ${comment}
`;

    // Создаем файл, если он не существует
    if (!vault.getAbstractFileByPath(filePath)) {
        await vault.create(filePath, content);
        new Notice(`Файл ${filePath} успешно создан!`);
        console.log("Файл успешно создан:", filePath);
    } else {
        new Notice(`Файл уже существует: ${filePath}`);
    }

    // Обновление frontmatter в файле `Учет финансов`
    const summaryFilePath = `${baseDirectory}/Учет финансов.md`;
    let summaryContent = "";
    let available = { тип: [], счет: [], категория: [] };
    let hasNewMetadata = false;

    // Проверка существования файла `Учет финансов` и загрузка его содержимого
    if (vault.getAbstractFileByPath(summaryFilePath)) {
        const summaryFile = vault.getAbstractFileByPath(summaryFilePath);
        summaryContent = await vault.read(summaryFile);

        // Извлечение текущего frontmatter
        const frontmatterMatch = summaryContent.match(/^---\n([\s\S]*?)\n---/);
        if (frontmatterMatch) {
            const frontmatter = frontmatterMatch[1];
            // Парсинг существующих значений `available_*`
            const availableТипMatch = frontmatter.match(/available_тип:\s*\n((\s*-\s*.*\n)*)/);
            const availableСчетMatch = frontmatter.match(/available_счет:\s*\n((\s*-\s*.*\n)*)/);
            const availableКатегорияMatch = frontmatter.match(/available_категория:\s*\n((\s*-\s*.*\n)*)/);

            available.тип = availableТипMatch ? availableТипMatch[1].split("\n").map(line => line.replace(/^\s*-\s*/, "").trim()).filter(line => line) : [];
            available.счет = availableСчетMatch ? availableСчетMatch[1].split("\n").map(line => line.replace(/^\s*-\s*/, "").trim()).filter(line => line) : [];
            available.категория = availableКатегорияMatch ? availableКатегорияMatch[1].split("\n").map(line => line.replace(/^\s*-\s*/, "").trim()).filter(line => line) : [];
        }
    } else {
        // Создание файла, если он отсутствует
        summaryContent = "---\navailable_тип:\n  - Доход\n  - Расход\navailable_счет:\n\navailable_категория:\n\navailable_date_filtered: []\navailable_count_filtered: []\n---\n";
    }

    // Добавление новых значений в `available_*`, если они еще не существуют
    if (!available.тип.includes(operationType)) {
        available.тип.push(operationType);
        hasNewMetadata = true;
    }
    if (!available.счет.includes(account)) {
        available.счет.push(account);
        hasNewMetadata = true;
    }
    if (!available.категория.includes(category)) {
        available.категория.push(category);
        hasNewMetadata = true;
    }

    // Формирование нового frontmatter блока с обновленным `available_*`
    const newFrontmatter = `---
available_тип:
${available.тип.map(item => `  - ${item}`).join("\n")}
available_счет:
${available.счет.map(item => `  - ${item}`).join("\n")}
available_категория:
${available.категория.map(item => `  - ${item}`).join("\n")}
---`;

    // Содержимое для основной части файла `Учет финансов`, включая таблицы и фильтр
const mainContent = `
\`\`\`meta-bind-js-view
{available_тип} as available_тип
{available_счет} as available_счет
{available_категория} as available_категория
---
const str = \`>[!info]+ Фильтрация по критериям
>---
>>[!multi-column]
>>>[!info]+ Фильтровать по типу:
>>>\\\`\\\`\\\`meta-bind
INPUT[multiSelect(\${context.bound.available_тип.map(x => \`option(\${x})\`).join(", ")}):run_filtered_type]
>>>\\\`\\\`\\\`
>>
>>>[!info]+ Фильтровать по счету:
>>>\\\`\\\`\\\`meta-bind
INPUT[multiSelect(\${context.bound.available_счет.map(x => \`option(\${x})\`).join(", ")}):run_filtered_account]
>>>\\\`\\\`\\\`
>>
>>>[!info]+ Фильтровать по категории:
>>>\\\`\\\`\\\`meta-bind
INPUT[multiSelect(\${context.bound.available_категория.map(x => \`option(\${x})\`).join(", ")}):run_filtered_category]
>>>\\\`\\\`\\\`
>>
>>>[!multi-column]
>>>>[!info]+ Фильтровать по периоду
>>>>>[!multi-column]
>>>>>Укажите начало периода:\\\`INPUT[datePicker:run_date_from]\\\`
>>>>>
>>>>>Укажите конец периода: \\\`INPUT[datePicker:run_date_to]\\\`
>>>
>>>>[!info]+ Фильтровать по сумме
>>>>---
>>>>>[!multi-column]
>>>>>>[!info] Больше: \\\`INPUT[toggle:run_more_price]\\\`
>>>>>
>>>>>>[!info] Меньше: \\\`INPUT[toggle:run_less_price]\\\`
>>>>>
>>>>>>[!info] Равно: \\\`INPUT[toggle:run_ident_price]\\\`
>>>>>
>>>> Сумма: \\\`INPUT[number:run_price]\\\`
\`;
return engine.markdown.create(str);
\`\`\`
>[!multi-column|right-fixed]
>\`\`\`meta-bind-button
>label: Обновить фильтрацию
>icon: ""
>hidden: false
>class: ""
>tooltip: ""
>id: учет-refresh-and-filter
>style: destructive
>actions:
>  - type: command
>    command: templater-obsidian:${path_to_templates}
>\`\`\`
---
>[!abstract] Учет финансов
>
>>[!info]+ Таблица Доходов
>>\`\`\`dataviewjs
>>let filterConditions = {"view_list_income":null,"view_list_expense":null};
>>let viewLimitIncome = filterConditions.view_list_income || Infinity;
>>
>>let incomePages = dv.pages('"Учет финансов/Доходы"')
>>    .where(p => {
>>        let matchType = filterConditions.type 
>>            ? filterConditions.type.includes(p["Тип"]) 
>>            : true;
>>
>>        let matchAccount = filterConditions.account 
>>            ? filterConditions.account.includes(p["Счет"]) 
>>            : true;
>>
>>        let matchCategory = filterConditions.category 
>>            ? filterConditions.category.includes(p["Категория"]) 
>>            : true;
>>
>>        let matchPrice = true;
>>        if (filterConditions.price) {
>>            if (filterConditions.price.more) {
>>                matchPrice = p["Сумма"] >= filterConditions.price.value;
>>            }
>>            if (filterConditions.price.less) {
>>                matchPrice = p["Сумма"] <= filterConditions.price.value;
>>            }
>>            if (filterConditions.price.exact) {
>>                matchPrice = p["Сумма"] == filterConditions.price.value;
>>            }
>>        }
>>
>>        let matchDate = true;
>>        if (filterConditions.date) {
>>            if (filterConditions.date.from) {
>>                matchDate = new Date(p["Дата"]) >= new Date(filterConditions.date.from);
>>            }
>>            if (filterConditions.date.to) {
>>                matchDate = matchDate && (new Date(p["Дата"]) <= new Date(filterConditions.date.to));
>>            }
>>        }
>>
>>        return matchType && matchAccount && matchCategory && matchPrice && matchDate;
>>    })
>>    .sort(p => new Date(p["Дата"]), 'desc')
>>    .slice(0, viewLimitIncome)
>>    .array();
>>
>>if (incomePages.length === 0) {
>>    dv.paragraph("Нет данных для отображения.");
>>} else {
>>    let amounts = incomePages.map(p => parseFloat(p["Сумма"] || '0'));
>>    let totalSum = amounts.reduce((acc, val) => acc + val, 0);
>>    let totalOperations = incomePages.length;
>>    let tableData = incomePages.map(p => [
>>        p.file.link,
>>        p["Счет"] || '—',
>>        p["Категория"] || '—',
>>        p["Сумма"] || '—',
>>        p["Дата"] && !isNaN(Date.parse(p["Дата"].split('.').reverse().join('-')))
>>    ? new Date(p["Дата"].split('.').reverse().join('-')).toLocaleDateString('ru-RU', { day: '2-digit', month: '2-digit', year: 'numeric' }) + ' г.'
>>    : '—'
>>    ]);
>>    dv.table(["Файл", "Счет", "Категория", "Сумма", "Дата"], tableData);
>>    dv.paragraph('<div style="text-align: right;"><strong>Итого:</strong> ' + totalOperations + ' операций, Общая сумма: ' + totalSum.toLocaleString('ru-RU', { style: 'currency', currency: 'RUB' }) + '</div>');
>>}
>>\`\`\`
>>>[!faq|right-fixed] Показать последние: \`INPUT[number:view_list_income]\` записей
>---
>>[!info]+ Таблица расходов
>>\`\`\`dataviewjs
>>let filterConditions = {"view_list_income":null,"view_list_expense":null};
>>let viewLimitExpense = filterConditions.view_list_expense || Infinity;
>>
>>let expensePages = dv.pages('"Учет финансов/Расходы"')
>>    .where(p => {
>>        let matchType = filterConditions.type 
>>            ? filterConditions.type.includes(p["Тип"]) 
>>            : true;
>>
>>        let matchAccount = filterConditions.account 
>>            ? filterConditions.account.includes(p["Счет"]) 
>>            : true;
>>
>>        let matchCategory = filterConditions.category 
>>            ? filterConditions.category.includes(p["Категория"]) 
>>            : true;
>>
>>        let matchPrice = true;
>>        if (filterConditions.price) {
>>            if (filterConditions.price.more) {
>>                matchPrice = p["Сумма"] >= filterConditions.price.value;
>>            }
>>            if (filterConditions.price.less) {
>>                matchPrice = p["Сумма"] <= filterConditions.price.value;
>>            }
>>            if (filterConditions.price.exact) {
>>                matchPrice = p["Сумма"] == filterConditions.price.value;
>>            }
>>        }
>>
>>        let matchDate = true;
>>        if (filterConditions.date) {
>>            if (filterConditions.date.from) {
>>                matchDate = new Date(p["Дата"]) >= new Date(filterConditions.date.from);
>>            }
>>            if (filterConditions.date.to) {
>>                matchDate = matchDate && (new Date(p["Дата"]) <= new Date(filterConditions.date.to));
>>            }
>>        }
>>
>>        return matchType && matchAccount && matchCategory && matchPrice && matchDate;
>>    })
>>    .sort(p => new Date(p["Дата"]), 'desc')
>>    .slice(0, viewLimitExpense)
>>    .array();
>>
>>if (expensePages.length === 0) {
>>    dv.paragraph("Нет данных для отображения.");
>>} else {
>>    let amounts = expensePages.map(p => parseFloat(p["Сумма"] || '0'));
>>    let totalSum = amounts.reduce((acc, val) => acc + val, 0);
>>    let totalOperations = expensePages.length;
>>    let tableData = expensePages.map(p => [
>>        p.file.link,
>>        p["Счет"] || '—',
>>        p["Категория"] || '—',
>>        p["Сумма"] || '—',
>>        p["Дата"] && !isNaN(Date.parse(p["Дата"].split('.').reverse().join('-')))
>>    ? new Date(p["Дата"].split('.').reverse().join('-')).toLocaleDateString('ru-RU', { day: '2-digit', month: '2-digit', year: 'numeric' }) + ' г.'
>>    : '—'
>>    ]);
>>    dv.table(["Файл", "Счет", "Категория", "Сумма", "Дата"], tableData);
>>    dv.paragraph('<div style="text-align: right;"><strong>Итого:</strong> ' + totalOperations + ' операций, Общая сумма: ' + totalSum.toLocaleString('ru-RU', { style: 'currency', currency: 'RUB' }) + '</div>');
>>}
>>\`\`\`
>>>[!faq|right-fixed] Показать последние: \`INPUT[number:view_list_expense]\` записей
`;

    const existingFile = vault.getAbstractFileByPath(summaryFilePath);

    if (existingFile) {
        // Если файл уже существует и метаданные были обновлены, изменяем только frontmatter
        if (hasNewMetadata) {
            const updatedSummaryContent = summaryContent.replace(/^---\n[\s\S]*?\n---/, newFrontmatter);
            await vault.modify(existingFile, updatedSummaryContent);
            console.log("Файл `Учет финансов.md` успешно обновлен с новыми метаданными.");
        } else {
            console.log("Новых метаданных нет, файл не обновлен.");
        }
    } else {
        // Если файл не существует, создаем его с полным содержимым
        const updatedSummaryContent = newFrontmatter + mainContent;
        await vault.create(summaryFilePath, updatedSummaryContent);
        console.log("Файл `Учет финансов.md` создан и заполнен полностью.");
    }
}

// Выполнение функции обработки данных
await processData();
%>

