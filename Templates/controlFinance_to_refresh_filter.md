<%*
console.log("Шаблон начал выполнение.");

if (!tp.config.active_file) {
    console.log("Ошибка: текущий активный файл не найден.");
} else {
    console.log("Активный файл найден, читаем содержимое...");

    const file = tp.config.active_file;
    const currentContent = await app.vault.read(file);
    const lines = currentContent.split('\n');

    console.log(`Содержимое файла загружено, всего строк: ${lines.length}`);

    const startIndex = lines.findIndex(line => line.trim().startsWith('>[!abstract] Учет финансов'));

    if (startIndex === -1) {
        console.log('Строка, начинающаяся с ">[!abstract] Учет финансов", не найдена.');
    } else {
        console.log(`Найдена строка для замены на позиции ${startIndex}`);

        const frontmatter = tp.frontmatter;

        const runVariables = Object.fromEntries(
            Object.entries(frontmatter).filter(([key, value]) => key.startsWith('run_'))
        );

        const createFilterConditions = (filters) => {
            let conditions = {};

            if (filters.run_filtered_type && filters.run_filtered_type.length > 0) {
                conditions.type = filters.run_filtered_type;
            }

            if (filters.run_filtered_account && filters.run_filtered_account.length > 0) {
                conditions.account = filters.run_filtered_account;
            }

            if (filters.run_filtered_category && filters.run_filtered_category.length > 0) {
                conditions.category = filters.run_filtered_category;
            }

            if (filters.run_price) {
                conditions.price = { 
                    value: filters.run_price, 
                    more: filters.run_more_price, 
                    less: filters.run_less_price, 
                    exact: filters.run_ident_price 
                };
            }

            if (filters.run_date_from || filters.run_date_to) {
                conditions.date = { 
                    from: filters.run_date_from, 
                    to: filters.run_date_to 
                };
            }

            conditions.view_list_income = filters.view_list_income || null;
            conditions.view_list_expense = filters.view_list_expense || null;

            return conditions;
        };

        const filterConditions = createFilterConditions(runVariables);
        console.log(`Условия фильтрации созданы: ${JSON.stringify(filterConditions)}`);

        const newContent = `>[!abstract] Учет финансов
>
>>[!info]+ Таблица Доходов
>>\`\`\`dataviewjs
>>let filterConditions = ${JSON.stringify(filterConditions)};
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
>>        new Date(p["Дата"]).toLocaleDateString('ru-RU') || '—'
>>    ]);
>>    dv.table(["Файл", "Счет", "Категория", "Сумма", "Дата"], tableData);
>>    dv.paragraph('<div style="text-align: right;"><strong>Итого:</strong> ' + totalOperations + ' операций, Общая сумма: ' + totalSum.toLocaleString('ru-RU', { style: 'currency', currency: 'RUB' }) + '</div>');
>>}
>>\`\`\`
>>>[!faq|right-fixed] Показать последние: \`INPUT[number:view_list_income]\` записей
>---
>>[!info]+ 
>>\`\`\`dataviewjs
>>let filterConditions = ${JSON.stringify(filterConditions)};
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
>>        new Date(p["Дата"]).toLocaleDateString('ru-RU') || '—'
>>    ]);
>>    dv.table(["Файл", "Счет", "Категория", "Сумма", "Дата"], tableData);
>>    dv.paragraph('<div style="text-align: right;"><strong>Итого:</strong> ' + totalOperations + ' операций, Общая сумма: ' + totalSum.toLocaleString('ru-RU', { style: 'currency', currency: 'RUB' }) + '</div>');
>>}
>>\`\`\`
>>>[!faq|right-fixed] Показать последние: \`INPUT[number:view_list_expense]\` записей
`;

        const updatedLines = lines.slice(0, startIndex).concat(newContent.split('\n'));
        const updatedContent = updatedLines.join('\n');

        await app.vault.modify(file, updatedContent);
        console.log("Шаблон выполнен: содержимое файла обновлено и сохранено.");
    }
}
%>