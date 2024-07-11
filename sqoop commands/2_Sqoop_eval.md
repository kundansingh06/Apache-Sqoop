# Sqoop eval

Sqoop-eval command is mainly used to run SQL queries and verify the results on console.
It can be used to test/preview the data before importing to HDFS.


We can either use `--query` or `--e` to provide the SQL command. 


*Using --e
```
sqoop eval \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_db \
  --username kundan_user \
  --password kundan@123 \
  --e "SELECT * FROM order_items LIMIT 10"
```

*Using --query
```
sqoop eval \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_db \
  --username kundan_user \
  --password kundan@123 \
  --query "SELECT * FROM order_items LIMIT 10"

sqoop eval \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_db \
  --username kundan_user \
  --password kundan@123 \
  --query "INSERT INTO orders VALUES (100000, '2017-10-31 00:00:00.0', 100000, 'DUMMY')"

sqoop eval \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_export \
  --username kundan_user \
  --password kundan@123 \
  --query "CREATE TABLE dummy (i INT)"

sqoop eval \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_export \
  --username kundan_user \
  --password kundan@123 \
  --query "INSERT INTO dummy VALUES (1)"

sqoop eval \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_export \
  --username kundan_user \
  --password kundan@123 \
  --query "SELECT * FROM dummy"
```