# Простой скрипт проверки быстродействия PHP

Работает со всеми версиями ПХП: от 4.3 до 7.4

## Зависимости

Необходимы модули для php:

- pcre
- mbstring
- json
- xmlrpc

Обычно они уже установлены или "вкомпилированны" в php.

Проверить наличие:

- в консоли `php -m`
- или через вывод функции `phpinfo()`

## Запуск

### 1. Через консоль

Команда:
```
php bench.php [-h|--help] [-d|--dont-recalc] [-D|--dumb-test-print] [-L|--list-tests] [-I|--system-info] [-m|--memory-limit=256] [-t|--time-limit=600] [-T|--run-test=name1 ...]

	-h|--help		- вывод помощи и выход
	-d|--dont-recalc	- не пересчитывать время выполнения тестов, если ограничения слишком низкие
	-D|--dumb-test-print	- вывод времени выполнения тупого теста, для отладки
	-L|--list-tests		- вывод списка доступных тестов и выход
	-I|--system-info	- вывод информации о системе без запуска тестов и выход
	-m|--memory-limit <Mb>	- установка значения параметра `memory_limit` в Мб, по-умолчанию равно 256 (Мб)
	-t|--time-limit <sec>	- установка значения параметра `max_execution_time` в секундах, по-умолчанию равно 600 (сек)
	-T|--run-test <name>	- запустить только указанные тесты, названия тестов из вывода параметра --list-tests, можно указать несколько раз
```
Например: `php bench.php -m=64 -t=30`

Второй вариант передачи значений для параметров - переменные окружения:
```
env PHP_MEMORY_LIMIT=64 PHP_TIME_LIMIT=30 php bench.php
```

Доступные переменные:

- PHP_MEMORY_LIMIT=<Мб>
- PHP_TIME_LIMIT=<Секунды>
- DONT_RECALCULATE_LIMITS=0/1
- LIST_TESTS=0/1
- SYSTEM_INFO=0/1
- RUN_TESTS=test1,test2,...

### 2. Через веб-сервера (apache + php)

Просто положите в любую доступную для выполнения php директорию сайта, например в корень.

Потом скрипт можно будет вызывать с параметрами, как из консоли:
`curl http://www.example.com/bench.php?memory_limit=64&time_limit=30`
или через браузер.

Доступные параметры:

- memory_limit=Мб
- time_limit=Секунды
- dont_recalculate_limits=0/1
- list_tests=0/1
- system_info=0/1
- run_tests=test1,test2,...

### Учет параметров хостинга

На многих хостингах параметры `memory_limit` и `max_execution_time` могут быть жестко зафиксированы.

В этом случа скрипт не сможет установить переданные в него значения параметров,
по крайней мере не выше лимитов.

Пересчет времени выполнения скрипта будет произведен по наименьшим результирующим значениям.

## ChangeLog

@ 2019-12-20, v1.0.35

 * Добавлена поддержка php-7.4.

@ 2019-05-10, v1.0.34

 * Поправлено определение модели CPU и частота в MHz для процессоров ARM.

@ 2019-05-01, v1.0.33

 * Новый тест для классов - доступ в данным через публичные свойства,
   геттеры-сеттеры, магические методы.
 * Детектирование xdebug - ругаемся и выходим
 * Вывод информации об операционной системе, если доступно

@ 2018-08-08, v1.0.32

 * Были неправы - stdClass есть в php-4 тоже

@ 2018-08-08, v1.0.31

 * Исправили тесты с различной сериализацией - объекты есть только в php-5+

@ 2018-08-08, v1.0.30

 * Добавили в некоторых тестах сериализации к объекту тестирования поля с разными типами данных
 * Поправили тесты xmlrpc - в php-7.2+libxmlrpc-epi проблемы со строками с html-тегами

@ 2018-08-08, v1.0.29

 * Добавили параметр -L для вывода списка тестов
 * Добавили параметр -T для запуска только конкретных тестов
 * Добавили параметр -I для вывода только информации о системе без запуска тестов

@ 2018-08-08, v1.0.28.1

 * Поправили вывод секунд - нужно на один символ больше места
 * Немного поменяли вывод информации

@ 2018-08-07, v1.0.28

 * Поправили пересчет размеров в единицы байт, при 0 происходила мат.ошибка

@ 2018-08-07, v1.0.27

 * Добавили новый параметр, отключающий пересчет ограничений по времени для тестов

@ 2018-08-07, v1.0.26

 * Добавили вывод общего кол-ва операций в секунду, и операций в секунду на МГц
 * Добавили вывод включенных необходимых модулей

@ 2018-08-06, v1.0.25

 * Добавили тестирование xmlrpc (xml)
 * Добавили вывод предупреждений, если не все необходимые модули php установлены

@ 2017-09-04, v1.0.24

 * Поправили пересчет времени тестов, если процессор Atom или ARM

@ 2017-09-04, v1.0.23

 * Обновили тест на работу с try-catch блоком - отдельные под-тесты: без блока, блок без exception, и с exception
 * Добавили пересчет времени тестов, если процессор Atom или ARM - они реально медленные

@ 2017-06-03, v1.0.22

 * Добавили тесты производительности новых операций в php-7
 * Вынесли инициализацию переменных за счетчики времени в тестах
 * Обновили счетчики времени для разных версий php
 * Тест array_range - насколько сильно влияет на следующий тест array_unset

@ 2017-05-25, v1.0.21

 * Добавили тесты производительности конвертации простых типов:
   `string => (int)`, `string => intval()`

@ 2017-05-19, v1.0.20

 * Поддержва длинных опций ком.строки только в php-5.3+
 * Добавили проверку форматирования строк - производительность сбора `''` строки с числами,
   или `""` строки с форматированием чисел внутри.
 * Очищаем данные после теста строк, массивов - меньше занятой памяти

@ 2017-05-19, v1.0.19

 * Попытка принудительно включить небуферизированный вывод
 * Спец-заголовок для nginx для отключения буферизации
 * Возможность загрузить основные тесты без файла php5.inc с тестом try/Exception/catch

@ 2017-05-18, v1.0.18

 * Проверка на совместимую версию php
 * Получение значений для настроек php - `max_execution_time` и `memory_limit` - из
   GET / getenv / getopt.

@ 2017-05-18, v1.0.17

 * Попытка укладываться в max_execution_time
   Т.к. зависимость от hardware не линейная - много hack-ов.
   Может не всегда срабатывать.

@ 2017-05-18, v1.0.16

 * Сделали поиск доступных алгоритмов хеширования для crypt()
 * По-умолчанию считаем, что доступен для всех MD5

@ 2017-05-17, v1.0.15

 * Поправили работу скрипта с php-7.x - больше ограничений по памяти
 * Добавили вывод используемой памяти (@ryr)

@ 2017-05-06, v1.0.14

 * Изменили работу скрипта, если доступно памяти менее 256Мб

@ 2017-05-06, v1.0.13

 * Поправили немного code-style (@ryr)
 * Добавили больше данных в тесты сериализации

@ 2017-04-21, v1.0.12

 * Правильная конвертация значений в единицы SI.
 * Считаем операции в секунду на МГц.
 * Обновил вывод - добавил заголовок столбцам

@ 2017-04-20, v1.0.11

 * Нагружаем процессор, чтобы определить MHz только если разница между
   значениями 'cpu MHz' и 'bogomips/2' большая.

@ 2017-04-20, v1.0.10

 * Тесты массивов теперь всегда включены, они больше не съедают много памяти
 * Добавлено определение CPU на Linux-системах, добавлен вывод операций на МГц
 * В выводе uname осталена только необходимая для сравнения информация
 * Обновлен README

@ 2017-04-06, v1.0.9

 * Поправлен подсчет операций в секунду для теста массивов

@ 2017-04-06, v1.0.8

 * Тесты, которых нет в php-4.4 вынесены в отдельный подключаемый файл

@ 2017-04-06, v1.0.7

 * Изменены названия функций-тестов для сортировки перед запуском
 * Обновлено форматирование вывода результатов тестов
 * Добавлены и обновлены тесты:
   - обращение к определенныи и неопределенным переменным/ключам массива
   - исключения (exceptions)
   - к хешированию добавлен тест crypt
   - тест массивов разбит на три уровня - время выполнения то же, памяти занимает меньше

@ 2015-07-16, v1.0.6

 * Добавлены тесты: preg & serialize

@ 2015-07-02, v1.0.5

 * Добавлен тест простейшего копирования строк

@ 2015-07-02, v1.0.4

 * Добавлено увеличение лимита по памяти и времени выполнения

@ 2015-07-02, v1.0.3

 * Исправлено определение доступных функций, сделан пропуск тестов для них

@ 2015-07-02, v1.0.2

 * Добавлено еще больше функций, теперь требуется наличие mbstring и json модулей
 * Потребление памяти увеличено из-за тестирования массивов - нужно более 1Гб

@ 2015-07-01, v1.0.1

 * Добавлен вывод потребления памяти
 * Добавлены новые функции, увеличен размер проверочной строки

## Пример вывода скрипта

```
<pre>
<<< WARNING >>>
CPU is in powersaving mode? Set CPU governor to 'performance'!
 Fire up CPU and recalculate MHz!
</pre>
<pre>
-------------------------------------------------------------------------------------------
|                                  PHP BENCHMARK SCRIPT                                   |
-------------------------------------------------------------------------------------------
Start               : 2019-05-01 15:35:12
Server              : Linux/4.4.0-146-generic x86_64
Platform            : Linux
System              : Ubuntu 16.04.6 LTS
CPU                 :
              model : Intel(R) Core(TM) i5-2300 CPU @ 2.80GHz
              cores : 4
                MHz : 3006.718MHz
Memory              : 256 Mb available
Benchmark version   : 1.0.33
PHP version         : 7.0.33-Rusoft/46.1
  available modules :
           mbstring : yes
               json : yes
             xmlrpc : yes
               pcre : yes
Max execution time  : 600 sec
Crypt hash algo     : MD5
-------------------------------------------------------------------------------------------
TEST NAME                      :      SECONDS |       OP/SEC |      OP/SEC/MHz |    MEMORY
-------------------------------------------------------------------------------------------
01_math                        :    3.163 sec | 316.12 kOp/s | 105.14  Ops/MHz |      4 Mb
02_string_concat               :    0.335 sec |  23.01 MOp/s |   7.65 kOps/MHz | 128.84 Mb
03_1_string_number_concat      :    2.342 sec |   2.13 MOp/s | 709.94  Ops/MHz |      4 Mb
03_2_string_number_format      :    1.929 sec |   2.59 MOp/s | 861.86  Ops/MHz |      4 Mb
04_string_simple_functions     :    2.852 sec | 455.77 kOp/s | 151.58  Ops/MHz |      4 Mb
05_string_multibyte            :    9.446 sec |  13.76 kOp/s |   4.58  Ops/MHz |      4 Mb
06_string_manipulation         :    6.753 sec | 192.51 kOp/s |  64.03  Ops/MHz |      4 Mb
07_regex                       :    3.589 sec | 362.25 kOp/s | 120.48  Ops/MHz |      4 Mb
08_1_hashing                   :    4.167 sec | 312.00 kOp/s | 103.77  Ops/MHz |      4 Mb
08_2_crypt                     :   11.056 sec | 904.48  Op/s |   0.30  Ops/MHz |      4 Mb
09_json_encode                 :    4.301 sec | 302.24 kOp/s | 100.52  Ops/MHz |      4 Mb
10_json_decode                 :    6.441 sec | 201.84 kOp/s |  67.13  Ops/MHz |      4 Mb
11_serialize                   :    2.939 sec | 442.33 kOp/s | 147.12  Ops/MHz |      4 Mb
12_unserialize                 :    3.908 sec | 332.69 kOp/s | 110.65  Ops/MHz |      4 Mb
13_array_fill                  :    3.112 sec |  16.07 MOp/s |   5.34 kOps/MHz |     12 Mb
14_array_range                 :    0.763 sec | 131.14 kOp/s |  43.62  Ops/MHz |     12 Mb
14_array_unset                 :    2.601 sec |  19.23 MOp/s |   6.39 kOps/MHz |     12 Mb
15_loops                       :    1.437 sec | 139.20 MOp/s |  46.30 kOps/MHz |      4 Mb
16_loop_ifelse                 :    1.805 sec |  27.69 MOp/s |   9.21 kOps/MHz |      4 Mb
17_loop_ternary                :    2.995 sec |  16.69 MOp/s |   5.55 kOps/MHz |      4 Mb
18_1_loop_defined_access       :    0.851 sec |  23.49 MOp/s |   7.81 kOps/MHz |      4 Mb
18_2_loop_undefined_access     :    5.845 sec |   3.42 MOp/s |   1.14 kOps/MHz |      4 Mb
19_type_functions              :    1.785 sec |   1.68 MOp/s | 558.94  Ops/MHz |      4 Mb
20_type_conversion             :    1.239 sec |   2.42 MOp/s | 805.26  Ops/MHz |      4 Mb
21_0_loop_exception_none       :    0.055 sec |  72.97 MOp/s |  24.27 kOps/MHz |      4 Mb
21_1_loop_exception_try        :    0.063 sec |  63.26 MOp/s |  21.04 kOps/MHz |      4 Mb
21_2_loop_exception_catch      :    4.011 sec | 997.35 kOp/s | 331.71  Ops/MHz |      4 Mb
22_loop_null_op                :    1.968 sec |  25.41 MOp/s |   8.45 kOps/MHz |      4 Mb
23_loop_spaceship_op           :    1.851 sec |  27.01 MOp/s |   8.98 kOps/MHz |      4 Mb
24_xmlrpc_encode               :    6.131 sec |  32.62 kOp/s |  10.85  Ops/MHz |      4 Mb
25_xmlrpc_decode               :    6.818 sec |   4.40 kOp/s |   1.46  Ops/MHz |      4 Mb
26_1_class_public_properties   :    0.155 sec |  32.31 MOp/s |  10.75 kOps/MHz |      4 Mb
26_2_class_getter_setter       :    0.484 sec |  10.33 MOp/s |   3.44 kOps/MHz |      4 Mb
26_3_class_magic_methods       :    1.452 sec |   3.44 MOp/s |   1.14 kOps/MHz |      4 Mb
-------------------------------------------------------------------------------------------
Total time:                    :  108.642 sec |   5.55 MOp/s |   1.84 kOps/MHz |
Current PHP memory usage:      :        4 Mb
Peak PHP memory usage:         :   125.45 Mb
</pre>
```
