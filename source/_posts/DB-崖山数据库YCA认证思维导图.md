---
title: YashanDB YCA 学习思维导图
index_img: /img_index/index/20261011-001.png
tags: [崖山数据库]
categories: [数据库]
abbrlink: 61990
date: 2026-10-11 21:41:54
updated: 2026-10-11 21:41:54
---

{% note primary %}
基于官网培训教程生成的思维导图，帮助学习者快速理解YCA认证课程的知识点，认证课程包含11课时知识地图，官网教程培训地址：https://v.kaoshixing.com/exam/pc/customprocess/#/detail/18139?companyId=507743
{% endnote %}



<!--more-->
<hr />

{% label success@崖山数据库 %} {% label primary@YCA %} {% label warning@证书 %}




## 第一章 基础介绍

### 品牌背景

- 深算院 SICS：深圳市“十大基础研究机构”之一，2018 年成立
- 领军人物：樊文飞：中、美、欧科学院数据库方向唯一华裔院士
- 团队规模：近 500 人，研发占比 87%，136 万行原创内核代码
- 发展历程：2018 理论奠基 → 2019-2021 内核发布 → 2022 产品化 → 2024 产品体系成型
- 荣誉资质：内核代码自主率 100%、信通院多项测试、GB 18030 最高级认证

### 产品体系

- 产品形态：集中式 / 分布式；OLTP、HTAP、OLAP；主备、共享集群、MPP、存算分离
- YMP 迁移平台：Oracle / MySQL / 达梦 → YashanDB；迁移评估、离线迁移、一致性校验
- YCM 运维平台：Web 化自动安装部署、监控告警、备份恢复、健康巡检
- YDC 开发平台：对象管理、SQL 智能开发、数据导入导出、SQL/存储过程调试
- SQLab：在线 SQL 学习与分享平台，免安装即可访问

### 关键特性

- Oracle 四层兼容：语法、语义、PL/SQL、工具生态（AWR / RMAN 等）
- 100% 自研内核：代码自主率 100%，非开源产品
- 共享集群：多写无锁并行引擎、Cohesive Memory、YFS 集群文件系统
- 分布式：MPP 并行、SMP 算子、Raft 高可用、列存/内存引擎、存算分离
- 性能表现：BenchmarkSQL、TPC-H 100G 均优于同类商业 / 开源数据库
- 稳定可靠：CPU / 内存 / IO 波动低于 1.4%，存储预分配 Runtime 零增长

### 客户案例

- 某数据平台：49 个数源单位、17 亿条数据、3669 个数据接口
- 改造收益：容量提升 20+ 倍，SQL 性能提升 2~30 倍
- 高可用：主备架构故障自动切换，RTO<10s、RPO=0

### 认证体系

- 三级认证：YCA（初级）→ YCP（专业）→ YCE（专家）
- 学习资源：YashanDB 学习中心：yashandb.com/learning_center

## 第二章 安装部署

### yasboot 运维工具

- 功能范围：安装部署、离线升级、实例启停、sys 改密、集成 yasql、IP 更换
- yasom 进程：全局 1 个，接收 yasboot 指令并下发控制，管理 yasagent
- yasagent 进程：每台服务器 1 个，无状态，执行具体查询与操作任务

### 服务器准备

- 操作系统：CentOS 7.6 以上 / Kylin V10
- CPU：x86_64、ARM64；实验 2C+，生产 4C+
- 内存与硬盘：实验 4G / 50G；生产 16G / 50G+，推荐 SSD
- 文件系统与网络：ext4 或 xfs；百兆以上（生产千兆），支持 TCP/UDP

### 初始环境调整

- 端口与防火墙：确认 1688 未被占用；建议关闭 firewalld
- 内核参数：写入 /etc/sysctl.conf 后执行 sysctl -p 生效 `vm.swappiness=0 / vm.max_map_count=2000000 / ip_local_port_range=32768 60999`
- 资源限制：配置 /etc/security/limits.conf 中 yashan 用户 `nofile 65536 · nproc 65536 · rss unlimited · stack 8192`
- 用户与权限：配置操作系统认证，后续可免密登录数据库 `useradd yashan · groupadd YASDBA · sudo 免密`

### 目录规划

- install：/home/yashan/install：软件包上传目录
- $YASDB_HOME：/home/yashan/yasdb_home：命令、库及关键组件
- $YASDB_DATA：/home/yashan/yasdb_data：系统数据文件、日志、配置

### 命令行安装五步

- ① 生成配置：生成 hosts.toml 与 yashandb.toml `yasboot package se gen --cluster yashandb ...`
- ② 安装软件：解压并分发软件到各节点 `yasboot package install -t hosts.toml -i 安装包`
- ③ 部署集群：启动 yasom / yasagent / yasdb 服务 `yasboot cluster deploy -t yashandb.toml`
- ④ 环境变量：使 yasql 等命令可直接调用 `source yashandb.bashrc 并追加到 ~/.bashrc`
- ⑤ 初始化密码：设置 sys 用户密码 `yasboot cluster password set -n yasdb_123 -c yashandb`

### 验证与启停

- 登录验证：需先配置操作系统认证 `yasql / as sysdba`
- 状态检查：配合 v$database 查看库名 `select status from v$instance;`
- 启停集群：库内也可 shutdown immediate `yasboot cluster start|stop -c yashandb`
- 卸载顺序：最后清理 ~/.bashrc 中的环境变量 `cluster clean --purge → package uninstall`

### yasql 使用

- 连接方式：省略 IP/端口默认连本地 1688 `yasql sys/pwd@192.168.33.167:1688`
- 操作系统认证：无需输入用户名密码 `yasql / as sysdba`
- 特殊字符密码：双引号包裹并转义，或外层用单引号 `yasql sys/\"yasdb@123\"@host:1688`
- 常用参数：脚本执行与信息查看 `-H 帮助 · -V 版本 · @a.sql · -f a.sql`
- 显示与交互：COLUMN 仅对字符列生效；! 执行 shell 命令 `col col1 for a8   |   SQL> ! ls -l`

### DBeaver 连接

- 环境准备：JDK 17 及以上；推荐官方 DBeaver for YashanDB
- 新建连接：主机 IP、库/模式默认 yasdb、用户 sys/yasdb_123
- 验证：点击“测试连接”，成功后查看实例状态

## 第三章 用户管理

### 用户与模式

- 用户 User：登录数据库的账号，含密码、权限、默认表空间
- 模式 Schema：数据库对象的集合，通过 schema_name.object_name 访问
- 一一对应：每个用户对应一个同名 Schema
- 用户分类：系统用户（SYS 超级管理员，拥有全部权限）/ 普通用户

### 普通用户管理

- 创建用户：默认表空间为 USERS `CREATE USER sales IDENTIFIED BY 123456;`
- 指定表空间：表空间需提前创建 `CREATE USER sales01 IDENTIFIED BY 123456 DEFAULT TABLESPACE others;`
- 修改用户：可改密码、默认表空间、ACCOUNT lock/unlock `ALTER USER sales IDENTIFIED BY sales;`
- 删除用户：需 DROP USER 权限；登录中的用户不可删 `DROP USER sales [CASCADE];`

### 权限分类

- 系统权限：CREATE SESSION、CREATE TABLE、CREATE ANY TABLE、INSERT ANY TABLE、ALTER SYSTEM/DATABASE、CREATE USER/ROLE、ALL PRIVILEGES
- 对象权限：SELECT、INSERT、UPDATE、DELETE、ALTER、INDEX、FLASHBACK、ALL PRIVILEGES

### 授权与回收

- 系统权限授予：带 ADMIN OPTION 可继续传递 `GRANT SELECT ANY TABLE TO sales [WITH ADMIN OPTION];`
- 对象权限授予：带 GRANT OPTION 可继续传递 `GRANT SELECT ON sales.area TO sales01 [WITH GRANT OPTION];`
- 权限回收：立即生效，回收需谨慎 `REVOKE ... FROM ...`
- 授权前提：带 ADMIN/GRANT OPTION、GRANT ANY PRIVILEGE、DBA 角色、三权分立下 SECURITY_ADMIN

### 角色管理

- 角色概念：一组权限的集合；只能授予用户，不能授予角色；用户与角色不能同名
- 生效时机：授予用户角色后，仅对新连接的会话生效
- 内置角色：DBA、AUDIT_ADMIN、SECURITY_ADMIN、SYSDBA、SYSOPER、PUBLIC、CONNECT、RESOURCE
- 自定义角色 `CREATE ROLE role1 → GRANT ... TO role1 → GRANT role1 TO sales → DROP ROLE role1`
- 注意事项：授给角色不能带 WITH ADMIN/GRANT OPTION；内置角色禁止删除

### 权限相关视图

- DBA_USERS / DBA_ROLES：用户信息 / 角色信息（SYS_MAINTAINED=NO 为自定义角色）
- DBA_ROLE_PRIVS：角色授权信息
- DBA_SYS_PRIVS / DBA_TAB_PRIVS：系统权限 / 对象权限授权信息

## 第四章 SQL 语言（基础篇）

### SQL 概述

- 定义：Structured Query Language，访问与管理数据库的标准语言
- 特点：集数据定义、操纵、控制于一体；关系型数据库通用、简单易学

### SQL 分类

- DDL：CREATE、ALTER、DROP、TRUNCATE TABLE
- DML：SELECT、INSERT、UPDATE、DELETE
- DCL：COMMIT、ROLLBACK、GRANT、REVOKE

### 常用数据类型

- 数值型：INT（-2147483648~2147483647）、NUMBER
- 字符型：CHAR 定长、VARCHAR / VARCHAR2 变长
- 日期型：DATE（年月日时分秒）、TIMESTAMP（含微秒）

### DDL 实操

- 建表 `CREATE TABLE area(area_no CHAR(2) PRIMARY KEY, area_name VARCHAR2(60), DHQ VARCHAR2(20) NOT NULL);`
- 修改表结构：配合 DESC 查看结构 `ALTER TABLE area ADD / MODIFY / DROP COLUMN`
- 删除表 `DROP TABLE area;`
- TRUNCATE 对比 DELETE：TRUNCATE 是 DDL 只清全部、不可回滚；DELETE 是 DML 可条件删除、可回滚

### DML 实操

- 插入：可省略列名或指定列名，需一一对应 `INSERT INTO area VALUES ('01','华东','Shanghai'); COMMIT;`
- 更新：不写 WHERE 则更新全部 `UPDATE area SET dhq='Haerbin' WHERE area_no='06';`
- 删除：不写 WHERE 则删除全部 `DELETE FROM area WHERE area_no='06';`
- 查询：ASC 升序为默认 `SELECT area_no, area_name FROM area WHERE ... ORDER BY area_no DESC;`
- 查看结构 `DESC area;`

### 多表关联查询

- INNER JOIN：返回两表公共部分的行
- LEFT JOIN：左表全量，右表无匹配补 NULL
- RIGHT JOIN：右表全量，左表无匹配补 NULL
- FULL JOIN：两表全量，缺失侧补 NULL
- 写法：逗号 + WHERE 为等价隐式写法；可用表别名 `FROM area a INNER JOIN branches b ON a.area_no=b.area_no`

### DCL 实操

- 建用户并授权：REVOKE CONNECT FROM ycauser 回收 `CREATE USER ycauser IDENTIFIED BY yca123456; GRANT CONNECT TO ycauser;`
- 提交与回滚：事务控制的两种结果 `COMMIT; / ROLLBACK;`

## 第五章 体系结构

### 三大模块

- 线程管理：工作线程 + 后台线程的多线程架构
- 内存管理：共享内存区域 SGA + 私有内存区域 SPA
- 物理存储：控制文件、数据文件、Redo / 归档日志、切片文件等

### 实例与数据库

- 实例：YashanDB 线程 + 其管理的内存结构，是提供服务的媒介
- 数据库：参数、控制、Redo、数据、归档等物理文件的集合
- 关系：实例加载并打开数据库；一个实例最多加载一个数据库

### 共享内存 SGA

- 内存共享池：SQL 缓存、日志缓存、PL 池、数据字典缓存、大对象池
- 数据缓存：缓存数据块，LRU 算法淘汰，减少物理 I/O `data_buffer_size（默认 256M，32M~64T）`
- 有界加速缓存：类似数据缓存，但只缓存 AC 对象
- 虚拟内存：供排序、物化、hash join 使用；不足时换出到 SWAP 表空间 `vm_buffer_size（默认 128M，8M~2T）`
- 修改方式：SCOPE=SPFILE 需重启生效 `ALTER SYSTEM SET data_buffer_size=1G SCOPE=SPFILE;`

### 私有内存 SPA

- 会话栈内存：存放会话执行中的局部变量，建议默认值 `WORK_AREA_STACK_SIZE`
- 会话堆内存：存放生命周期较长的运行期数据，建议默认值 `WORK_AREA_HEAP_SIZE`
- 生命周期：会话创建时分配、退出时释放，会话独占

### 线程模型

- YASDB 进程：实例启动后创建，处理所有连接请求
- 工作线程 WORKER：每连接 1 个，负责会话业务调度，连接退出即退出 `MAX_SESSIONS 控制上限`
- TCP_LSNR：监听 TCP 端口，处理连接请求并创建会话
- DBWR：将脏数据块从缓存写回数据文件
- LOGW：按策略将 redo 日志从内存刷盘到日志文件
- CKPT：调度全量与增量检查点任务

### 事务

- 事务定义：一个或一组 SQL 组成的工作单元，全部提交生效或全部回滚撤销
- ACID：原子性、一致性、隔离性、持久性
- 并发现象：脏读、不可重复读、幻读
- 隔离级别：读未提交 / 读已提交 / 可重复读 / 可串行化
- YashanDB 支持：读已提交（默认）与可串行化两种

### 锁机制

- 表锁 S：共享锁，DML 时加，允许 DML 并发，阻塞并发 DDL
- 表锁 X：排他锁，DDL 时加，阻塞其他 DDL 与 DML
- 行锁：DML 修改行时加，仅排他锁，无行级共享锁
- 相关视图：配合 SELECT ... FOR UPDATE 观察 `SELECT * FROM v$lock;  SELECT * FROM v$locked_object;`

## 第六章 实例管理

### 启动三阶段

- NOMOUNT：读参数文件、启动进程、分配内存；可查实例视图、重建控制文件
- MOUNT：加载控制文件与数据文件信息，数据库仍关闭；可做恢复
- OPEN：初始化数据库、启动所有线程、开放全部功能
- 状态值：依次为 STARTED / MOUNTED / OPEN `SELECT status FROM v$instance;`

### 启动命令

- yasboot 方式 `yasboot cluster start -c yashandb -m nomount|mount|open`
- SQL 方式：需先启动到前一阶段 `ALTER DATABASE MOUNT;  ALTER DATABASE OPEN;`

### 数据库打开模式

- READWRITE：完整读写、对外提供服务；默认模式，生产推荐
- RESETLOGS：重置日志号，用于故障后的不完全恢复
- UPGRADE：数据库升级时使用
- 限制：OPEN 状态下不能切换到其他模式

### 关闭实例

- NORMAL：等待所有事务正常结束后关闭，默认方式
- IMMEDIATE：中断连接、终止执行中事务并回退，一般推荐
- ABORT：强制关闭，启动时需实例恢复，生产不推荐
- 命令 `yasboot cluster stop -c yashandb  |  SHUTDOWN IMMEDIATE;`

### 参数管理

- 参数文件：文本文件，非 memory 修改会持久化到此 `$YASDB_DATA/config/yasdb.ini`
- 查看参数 `SHOW PARAMETER data_buffer_size`
- 修改参数：默认 both；ALTER SESSION 仅当前会话生效 `ALTER SYSTEM|SESSION SET 参数=值 [SCOPE=spfile|memory|both];`
- SCOPE 取值：spfile 需重启；memory 立即生效但重启失效；both 两者都生效
- 直接编辑：不推荐；需重启生效，修改前先备份参数文件

### 运行日志

- 路径：受 RUN_LOG_FILE_PATH 控制，文件名不可修改 `$YASDB_HOME/log/yashandb/db-1-1/run/run.log`
- 作用：记录运行轨迹、调试信息、状态变迁、潜在问题与错误
- 日志级别：OFF、FATAL、ERROR、WARN、INFO（默认）、DEBUG、TRACE、ALL

## 第七章 存储管理

### 物理存储结构

- 参数文件：定义实例的参数配置信息
- 密码文件：存放数据库管理用户 sys 的密码
- 控制文件：持久化数据库基本元数据，是挂载数据库的入口
- Redo 日志文件：记录数据库变更，用于故障恢复与主备同步
- 数据文件 / 归档文件：存储数据 / Redo 归档后形成的日志

### 逻辑存储结构

- 表空间：逻辑单元，包含表、索引等数据库对象
- 数据段：每个对象至少 1 个段；含表段、索引段、回滚段
- 数据区：一组物理上连续的数据块，提高空间管理效率
- 数据块：最小逻辑单元，默认 8K；含表块、索引块、UNDO 块

### 内置表空间

- SYSTEM：数据字典、数据库管理信息、已编译存储对象
- SYSAUX：SYSTEM 的辅助表空间，AWR 快照默认存放
- USERS / TEMP：默认用户表空间 / 临时表段与临时数据
- UNDO / SWAP：回滚（撤销）信息 / 临时数据排序空间

### 表空间管理

- 创建 `CREATE TABLESPACE yashan1 DATAFILE 'yashan1' SIZE 4M AUTOEXTEND ON NEXT 4M MAXSIZE 1G;`
- 指定默认表空间：建用户时也可直接指定 `ALTER USER yashan_user1 DEFAULT TABLESPACE users;`
- 扩容 `ALTER TABLESPACE yashan1 ADD DATAFILE 'yashan1_02' SIZE 4M AUTOEXTEND OFF;`
- 缩容 `ALTER TABLESPACE yashan1 SHRINK SPACE;`
- 删除 `DROP TABLESPACE yashan1 [INCLUDING CONTENTS] [AND|KEEP DATAFILES] [CASCADE CONSTRAINTS];`

### 删除限制

- 默认表空间：不能删用户 DEFAULT TABLESPACE，需先 ALTER USER 变更
- 含对象：必须指定 INCLUDING CONTENTS
- 外键引用：即使有 INCLUDING CONTENTS，也需 CASCADE CONSTRAINTS
- 内置表空间：SYSTEM、SYSAUX、UNDO、SWAP、TEMPORARY 等不可删除

### 多表空间优点

- I/O 与隔离：用户数据与字典数据分离、应用间隔离、可分布到不同磁盘
- 可用性：单表空间可脱机而不影响其他表空间与整个数据库
- 备份：支持按单个表空间执行逻辑备份

### 相关视图

- DBA_TABLESPACES `SELECT tablespace_name,total_bytes/1024/1024 total_mb FROM dba_tablespaces;`
- DBA_DATA_FILES `SELECT tablespace_name,file_name FROM dba_data_files;`
- V$TABLESPACE / V$DATAFILE `SELECT name,status FROM v$datafile;`

## 第八章 文件管理

### 文件全景

- 参数文件：实例参数配置，yasdb.ini，位于 $YASDB_DATA/config
- 密码文件：保存数据库管理用户 sys 的密码
- 控制文件：数据库基本元数据，是打开（挂载）数据库的入口
- 数据文件：实际存储数据库数据，隶属某个表空间
- Redo 日志文件：记录数据库变更的物理日志，用于恢复与主备同步
- 归档文件：Redo 日志归档后形成，支撑不完全恢复

### 运维要点

- 定位文件：通过数据字典与动态视图中记录的路径、状态、大小定位
- 文件与实例：NOMOUNT 读参数文件，MOUNT 读控制文件，OPEN 打开数据与日志文件
- 关联章节：与第六章参数管理、第七章物理存储结合理解

## 第九章 对象管理（基础篇）

### 数据库对象

- 对象类型：表、索引、视图、序列、同义词、存储过程、触发器
- 表：组织管理数据的基本单位，按行与列组织
- 索引：建立在表上的逻辑对象，加速数据访问
- 视图：基于查询的虚定义逻辑对象

### 管理表

- 建表语法：省略用户名则建在当前用户下 `CREATE TABLE [user.]table_name (col type [约束], ...);`
- 约束与默认值：保证数据有效性 `NOT NULL / DEFAULT 'ShenZhen'`
- 修改表名 `ALTER TABLE area_04 RENAME TO area_05;`
- 修改列名 / 长度：MODIFY 可改长度 `ALTER TABLE area_05 RENAME COLUMN area_name TO area_name_01;`
- 增删字段：DESC 验证结果 `ALTER TABLE area_05 ADD NUM number(20);  ALTER TABLE area_05 DROP COLUMN num;`

### 管理索引

- 创建索引 `CREATE INDEX idx_area_06_info_1 ON area_06(area_no);`
- 可见性：控制是否对优化器可见 `ALTER INDEX idx INVISIBLE|VISIBLE;`
- 不可用与重建：UNUSABLE 后表操作不再维护该索引 `ALTER INDEX idx UNUSABLE;  ALTER INDEX idx REBUILD;`
- 删除索引 `DROP INDEX idx_area_06_info_1;`

### 管理视图

- 视图作用：简化查询、解耦基表变化、权限隔离
- 创建视图 `CREATE [OR REPLACE] VIEW v_area_06 AS SELECT area_no,area_name FROM area_06;`
- 删除视图：不影响基表；删基表则视图失效 `DROP VIEW v_area_06;`

### 对象视图

- DBA_OBJECTS `SELECT owner,object_name,object_type,status FROM dba_objects;`
- DBA_TABLES / DBA_INDEXES：表信息 / 索引信息
- DBA_VIEWS `SELECT owner,view_name,text FROM dba_views;`

## 第十章 数据字典

### 概念

- 数据字典：管理并显示元数据信息的系统表、系统视图与动态视图
- 系统表：由数据库自动维护，不建议手工修改
- 系统视图：基于系统表创建，一般以 DBA_、ALL_、USER_ 开头
- 动态视图：以 V$、GV$、DV$ 开头，实时反映运行与性能数据

### 系统视图三组

- DBA_：整个数据库的全局视图，含所有用户对象；需 DBA 权限或被授权
- ALL_：当前用户可访问的所有对象
- USER_：当前用户模式下的对象；一般无 OWNER 列
- 包含关系：DBA_ ≥ ALL_ ≥ USER_；有 DBA 权限时 DBA_ 与 ALL_ 等价

### 常用动态视图

- V$DYNAMIC_VIEWS：查看全部动态视图名称 `SELECT name FROM v$dynamic_views WHERE name LIKE '%LOG%';`
- V$INSTANCE：实例名、状态、版本 `SELECT instance_name,status,version FROM v$instance;`
- V$DATABASE：库名、归档模式、打开模式、保护模式、块大小 `SELECT database_name,log_mode,open_mode,protection_mode,block_size FROM v$database;`
- V$SESSION / V$PROCESS：会话信息 / 线程信息（THREAD_ID 即 OS PID） `SELECT sid,serial#,username FROM v$session WHERE type='USER';`
- V$LOGFILE：所有 redo 日志文件信息

## 第十一章 第十一章 · 应用开发

### 数据库驱动

- 作用：应用程序与数据库之间的接口（翻译员），抽象复杂的操作与通信
- 支持类型：JDBC、C、Python、ADO.NET、ODBC

### JDBC

- 原理：Java 提供 JDBC 接口规范，YashanDB 提供具体实现 jar
- 连接串 `jdbc:yasdb://host:port/database_name`
- 核心对象：DriverManager、Connection、Statement、PreparedStatement、CallableStatement、ResultSet
- 三条路线：静态 SQL / 预编译 SQL（重点）/ 标准存储过程
- 操作步骤：注册驱动 → 获取连接 → 创建 PreparedStatement → 占位符赋值 → 执行 → 解析结果集 → 关闭资源

### Python 驱动

- yaspy 模块：支持 Python DB API：connect()、Globals、Connection、Cursor
- 安装 `pip3 install yaspy-1.0.0-cp36-cp36m-linux_x86_64.whl`
- 操作步骤：引入驱动 → 获取连接 → 获取游标 → 执行 SQL → 关闭资源

### ODBC

- 定位：开放数据库互连，提供标准 API 访问数据库
- 标准内容：函数调用库、SQL-99 语法、标准错误码、标准登录方式、标准数据类型描述
