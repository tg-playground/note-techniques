# SQL Statements in MySQL

- Basics
- [Data Definition Statements](#Data Definition Statements) (DDL)
  - create, alter, drop, rename, truncate, comment
- Data Query Statements (DQL)
  - select
- Data Manipulation Statements (DML)
  - insert, update, delete, merge, call, explain plan, lock table
- Data Control Statements (DCL)
  - grant, revoke

## Basics

mysql -u arron -h 104.225.148.90 -p 

## Data Definition Statements

### Database

```sql
CREATE DATABASE `<db_name>` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

```sql
CREATE DATABASE IF NOT EXISTS `<db_name>` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

```sql
ALTER DATABASE `<db_name>` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Tables

Create Tables

```sql
DROP TABLE IF EXISTS `<table_name>`;
```



```sql
CREATE TABLE `<table_name>` (
    id INT(8) NOT NULL AUTO_INCREMENT PRIMARY KEY COMMENT 'ID',
    <column_name> <data_type> <is_null> <DEFAULT> <AUTO_INCREMENT> <UNIQUE/PRIMARY key> COMMENT '<column_comment>',
	... )
ENGINE='InnoDB'
COMMENT='<table_comment>';
```

**Keys**

CREATE INDEX Statement

```
CREATE [UNIQUE | FULLTEXT | SPATIAL] INDEX index_name
	[index_type]
    ON tbl_name (key_part,...)
    [index_option]
    [algorithm_option | lock_option]
    
key_part: {col_name [(length)] | (expr)} [ASC | DESC]

index_option: {
    KEY_BLOCK_SIZE [=] value
  | index_type
  | WITH PARSER parser_name
  | COMMENT 'string'
  | {VISIBLE | INVISIBLE}
  | ENGINE_ATTRIBUTE [=] 'string'
  | SECONDARY_ENGINE_ATTRIBUTE [=] 'string'
}

index_type:
    USING {BTREE | HASH}

algorithm_option:
    ALGORITHM [=] {DEFAULT | INPLACE | COPY}

lock_option:
    LOCK [=] {DEFAULT | NONE | SHARED | EXCLUSIVE}
```

Create Index Example

```sql
CREATE INDEX part_of_name ON customer (name(10));
CREATE INDEX part_of_name USING BTREE ON customer (name(10));
CREATE INDEX part_of_name_and_age USING BTREE ON customer (name(10), age);
ALTER TABLE employees ADD INDEX idx (generated_col);
```

Foreign key 

```sql
ALTER TABLE table_name1 add foreign key foreign_key_name(column_name) references table_name2(column_name2);
ALTER TABLE table_name DROP INDEX fk_name ;
```

Unique Key

```sql
ALTER TABLE table_name ADD CONSTRAINT fk_name UNIQUE (column_name_list);
```

