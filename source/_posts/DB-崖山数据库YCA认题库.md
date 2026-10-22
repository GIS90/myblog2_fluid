---
title: 崖山数据库工程师YCA认证考试题库
index_img: /img_index/index/20261018-001.png
tags: [崖山数据库]
categories: [数据库]
abbrlink: 14672
date: 2026-10-18 22:32:01
updated: 2026-10-18 22:32:01
---


{% note primary %}
依据11份官方课件（YCA V1.0 培训教材）命制，共 117 题：单选 96 / 多选 10 / 判断 11。。
{% endnote %}



<!--more-->
<hr />

{% label info@崖山数据库 %} {% label warning@YCA %} {% label default@证书 %}


## 第一章 基础介绍（9 题）

### 1、（单选）YashanDB 是由哪家机构自主研发的？

- **A. 深圳计算科学研究院（深算院）  ✔**
- B. 某国外数据库厂商中国分部
- C. 开源社区基金会
- D. 国内高校联合实验室

**答案：A**

> 课件明确：深圳计算科学研究院（SICS）是深圳市“十大基础研究机构”之一，2018 年成立，YashanDB 为其原创自研数据库内核。

### 2、（单选）以下属于 YashanDB 原创理论的是（ ）

- **A. 有界计算理论  ✔**
- B. 关系数据库理论
- C. 分布式计算理论
- D. 高并发理论

**答案：A**

> 有界计算理论是 YashanDB（深算院）原创理论体系，实现与数据规模无关的查询响应能力，真正做到“大数据变小”。

### 3、（单选）以下不属于 YashanDB 产品特点的是（ ）

- **A. 免费开源  ✔**
- B. 100% 自研
- C. 超越融合
- D. 一站式应用迁移

**答案：A**

> YashanDB 是 100% 自研的商业数据库产品（内核代码自主率 100%），并非免费开源软件。

### 4、（单选）以下关于有界计算的说法错误的是（ ）

- A. 有界计算是 YashanDB 原创理论
- B. 有界计算实现与数据规模无关的查询响应能力，真正做到大数据变小
- **C. 有界计算主要解决数据快速恢复的问题  ✔**
- D. 有界计算是一种大数据查询分析方法

**答案：C**

> 有界计算是一种大数据查询分析方法，解决查询响应与数据规模无关的问题，与数据快速恢复无关（恢复由 Redo / 备份机制保障）。

### 5、（单选）崖山迁移平台（YMP）支持的源数据库不包括（ ）

- A. Oracle
- B. MySQL
- C. 达梦
- **D. PostgreSQL  ✔**

**答案：D**

> 课件列出 YMP 支持 Oracle、MySQL、达梦到 YashanDB 的迁移评估、离线迁移与一致性校验。

### 6、（单选）提供 Web 化自动安装部署、监控告警、备份恢复、健康巡检能力的平台是（ ）

- A. YMP
- **B. YCM  ✔**
- C. YDC
- D. SQLab

**答案：B**

> YCM（YashanDB Cloud Manager）是配套的一站式可视化运维监控管理平台；YMP 负责迁移，YDC 负责开发，SQLab 是在线 SQL 学习平台。

### 7、（多选）崖山共享集群的三大核心技术包括（多选）

- **A. 多写无锁并行引擎  ✔**
- **B. Cohesive Memory  ✔**
- **C. YFS 集群文件系统  ✔**
- D. 分布式一致性哈希路由

**答案：ABC**

> 共享集群三大核心技术：多写无锁并行引擎、Cohesive Memory（聚合内存）、自研 YFS 集群文件系统，用于突破金融核心“无人区”。

### 8、（单选）崖山认证体系的进阶顺序是（ ）

- **A. YCA → YCP → YCE  ✔**
- B. YCE → YCP → YCA
- C. YCP → YCA → YCE
- D. YCA → YCE → YCP

**答案：A**

> 认证分为 YCA（初级）、YCP（专业）、YCE（专家）三个层级。

### 9、（判断）YashanDB 内核代码自主率达 100%，并通过权威机构测试认证。

- **A. 正确  ✔**
- B. 错误

**答案：A**

> 课件“荣誉资质”中明确：某权威机构内核代码自主率 100% 测试，并通过信通院多项性能测试与 GB 18030-2022 最高级别认证。

## 第二章 安装部署（12 题）

### 1、（单选）yasboot 启动 YashanDB 集群的命令是（ ）

- **A. yasboot cluster start -c yashandb  ✔**
- B. yasboot process yasom start -c yashandb
- C. yasboot process yasagent start -c yashandb
- D. yasboot cluster stop -c yashandb

**答案：A**

> yasboot cluster start -c <集群名> 用于启动数据库；yasom / yasagent 由部署过程自动拉起，通常不需要单独启动。

### 2、（单选）以下关于 yasql 的说法，错误的是（ ）

- A. 使用 yasql / as sysdba 登录数据库，需要先配置操作系统认证
- B. yasql 可以通过 -f 参数执行 sql 文件
- **C. 使用 yasql 连接数据库时，如果密码包含特殊字符将会连接失败且无法避免  ✔**
- D. 可以通过 yasql -V 查看数据库版本信息

**答案：C**

> 密码含特殊符号时可用双引号包裹并转义（如 sys/"pwd@123"@host:port），或外层用单引号包裹，因此可以正常连接。

### 3、（单选）安装时用于生成 hosts.toml 与 yashandb.toml 配置文件的命令是（ ）

- **A. yasboot package se gen  ✔**
- B. yasboot package install
- C. yasboot cluster deploy
- D. yasboot cluster password set

**答案：A**

> 第一步：yasboot package se gen --cluster yashandb -u yashan -p yashan --ip ... 生成服务器配置与集群配置两个 toml 文件。

### 4、（单选）部署数据库（启动 yasdb 服务）的命令是（ ）

- A. yasboot package install -t hosts.toml -i 安装包
- **B. yasboot cluster deploy -t yashandb.toml  ✔**
- C. yasboot cluster start -c yashandb
- D. yasboot package uninstall --cluster yashandb

**答案：B**

> 第三步：yasboot cluster deploy -t yashandb.toml，会启动 yasom、yasagent 并最终启动 yasdb 服务。

### 5、（单选）关于 yasom 与 yasagent 进程，说法正确的是（ ）

- **A. yasom 全局 1 个，yasagent 每台服务器 1 个  ✔**
- B. yasom 每台服务器 1 个，yasagent 全局 1 个
- C. 两者都只在主节点各 1 个
- D. 两者都只有 1 个

**答案：A**

> yasom 是全局唯一的运维服务进程，负责接收 yasboot 指令并下发控制；yasagent 运行在每台服务器上，无状态地执行具体操作。

### 6、（单选）关于目录规划，下列说法正确的是（ ）

- A. $YASDB_HOME 存放数据文件与配置文件
- **B. $YASDB_DATA 存放数据库数据文件、部分日志与配置文件  ✔**
- C. install 目录是软件运行目录
- D. 三者可以随意互换

**答案：B**

> $YASDB_HOME 是软件安装目录（命令、库、组件）；$YASDB_DATA 是数据目录（系统数据文件、日志、配置）；install 仅用于上传软件包。

### 7、（单选）YashanDB 默认监听端口是（ ）

- A. 1521
- B. 3306
- **C. 1688  ✔**
- D. 5432

**答案：C**

> 默认监听端口 1688，安装时可通过 --begin-port 自定义，环境准备阶段需确认该端口未被占用。

### 8、（单选）内核参数 vm.max_map_count=2000000 的调整目的是（ ）

- **A. 限制一个进程可拥有的 VMA（虚拟内存区域）数量，使数据库进程充分使用主机资源  ✔**
- B. 关闭交换分区
- C. 调整本地端口范围
- D. 限制文件描述符数量

**答案：A**

> 该参数用于限制进程可拥有的虚拟内存区域数量，调大以让数据库进程充分使用主机资源；关闭交换分区对应 vm.swappiness=0。

### 9、（单选）在 /etc/security/limits.conf 中，nofile 表示（ ）

- **A. 用户可打开的最大文件描述符数量  ✔**
- B. 用户可创建的最大进程数
- C. 常驻内存大小上限
- D. 栈空间大小

**答案：A**

> nofile 为文件描述符数量（建议 65536），nproc 为进程数，rss 为常驻内存，stack 为栈空间（8192KB）。

### 10、（单选）为配置操作系统认证（免密登录数据库），需要创建的操作系统用户组是（ ）

- **A. YASDBA  ✔**
- B. oinstall
- C. dba
- D. yashangroup

**答案：A**

> 需创建 yashan 用户并加入 YASDBA 用户组，再配置 sudo 免密，之后即可使用 yasql / as sysdba 免密登录。

### 11、（单选）初始化 sys 用户密码的命令是（ ）

- **A. yasboot cluster password set -n yasdb_123 -c yashandb  ✔**
- B. yasboot cluster deploy -t yashandb.toml
- C. alter user sys identified by yasdb_123
- D. yasboot package se gen -p yasdb_123

**答案：A**

> 部署完成后执行 yasboot cluster password set -n <密码> -c <集群名> 初始化 sys 用户密码。

### 12、（判断）生产环境中关闭数据库一般不推荐使用 SHUTDOWN ABORT 方式。

- **A. 正确  ✔**
- B. 错误

**答案：A**

> ABORT 为强制中断，不会等待事务回滚完成，下次启动需要做实例恢复；生产一般推荐 SHUTDOWN IMMEDIATE。

## 第三章 用户管理（12 题）

### 1、（单选）下面关于数据库用户的说法错误的是（ ）

- A. 在崖山数据库中，通过 CREATE USER 语法创建一个普通用户
- B. 系统用户 SYS 是超级管理员，拥有全部权限
- **C. 每一个用户对应一个 Schema，该 Schema 名称可以与该用户不同名  ✔**
- D. 删除用户时，处于登录状态的用户无法删除

**答案：C**

> YashanDB 中每一个用户对应一个 Schema，且 Schema 名称与用户名相同。

### 2、（单选）下面关于授予和回收权限的语句，错误的是（ ）

- A. GRANT ALL PRIVILEGES TO sales;
- B. GRANT ALL PRIVILEGES ON sales.area TO sales01;
- **C. REVOKE ALL PRIVILEGES TO sales;  ✔**
- D. REVOKE ALL PRIVILEGES ON sales.area FROM sales01;

**答案：C**

> 回收权限语法为 REVOKE ... FROM ...，不能用 TO；系统权限回收用 REVOKE 权限 FROM 用户。

### 3、（单选）使用 CREATE USER 创建用户且未指定默认表空间时，默认表空间为（ ）

- A. SYSTEM
- **B. USERS  ✔**
- C. SYSAUX
- D. TEMP

**答案：B**

> 未指定 DEFAULT TABLESPACE 时，默认表空间为 USERS。

### 4、（单选）执行删除用户操作需要具备（ ）

- **A. DELETE USER 权限  ✔**
- B. DROP ANY TABLE 权限
- C. SYSDBA 角色
- D. 无需任何权限

**答案：A**

> 课件明确：执行删除用户操作需要拥有 DELETE USER 权限；且处于登录状态的用户无法删除。

### 5、（单选）DROP USER sales CASCADE 的作用是（ ）

- **A. 删除 sales 用户及其下的所有数据库对象  ✔**
- B. 仅删除 sales 用户，对象保留
- C. 删除 sales 用户的权限
- D. 删除 sales 用户的所有会话

**答案：A**

> CASCADE 表示同时删除该用户下的所有对象；不指定 CASCADE 时，用户必须不包含任何对象才能删除成功。

### 6、（单选）SELECT ANY TABLE 属于（ ）

- **A. 系统权限  ✔**
- B. 对象权限
- C. 角色
- D. 系统内置用户

**答案：A**

> 带 ANY 关键字、作用于全库范围的属于系统权限；作用在指定对象上的（如 SELECT ON sales.area）属于对象权限。

### 7、（单选）授予系统权限时，允许被授权者继续传递该权限的子句是（ ）

- **A. WITH ADMIN OPTION  ✔**
- B. WITH GRANT OPTION
- C. WITH PASS OPTION
- D. WITH TRANSFER OPTION

**答案：A**

> 系统权限使用 WITH ADMIN OPTION；对象权限使用 WITH GRANT OPTION。

### 8、（多选）以下属于系统权限的是（多选）

- **A. CREATE SESSION  ✔**
- **B. CREATE TABLE  ✔**
- C. SELECT ON sales.area
- **D. ALTER SYSTEM  ✔**

**答案：ABD**

> CREATE SESSION、CREATE TABLE、ALTER SYSTEM 均为系统级权限；SELECT ON 具体对象属于对象权限。

### 9、（判断）角色可以授予给另一个角色，实现权限的嵌套管理。

- A. 正确
- **B. 错误  ✔**

**答案：B**

> YashanDB 中角色只能授予给用户，不能将角色授予给另外一个角色；且用户与角色不能同名。

### 10、（判断）将角色授予用户后，该用户已建立的会话会立即获得该角色的权限。

- A. 正确
- **B. 错误  ✔**

**答案：B**

> 授予角色给用户后，仅对新连接的会话生效，已有会话不受影响。

### 11、（单选）内置角色中，使用户具备登录数据库（CREATE SESSION）能力的是（ ）

- **A. CONNECT  ✔**
- B. RESOURCE
- C. DBA
- D. PUBLIC

**答案：A**

> CONNECT 角色具有 CREATE SESSION 权限；RESOURCE 具有 CREATE TABLE/SEQUENCE/PROCEDURE/TRIGGER 权限。

### 12、（单选）查看对象权限授权信息应查询（ ）

- A. DBA_SYS_PRIVS
- **B. DBA_TAB_PRIVS  ✔**
- C. DBA_ROLE_PRIVS
- D. DBA_USERS

**答案：B**

> DBA_TAB_PRIVS 显示所有已授予的对象权限；DBA_SYS_PRIVS 显示系统权限；DBA_ROLE_PRIVS 显示角色授权。

## 第四章 SQL 语言（基础篇）（11 题）

### 1、（单选）以下关于 SQL 语言说法错误的是（ ）

- A. SQL 是结构化查询语言
- **B. SQL 是一种只支持查询的语言  ✔**
- C. SQL 是用于访问和管理数据库的标准计算机语言
- D. SQL 语言集数据定义、操纵、控制功能于一体

**答案：B**

> SQL 集 DDL、DML、DCL 于一体，不仅能查询，还能定义对象、操纵数据和控制事务与权限。

### 2、（单选）以下 SQL 分类中不属于 DDL 语句的是（ ）

- A. ALTER
- **B. SELECT  ✔**
- C. CREATE
- D. DROP

**答案：B**

> DDL 包括 CREATE、ALTER、DROP、TRUNCATE；SELECT 属于 DML。

### 3、（单选）以下哪个 SQL 关键字用于排序（ ）

- A. FROM
- B. WHERE
- **C. ORDER BY  ✔**
- D. SELECT

**答案：C**

> ORDER BY 用于对查询结果排序，DESC 降序、ASC 升序（默认）。

### 4、（多选）以下属于 DML 语句的是（多选）

- **A. SELECT  ✔**
- **B. INSERT  ✔**
- **C. UPDATE  ✔**
- **D. DELETE  ✔**

**答案：ABCD**

> DML 包括 SELECT、INSERT、UPDATE、DELETE，用于对表数据的查询与增删改。

### 5、（单选）关于 TRUNCATE 与 DELETE 的区别，说法正确的是（ ）

- **A. TRUNCATE 是 DDL，执行后不能回滚；DELETE 是 DML，未提交可回滚  ✔**
- B. TRUNCATE 可以带 WHERE 条件删除部分数据
- C. DELETE 只能删除表中全部数据
- D. 两者都需要显式 COMMIT 才能生效

**答案：A**

> TRUNCATE 是 DDL，立即生效、不可回滚且只能清空全表；DELETE 是 DML，可按条件删除，未提交前可回滚。

### 6、（单选）关于 CHAR 与 VARCHAR2，说法正确的是（ ）

- **A. CHAR 是定长字符串，VARCHAR2 是变长字符串  ✔**
- B. CHAR 是变长字符串，VARCHAR2 是定长字符串
- C. 两者都是数值类型
- D. 两者都是日期类型

**答案：A**

> CHAR 定长、VARCHAR/VARCHAR2 变长，均用于存储字母、汉字、数字与特殊字符。

### 7、（单选）TIMESTAMP 类型相比 DATE 类型多出的精度是（ ）

- A. 毫秒
- **B. 微秒  ✔**
- C. 分钟
- D. 时区

**答案：B**

> DATE 包含年月日时分秒，TIMESTAMP 额外包含微秒，如 2023-01-01 12:30:30.123456。

### 8、（单选）INT 类型的取值范围是（ ）

- **A. [-2147483648, 2147483647]  ✔**
- B. [0, 65535]
- C. [-32768, 32767]
- D. [0, 1E126]

**答案：A**

> 课件数据类型表：INT 取值范围 [-2147483648, 2147483647]；NUMBER 绝对值范围为 [1E-130, 1E126)。

### 9、（单选）LEFT JOIN 的查询结果是（ ）

- **A. 返回左表全部行，右表无匹配时补 NULL  ✔**
- B. 返回两表公共部分
- C. 返回右表全部行
- D. 返回两表全部行并去重

**答案：A**

> LEFT JOIN 从左表返回所有行，即使右表没有匹配，缺失侧以 NULL 填充；INNER JOIN 返回公共部分。

### 10、（判断）执行 INSERT 语句后，需要执行 COMMIT 才能将数据持久化提交。

- **A. 正确  ✔**
- B. 错误

**答案：A**

> COMMIT 属于 DCL，用于提交事务；未提交前可通过 ROLLBACK 回滚。

### 11、（单选）在 yasql 中查看表结构的命令是（ ）

- **A. DESC 表名  ✔**
- B. SHOW 表名
- C. LIST 表名
- D. DISPLAY 表名

**答案：A**

> DESC area 可查看表的列名、数据类型与约束等结构信息。

## 第五章 体系结构（12 题）

### 1、（单选）关于 DATA_BUFFER_SIZE 参数的作用，描述正确的是（ ）

- **A. 数据缓存区的大小  ✔**
- B. 数据块的大小
- C. 共享缓存区中 SQL 缓存的大小
- D. 全局执行内存区大小

**答案：A**

> DATA_BUFFER_SIZE 指定数据缓存区大小，默认 256M，取值范围 [32M, 64T]，越大数据库整体性能越好。

### 2、（多选）YashanDB 当前版本支持的事务隔离级别是（多选）

- A. 读未提交
- **B. 读已提交  ✔**
- C. 可重复读
- **D. 可串行化  ✔**

**答案：BD**

> 课件明确：YashanDB 支持读已提交与可串行化两种隔离级别，默认为读已提交。

### 3、（单选）以下不属于共享内存区域（SGA）的是（ ）

- A. 内存共享池
- B. 数据缓存
- **C. 会话栈内存  ✔**
- D. 虚拟内存

**答案：C**

> 会话栈内存与会话堆内存属于私有内存区域（SPA），SGA 包括共享池、数据缓存、有界加速缓存与虚拟内存。

### 4、（单选）控制 SQL 排序、物化、Hash Join 等算子可用内存的参数是（ ）

- **A. VM_BUFFER_SIZE  ✔**
- B. DATA_BUFFER_SIZE
- C. WORK_AREA_STACK_SIZE
- D. MAX_SESSIONS

**答案：A**

> VM_BUFFER_SIZE 指定标准计算内存，默认 128M，取值范围 [8M, 2T]；不足时会将虚拟内存交换到 SWAP 表空间。

### 5、（单选）负责将脏数据块从缓存写回数据文件的后台线程是（ ）

- **A. DBWR  ✔**
- B. LOGW
- C. CKPT
- D. TCP_LSNR

**答案：A**

> DBWR 写脏块；LOGW 负责 redo 刷盘；CKPT 调度检查点；TCP_LSNR 监听连接请求。

### 6、（单选）负责按策略将内存中的 redo 日志刷盘到 redo 日志文件的线程是（ ）

- **A. LOGW  ✔**
- B. DBWR
- C. CKPT
- D. WORKER

**答案：A**

> LOGW 基于定期或 redo 数量阈值等策略执行刷盘。

### 7、（单选）工作线程（WORKER）的连接数量上限由哪个参数控制（ ）

- **A. MAX_SESSIONS  ✔**
- B. DATA_BUFFER_SIZE
- C. VM_BUFFER_SIZE
- D. RUN_LOG_FILE_PATH

**答案：A**

> MAX_SESSIONS 控制会话连接上限；专用模式下每个客户端连接对应一个 WORKER 线程。

### 8、（单选）关于 YashanDB 行锁，说法正确的是（ ）

- **A. 行锁只有排他锁一种类型，不支持行级共享锁  ✔**
- B. 行锁分为共享锁与排他锁
- C. 行锁在 DDL 时自动加
- D. 行锁由 CKPT 线程管理

**答案：A**

> DML 修改数据时会锁定所修改的行记录，行锁仅排他锁，无行级共享锁。


### 9、（单选）以下不属于事务 ACID 特性的是（ ）

- A. 原子性
- B. 一致性
- C. 隔离性
- **D. 并发性  ✔**

**答案：D**

> 事务四大特性：原子性、一致性、隔离性、持久性。

### 10、（判断）在读已提交（Read Committed）隔离级别下，不会出现脏读现象。

- **A. 正确  ✔**
- B. 错误

**答案：A**

> 隔离级别对照表：读未提交会出现脏读/不可重复读/幻读；读已提交不会出现脏读。

### 11、（单选）关于实例与数据库的关系，说法正确的是（ ）

- **A. 一个实例最多加载一个数据库  ✔**
- B. 一个实例可同时加载多个数据库
- C. 数据库可以脱离实例独立提供服务
- D. 实例就是数据文件的集合

**答案：A**

> 实例由线程与内存结构组成，是提供服务的媒介；数据库是物理文件集合；一个实例最多加载一个数据库。

### 12、（单选）修改 DATA_BUFFER_SIZE 并指定 SCOPE=SPFILE 后，生效方式为（ ）

- **A. 写入参数文件，需重启数据库生效  ✔**
- B. 立即生效且重启后仍生效
- C. 仅当前会话生效
- D. 立即生效但重启后失效

**答案：A**

> SCOPE=spfile 写入参数文件需重启；memory 立即生效但重启失效；both 两者都生效（默认）。

## 第六章 实例管理（11 题）

### 1、（单选）以下关于 YashanDB 实例启动的说法错误的是（ ）

- A. NOMOUNT 状态会读取参数文件
- **B. NOMOUNT 状态会加载数据库  ✔**
- C. MOUNT 状态会读取控制文件
- D. OPEN 状态会打开数据库

**答案：B**

> NOMOUNT 只启动实例（读参数文件、启进程、分配内存），并不加载数据库；加载数据库发生在 MOUNT 阶段。

### 2、（单选）以下属于推荐的关闭 YashanDB 实例命令的是（ ）

- **A. SHUTDOWN IMMEDIATE  ✔**
- B. SHUTDOWN NORMAL
- C. SHUTDOWN ABORT
- D. 直接 kill 进程

**答案：A**

> IMMEDIATE 会中断连接、终止并回退执行中的事务，是一般推荐方式；ABORT 为强制关闭，生产不推荐。

### 3、（单选）实例启动到 NOMOUNT 后，查询 V$INSTANCE 的 STATUS 值为（ ）

- **A. STARTED  ✔**
- B. MOUNTED
- C. OPEN
- D. NOMOUNT

**答案：A**

> 三个阶段对应状态值：NOMOUNT→STARTED，MOUNT→MOUNTED，OPEN→OPEN。

### 4、（单选）数据库处于 MOUNT 状态时，可以执行的操作是（ ）

- **A. 恢复数据库  ✔**
- B. 对外提供完整读写服务
- C. 创建业务表并读写数据
- D. 切换打开模式

**答案：A**

> MOUNT 状态下已完成控制文件与数据文件加载，可执行恢复；完整读写需进入 OPEN。

### 5、（多选）YashanDB 数据库的打开模式包括（多选）

- **A. READWRITE  ✔**
- **B. RESETLOGS  ✔**
- **C. UPGRADE  ✔**
- D. READONLY

**答案：ABC**

> 三种打开模式：READWRITE（默认，生产读写）、RESETLOGS（重置日志号，不完全恢复）、UPGRADE（升级）。

### 6、（单选）YashanDB 实例参数文件的名称与默认路径是（ ）

- **A. yasdb.ini，位于 $YASDB_DATA/config  ✔**
- B. yashandb.toml，位于 $YASDB_HOME
- C. spfile.ora，位于 $YASDB_DATA
- D. hosts.toml，位于 $YASDB_HOME/conf

**答案：A**

> 参数文件是名为 yasdb.ini 的文本文件，存放在 $YASDB_DATA/config 路径下，非 memory 方式的修改会持久化到此文件。

### 7、（单选）ALTER SYSTEM SET 参数=值 未指定 SCOPE 时，默认生效方式为（ ）

- **A. both  ✔**
- B. memory
- C. spfile
- D. 不生效

**答案：A**

> SCOPE 默认为 both，即同时写入内存和参数文件，立即生效且重启后仍生效。

### 8、（单选）查看参数当前取值的命令是（ ）

- **A. SHOW PARAMETER 参数名  ✔**
- B. SELECT * FROM v$parameter
- C. DISPLAY PARAMETER
- D. ALTER SYSTEM SHOW

**答案：A**

> 例如 SHOW PARAMETER data_buffer_size 可查看参数值。

### 9、（单选）YashanDB 运行日志的默认路径与文件名是（ ）

- **A. $YASDB_HOME/log/yashandb/db-1-1/run/run.log  ✔**
- B. $YASDB_DATA/config/run.log
- C. $YASDB_HOME/log/yashandb.log
- D. $YASDB_DATA/log/alert.log

**答案：A**

> 默认路径受参数 RUN_LOG_FILE_PATH 控制，文件名 run.log 不可修改。

### 10、（判断）运行日志文件名 run.log 可以由管理员自行修改。

- A. 正确
- **B. 错误  ✔**

**答案：B**

> 课件明确：路径受 RUN_LOG_FILE_PATH 控制，但文件名 run.log 不可修改。

### 11、（单选）YashanDB 运行日志的默认级别是（ ）

- **A. INFO  ✔**
- B. DEBUG
- C. WARN
- D. ERROR

**答案：A**

> 日志级别由低到高：OFF、FATAL、ERROR、WARN、INFO（默认）、DEBUG、TRACE、ALL。

## 第七章 存储管理（11 题）

### 1、（单选）以下不属于 YashanDB 内置表空间的是（ ）

- A. SYSTEM 表空间
- B. SYSAUX 表空间
- **C. Redo 表空间  ✔**
- D. UNDO 表空间

**答案：C**

> 内置表空间包括 SYSTEM、SYSAUX、USERS、TEMP、UNDO、SWAP，不存在 Redo 表空间（Redo 以日志文件形式存在）。

### 2、（单选）以下不属于表空间维护操作的是（ ）

- A. 增加数据文件
- B. 删除数据文件
- C. 空闲空间回收
- **D. 增加 Redo 日志文件  ✔**

**答案：D**

> 常见维护：ADD DATAFILE、SHRINK SPACE 回收空间；增加 Redo 日志文件属于日志文件管理，不是表空间维护。

### 3、（单选）YashanDB 逻辑存储结构由大到小的正确顺序是（ ）

- **A. 表空间 → 数据段 → 数据区 → 数据块  ✔**
- B. 数据块 → 数据区 → 数据段 → 表空间
- C. 数据段 → 表空间 → 数据区 → 数据块
- D. 表空间 → 数据区 → 数据段 → 数据块

**答案：A**

> 表空间包含若干对象，每个对象至少包含一个数据段，段由若干数据区组成，区由物理连续的数据块组成。

### 4、（单选）YashanDB 数据块的默认大小是（ ）

- **A. 8K  ✔**
- B. 4K
- C. 16K
- D. 32K

**答案：A**

> 数据块是数据存储的最小逻辑单元，默认大小 8K。

### 5、（单选）作为 SYSTEM 辅助表空间、且为 AWR 快照信息默认表空间的是（ ）

- **A. SYSAUX 表空间  ✔**
- B. USERS 表空间
- C. TEMP 表空间
- D. SWAP 表空间

**答案：A**

> SYSAUX 承担 SYSTEM 的辅助职能，存放 YashanDB AWR 快照等信息。

### 6、（单选）用于保存回滚（撤销数据库更改）信息的表空间是（ ）

- **A. UNDO 表空间  ✔**
- B. TEMP 表空间
- C. SWAP 表空间
- D. USERS 表空间

**答案：A**

> UNDO 表空间用于创建和管理回滚信息；SWAP 用于临时数据排序，TEMP 用于临时表段。

### 7、（单选）删除一个包含数据库对象的表空间，需要指定（ ）

- **A. INCLUDING CONTENTS  ✔**
- B. KEEP DATAFILES
- C. CASCADE CONSTRAINTS
- D. AUTOEXTEND ON

**答案：A**

> INCLUDING CONTENTS 表示连同表空间内所有对象一起删除；缺省会删除失败。

### 8、（单选）表空间中对象被其他表空间的对象定义了外键约束时，删除该表空间需要指定（ ）

- **A. CASCADE CONSTRAINTS  ✔**
- B. INCLUDING CONTENTS
- C. AND DATAFILES
- D. SHRINK SPACE

**答案：A**

> 需先删除这些外键约束项，即同时指定 CASCADE CONSTRAINTS，否则即使指定 INCLUDING CONTENTS 也无法删除。

### 9、（单选）查看数据文件信息可使用（ ）

- **A. DBA_DATA_FILES  ✔**
- B. DBA_TABLESPACES
- C. DBA_USERS
- D. V$SESSION

**答案：A**

> DBA_DATA_FILES 显示所有数据文件信息；V$DATAFILE 显示数据文件汇总信息；DBA_TABLESPACES 显示表空间信息。

### 10、（判断）数据文件是 YashanDB 中最小的逻辑存储单元。

- A. 正确
- **B. 错误  ✔**

**答案：B**

> 最小逻辑存储单元是数据块（默认 8K），数据文件属于物理存储结构。

### 11、（单选）控制文件的作用是（ ）

- **A. 持久化数据库基本元数据，是挂载数据库的入口  ✔**
- B. 存储用户业务数据
- C. 记录 SQL 执行计划
- D. 存放 sys 用户密码

**答案：A**

> 控制文件持久化其他物理文件信息等基本元数据，是数据库挂载的入口。

## 第八章 文件管理（8 题）

### 1、（单选）以下文件中，用于定义数据库实例参数配置的是（ ）

- **A. 参数文件  ✔**
- B. 控制文件
- C. 数据文件
- D. 归档文件

**答案：A**

> 参数文件（yasdb.ini）定义实例的参数配置信息，位于 $YASDB_DATA/config。

### 2、（单选）存放数据库管理用户 sys 密码的文件是（ ）

- **A. 密码文件  ✔**
- B. 控制文件
- C. Redo 日志文件
- D. 归档文件

**答案：A**

> 密码文件保存数据库管理用户 sys 的密码，用于管理员身份认证。

### 3、（单选）Redo 日志文件的主要作用是（ ）

- **A. 记录数据库变更，用于故障恢复或主备同步  ✔**
- B. 保存数据字典信息
- C. 存储临时排序数据
- D. 保存审计记录

**答案：A**

> Redo 日志是记录数据库变更的物理日志，通常用于故障恢复或者主备同步。

### 4、（单选）归档文件是（ ）

- **A. Redo 日志完成归档后形成的日志  ✔**
- B. 控制文件的备份
- C. 数据文件的副本
- D. 参数文件的快照

**答案：A**

> 归档日志由 Redo 日志归档后形成，用于支撑基于时间点的恢复。

### 5、（多选）以下属于数据库物理存储结构的文件有（多选）

- **A. 参数文件  ✔**
- **B. 控制文件  ✔**
- **C. 数据文件  ✔**
- **D. Redo 日志文件  ✔**

**答案：ABCD**

> 物理存储结构指操作系统层面的物理文件：参数文件、密码文件、控制文件、数据文件、Redo 日志文件、归档文件等。

### 6、（单选）实例启动到 NOMOUNT 阶段时，读取的文件是（ ）

- **A. 参数文件  ✔**
- B. 控制文件
- C. 数据文件
- D. 归档文件

**答案：A**

> NOMOUNT 读取参数文件、启动进程、分配内存；MOUNT 阶段才加载控制文件；OPEN 阶段打开数据与日志文件。

### 7、（判断）数据文件用于存储数据库数据，是物理存储结构的组成部分。

- **A. 正确  ✔**
- B. 错误

**答案：A**

> 数据文件用于存储数据库数据，隶属于某个表空间，属于物理存储结构。

### 8、（单选）可通过哪个视图查看 redo 日志文件信息（ ）

- **A. V$LOGFILE  ✔**
- B. DBA_DATA_FILES
- C. DBA_TABLESPACES
- D. V$INSTANCE

**答案：A**

> V$LOGFILE 显示所有 redo 文件信息，属于动态性能视图。

## 第九章 对象管理（基础篇）（11 题）

### 1、（单选）以下不属于 YashanDB 数据库对象的是（ ）

- **A. 数据目录  ✔**
- B. 表
- C. 索引
- D. 视图

**答案：A**

> 数据库对象包括表、索引、视图、序列、同义词、存储过程、触发器等；“数据目录”不是数据库对象。

### 2、（判断）设置索引 INVISIBLE 之后，必须通过 REBUILD 重建索引才能使索引生效。

- A. 正确
- **B. 错误  ✔**

**答案：B**

> INVISIBLE / VISIBLE 只是控制索引对优化器是否可见，改为 VISIBLE 即可恢复，无需 REBUILD。

### 3、（单选）创建索引的正确语法是（ ）

- **A. CREATE INDEX idx_name ON table_name(column_name)  ✔**
- B. CREATE INDEX idx_name FOR table_name(column_name)
- C. ALTER TABLE table_name ADD INDEX idx_name
- D. CREATE INDEX idx_name(column_name) ON table_name

**答案：A**

> 语法：CREATE INDEX [user.]index_name ON table_name(column_name)。

### 4、（多选）视图的作用包括（多选）

- **A. 简化高频复杂查询  ✔**
- **B. 解耦基表结构变化  ✔**
- **C. 实现权限隔离  ✔**
- D. 提升数据写入性能

**答案：ABC**

> 视图可简化查询、解耦基表、隔离权限，但并不提升写入性能。

### 5、（单选）删除基表后，基于该基表创建的视图会（ ）

- **A. 变得不可用  ✔**
- B. 自动删除
- C. 仍然可用
- D. 自动重建

**答案：A**

> 删除视图不影响基表；但删除基表后，视图将变得不可用。

### 6、（单选）执行 ALTER INDEX idx UNUSABLE 后，效果为（ ）

- **A. 索引置为不可用状态，之后对表操作不会触发该索引更新  ✔**
- B. 立即删除索引
- C. 索引对优化器不可见但仍维护
- D. 自动触发索引重建

**答案：A**

> UNUSABLE 使索引不可用且不再随表操作更新；恢复可用需执行 ALTER INDEX ... REBUILD。

### 7、（单选）将表名 area_04 修改为 area_05 的语句是（ ）

- **A. ALTER TABLE area_04 RENAME TO area_05;  ✔**
- B. ALTER TABLE area_04 MODIFY area_05;
- C. RENAME TABLE area_04 TO area_05;
- D. ALTER TABLE area_04 RENAME COLUMN TO area_05;

**答案：A**

> 修改表名用 ALTER TABLE ... RENAME TO；修改列名用 RENAME COLUMN ... TO。

### 8、（单选）查询索引信息应使用的视图是（ ）

- **A. DBA_INDEXES  ✔**
- B. DBA_TABLES
- C. DBA_OBJECTS
- D. DBA_VIEWS

**答案：A**

> DBA_INDEXES 显示索引信息；DBA_OBJECTS 显示所有对象，DBA_VIEWS 显示视图及其定义文本。

### 9、（单选）序列（SEQUENCE）的主要用途是（ ）

- **A. 产生一组等间隔数值，主要用于表主键自增  ✔**
- B. 提高查询效率
- C. 替代索引
- D. 存储大对象数据

**答案：A**

> 序列可产生等间隔数值，常用于表主键的自增场景。

### 10、（单选）查询视图定义文本（text）应查询（ ）

- **A. DBA_VIEWS  ✔**
- B. DBA_OBJECTS
- C. DBA_INDEXES
- D. DBA_TABLES

**答案：A**

> SELECT owner, view_name, text FROM dba_views WHERE owner='SALES';

### 11、（单选）创建视图时，若视图已存在需要覆盖，应使用的语法是（ ）

- **A. CREATE OR REPLACE VIEW  ✔**
- B. CREATE VIEW IF NOT EXISTS
- C. ALTER VIEW
- D. REPLACE VIEW

**答案：A**

> CREATE [OR REPLACE] VIEW view_name AS subquery，OR REPLACE 表示已存在时替换。

## 第十章 数据字典（10 题）

### 1、（单选）以下关于 V$INSTANCE 视图的说法正确的是（ ）

- A. 只有 sys 用户才能查询 v$instance 视图
- **B. status 结果为 OPEN 说明数据库已经启动  ✔**
- C. 实例关闭状态也可以查询 v$instance 视图
- D. status 结果为空说明数据库已经启动

**答案：B**

> V$INSTANCE 显示实例状态汇总信息，status=OPEN 表示数据库处于打开状态；实例关闭后无法查询。

### 2、（单选）以下关于 DBA_VIEWS / ALL_VIEWS / USER_VIEWS 说法错误的是（ ）

- **A. 查询 DBA_VIEWS 必须需要 DBA 权限  ✔**
- B. 当某一用户拥有 DBA 权限时，DBA_VIEWS 与 ALL_VIEWS 查询的内容一致
- C. 查询 ALL_VIEWS 显示当前用户所有可访问的视图信息
- D. 查询 USER_VIEWS 显示当前用户下拥有的所有视图信息

**答案：A**

> 查询 DBA 视图需具有 DBA 权限或由其他用户进行对象权限授权后才可查询，并非“必须有 DBA 权限”。

### 3、（多选）YashanDB 动态视图的名称前缀包括（多选）

- **A. V$  ✔**
- **B. GV$  ✔**
- **C. DV$  ✔**
- D. DBA_

**答案：ABC**

> 动态视图以 V$、GV$ 或 DV$ 开头；DBA_ 属于系统视图前缀。

### 4、（单选）关于三组系统视图的包含关系，正确的是（ ）

- **A. DBA_ ≥ ALL_ ≥ USER_  ✔**
- B. USER_ ≥ ALL_ ≥ DBA_
- C. ALL_ ≥ DBA_ ≥ USER_
- D. 三者完全等价

**答案：A**

> DBA_ 为全库全局视图，ALL_ 为当前用户可访问对象，USER_ 为当前用户模式下对象，范围依次递减。

### 5、（单选）相对于 ALL_ 与 DBA_ 视图，USER_ 视图的结构特点是（ ）

- **A. 一般没有 OWNER 相关列  ✔**
- B. 多出 OWNER 列
- C. 只能由 sys 查询
- D. 包含全库对象

**答案：A**

> USER_ 视图展示当前用户自身对象，因此一般不含 OWNER 列；DBA_ 与 ALL_ 结构基本一样。

### 6、（单选）V$PROCESS 视图中的 THREAD_ID 对应（ ）

- **A. 操作系统的 PID  ✔**
- B. 会话 SID
- C. 事务 ID
- D. 数据文件编号

**答案：A**

> 课件明确：V$PROCESS 中 THREAD_ID 对应的是操作系统的 PID。

### 7、（单选）用于查询当前系统提供的所有动态视图名称的视图是（ ）

- **A. V$DYNAMIC_VIEWS  ✔**
- B. V$DATABASE
- C. V$SESSION
- D. V$LOGFILE

**答案：A**

> SELECT name FROM V$DYNAMIC_VIEWS WHERE name LIKE '%LOG%' 可模糊查询动态视图名。

### 8、（单选）关于系统表，说法正确的是（ ）

- **A. 由 YashanDB 自动维护，不建议数据库管理员自行修改  ✔**
- B. 可由管理员直接增删改以提高效率
- C. 仅在实例关闭时可访问
- D. 存放用户业务数据

**答案：A**

> 数据字典中的系统表由数据库自动维护，不建议手工修改其中数据。

### 9、（多选）通过 V$DATABASE 可以查询的信息包括（多选）

- **A. 归档模式（log_mode）  ✔**
- **B. 打开模式（open_mode）  ✔**
- **C. 保护模式（protection_mode）  ✔**
- **D. 数据块大小（block_size）  ✔**

**答案：ABCD**

> SELECT database_name, log_mode, open_mode, protection_mode, block_size FROM v$database;

### 10、（单选）查询当前连接到数据库的所有会话信息应使用（ ）

- **A. V$SESSION  ✔**
- B. V$PROCESS
- C. V$INSTANCE
- D. V$DATABASE

**答案：A**

> SELECT sid, serial#, username FROM v$session WHERE type='USER';

## 第十一章 应用开发（10 题）

### 1、（单选）使用 Java 连接 YashanDB，需要用到的驱动是（ ）

- A. C 驱动
- B. Python 驱动
- **C. JDBC 驱动  ✔**
- D. ADO.NET 驱动

**答案：C**

> Java 语言通过 JDBC 规范连接，YashanDB 提供符合 JDBC 规范的实现 jar 包。

### 2、（单选）YashanDB JDBC 连接格式为 jdbc:yasdb://host:port/database_name，以下说法错误的是（ ）

- **A. host 参数只能填 IP，不能填域名  ✔**
- B. host 参数不填时表示连接 localhost 主机
- C. database_name 参数是数据库名称
- D. port 参数是数据库服务端口

**答案：A**

> host 可以填写 IP 或域名，并非只能填 IP。

### 3、（多选）YashanDB 提供的数据库驱动包括（多选）

- **A. JDBC  ✔**
- **B. C  ✔**
- **C. Python  ✔**
- **D. ADO.NET / ODBC  ✔**

**答案：ABCD**

> 课件列出：JDBC、C、Python、ADO.NET、ODBC 等驱动。

### 4、（单选）YashanDB 的 Python 驱动模块名称是（ ）

- **A. yaspy  ✔**
- B. pymysql
- C. cx_Oracle
- D. psycopg2

**答案：A**

> yaspy 是支持 Python DB API 协议的 Python 拓展模块，通过 pip 安装 whl 包后使用。

### 5、（单选）SQL 语句中包含动态值（参数）时，推荐使用（ ）

- **A. PreparedStatement  ✔**
- B. Statement
- C. CallableStatement
- D. DriverManager

**答案：A**

> JDBC 三条路线：静态 SQL 用 Statement，含动态值用 PreparedStatement（重点），调用存储过程用 CallableStatement。

### 6、（单选）用于执行标准存储过程的对象是（ ）

- **A. CallableStatement  ✔**
- B. PreparedStatement
- C. ResultSet
- D. DriverManager

**答案：A**

> CallableStatement 用于执行标准存储过程路线。

### 7、（单选）关于 ResultSet，说法正确的是（ ）

- **A. 只有查询才会返回 ResultSet，其他操作返回 int  ✔**
- B. 所有 SQL 操作都返回 ResultSet
- C. ResultSet 由 DriverManager 创建
- D. ResultSet 不需要解析即可取值

**答案：A**

> ResultSet 存储 DQL 查询结果，需解析获取具体数据；增删改返回影响行数（int）。

### 8、（判断）YashanDB Python 驱动 yaspy 支持 Python DB API 协议。

- **A. 正确  ✔**
- B. 错误

**答案：A**

> 课件：yaspy 是支持 Python DB API 协议的 Python 拓展模块，包含 connect()、Connection、Cursor 等。

### 9、（单选）ODBC 标准接口所基于的 SQL 语法规范是（ ）

- **A. SQL-99  ✔**
- B. SQL-86
- C. SQL-2016
- D. T-SQL

**答案：A**

> ODBC 定义的内容包括函数调用库、基于 SQL-99 规范的 SQL 语法、标准错误码、标准登录方式与数据类型描述。

### 10、（单选）JDBC 编程中获取数据库连接的对象是（ ）

- **A. DriverManager  ✔**
- B. ResultSet
- C. Statement
- D. Cursor

**答案：A**

> DriverManager 负责注册第三方驱动并根据连接信息获取 Connection；Cursor 属于 Python 驱动概念。
