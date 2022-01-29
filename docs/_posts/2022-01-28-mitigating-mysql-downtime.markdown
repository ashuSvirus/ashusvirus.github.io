---
layout: post
title:  "Mitigating MySQL Downtime for big migrations"
date:   2022-01-28 22:10:21 +0530
categories: mysql downtime percona
---


I have been using MySQL for the last 3 years. In the beginning, the data size was small and manageable.

The problem started occurring when one of our tables grew to a size of 15 GB.

But 15 GB is nothing for MySQL, right? That’s what we thought.

Digging deeper we found that our index size was also 15GB. This can be solved if keep on increasing the database systems’ size and upgrade it.

To remove indexes, MySQL has `drop index` available with multiple options like

```
algorithm_option:
    ALGORITHM [=] {DEFAULT | INPLACE | COPY}

lock_option:
    LOCK [=] {DEFAULT | NONE | SHARED | EXCLUSIVE}
```
There are still things we need to consider before running this, like what will happen if the table is heavily used and updated very frequently.
To avoid that we can do the following things
1. Create New table
2. Copy data to the new table
3. Create procedures for inserts/updates in the original table
4. Drop index ( or any heavy operation, like optimize table )
5. Rename the table back to original
6. Drop old table
7. Drop procedures

Those are a lot of things. Wish we had a tool for this, so we could just automate it.

**Percona Toolkit by Percona**

More specifically for this change, we need pt-online-schema-change [https://www.percona.com/doc/percona-toolkit/LATEST/pt-online-schema-change.html](https://www.percona.com/doc/percona-toolkit/LATEST/pt-online-schema-change.html)
With this tool, any operation related to the schema can be done in the background.
```
pt-online-schema-change --alter=\"ADD INDEX big_table_name (some_column_name)\" \\ 
--analyze-before-swap --recursion-method=none D=db_name,t=big_table_name,h=hostname,\\
u=user_name --ask-pass --preserve-triggers --execute
```

Above is a simple add index operation. Even if the table is big, in the options we can specify how much CPU should be maintained. How many resources could be consumed by this utility.

What if you had replicas, and the lag becomes higher than usual. We can skip checks for them to avoid interruption.
List of options which can be passed are available here 
[https://www.percona.com/doc/percona-toolkit/3.0/pt-online-schema-change.html#options](https://www.percona.com/doc/percona-toolkit/3.0/pt-online-schema-change.html#options)
