---
title: DB2数据库表空间操作
index_img: /img_index/index/20220630-001.jpeg
categories:
  - [数据库]
tags: [DB2]
abbrlink: 5556
date: 2022-06-30 22:56:11
updated: 2022-06-30 22:56:11
desc: DB2数据库表空间操作
keywords: DB2, 数据库, 表空间
---



{% note primary %}
表空间对于DB2来说，就像一个容器，数据需要灌到这个容器里面。早上去项目，发现表空间报警提示马上满了，一定要提前扩容，不然数据库数据写不进去了。对表空间操作方式有2种：ADD与RESIZE。
{% endnote %}


<!--more-->
<hr />
{% label info@DB2 %} {% label warning@表空间 %}
<hr />

本项目使用的DB29.7版本，估计不同的版本表空间扩容都差不多。

### 1、扩容表空间

DB2CMD中执行，先连上数据库：
```
su - db2inst1
db2 connect to ykspas3 user pas using pas
```

#### 1.1、方式一：新增

```
db2 "alter tablespace TBS_IDX ADD (FILE '/home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_IDX/TBS_IDX3' 30G)"
```
文件改成项目对应的表空间实体文件。

#### 1.2、方式二：调整

```
-- 调整【resize立马查看发生变化】
db2 "alter tablespace TBS_IDX RESIZE (file '/home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_IDX/TBS_IDX2' 50G)"
```

### 2、表空间使用率

直接上SQL：
```
SELECT
	tbsp_id AS ID,
--	tbsp_create_time AS CREATE_TIME,
	tbsp_name as NAME,
	tbsp_content_type AS TYPE,
	tbsp_state AS STATE,
	sum(tbsp_total_size_kb)/1024 AS TOTAL_MB,
    sum(tbsp_used_size_kb)/1024 AS USED_MB,
    sum(tbsp_free_size_kb)/1024 AS FREE_MB,
    tbsp_page_size AS PAGE_SIZE
FROM SYSIBMADM.TBSP_UTILIZATION
--WHERE tbsp_name LIKE 'TBS_%'
GROUP BY tbsp_id, tbsp_create_time, tbsp_name, tbsp_content_type, tbsp_state, tbsp_page_size
ORDER BY tbsp_name
```

![](tablespace-sql.png)


### 3、其他查询表空间SQL

#### 3.1、查看表空间

```
-- TBSP_UTILIZATION
SELECT * FROM SYSIBMADM.TBSP_UTILIZATION


-- SNAPTBSP
SELECT * FROM SYSIBMADM.SNAPTBSP

-- SNAPTBSP_PART
SELECT * FROM SYSIBMADM.SNAPTBSP_PART

-- SYSTABLESPACES
SELECT * FROM SYSIBM.SYSTABLESPACES
```
具体的区别有兴趣的可以baidu，这里作为参考SQL。


#### 3.2、查看表空间列表

```
db2 list tablespaces show detail

           Tablespaces for Current Database

 Tablespace ID                        = 0
 Name                                 = SYSCATSPACE
 Type                                 = Database managed space
 Contents                             = All permanent data. Regular table space.
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 401408
 Useable pages                        = 401404
 Used pages                           = 398888
 Free pages                           = 2516
 High water mark (pages)              = 398888
 Page size (bytes)                    = 4096
 Extent size (pages)                  = 4
 Prefetch size (pages)                = 4
 Number of containers                 = 1

 Tablespace ID                        = 1
 Name                                 = SYSTOOLSTMPSPACE
 Type                                 = System managed space
 Contents                             = User Temporary data
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 1
 Useable pages                        = 1
 Used pages                           = 1
 Free pages                           = Not applicable
 High water mark (pages)              = Not applicable
 Page size (bytes)                    = 4096
 Extent size (pages)                  = 4
 Prefetch size (pages)                = 4
 Number of containers                 = 1

 Tablespace ID                        = 2
 Name                                 = TBS_MXZ
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 58982400
 Useable pages                        = 58982112
 Used pages                           = 55210560
 Free pages                           = 3771552
 High water mark (pages)              = 55210688
 Page size (bytes)                    = 8192
 Extent size (pages)                  = 32
 Prefetch size (pages)                = 256
 Number of containers                 = 9

 Tablespace ID                        = 3
 Name                                 = SYSTOOLSPACE
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 8192
 Useable pages                        = 8188
 Used pages                           = 620
 Free pages                           = 7568
 High water mark (pages)              = 620
 Page size (bytes)                    = 4096
 Extent size (pages)                  = 4
 Prefetch size (pages)                = 4
 Number of containers                 = 1

 Tablespace ID                        = 4
 Name                                 = TBS_PAS
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 7864320
 Useable pages                        = 7864256
 Used pages                           = 2023904
 Free pages                           = 5840352
 High water mark (pages)              = 2023904
 Page size (bytes)                    = 8192
 Extent size (pages)                  = 32
 Prefetch size (pages)                = 64
 Number of containers                 = 2

 Tablespace ID                        = 5
 Name                                 = TBS_JKSJ
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 655360
 Useable pages                        = 655344
 Used pages                           = 505808
 Free pages                           = 149536
 High water mark (pages)              = 516144
 Page size (bytes)                    = 8192
 Extent size (pages)                  = 16
 Prefetch size (pages)                = 32
 Number of containers                 = 1

 Tablespace ID                        = 6
 Name                                 = TBS_JXDX
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 7864320
 Useable pages                        = 7864256
 Used pages                           = 4799008
 Free pages                           = 3065248
 High water mark (pages)              = 4799008
 Page size (bytes)                    = 8192
 Extent size (pages)                  = 32
 Prefetch size (pages)                = 64
 Number of containers                 = 2

 Tablespace ID                        = 7
 Name                                 = TBS_NBZZ
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 19660800
 Useable pages                        = 19660640
 Used pages                           = 17532096
 Free pages                           = 2128544
 High water mark (pages)              = 17532096
 Page size (bytes)                    = 8192
 Extent size (pages)                  = 32
 Prefetch size (pages)                = 64
 Number of containers                 = 5

 Tablespace ID                        = 8
 Name                                 = TBS_SYSTMP_32
 Type                                 = System managed space
 Contents                             = System Temporary data
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 9
 Useable pages                        = 9
 Used pages                           = 9
 Free pages                           = Not applicable
 High water mark (pages)              = Not applicable
 Page size (bytes)                    = 32768
 Extent size (pages)                  = 32
 Prefetch size (pages)                = 32
 Number of containers                 = 1

 Tablespace ID                        = 9
 Name                                 = TBS_IDX
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 10485760
 Useable pages                        = 10485696
 Used pages                           = 7860992
 Free pages                           = 2624704
 High water mark (pages)              = 7860992
 Page size (bytes)                    = 8192
 Extent size (pages)                  = 32
 Prefetch size (pages)                = 64
 Number of containers                 = 2

 Tablespace ID                        = 10
 Name                                 = TBS_LSB
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 1310720
 Useable pages                        = 1310704
 Used pages                           = 74496
 Free pages                           = 1236208
 High water mark (pages)              = 77232
 Page size (bytes)                    = 8192
 Extent size (pages)                  = 16
 Prefetch size (pages)                = 32
 Number of containers                 = 1

 Tablespace ID                        = 11
 Name                                 = TBS_SYSTMP_04
 Type                                 = System managed space
 Contents                             = System Temporary data
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 12
 Useable pages                        = 12
 Used pages                           = 12
 Free pages                           = Not applicable
 High water mark (pages)              = Not applicable
 Page size (bytes)                    = 4096
 Extent size (pages)                  = 16
 Prefetch size (pages)                = 16
 Number of containers                 = 1

 Tablespace ID                        = 12
 Name                                 = TBS_SYSTMP_08
 Type                                 = System managed space
 Contents                             = System Temporary data
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 1
 Useable pages                        = 1
 Used pages                           = 1
 Free pages                           = Not applicable
 High water mark (pages)              = Not applicable
 Page size (bytes)                    = 8192
 Extent size (pages)                  = 32
 Prefetch size (pages)                = 32
 Number of containers                 = 1

 Tablespace ID                        = 13
 Name                                 = TBS_USERTMP_08
 Type                                 = System managed space
 Contents                             = User Temporary data
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 135189
 Useable pages                        = 135189
 Used pages                           = 135189
 Free pages                           = Not applicable
 High water mark (pages)              = Not applicable
 Page size (bytes)                    = 8192
 Extent size (pages)                  = 32
 Prefetch size (pages)                = 32
 Number of containers                 = 1

 Tablespace ID                        = 14
 Name                                 = TBS_USERTMP_32
 Type                                 = System managed space
 Contents                             = User Temporary data
 State                                = 0x0000
   Detailed explanation:
     Normal
 Total pages                          = 1
 Useable pages                        = 1
 Used pages                           = 1
 Free pages                           = Not applicable
 High water mark (pages)              = Not applicable
 Page size (bytes)                    = 32768
 Extent size (pages)                  = 32
 Prefetch size (pages)                = 32
 Number of containers                 = 1


 =======================================================================================================

[db2inst1@localhost PAS_BACKUP]$ db2 list tablespaces

           Tablespaces for Current Database

 Tablespace ID                        = 0
 Name                                 = SYSCATSPACE
 Type                                 = Database managed space
 Contents                             = All permanent data. Regular table space.
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 1
 Name                                 = SYSTOOLSTMPSPACE
 Type                                 = System managed space
 Contents                             = User Temporary data
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 2
 Name                                 = TBS_MXZ
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 3
 Name                                 = SYSTOOLSPACE
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 4
 Name                                 = TBS_PAS
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 5
 Name                                 = TBS_JKSJ
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 6
 Name                                 = TBS_JXDX
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 7
 Name                                 = TBS_NBZZ
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 8
 Name                                 = TBS_SYSTMP_32
 Type                                 = System managed space
 Contents                             = System Temporary data
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 9
 Name                                 = TBS_IDX
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 10
 Name                                 = TBS_LSB
 Type                                 = Database managed space
 Contents                             = All permanent data. Large table space.
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 11
 Name                                 = TBS_SYSTMP_04
 Type                                 = System managed space
 Contents                             = System Temporary data
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 12
 Name                                 = TBS_SYSTMP_08
 Type                                 = System managed space
 Contents                             = System Temporary data
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 13
 Name                                 = TBS_USERTMP_08
 Type                                 = System managed space
 Contents                             = User Temporary data
 State                                = 0x0000
   Detailed explanation:
     Normal

 Tablespace ID                        = 14
 Name                                 = TBS_USERTMP_32
 Type                                 = System managed space
 Contents                             = User Temporary data
 State                                = 0x0000
   Detailed explanation:
     Normal




	 =================================================================
[db2inst1@localhost PAS_BACKUP]$ db2 get snapshot for tablespaces on ykspas3

             Tablespace Snapshot

First database connect timestamp           = 2022-08-02 08:09:18.739628
Last reset timestamp                       =
Snapshot timestamp                         = 2022-08-02 14:19:43.798840
Database name                              = YKSPAS3
Database path                              = /home/db2inst1/db2inst1/NODE0000/SQL00002/
Input database alias                       = YKSPAS3
Number of accessed tablespaces             = 15


Tablespace name                            = SYSCATSPACE
  Tablespace ID                            = 0
  Tablespace Type                          = Database managed space
  Tablespace Content Type                  = All permanent data. Regular table space.
  Tablespace Page size (bytes)             = 4096
  Tablespace Extent size (pages)           = 4
  Automatic Prefetch size enabled          = Yes
  Buffer pool ID currently in use          = 1
  Buffer pool ID next startup              = 1
  Using automatic storage                  = Yes
  Auto-resize enabled                      = Yes
  File system caching                      = No
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Tablespace Prefetch size (pages)         = 4
  Total number of pages                    = 401408
  Number of usable pages                   = 401404
  Number of used pages                     = 398888
  Number of pending free pages             = 0
  Number of free pages                     = 2516
  High water mark (pages)                  = 398888
  Initial tablespace size (bytes)          = 33554432
  Current tablespace size (bytes)          = 1644167168
  Maximum tablespace size (bytes)          = NONE
  Increase size (bytes)                    = AUTOMATIC
  Time of last successful resize           =
  Last resize attempt failed               = No
  Rebalancer Mode                          = No Rebalancing
  Storage paths have been dropped          = No
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 1

  Container Name                           = /home/yksbf/db2inst1/NODE0000/YKSPAS3/T0000000/C0000000.CAT
      Container ID                         = 0
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 401408
      Usable Pages in Container            = 401404
      Stripe Set                           = 0
      Container is accessible              = Yes

  Table space map:

   Range  Stripe Stripe  Max         Max  Start  End    Adj.   Containers
   Number Set    Offset  Extent      Page Stripe Stripe
   [   0] [   0]      0  100350    401403      0 100350   0    1 (0)

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = SYSTOOLSTMPSPACE
  Tablespace ID                            = 1
  Tablespace Type                          = System managed space
  Tablespace Content Type                  = User Temporary data
  Tablespace Page size (bytes)             = 4096
  Tablespace Extent size (pages)           = 4
  Automatic Prefetch size enabled          = Yes
  Buffer pool ID currently in use          = 1
  Buffer pool ID next startup              = 1
  Using automatic storage                  = Yes
  File system caching                      = Yes
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Tablespace Prefetch size (pages)         = 4
  Total number of pages                    = 0
  Number of usable pages                   = 0
  Number of used pages                     = 0
  Storage paths have been dropped          = No
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 1

  Container Name                           = /home/yksbf/db2inst1/NODE0000/YKSPAS3/T0000001/C0000000.UTM
      Container ID                         = 0
      Container Type                       = Path
      Total Pages in Container             = 0
      Usable Pages in Container            = 0
      Stripe Set                           = 0
      Container is accessible              = Yes

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = TBS_MXZ
  Tablespace ID                            = 2
  Tablespace Type                          = Database managed space
  Tablespace Content Type                  = All permanent data. Large table space.
  Tablespace Page size (bytes)             = 8192
  Tablespace Extent size (pages)           = 32
  Automatic Prefetch size enabled          = No
  Tablespace Prefetch size (pages)         = 256
  Buffer pool ID currently in use          = 2
  Buffer pool ID next startup              = 2
  Using automatic storage                  = No
  Auto-resize enabled                      = No
  File system caching                      = No
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Total number of pages                    = 58982400
  Number of usable pages                   = 58982112
  Number of used pages                     = 55398464
  Number of pending free pages             = 0
  Number of free pages                     = 3583648
  High water mark (pages)                  = 55398528
  Current tablespace size (bytes)          = 483183820800
  Rebalancer Mode                          = No Rebalancing
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 9

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/TBS_MXZ1
      Container ID                         = 0
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/TBS_MXZ2
      Container ID                         = 1
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/TBS_MXZ3
      Container ID                         = 2
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/TBS_MXZ4
      Container ID                         = 3
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/TBS_MXZ5
      Container ID                         = 4
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/TBS_MXZ6
      Container ID                         = 5
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 11796480
      Usable Pages in Container            = 11796448
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/TBS_MXZ7
      Container ID                         = 6
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 7864320
      Usable Pages in Container            = 7864288
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/TBS_MXZ8
      Container ID                         = 7
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 11796480
      Usable Pages in Container            = 11796448
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/TBS_MXZ9
      Container ID                         = 8
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 7864320
      Usable Pages in Container            = 7864288
      Stripe Set                           = 0
      Container is accessible              = Yes

  Table space map:

   Range  Stripe Stripe  Max         Max  Start  End    Adj.   Containers
   Number Set    Offset  Extent      Page Stripe Stripe
   [   0] [   0]      0  983031  31457023      0 122878   0    8 (0,1,2,3,4,5,6,7)
   [   1] [   0]      0  983034  31457119 122879 122879   0    3 (5,6,7)
   [   2] [   0]      0 1474550  47185631 122880 245758   0    4 (5,6,7,8)
   [   3] [   0]      0 1843190  58982111 245759 368638   0    3 (5,7,8)

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = SYSTOOLSPACE
  Tablespace ID                            = 3
  Tablespace Type                          = Database managed space
  Tablespace Content Type                  = All permanent data. Large table space.
  Tablespace Page size (bytes)             = 4096
  Tablespace Extent size (pages)           = 4
  Automatic Prefetch size enabled          = Yes
  Buffer pool ID currently in use          = 1
  Buffer pool ID next startup              = 1
  Using automatic storage                  = Yes
  Auto-resize enabled                      = Yes
  File system caching                      = No
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Tablespace Prefetch size (pages)         = 4
  Total number of pages                    = 8192
  Number of usable pages                   = 8188
  Number of used pages                     = 620
  Number of pending free pages             = 0
  Number of free pages                     = 7568
  High water mark (pages)                  = 620
  Initial tablespace size (bytes)          = 33554432
  Current tablespace size (bytes)          = 33554432
  Maximum tablespace size (bytes)          = NONE
  Increase size (bytes)                    = AUTOMATIC
  Time of last successful resize           =
  Last resize attempt failed               = No
  Rebalancer Mode                          = No Rebalancing
  Storage paths have been dropped          = No
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 1

  Container Name                           = /home/yksbf/db2inst1/NODE0000/YKSPAS3/T0000003/C0000000.LRG
      Container ID                         = 0
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 8192
      Usable Pages in Container            = 8188
      Stripe Set                           = 0
      Container is accessible              = Yes

  Table space map:

   Range  Stripe Stripe  Max         Max  Start  End    Adj.   Containers
   Number Set    Offset  Extent      Page Stripe Stripe
   [   0] [   0]      0    2046      8187      0   2046   0    1 (0)

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = TBS_PAS
  Tablespace ID                            = 4
  Tablespace Type                          = Database managed space
  Tablespace Content Type                  = All permanent data. Large table space.
  Tablespace Page size (bytes)             = 8192
  Tablespace Extent size (pages)           = 32
  Automatic Prefetch size enabled          = No
  Tablespace Prefetch size (pages)         = 64
  Buffer pool ID currently in use          = 2
  Buffer pool ID next startup              = 2
  Using automatic storage                  = No
  Auto-resize enabled                      = No
  File system caching                      = No
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Total number of pages                    = 7864320
  Number of usable pages                   = 7864256
  Number of used pages                     = 2028416
  Number of pending free pages             = 0
  Number of free pages                     = 5835840
  High water mark (pages)                  = 2028416
  Current tablespace size (bytes)          = 64424509440
  Rebalancer Mode                          = No Rebalancing
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 2

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_DATA/TBS_PAS1
      Container ID                         = 0
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_DATA/TBS_PAS2
      Container ID                         = 1
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Table space map:

   Range  Stripe Stripe  Max         Max  Start  End    Adj.   Containers
   Number Set    Offset  Extent      Page Stripe Stripe
   [   0] [   0]      0  245757   7864255      0 122878   0    2 (0,1)

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = TBS_JKSJ
  Tablespace ID                            = 5
  Tablespace Type                          = Database managed space
  Tablespace Content Type                  = All permanent data. Large table space.
  Tablespace Page size (bytes)             = 8192
  Tablespace Extent size (pages)           = 16
  Automatic Prefetch size enabled          = No
  Tablespace Prefetch size (pages)         = 32
  Buffer pool ID currently in use          = 2
  Buffer pool ID next startup              = 2
  Using automatic storage                  = No
  Auto-resize enabled                      = No
  File system caching                      = No
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Total number of pages                    = 655360
  Number of usable pages                   = 655344
  Number of used pages                     = 508512
  Number of pending free pages             = 0
  Number of free pages                     = 146832
  High water mark (pages)                  = 508912
  Current tablespace size (bytes)          = 5368709120
  Rebalancer Mode                          = No Rebalancing
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 1

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_DATA/TBS_JKSJ1
      Container ID                         = 0
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 655360
      Usable Pages in Container            = 655344
      Stripe Set                           = 0
      Container is accessible              = Yes

  Table space map:

   Range  Stripe Stripe  Max         Max  Start  End    Adj.   Containers
   Number Set    Offset  Extent      Page Stripe Stripe
   [   0] [   0]      0   40958    655343      0  40958   0    1 (0)

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = TBS_JXDX
  Tablespace ID                            = 6
  Tablespace Type                          = Database managed space
  Tablespace Content Type                  = All permanent data. Large table space.
  Tablespace Page size (bytes)             = 8192
  Tablespace Extent size (pages)           = 32
  Automatic Prefetch size enabled          = No
  Tablespace Prefetch size (pages)         = 64
  Buffer pool ID currently in use          = 2
  Buffer pool ID next startup              = 2
  Using automatic storage                  = No
  Auto-resize enabled                      = No
  File system caching                      = No
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Total number of pages                    = 7864320
  Number of usable pages                   = 7864256
  Number of used pages                     = 4821632
  Number of pending free pages             = 0
  Number of free pages                     = 3042624
  High water mark (pages)                  = 4821632
  Current tablespace size (bytes)          = 64424509440
  Rebalancer Mode                          = No Rebalancing
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 2

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_DATA/TBS_JXDX1
      Container ID                         = 0
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_DATA/TBS_JXDX2
      Container ID                         = 1
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Table space map:

   Range  Stripe Stripe  Max         Max  Start  End    Adj.   Containers
   Number Set    Offset  Extent      Page Stripe Stripe
   [   0] [   0]      0  245757   7864255      0 122878   0    2 (0,1)

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = TBS_NBZZ
  Tablespace ID                            = 7
  Tablespace Type                          = Database managed space
  Tablespace Content Type                  = All permanent data. Large table space.
  Tablespace Page size (bytes)             = 8192
  Tablespace Extent size (pages)           = 32
  Automatic Prefetch size enabled          = No
  Tablespace Prefetch size (pages)         = 64
  Buffer pool ID currently in use          = 2
  Buffer pool ID next startup              = 2
  Using automatic storage                  = No
  Auto-resize enabled                      = No
  File system caching                      = No
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Total number of pages                    = 19660800
  Number of usable pages                   = 19660640
  Number of used pages                     = 17548480
  Number of pending free pages             = 0
  Number of free pages                     = 2112160
  High water mark (pages)                  = 17548480
  Current tablespace size (bytes)          = 161061273600
  Rebalancer Mode                          = No Rebalancing
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 5

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_DATA/TBS_NBZZ1
      Container ID                         = 0
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_DATA/TBS_NBZZ2
      Container ID                         = 1
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_DATA/TBS_NBZZ3
      Container ID                         = 2
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_DATA/TBS_NBZZ4
      Container ID                         = 3
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_DATA/TBS_NBZZ5
      Container ID                         = 4
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Table space map:

   Range  Stripe Stripe  Max         Max  Start  End    Adj.   Containers
   Number Set    Offset  Extent      Page Stripe Stripe
   [   0] [   0]      0  614394  19660639      0 122878   0    5 (0,1,2,3,4)

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = TBS_SYSTMP_32
  Tablespace ID                            = 8
  Tablespace Type                          = System managed space
  Tablespace Content Type                  = System Temporary data
  Tablespace Page size (bytes)             = 32768
  Tablespace Extent size (pages)           = 32
  Automatic Prefetch size enabled          = No
  Tablespace Prefetch size (pages)         = 32
  Buffer pool ID currently in use          = 3
  Buffer pool ID next startup              = 3
  Using automatic storage                  = No
  File system caching                      = Yes
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Total number of pages                    = 0
  Number of usable pages                   = 0
  Number of used pages                     = 0
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 1

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_TMP/TBS_SYSTMP_32
      Container ID                         = 0
      Container Type                       = Path
      Total Pages in Container             = 0
      Usable Pages in Container            = 0
      Stripe Set                           = 0
      Container is accessible              = Yes

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = TBS_IDX
  Tablespace ID                            = 9
  Tablespace Type                          = Database managed space
  Tablespace Content Type                  = All permanent data. Large table space.
  Tablespace Page size (bytes)             = 8192
  Tablespace Extent size (pages)           = 32
  Automatic Prefetch size enabled          = No
  Tablespace Prefetch size (pages)         = 64
  Buffer pool ID currently in use          = 2
  Buffer pool ID next startup              = 2
  Using automatic storage                  = No
  Auto-resize enabled                      = No
  File system caching                      = No
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Total number of pages                    = 10485760
  Number of usable pages                   = 10485696
  Number of used pages                     = 7891584
  Number of pending free pages             = 0
  Number of free pages                     = 2594112
  High water mark (pages)                  = 7891584
  Current tablespace size (bytes)          = 85899345920
  Rebalancer Mode                          = No Rebalancing
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 2

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_IDX/TBS_IDX1
      Container ID                         = 0
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 3932160
      Usable Pages in Container            = 3932128
      Stripe Set                           = 0
      Container is accessible              = Yes

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_IDX/TBS_IDX2
      Container ID                         = 1
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 6553600
      Usable Pages in Container            = 6553568
      Stripe Set                           = 0
      Container is accessible              = Yes

  Table space map:

   Range  Stripe Stripe  Max         Max  Start  End    Adj.   Containers
   Number Set    Offset  Extent      Page Stripe Stripe
   [   0] [   0]      0  245757   7864255      0 122878   0    2 (0,1)
   [   1] [   0]      0  327677  10485695 122879 204798   0    1 (1)

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = TBS_LSB
  Tablespace ID                            = 10
  Tablespace Type                          = Database managed space
  Tablespace Content Type                  = All permanent data. Large table space.
  Tablespace Page size (bytes)             = 8192
  Tablespace Extent size (pages)           = 16
  Automatic Prefetch size enabled          = No
  Tablespace Prefetch size (pages)         = 32
  Buffer pool ID currently in use          = 2
  Buffer pool ID next startup              = 2
  Using automatic storage                  = No
  Auto-resize enabled                      = No
  File system caching                      = No
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Total number of pages                    = 1310720
  Number of usable pages                   = 1310704
  Number of used pages                     = 74560
  Number of pending free pages             = 0
  Number of free pages                     = 1236144
  High water mark (pages)                  = 77232
  Current tablespace size (bytes)          = 10737418240
  Rebalancer Mode                          = No Rebalancing
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 1

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_DATA/TBS_LSB1
      Container ID                         = 0
      Container Type                       = File (extent sized tag)
      Total Pages in Container             = 1310720
      Usable Pages in Container            = 1310704
      Stripe Set                           = 0
      Container is accessible              = Yes

  Table space map:

   Range  Stripe Stripe  Max         Max  Start  End    Adj.   Containers
   Number Set    Offset  Extent      Page Stripe Stripe
   [   0] [   0]      0   81918   1310703      0  81918   0    1 (0)

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = TBS_SYSTMP_04
  Tablespace ID                            = 11
  Tablespace Type                          = System managed space
  Tablespace Content Type                  = System Temporary data
  Tablespace Page size (bytes)             = 4096
  Tablespace Extent size (pages)           = 16
  Automatic Prefetch size enabled          = No
  Tablespace Prefetch size (pages)         = 16
  Buffer pool ID currently in use          = 1
  Buffer pool ID next startup              = 1
  Using automatic storage                  = No
  File system caching                      = Yes
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Total number of pages                    = 0
  Number of usable pages                   = 0
  Number of used pages                     = 0
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 1

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_TMP/TBS_SYSTMP_04
      Container ID                         = 0
      Container Type                       = Path
      Total Pages in Container             = 0
      Usable Pages in Container            = 0
      Stripe Set                           = 0
      Container is accessible              = Yes

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = TBS_SYSTMP_08
  Tablespace ID                            = 12
  Tablespace Type                          = System managed space
  Tablespace Content Type                  = System Temporary data
  Tablespace Page size (bytes)             = 8192
  Tablespace Extent size (pages)           = 32
  Automatic Prefetch size enabled          = No
  Tablespace Prefetch size (pages)         = 32
  Buffer pool ID currently in use          = 2
  Buffer pool ID next startup              = 2
  Using automatic storage                  = No
  File system caching                      = Yes
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Total number of pages                    = 0
  Number of usable pages                   = 0
  Number of used pages                     = 0
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 1

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_TMP/TBS_SYSTMP_08
      Container ID                         = 0
      Container Type                       = Path
      Total Pages in Container             = 0
      Usable Pages in Container            = 0
      Stripe Set                           = 0
      Container is accessible              = Yes

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = TBS_USERTMP_08
  Tablespace ID                            = 13
  Tablespace Type                          = System managed space
  Tablespace Content Type                  = User Temporary data
  Tablespace Page size (bytes)             = 8192
  Tablespace Extent size (pages)           = 32
  Automatic Prefetch size enabled          = No
  Tablespace Prefetch size (pages)         = 32
  Buffer pool ID currently in use          = 2
  Buffer pool ID next startup              = 2
  Using automatic storage                  = No
  File system caching                      = Yes
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Total number of pages                    = 0
  Number of usable pages                   = 0
  Number of used pages                     = 0
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 1

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_TMP/TBS_USERTMP_08
      Container ID                         = 0
      Container Type                       = Path
      Total Pages in Container             = 0
      Usable Pages in Container            = 0
      Stripe Set                           = 0
      Container is accessible              = Yes

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected


Tablespace name                            = TBS_USERTMP_32
  Tablespace ID                            = 14
  Tablespace Type                          = System managed space
  Tablespace Content Type                  = User Temporary data
  Tablespace Page size (bytes)             = 32768
  Tablespace Extent size (pages)           = 32
  Automatic Prefetch size enabled          = No
  Tablespace Prefetch size (pages)         = 32
  Buffer pool ID currently in use          = 3
  Buffer pool ID next startup              = 3
  Using automatic storage                  = No
  File system caching                      = Yes
  Tablespace State                         = 0x'00000000'
   Detailed explanation:
     Normal
  Total number of pages                    = 0
  Number of usable pages                   = 0
  Number of used pages                     = 0
  Minimum Recovery Time                    =
  Number of quiescers                      = 0
  Number of containers                     = 1

  Container Name                           = /home/db2inst1/YKSPAS3/PAS_DATA/PAS_SPACE_TMP/TBS_USERTMP_32
      Container ID                         = 0
      Container Type                       = Path
      Total Pages in Container             = 0
      Usable Pages in Container            = 0
      Stripe Set                           = 0
      Container is accessible              = Yes

  Buffer pool data logical reads           = Not Collected
  Buffer pool data physical reads          = Not Collected
  Buffer pool temporary data logical reads   = Not Collected
  Buffer pool temporary data physical reads  = Not Collected
  Asynchronous pool data page reads        = Not Collected
  Buffer pool data writes                  = Not Collected
  Asynchronous pool data page writes       = Not Collected
  Buffer pool index logical reads          = Not Collected
  Buffer pool index physical reads         = Not Collected
  Buffer pool temporary index logical reads  = Not Collected
  Buffer pool temporary index physical reads = Not Collected
  Asynchronous pool index page reads       = Not Collected
  Buffer pool index writes                 = Not Collected
  Asynchronous pool index page writes      = Not Collected
  Total buffer pool read time (millisec)   = Not Collected
  Total buffer pool write time (millisec)  = Not Collected
  Total elapsed asynchronous read time     = Not Collected
  Total elapsed asynchronous write time    = Not Collected
  Asynchronous data read requests          = Not Collected
  Asynchronous index read requests         = Not Collected
  No victim buffers available              = Not Collected
  Direct reads                             = Not Collected
  Direct writes                            = Not Collected
  Direct read requests                     = Not Collected
  Direct write requests                    = Not Collected
  Direct reads elapsed time (ms)           = Not Collected
  Direct write elapsed time (ms)           = Not Collected
  Number of files closed                   = Not Collected
```
