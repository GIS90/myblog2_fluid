---
title: DB2存储过程异常处理
index_img: /img_index/index/20210507-001.png
categories:
  - 数据库
tags:
  - DB2
  - 存储过程
desc: 记录一下DB2数据库存储过程对异常的处理
keywords: >-
  DB2, 存储过程, DECLARE, EXIT, CONTINUE, UNDO, HANDLER, NOT FOUND, SQLEXCEPTIOIN,
  SQLWARNING
abbrlink: 40004
date: 2021-05-07 23:43:48
updated: 2021-05-07 23:43:48
---



{% note primary %}
记录下DB2数据库存储过程对异常的处理，DECLARE声明异常的方式。
{% endnote %}



<!--more-->
<hr />


{% label info@DB2 %} {% label default@存储过程 %} {% label danger@SQLEXCEPTIOIN %} {% label primary@DECLARE %}

{% raw %}
<div class="post_cus_note">语法糖</div>
{% endraw %}

```
DECLARE [handler-type] HANDLER FOR [condition] [handler-action]
```

> handler-type

异常处理器类型(handler-type)有以下几种：
- CONTINUE：处理器完成handler-action操作之后，继续执行产生这个异常语句的下一条语句，说白了就是执行一下handler-action。
- EXIT：处理器完成handler-action操作之后，存储过程终止，并将handler-action操作结果控制返回给调用者。
- UNDO：处理器操作handler-action执行之前，DB2会回滚存储过程中执行的SQL操作。在处理器操作完成之后，存储过程会终止，并将控制返回给调用者。

> condition

异常处理器可以处理基于特定SQLSTATE值的定制异常，或者处理预定义异常的类。预定义的3种异常如下所示：
- NOT FOUND：标识导致SQLCODE值为+100或者SQLSATE值为02000的异常。这个异常通常在SELECT没有返回行的时候出现。
- SQLEXCEPTIOIN：标识导致SQLCODE值为负的异常。
- SQLWARNING：标识导致警告异常或者导致+100以外的SQLCODE正值的异常。

> handler-action

handler-action为定义处理异常的sql语句，一般主要用来设定返回值、记录日志等操作。


{% raw %}
<div class="post_cus_note">实例</div>
{% endraw %}

```
declare exit handler for sqlexception
begin
    set i_code=sqlcode;
    set i_err_no=1;
    call SP_PASSYS_ERRHANDLE(v_proc_name,i_code);
    commit;
end;
```

- i_code：sqlexception产生的错误代码sqlcode。
- i_err_no：返回给调用者的返回值。
- SP_PASSYS_ERRHANDLE：调用日志记录存储过程。
