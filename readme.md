<h1>Линецкий владислав дмитриевич</h1>
<h2>Группа 3282</h2>

<h3>Запросы<h3>
<h4> 1. Выведите на экран любое сообщение </h4>

```plpgsql
SELECT 'Hello World'
```
![1](1.jpg)



<h4> 2. Выведите на экран текущую дату </h4>

```plpgsql
SELECT CURRENT_TIME
```
![2](2.png)



<h4> 3. Создайте две числовые переменные и присвойте им значение. Выполните математические действия с этими числами и выведите результат на экран. </h4>

```plpgsql
CREATE FUNCTION mm(x int, y int) RETURNS int AS $$
BEGIN
    RETURN x + y;
END
$$ LANGUAGE plpgsql
```
далее:

```plpgsql
SELECT mm(2,3);
```
![3](3.png)



<h4>4. Написать программу двумя способами 1 - использование IF, 2 - использование CASE. Объявите числовую переменную и присвоейте ей значение. Если число равно 5 - выведите на экран "Отлично". 4 - "Хорошо". 3 - Удовлетворительно". 2 - "Неуд". В остальных случаях выведите на экран сообщение, что введённая оценка не верна.</h4>  

if:
```plpgsql
CREATE FUNCTION iff(x int) RETURNS char AS $$
BEGIN
    IF (x = 5) THEN RETURN 'Отлично';
	  ELSIF (x = 4) THEN RETURN 'Хорошо';
	  ELSIF (x = 3) THEN RETURN 'Удовлетворительно';
	  ELSIF (x = 2) THEN RETURN 'Неуд';
	  ELSE RETURN 'Введите число от 2 до 5';
	  END IF;
END
$$ LANGUAGE plpgsql
```
![4_1](4_1.png)

switch case:

```plpgsql
CREATE OR REPLACE FUNCTION casef(x int) RETURNS char AS $$
BEGIN
    CASE WHEN x = 5 THEN RETURN 'Отлично';
	WHEN x = 4 THEN RETURN 'Хорошо';
	WHEN x = 3 THEN RETURN 'Удовлетворительно';
	WHEN x = 2 THEN RETURN 'Неуд';
	ELSE RETURN 'Введите число от 2 до 5';
	END CASE;
END
$$ LANGUAGE plpgsql
```
![4_2](5_2.png)



<h4>5. Выведите все квадраты чисел от 20 до 30 3-мя разными способами (LOOP, WHILE, FOR).</h4>  

LOOP:

```plpgsql
CREATE PROCEDURE loopp() AS $$
DECLARE
	x int := 20;
BEGIN
LOOP
	RAISE asd '%', x*x;
	x := x + 1;
	EXIT WHEN x > 30;		
END LOOP;
END
$$ LANGUAGE plpgsql;
```

![5_1](5_1.png)

WHILE:

```plpgsql
CREATE PROCEDURE whilee() AS $$
DECLARE
	x int := 20;
BEGIN
WHILE x <= 30 LOOP
	RAISE NOTICE '%', x*x;
	x := x + 1;	
END LOOP;
END
$$ LANGUAGE plpgsql;
```
![5_2](5_2.png)

FOR:

```plpgsql
CREATE PROCEDURE forr() AS $$
BEGIN
FOR x IN 20..30 LOOP
	RAISE NOTICE '%', x*x;	
END LOOP;
END
$$ LANGUAGE plpgsql;
```
![5_3](5_3.png)



<h4>6. Последовательность Коллатца. Берётся любое натуральное число. Если чётное - делим его на 2, если нечётное, то умножаем его на 3 и прибавляем 1. Такие действия выполняются до тех пор, пока не будет получена единица. Гипотеза заключается в том, что какое бы начальное число n не было выбрано, всегда получится 1 на каком-то шаге. Задания: написать функцию, входной параметр - начальное число, на выходе - количество чисел, пока не получим 1; написать процедуру, которая выводит все числа последовательности. Входной параметр - начальное число.</h4>

Функция:  

```plpgsql
    CREATE FUNCTION kollats1(x int) RETURNS int AS $$
    DECLARE
        aa int := 0;
    BEGIN
        WHILE x != 1 LOOP
            IF mod(x, 2) = 0 THEN
                x := x / 2;
            ELSE
                x := x * 3 + 1;
            END IF;
            aa := aa + 1;
        END LOOP;
    RETURN aa;
    END
    $$ LANGUAGE plpgsql;
```
![6_1](6_1.png)

Процедура:  

```plphsql
CREATE PROCEDURE kollats2(x int) AS $$
BEGIN
	WHILE x != 1 LOOP
		RAISE NOTICE '%', x;
		IF mod(x, 2) = 0 THEN
			x := x / 2;
		ELSE
			x := x * 3 + 1;
		END IF;
	END LOOP;
END
$$ LANGUAGE plpgsql;
```
![6_2](6_2.png)



<h4>7. Числа Люка. Объявляем и присваиваем значение переменной - количество числе Люка. Вывести на экран последовательность чисел. Где L0 = 2, L1 = 1 ; Ln=Ln-1 + Ln-2 (сумма двух предыдущих чисел). Задания: написать фунцию, входной параметр - количество чисел, на выходе - последнее число (Например: входной 5, 2 1 3 4 7 - на выходе число 7); написать процедуру, которая выводит все числа последовательности. Входной параметр - количество чисел.</h4>

Функция:

```plpgsql
CREATE FUNCTION luk1(x int) RETURNS int AS $$
DECLARE
l0 int := 2;
l1 int := 1;
l2 int;
BEGIN
	FOR x IN 3..x LOOP
		l2 := l1 + l0;
		l0 := l1;
		l1 := l2;
	END LOOP;
	RETURN l2;
END
$$ LANGUAGE plpgsql;
```
![7_1](7_1.png)

Процедура:

```plpgsql
CREATE PROCEDURE luk2(x int) AS $$
DECLARE
l0 int := 2;
l1 int := 1;
l2 int;
BEGIN
	RAISE NOTICE '%', l0;
	RAISE NOTICE '%', l1;
	FOR x IN 3..x LOOP
		l2 := l1 + l0;
		l0 := l1;
		l1 := l2;
		RAISE NOTICE '%', l2;
	END LOOP;
END
$$ LANGUAGE plpgsql;
```
Вызов процедуры производится при помощи запроса:
~~~plpgsql
CALL skywhywhalker(10);
~~~
![7_2](7_2.png)



<h4>8. Напишите функцию, которая возвращает количество человек родившихся в заданном году.</h4>

```plpgsql
CREATE FUNCTION pp(yearr int) RETURNS int AS $$
DECLARE
	aa int;
BEGIN
	SELECT count(*) INTO aa
	FROM people
	WHERE EXTRACT(year FROM people.birth_date) = yearr;
	RETURN aa;
END
$$ LANGUAGE plpgsql;
```
![8](8.png)



<h4>9. Напишите функцию, которая возвращает количество человек с заданным цветом глаз.</h4>

~~~plpgsql
CREATE FUNCTION pp2(c varchar) RETURNS int AS $$
DECLARE
	aa int;
BEGIN
	SELECT count(*) INTO aa
	FROM people
	WHERE people.eyes = c;
	RETURN aa;
END
$$ LANGUAGE plpgsql;
~~~
![9](9.png)



<h4>10. Напишите функцию, которая возвращает ID самого молодого человека в таблице.</h4>

```plpgsql
CREATE FUNCTION yong() RETURNS int AS $$
DECLARE
	aa int;
BEGIN
	SELECT id INTO aa
	FROM people
	WHERE birth_date = (SELECT max(birth_date) FROM people);
	RETURN aa;
END
$$ LANGUAGE plpgsql;
```
![10](10.png)



<h4>11. Напишите процедуру, которая возвращает людей с индексом массы тела больше заданного. ИМТ = масса в кг / (рост в м)^2.</h4>

```plpgsql

```



<h4>12. Измените схему БД так, чтобы в БД можно было хранить родственные связи между людьми. Код должен быть представлен в виде транзакции (Например (добавление атрибута): BEGIN; ALTER TABLE people ADD COLUMN leg_size REAL; COMMIT;). Дополните БД данными.</h4>  

Создаём новую таблицу :
~~~plpgsql
CREATE TABLE "rod" (
	"id" integer primary key,
	"people_id" INTEGER NOT NULL,
	"people_id_2" INTEGER NOT NULL, 
	"link" VARCHAR,
	FOREIGN KEY ("people_id") REFERENCES "people" ("id") ON UPDATE NO ACTION ON DELETE NO ACTION,
	FOREIGN KEY ("people_id_2") REFERENCES "people" ("id") ON UPDATE NO ACTION ON DELETE NO ACTION
);
~~~

Добавляем данные в неё через транзакции: 

```plpgsql
BEGIN;
	INSERT INTO rod (id, people_id, people_id_2, link)
	VALUES  (1, 2, 6, 'брат'),
			(2, 4, 5, 'сестра');
COMMIT;
```
![12](12.png)



<h4>13. Напишите процедуру, которая позволяет создать в БД нового человека с указанным родством.</h4>

```plpgsql

```

<h4>14. Измените схему БД так, чтобы в БД можно было хранить время актуальности данных человека (выполнить также, как п.12).</h4>
  
Для этого создаём дополнительный столбец :

```plpgsql
ALTER TABLE people ADD COLUMN update_date date default now()
```
![14](14.png)

<h4>15. Напишите процедуру, которая позволяет актуализировать рост и вес человека.</h4>

```plpgsql
CREATE PROCEDURE update(id_p int, new_growth numeric, new_weight numeric) AS $$
BEGIN
	UPDATE people SET growth = new_growth WHERE id = id_p;
	UPDATE people SET weight = new_weight WHERE id = id_p;
	UPDATE people SET update_date = now() WHERE id = id_p;
END;
$$ LANGUAGE plpgsql
```

Вызов процедуры производится при помощи запроса:

```plpgsql
call update(1, 150, 40);
```
![15_1](15_1.png)
![15_2](15_2.png)