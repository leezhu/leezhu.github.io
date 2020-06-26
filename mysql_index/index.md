# Mysql索引操作

<!--more-->
长期在用sql，所以想对mysql做一下简要的整理，主要是最近自己工作需要用到的，和写代码要注意sql语句的地方。

## Tips

- Update：不能用子查询，例如`update table_name set xx='' where id in (select id ....)`,mysql5.7不支持，只能用inner join方式进行处理，将此表联合起来处理。正确姿势是`update table_name  a inner join(select id from table where id>=.. limit 1,1)t on a.id=t.id set a.'字段'=2;`这段代码意思是指定范围内的id进行更新。

- index：1、mysql的索引网上内容很多，这里详细就不赘述了。在实际开发中，对于上百万的行的mysql表来说，将经常查询的语句加上索引能够**加快查询**，不至于拖垮数据库。2、索引最好在建表的时候就想好建立，不然等到表行数超过上千万时，建索引非常慢。3、对于sql语句，索引的字段类型要符合表字段类型，例如字段是varchar类型，最好用字符型，这样能够加快查找。4、如果涉及数据库的程序比较慢时，最好查下数据库的慢查询日志，看是否是哪条记录。然后就进行优化。5、sql语句没有走索引会很慢，必要时可以加强制索引。


## Sql

### 1、添加索引：

`ALTER  TABLE  table_name  ADD  INDEX index_name (column1,  column2,column3 )`

### 2、增加列： 

`alter table TABLE_NAME add column NEW_COLUMN_NAME varchar(20) not null default '' comment '';`

### 3、修改索引：

```
-- 先删除

ALTER TABLE user

DROP INDEX idx_user_username;

--再以修改后的内容创建同名索引

CREATE INDEX idx_user_username ON user (username(8));
```


