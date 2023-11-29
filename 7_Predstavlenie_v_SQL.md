## Тема 7. Представление  в SQL

## Введение в представления SQL

Представление SQL (SQL view) - это виртуальная таблица, создаваемая на основе одной или нескольких таблиц, которая позволяет пользователям выполнять запросы к данным, как если бы они работали с обычной таблицей. Представления упрощают доступ к данным и обеспечивают абстракцию от фактической структуры базы данных.

## Создание представления SQL

Для создания представления SQL используется оператор `CREATE VIEW`. 

```sql
CREATE VIEW products_suppliers_categories AS
SELECT product_name, quantity_per_unit, unit_price, units_in_stock,
company_name, contact_name, phone,
category_name, description
FROM products
JOIN suppliers USING (supplier_id)
JOIN categories USING (category_id);

SELECT *
FROM products_suppliers_categories;

SELECT *
FROM products_suppliers_categories
WHERE unit_price > 20;

DROP VIEW products_suppliers_categories;

DROP VIEW IF EXISTS products_suppliers_categories;
```

## Обновляемые представления SQL

Однако в некоторых случаях можно создать обновляемые представления с помощью правил или триггеров.

```sql
SELECT * FROM orders;

CREATE OR REPLACE VIEW heavy_orders AS
SELECT * 
FROM orders
WHERE freight > 50;

SELECT * 
FROM heavy_orders
ORDER BY freight;

CREATE OR REPLACE VIEW heavy_orders AS
SELECT * 
FROM orders
WHERE freight > 100;

CREATE OR REPLACE VIEW products_suppliers_categories AS
SELECT product_name, quantity_per_unit, unit_price, units_in_stock, discontinued,
company_name, contact_name, phone, country,
category_name, description
FROM products
JOIN suppliers USING (supplier_id)
JOIN categories USING (category_id);


SELECT MAX(order_id)
FROM orders;

INSERT INTO heavy_orders
VALUES (11078, 'VINET', 5, '2019-12-10', '2019-12-15', '2019-12-14', 1, 120, 
		'Hanari Carnes', 'Rua do Paco', 'Bern', NULL, 3012, 'Switzerland');
		
SELECT * 
FROM heavy_orders
ORDER BY order_id DESC;


SELECT MIN(freight)
FROM orders;

DELETE FROM heavy_orders WHERE freight < 0.05;

SELECT MIN(freight)
FROM heavy_orders;

DELETE FROM heavy_orders WHERE freight < 100.25;

DELETE FROM order_details
WHERE order_id = 10854;
```


## Опция CHECK в представлениях

Опция `CHECK` в представлениях SQL позволяет применять ограничения к данным, возвращаемым представлением.

```sql
CREATE OR REPLACE VIEW heavy_orders AS
SELECT * 
FROM orders
WHERE freight > 100;

SELECT *
FROM heavy_orders
ORDER BY freight;

INSERT INTO heavy_orders
VALUES(11900, 'FOLIG', 1, '2000-01-01', '2000-01-05', '2000-01-04', 1, 80, 'Folies gourmandes', '184, chaussee de Tournai',
	   'Lille', NULL, 59000, 'FRANCE');
	   
SELECT *
FROM heavy_orders
WHERE order_id = 11900;

CREATE OR REPLACE VIEW heavy_orders AS
SELECT * 
FROM orders
WHERE freight > 100
WITH LOCAL CHECK OPTION;

CREATE OR REPLACE VIEW heavy_orders AS
SELECT * 
FROM orders
WHERE freight > 100
WITH CASCADE CHECK OPTION;
```