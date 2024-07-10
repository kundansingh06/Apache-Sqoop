# Sqoop import

Sqoop import command is used to import data to HDFS from RDBMS such Oracle,MySql etc.

Below is the sample import command.


```
sqoop import \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_db \
  --username kundan_user \
  --password kundan@123 \
  --table order_items \
  --target-dir /user/kundan/sqoop_import/retail_db/order_items

```

There are two ways to provide the destination folder.

1. target-dir
2. warehouse-dir

Let us have a look on them.

## --target-dir

target-dir will create part files in the specified path.

```
sqoop import \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_db \
  --username kundan_user \
  --password kundan@123 \
  --table order_items \
  --target-dir /user/kundan/sqoop_import/retail_db/order_items
```

```
[kundan@gw02 ~]$ hadoop fs -ls /user/kundan/sqoop_import/retail_db/order_items
Found 5 items
-rw-r--r--   2 kundan hdfs          0 2019-06-01 04:02 /user/kundan/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   2 kundan hdfs    1303818 2019-06-01 04:02 /user/kundan/sqoop_import/retail_db/order_items/part-m-00000
-rw-r--r--   2 kundan hdfs    1343222 2019-06-01 04:02 /user/kundan/sqoop_import/retail_db/order_items/part-m-00001
-rw-r--r--   2 kundan hdfs    1371917 2019-06-01 04:02 /user/kundan/sqoop_import/retail_db/order_items/part-m-00002
-rw-r--r--   2 kundan hdfs    1389923 2019-06-01 04:02 /user/kundan/sqoop_import/retail_db/order_items/part-m-00003

```
Now run the above import command again,sqoop job will fail with error `"/user/kundan/sqoop_import/retail_db/order_items already exists"`.So by default,if target directory is already available sqoop import will fail.

*Directory can be overwritten by using option  `--delete-target-dir`*.This will delete the existing files and import the data.

```
sqoop import \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_db \
  --username kundan_user \
  --password kundan@123 \
  --table order_items \
  --target-dir /user/kundan/sqoop_import/retail_db/order_items \
  --delete-target-dir
```
*Directory can be appended by using option  `--append`*.This will append directory with new data.

```
sqoop import \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_db \
  --username kundan_user \
  --password kundan@123 \
  --table order_items \
  --target-dir /user/kundan/sqoop_import/retail_db/order_items \
  --append
```

## --warehouse-dir

warehouse dir will create a folder with the name of the table name provided in the command and files will be created inside that folder.


```
sqoop import \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_db \
  --username kundan_user \
  --password kundan@123 \
  --table order_items \
  --warehouse-dir /user/kundan/sqoop_import/retail_db

```

Output

```

[kundan@gw02 ~]$ hadoop fs -ls /user/kundan/sqoop_import/retail_db
Found 1 items
drwxr-xr-x   - kundan hdfs          0 2019-06-01 04:06 /user/kundan/sqoop_import/retail_db/order_items


[kundan@gw02 ~]$ hadoop fs -ls /user/kundan/sqoop_import/retail_db/order_items
Found 5 items
-rw-r--r--   2 kundan hdfs          0 2019-06-01 04:06 /user/kundan/sqoop_import/retail_db/order_items/_SUCCESS
-rw-r--r--   2 kundan hdfs    1303818 2019-06-01 04:06 /user/kundan/sqoop_import/retail_db/order_items/part-m-00000
-rw-r--r--   2 kundan hdfs    1343222 2019-06-01 04:06 /user/kundan/sqoop_import/retail_db/order_items/part-m-00001
-rw-r--r--   2 kundan hdfs    1371917 2019-06-01 04:06 /user/kundan/sqoop_import/retail_db/order_items/part-m-00002
-rw-r--r--   2 kundan hdfs    1389923 2019-06-01 04:06 /user/kundan/sqoop_import/retail_db/order_items/part-m-00003


```


*Directory can be overwritten by using option  `--delete-target-dir`*.This will delete the existing files and import the data.

```
sqoop import \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_db \
  --username kundan_user \
  --password kundan@123 \
  --table order_items \
  --warehouse-dir /user/kundan/sqoop_import/retail_db \
  --delete-target-dir
```
*Directory can be appended by using option  `--append`*.This will append directory with new data.

```
sqoop import \
  --connect jdbc:mysql://KUNDAN_IP_ADDRESS:3306/retail_db \
  --username kundan_user \
  --password kundan@123 \
  --table order_items \
  --warehouse-dir /user/kundan/sqoop_import/retail_db \
  --append
```

## Lifecycle of sqoop import execution.



* Connect to source database and get metadata
* Generate java file with metadata and compile to jar file
* Apply boundaryvalsquery to apply split logic, default 4
* Use split boundaries to issue queries against source database
* Each thread will have different connection to issue the query
* Each thread will get mutually exclusive sub set of the data
* Data will be written to HDFS in a separate file per thread

