---
layout: post
title: Return a list of tables with a matching column name
date:   2015-08-27
---
Sometimes reverse engineering a database requires searching for specific strings in all of the column names in all of the tables in the database. The following query will do such a search in Microsoft SQL Server.

```sql
SELECT
  COLUMN_NAME, TABLE_NAME 
FROM 
  INFORMATION_SCHEMA.COLUMNS 
WHERE 
  COLUMN_NAME LIKE '%searchString%'
```