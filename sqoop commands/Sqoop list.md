# Sqoop list databases

Run below command to list databases.
```
sqoop list-databases \
  --connect jdbc:mysql://localhost:3306 \
  --username kundan_user \
  --password kundan@123

```
# Sqoop list tables
Run below command to list tables.

```
sqoop list-tables \
  --connect jdbc:mysql://localhost:3306/retail-db \
  --username kundan_user \
  --password kundan@123
```