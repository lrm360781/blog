---
title: Error【1146】：Table 'xxx.xxx' doesn't exist问题原因及解决方法
date: 2019-03-06 11:15:53
tags:
    - MySQL
---
前言
===
我们在使用mysql数据库的时候，有时会因为各种误操作而导致各种问题。下面介绍的导致1146报错的原因及解决方法。

原因
===
* 由报错Table ‘xxx.xxxxx’ doesn’t exist可知，其中的mysql.proc表不存在而发生错误。
* 插入数据或更改数据时使用的表输入错误
* linux的mysql区分大小写，数据库中的表名与输入的sql语句中的使用的表名大小写不一致导致的
* 数据库操作时，误删mysql的文件导致(常见于数据库升级或迁移)
* 在编译安装mysql时，没有指定innodb存储引擎

解决步骤
===
* 查看自己的sql语句是否正确
* 查看是否有此表，不要忽视大小写
* 如该表真的不存在，则可能是表被误删或数据库迁移缺失文件等原因导致
   (a)创建此表
   (b)repair table 表名;   修复损坏表   
   (c)拷贝缺失文件(最常用方法)
   ```mysql
   原理：
   
   当表类型是MyISAM时,数据文件则以”Table.frm””Table.MYD””Table.MYI””三个文件存储于”/data/$databasename/”目录中。
   
   当表类型是InnoDB时,数据文件则存储在”$innodb_data_home_dir/″中的ibdata1文件中(一般情况)，结构文件存在于table_name.frm中。 
   
   MySQL的数据库文件直接复制便可以使用，但是那是指“MyISAM”类型的表。 而使用MySQL-Front直接创建表，默认是“InnoDB”类型，这种类型的一个表在磁盘上只对应一个“*.frm”文件，不像MyISAM那样还“*.MYD,*.MYI”文件。
   
   MyISAM类型的表直接拷到另一个数据库就可以直接使用，但是InnoDB类型的表却不行。 解决方法就是同时拷贝innodb数据库表“*.frm”文件和innodb数据“ibdata1”文件到合适的位置。
   ```
<1>从另外相同的mysql数据库或之前的数据库备份中导出该表的数据，然后通过命令行导入进去

<2>或直接拷贝原有数据库文件".frm"、".MYD"、"*.MYI"等文件，如果原数据库引擎是InnoDB,切记还需拷贝ibdata1文件
（暴力点的是直接拷贝之前备份了的data）

<3>重启数据库
* 如果是编译安装mysql时，没有指定innodb存储引擎
重新编译
```sql
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/usr/local/mysql/data -DSYSCONFDIR=/etc -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DMYSQL_UNIX_ADDR=/tmp/mysqld.sock -DMYSQL_TCP_PORT=3306 -DENABLED_LOCAL_INFILE=1 -DWITH_PARTITION_STORAGE_ENGINE=1 -DEXTRA_CHARSETS=all -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci
 24 

make && make install

```