# Проект 1.  Анализ резюме из HeadHunter 

## Оглавление
[1. Описание проекта](https://github.com/murattumov/project1/blob/master/README.md#Описание-проекта)

[2. Какой кейс решаем?](https://github.com/murattumov/project1/blob/master/README.md#Какой-кейс-решаем)

[3. Краткая информация о данных](https://github.com/murattumov/project1/blob/master/README.md#abcd)

[4. Этапы работы над проектом](https://github.com/murattumov/project1/blob/master/README.md#Этапы-работы-над-проектом)

[5. Краткая информация](https://github.com/murattumov/project1/blob/master/README.md#Краткая-информация)

### **Описание проекта**
В нашем распоряжении будет база резюме, выгруженная с сайта поиска вакансий hh.ru. Часть соискателей не указывает желаемую заработную плату, когда составляет своё резюме. Чтобы построить модель, которая бы автоматически определяла примерный уровень заработной платы, подходящей пользователю, исходя из информации, которую он указал о себе необходимо преобразовать, исследовать и очистить данные.

### **Какой кейс решаем?**
Нам важно понять, как устроены признаки в данных и какие типы они имеют, чтобы произвести дальнейшие преобразования. Далее нужно преобразовать данные для удобной очистки и анализа данных. Провести первичный анализ зависимостей в наших данных о резюме. Обработка пропусков и выбросов данных.

### **Краткая информация о данных**
Данные представляют собой базу резюме, выгруженная с сайта поиска вакансий hh.ru. Состоит из 44744строк и 12столбцов (Пол, возраст;	ЗП; Ищет работу на должность:; Город, переезд, командировки; Занятость;	График; Опыт работы;	Последнее/нынешнее место работы;	Последняя/нынешняя должность;	Образование и ВУЗ; Обновление резюме; Авто). Также дополнительно используется данные о курсах различных валют, скачанных из общедоступных сайтов.

### **Этапы работы над проектом**
- Базовый анализ структуры данных

  1. Выводим несколько первых (последних) строк таблицы, чтобы убедиться в том, что ваши данные не повреждены. Познакомились с признаками и их структурой.
  2. Выведим основную информацию по таблице. Обращаем внимание на информацию о числе непустых значений в столбцах. 
  
- Преобразование данных
  1. Создаем с помощью функции-преобразования новый признак Образование" из признака "Образование и ВУЗ", который должен иметь 4 категории: "высшее", "неоконченное высшее", "среднее специальное" и "среднее".
  2. Создаем два новых признака "Пол" и "Возраст" из признака "Пол, Возраст". При этом важно учесть:
  
     * Признак пола должен иметь 2 уникальных строковых значения: 'М' - мужчина, 'Ж' - женщина.
     * Признак возраста должен быть представлен целыми числами.
  3. Из столбца "Опыт работы" выделяем общий опыт работы соискателя в месяцах, новый признак назовем "Опыт работы (месяц)".
  4. Создадим отдельные признаки "Город", "Готовность к переезду", "Готовность к командировкам" из признака "Город, переезд, командировки". При этом важно учесть:
     * Признак "Город" должен содержать только 4 категории: "Москва", "Санкт-Петербург" и "город-миллионник" (их список ниже), остальные обозначьте как "другие". 
     Список городов-миллионников: million_cities = ['Новосибирск', 'Екатеринбург','Нижний Новгород','Казань', 'Челябинск','Омск', 'Самара', 'Ростов-на-Дону', 'Уфа', 'Красноярск', 'Пермь', 'Воронеж','Волгоград']
     * Признак "Готовность к переезду" должен иметь два возможных варианта: True или False. 
     * Признак "Готовность к командировкам" должен иметь два возможных варианта: True или False. 
  5. Создадим признаки-мигалки для каждой категории из признаков "Занятость" и "График": если категория присутствует в списке желаемых соискателем, то в столбце на месте строки рассматриваемого соискателя ставится True, иначе - False.
  6. Переводим заработную плату в единую валюту, например, в рубли. Для этого экспортировав данные о курсах различных валют и акций за указанные периоды в виде csv файла создаем новый датафрейм со столбцами:
     * "currency" - наименование валюты в ISO кодировке,
     * "date" - дата,
     * "proportion" - пропорция,
     * "close" - цена закрытия (последний зафиксированный курс валюты на указанный день).
  Алгоритм преобразования:
     * Переводим признак "Обновление резюме" из таблицы с резюме в формат datetime и достаем из него дату. В тот же формат приводим признак "date" из таблицы с валютами.
     * Выделяем из столбца "ЗП" сумму желаемой заработной платы и наименование валюты, в которой она исчисляется. Наименование валюты переводим в стандарт ISO согласно с таблицей выше.
     * Присоедияем к таблице с резюме таблицу с курсами по столбцам с датой и названием валюты.
     * Умножаем сумму желаемой заработной платы на присоединенный курс валюты (close) и разделим на пропорцию, результат занесим в новый столбец "ЗП(руб)".
    
- Разведывательный анализ
  1. Построим график распределения признака "Возраст".
  ![](https://github.com/murattumov/project1/blob/master/plotly/hist1.html)
  2. Распределение признака "Опыт работы (месяц)".
  ![](https://github.com/murattumov/project1/blob/master/plotly/hist2.html)
  3. Распределение признака "ЗП (руб)".
  ![](https://github.com/murattumov/project1/blob/master/plotly/hist3.html)
  4. Диаграмма, которая показывает зависимость медианной желаемой заработной платы ("ЗП(руб)") от уровня образования ("Образование"). 
  ![](https://github.com/murattumov/project1/blob/master/plotly/bar4.html)
  5. Диаграмма, которая показывает распределение желаемой заработной платы ("ЗП(руб)") в зависимости от города ("Город"). 
  ![](https://github.com/murattumov/project1/blob/master/plotly/box5.html)
  6. Многоуровневая столбчатая диаграмма, которая показывает зависимость медианной заработной платы ("ЗП(руб)") от признаков "Готовность к переезду" и "Готовность к командировкам". 
  ![](https://github.com/murattumov/project1/blob/master/plotly/bar6.html)
  7. Тепловая карта, иллюстрирующую зависимость медианной желаемой заработной платы от возраста ("Возраст") и образования ("Образование").
  ![](https://github.com/murattumov/project1/blob/master/plotly/imshow7.html)
  8. Ддиаграмма рассеяния, показывающую зависимость опыта работы ("Опыт работы (месяц)") от возраста ("Возраст"). Возраст в годах.
  ![](https://github.com/murattumov/project1/blob/master/plotly/scatter_line8.png)
  9. Распределение медианной желаемой заработной платы («ЗП(руб)») в зависимости от пола («Пол»).
  ![](https://github.com/murattumov/project1/blob/master/plotly/box_add1.html)
  10. Зависимость медианной зарплаты («ЗП(руб)») от признаков "Пол" и "Возраст".
  ![](https://github.com/murattumov/project1/blob/master/plotly/bar_add2.html)

- Очистка данных
  1. Дубликаты: удаляем дубликаты в таблице с резюме.
  2. Пропуски: у нас есть пропуски в 3-х столбцах: "Опыт работы (месяц)", "Последнее/нынешнее место работы", "Последняя/нынешняя должность". Удаляем строки, где есть пропуск в столбцах с местом работы и должностью. Пропуски в столбце с опытом работы заполняем медианным значением.
  3. Выбросы: 
    * сначала очищаем данные вручную. Удаляем резюме, в которых указана заработная плата либо выше 1 млн. рублей, либо ниже 1 тыс. рублей.
    * В процессе разведывательного анализа мы обнаружили резюме, в которых опыт работы в годах превышал возраст соискателя. Удаляем их из данных.
    * В результате анализа мы обнаружили потенциальные выбросы в признаке "Возраст". Это оказались резюме людей чересчур преклонного возраста для поиска работы. Построили распределение признака в логарифмическом масштабе. 
    ![](https://github.com/murattumov/project1/blob/master/plotly/histplot_z.png)
    С помощью метода z-отклонений удаляем выбросы.

### **Краткая информация**

[Ссылка на Google Colab](https://drive.google.com/file/d/1e8sO0R4EqqfR8kJuj5rFaEbwD6-nntwz/view?usp=sharing)
****

:arrow_up:[к оглавлению](https://github.com/murattumov/project1/blob/master/README.md#Оглавление)
