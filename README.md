# Задача 1

![Схема БД](https://i.imgur.com/W0hR4nk.png)

В таблице Persons хранятся данные о людях, а таблица Relations отвечает за отношения между людьми. 
# Задача 2

![Схема БД](https://i.imgur.com/DS72b9R.png)
Таблица PaymentMethod - метод оплаты (например банкомат)
Таблица PaymentOperation - платежные операции (например мобильная связь)
Таблица Limit - ограничения. 
По моему мнению в эту схему нужно добавить сущность клиент, чтобы у каждого клиента были свои лимиты и можно было отслеживать, сколько клиент потратил за день, но в задаче 3, во входных данных нет ничего о клиентах, поэтому и в эту задачу я их не добавил
# Задача 3
```
function isPossblePayment(sum, payment_operation, payment_method){
	payment_operation_id = PaymentOperation.operation_id
	                       where PaymentOperation.operation_id == payment_operation
	payment_method_id = PaymentMethod.method_id
	                       where PaymentMethod.method_id == payment_method
	
	min_payment, max_payment, daily_limit = (Limit.min_payment, Limit.max_payment, Limit.daily_limit)
	                                        where (Limit.payment_method_id == payment_method_id and
	                                               Limit.payment_operation_id == payment_operation_id)
	greater_than_min = (sum >= min_payment) or min_payment is NULL
	lower_than_max = ((sum <= max_payment) or max_payment is NULL) and 
	                 ((sum <= daily_limit) or daily_limit is NULL)
	if greater_than_min and lower_than_max:
		return "Y"
	return "N"
}
```
1. Получаем id метода оплаты и id платежной операции
2. Получаем лимиты по данным входным данным
3. Проверяем что сумма проходит по лимитам (или лимитов нет)
# Задача 4
1
```
SELECT * 
FROM account 
WHERE client_id = <номер клиента> AND
	  created <= CURRENT_DATE AND
	  (closed > CURRENT_DATE OR closed IS NULL)
```
Простой запрос с проверкой, что счет открыт на данный момент

2
```
SELECT client_id 
FROM account 
GROUP BY client_id 
HAVING COUNT(*) = SUM(CASE WHEN closed <= CURRENT_DATE THEN 1 ELSE 0 END)
```
В этом запросе, мы получим только тех клиентов, у которых общее количество счетов равно количеству закрытых счетов (проверка в последней строке)

3 
```
SELECT client_id, SUM(amount) AS total_balance
FROM account
WHERE closed > CURRENT_DATE
GROUP BY client_id
ORDER BY total_balance DESC
```
Ценность клиентов оценивается по общему балансу на активных счетах.
