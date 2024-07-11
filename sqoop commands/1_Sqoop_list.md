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
  --connect jdbc:mysql://localhost:3306/kundandb \
  --username kundan_user \
  --password kundan@123
```

# Sqoop list job
Run below command to list jobs.
```
sqoop job --list
```

# Sqoop create job
Run below command to create job.
```
sqoop job --create  myjob \
  --import \
  --connect jdbc:mysql://localhost/db \
  --username kundan_user \
  --password kundan@123
  --table employee \
  --m 1 
```

# Sqoop show job
Run below command to show particular job.
```
sqoop job --show   myjob
```

# Sqoop exec job
Run below command to execute particular job.
```
sqoop job --exec   myjob
```

# Sqoop delete job
Run below command to delete particular job.
```
sqoop job --delete   myjob
```