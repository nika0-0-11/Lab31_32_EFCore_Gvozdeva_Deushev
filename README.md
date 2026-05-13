# Лабораторная работа №31-32. Введение в SQLite и Entity Framework Core

---

## Основная информация 

**ФИО:** Гвоздева В.А, Деушев Т.Т  
**Группа:** ИСП-231  
**Дата:** 13.05.2026 

---

## Краткое описание работы

В ходе лабораторной работы мы изучили:

- **SQLite** - для базы данных 
-  **Entity Framework Core**
- **LINQ** для выполнения запросов


---

##  Полезные команды dotnet ef

|Команда|Назначение|
|:------|:----------|
|`dotnet ef --version`|Показывает версию|
|`dotnet ef migrations add InitialCreate`|Создать новую миграцию|
|`dotnet ef database update`|Применить все непримененные миграции к БД|
|`dotnet ef migrations list`|Посмотреть список миграций и их статус|
|`dotnet ef migrations remove`|Удалить последнюю миграцию (только если она ещё не применена)|
|`dotnet ef migrations script`|Посмотреть SQL без применения к БД|

---

## Структура проекта

- Lab31-32_EFCore_Gvozdeva_Deushev/ 
    - img/
        - gitPushLab31_32_Gvozdeva_Deushev.png
        - step4_migrationLab31_32_Gvozdeva_Deushev.png
        - step5_crudLab31_32_Gvozdeva_Deushev.png
        - step6_linqLab31_32_Gvozdeva_Deushev.png
        - step7_migrationLab31_32_Gvozdeva_Deushev.png
        - step8_sqlLogsLab31_32_Gvozdeva_Deushev.png
    - TaskDb/
        - Controllers/
            - TasksController.cs
        - Data/
            - AppDbContext.cs
        - Migrations/
            - 20260510152625_InitialCreate.cs
            - 20260510152625_InitialCreate.Designer.cs
            - 20260513122403_AddDueDateToTask.cs
            - 20260513122403_AddDueDateToTask.Designer.cs
            - AppDbContextModelSnapshot.cs
        - Models/ 
            - TaskDtos.cs
            - TaskItem.cs
    - .editorconfig
    - README.md

---

## Таблица применённых миграций

|Название миграции|Назначение|
|:----------------|:---------|
|`dotnet ef migrations add InitialCreate`|Создание таблицы (БД) **Tasks**|
|`dotnet ef migrations add AddDueDateToTask`|Добавление поля **DueDate** в таблицу (БД) **Tasks**|

---

## Сравнительная таблица LINQ vs SQL

|Концепция|Хранение в памяти|EF Core + SQLite|
|:--------|:----------------|:---------------|
|**Хранение данных**|static List<T> в RAM|Файл .db на диске|
|**После перезапуска**|Данные пропадают|Данные сохраняются|
|**Поиск по условию**|LINQ to Objects|LINQ to Entities → SQL|
|**Создание структуры**|Не нужно|Миграции (dotnet ef)|
|**Начальные данные**|Хардкод в коде|HasData() в миграции|
|**Получение данных**|list.FirstOrDefault(...)|await db.Table.FindAsync(id)|
|**Добавление**|list.Add(item)|db.Table.Add(item) + SaveChangesAsync()|
|**Удаление**|list.Remove(item)|db.Table.Remove(item) + SaveChangesAsync()|
|**Масштабируемость**|Ограничена RAM|Гигабайты данных|
|**Транзакции**|Нет|Встроены в EF Core|

---

## Главные выводы

1. **EF Core** — это переводчик между **C#** и **SQL**. Вы пишете **LINQ**, он пишет **SQL**.
2. **Миграции** — это система контроля версий для структуры БД. Так же, как Git для кода.
3. **Code First** удобнее, чем писать SQL вручную: изменил класс -> создал миграцию -> база обновлена.
4. **SaveChangesAsync()** — ключевой момент. До него все изменения живут только в памяти.
5. **async/await** при работе с **БД** — это не опционально, а стандарт. Блокировать поток
на время ожидания БД — плохая практика.