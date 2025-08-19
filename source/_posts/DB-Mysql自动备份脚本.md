---
title: Mysql自动备份脚本
index_img: /img_index/index/20201216-001.png
categories:
  - 数据库
tags:
  - Mysql
top: false
desc: Mysqldump自动备份脚本，基于shell脚本，加入crontab定时任务
keywords: 'Mysql, Mysqldump, shell, crontab'
abbrlink: 8234
date: 2020-12-16 10:02:23
updated: 2020-12-16 10:02:23
---




{% note primary %}
Mysqldump自动备份脚本，基于shell脚本，加入crontab定时任务。
{% endnote %}

{% label info@Mysql %} {% label primary@Mysqldump %} {% label success@备份 %}

<!--more-->
<hr />

#### 定时任务

crontab -e

```
30 02 * * * bash /home/mingliang.gao/crontab/db_backup/db_backup_task.sh > /dev/null 2>&1
```

#### 备份脚本

> db parameters

db相关的配置，***可更改***。
- db_user：账号
- db_passwd：密码
- db_host：数据库服务器IP
- db_port：数据库端口
- db_backup_dir：备份数据库存放路径
- db_names：为要备份的数据库名称，如果不设置指定的备份数据库，直接注释，在下面会把information_schema、mysql、performance_schem系统数据库除外的都进行备份。

>dir parameters

备份数据的存放位置，以及记录备份历史，**不可更改**。
- cur_date：执行的本次日期。
- backup_history_file：执行记录文件。
- today_backup_dir：备份文件夹。

> other parameters

设置备份数据保留天数。
- backup_keep_days：备份数据保留天数。
任务脚本最下面有个find去处理保留的备份文件。


> 内容

```
#!/bin/bash

# ========================================================================================
# version: v.1.0
# author: mingliang.gao
# time: 2020/12/15 23:56:28
# mail: mingliang.gao@163.com
# summary: This script daily use to backup database
# crontab: 30 02 * * * bash /home/mingliang.gao/crontab/db_backup/db_backup_task.sh > /dev/null 2>&1
#
#           Enjoy the good lift everyday！！!
# ========================================================================================

echo "***************************************************"
echo "-------------Server Host: 121.4.56.169-------------"
echo "------------------DB start backup------------------"

# db parameters
db_user="mingliang.gao"
db_passwd="910809ecb44c92db12ad5fa369375d00"
db_host="127.0.0.1"
db_port=3306
db_backup_dir="/home/mingliang.gao/db_backup/"
db_names=""
db_names=(elt enterprise ht)   # config dbs

# base command
rm="$(which rm)"
find="$(which find)"
mkdir="$(which mkdir)"
touch="$(which touch)"

# dir parameters
cur_date="$(date +"%Y%m%d")"
backup_history_file="${db_backup_dir}backup_history.log"
today_backup_dir="${db_backup_dir}${cur_date}"
if [ ! -d "$today_backup_dir" ];then
    $mkdir -p "$today_backup_dir"
fi
if [ ! -e "$backup_history_file" ];then
    $touch "$backup_history_file"
fi

# other parameters
backup_keep_days=2
start_time="$(date +"%Y-%m-%d %H:%M:%S")"

# mysql command
mysql="$(which mysql)"
if [ $? -eq 0 ];
  then
    echo ""Mysql command: "$mysql"
  else
    echo "Not found mysql, exit."
    exit 1
fi
mysqldump="$(which mysqldump)"
if [ $? -eq 0 ];
  then
    echo ""Mysqldump command: "$mysqldump"
  else
    echo "Not found mysqldump, exit."
    exit 1
fi

if [ -z "$db_names" ]
  then
    db_names=$(mysql -h$db_host -P$db_port -u$db_user -p$db_passwd -e 'show databases'| grep -vE 'Database|information_schema|mysql|performance_schema')
fi

for db in ${db_names[*]}; do
  echo ""=============: "$db"
  $mysqldump -h$db_host -P$db_port -u$db_user -p$db_passwd $db > "${today_backup_dir}/${db}.sql"
done

# delete +3 db backup dir
$find $db_backup_dir -type d -mtime +$backup_keep_days -exec $rm -rf {} \;

# record history
end_time="$(date +"%Y-%m-%d %H:%M:%S")"
start_seconds=$(date --date="$start_time" +%s);
end_seconds=$(date --date="$end_time" +%s);
echo "${cur_date}: "$((end_seconds-start_seconds)) >> "$backup_history_file"

echo "${cur_date}本次运行时间： "$((end_seconds-start_seconds))"s"
echo "---------------Author: mingliang.gao---------------"
echo "-------------------DB end backup-------------------"
echo "***************************************************"
exit 0
```

{% raw %}
<div class="post_cus_note">Enjoy the good life everyday！！!</div>
{% endraw %}
