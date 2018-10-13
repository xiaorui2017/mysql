#### MySQL外键约束的参照操作

1. **CASCADE**:从父表删除或更新且自动删除或更新子表中匹配的行

2. **SET NULL**：从父表删除或更新行，并设置子表中的外键列

3. **RESTRICT**:拒绝对父表的删除或更新操作

4. **NO ACTION**:标准SQL的关键字，在MySQL中RESTRICT相同



   **物理外键**指的是使用foreign key 作为外键关联另一张的字段的连接方法，而且限定了引擎为InnoDB,而逻辑外键，又叫做事实外键，是因为存在语法上的逻辑关联而产生的外键，需要有连接关键词inner join 或者left join 等等和连接部分，也就是on后面的部分,如果需要对应的设置，也可以加上set等语句

   alter table tbl_name add [column] col_name column_definition(列定义) [first | after col_name]这个语句是用来新增列的。不可以新增名字相同的列，但是若该列为外键约束，则可以直接修改其参数。

   直接修改已完成的外键约束:alter table tbl_name(子表) add froeign key(pid(外键)) references tbl_name1(父表) on delete cascade on update cascade;

   为外键约束增加随父表删除/更新自动删除/更新的参数。

   ```sql
   create table `provinces`(
   	'id' snallint(5) unsigned NOT NULL AUTO_INCREMENT,
       'pname' varchar(20) NOT NULL,
       PRIMARY KEY('id')
   )ENGINE=InnoDB DEFAULT CHARSET=utf8
   ```


```sql
create table `users`(
	'id' smallint(5) unsigned NOT NULL AUTO_INCREMENT,
	'username' varchar(10) NOT NULL,
    'pid' smallint(5) unsigned DEFAULT NULL,
    PRIMARY KEY('id'),
    KEY `pid` ('pid')
    CONSTRAINT `users_ibfk_1` FOREIGN KEY (`pid`) REFERENCES `provinces` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

#### **修改数据表**

【**添加单列**】：

​		ALTER TABLE tabl_name ADD [COLUMN] col_name column_definition [FIRS | AFTER col_name ] //FIRS：将添加的列添加到表的最前列，AFTER col_name：添加到col_name列的后面，如果两个都省略，则添加	到表的最后面。

【添加多列】：

​		ALTER TABLE tbl_name ADD [COLUMN] (col_name column_definiton,........);

【**删除单列**】：

​		 ALTER TABLE tbl_name DROP[COLUMN] col_name 

```sql
新增多列：
alter table users1 add test varchar(10) after age,add text varchar(20) first;
删除多列：
alter table users1 drop test,drop text;
```



```sql
test>SHOW COLUMNS FROM users1;
test>ALTER TABLE users1 ADD age TINYINT UNSIGNED NOT NULL DEFAULT 10;
test>ALTER TABLE users1 ADD password VARCHAR(32) NOT NULL AFTER username;
test>ALTER TABLE users1 ADD truename VARCHAR(20) NOT NULL FIRST;
test>ALTER TABLE users1 DROP truename;
test>ALTER TABLE users1 DROP password ,DROP age;
```

【**添加或删除约束**】：

ALTER TABLE table_name ADD [CONSTRAINT [symbol]] PRIMARY KEY  \[index_type] (index_col_name,...)//这是添加主键约束(只能有一个)

ALTER TABLE table_name ADD [CONSTRAINT [symbol]] UNIQUE \[INDEX/KEY] \[index_name] [index_type] (index_col_name,...);//这是添加唯一约束(可以有多个)

ALTER TABLE table_name ADD [CONSTRAINT [symbol]] FOREIGN KEY [index_name] (index_col_name,...) reference_definition;//这是添加外键约束(可以有多个)

ALTER TABLE table_name ALTER [COLUMN] col_name {SET DEFAULT literal(这个literal的意思是加上的default)/DROP DEFAULT}//添加或删除默认约束

ALTER TABLE table_name DROP PRIMARY KEY;//删除主键约束

ALTER TABLE table_name DROP {INDEX/KEY} index_name;//删除唯一约束

ALTER TABLE table_name DROP FOREIGN KEY fk_symbol;//删除外键约束

```sql
alter table user2 add unique (username)，(password);	//添加唯一约束

alter table user2 add constraint 主键约束名primary key (id) 1;	//添加主键约束

show create table user2	##展现表结构

show columns from user2	##查看表结构

alter table user2 alter sge set default 15	//默认约束

alter table user2 alter age drop default	//删除默认约束
```

**修改列定义**(比如将varchar修改为int 将该字段放在第一个)

```sql
 ALTER TABLE users MODIFY pid int UNSIGNED NOT NULL FIRST;
```

**修改列名称并修改列定义**(将列名pid修改为p_id 并修改字段类型)

```sql
ALTER TABLE users CHANGE pid p_id SMALLINT UNSIGNED NOT NULL;
```

 **修改表名**

```sql
ALTER TABLE users RENAME users2;
```

修改表名和修改列名尽量少用
