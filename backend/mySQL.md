## ubuntu16.04安裝MySQL 5.7
```bash
sudo apt-get install -y mysql-server
# 要記得設定root密碼
# 啟動服務和開機後自動啟動服務
sudo systemctl start mysql
sudo systemctl enable mysql
# 查看服務狀態是否成功
systemctl status mysql.service
```

## 基本操作指令
操作功能 | SQL 語法 | 說明
|--- |---|---|
建立資料庫 | create database 資料庫名稱; 	
列出所有資料庫 | show databases;	
刪除資料庫 | drop database 資料庫名稱;	
使用資料庫 | use 資料庫名稱;	

建立資料表 | 
    ```sql 
    create table 資料表名稱(
    sn integer auto_increment primary key,name char(20),
    messages char(50));|常用資料庫資料型態
    1. INT (整數)
    2. CHAR (1~255字元字串)
    3. VARCHAR (不超過255字元不定長度字串)
    4. TEXT (不定長度字串最多65535字元) 
    ```        
列出資料表欄位資訊 | describe 資料表名稱;	
修改資料表欄位 | alter table 資料表名稱<br>change column 原來欄位名稱<br>新欄位名稱資料型態;	
新增資料表欄位 | alter table 資料表名稱 add column 欄位名稱 資料型態;	
刪除資料表欄位 | alter table 資料表名稱 drop column 欄位名稱;	
刪除資料表 | drop table 資料表名稱;	
清空資料表 | truncate table 資料表名稱; | 只清除資料並保留結構、欄位、索引 …
插入欄位資料 | insert into 資料表名稱(欄位1,欄位2,欄位3,欄位4, ...... 欄位N)
              values('值1','值2','值3','值4', ...... '值N');	
更新修改欄位資料 | update 資料表名稱 set 欄位1='值1',欄位2='值2',欄位3='值3',... 欄位N='值N'
                 where 條件式 (例如 sn='5' 或 name='塔司尼' );	
查詢單一欄位資料 | select 欄位名 from 資料表名稱;	
查詢多個欄位資料 | select 欄位名, 欄位名, 欄位名 from 資料表名稱;	
查詢欄位資料的唯一值 | select distinct 欄位名 from 資料表名稱; | 重複值只列一次
查詢所有欄位資料 | select * from 資料表名稱;	
條件式查詢 | select * from 資料表名稱 where 條件式 (例如 sn='5'); | （=, <, >,!=）
條件式查詢 and | select * from 資料表名稱 where 條件式1 and 條件式2;	
條件式查詢 or | select * from 資料表名稱 where 條件式1 or 條件式2;	
查詢某一範圍 between | select * from 資料表名稱 where 欄位名 between 值1 and 值2; | 值為數字
查詢空值欄位的資料 | select * from 資料表名稱 where 欄位名 is null | not null;
查詢特定筆數資料 | select * from 資料表名稱 limit 8, 10;	第9筆開始選取10筆
查詢結果遞增排序 | select * from 資料表名稱 order by 欄位名;	
查詢結果遞減排序 | select * from 資料表名稱 order by 欄位名 desc ;	
查詢比對字串列出單一欄位 | select 欄位名 from 資料表名稱 where 欄位名 like '%字串%';	
查詢比對字串列出所有欄位 | select * from 資料表名稱 where 欄位名 like '%字串%';	
刪除條件值資料	delete from 資料表名稱 where 條件式 (例如 sn='5' 或 id='91001' );	
刪除條件值資料	delete from 資料表名稱 where 條件式1 
and 條件式2;	
刪除條件值資料	delete from 資料表名稱 where 條件式1 or 條件式2;	
比對刪除條件值資料	delete from 資料表名稱 where 欄位名 like '%字串%';	

## root權限管理使用者和資料庫
mysql -h (ip address，localhost就不需要了) -u (username) -p (password)#以root權限登入

create database (dbname);

create user 'user'@'localhost' identified by 'passwword';

grant all privileges on (dbname).* to 'user'@'localhost';

flush privileges;

quit

mysql -u user -p #確認可以使用(dbname)的資料庫

若要讓所有ip都可連進某user的mysql
```
use mysql
select user,host from user;
update user set host='%' where user='user';
```

