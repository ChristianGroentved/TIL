# Bigquery: Restore Deleted Table

You can restore a table if it hasn't been more than 7 days since it was deleted. 
To restore the table open the Cloud Shell command line and run the following command:

```shell
bq cp mydataset.mytable@-3600000 mydataset.newtable
```

`mydataset` is the name of the dataset where the table was located.
`mytable` is the name of the table to be restored.
`-3600000` is the number of milliseconds since the table was deleted. Eg. if the table was deleted on `2020-01-01 00:00:00`, then `-3600000` is the number of milliseconds since `2020-01-01 00:00:00`.
`mydataset.newtable` is the name of the new table.