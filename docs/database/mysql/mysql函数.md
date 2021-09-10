### 1.存储

```mysql
DROP PROCEDURE  IF EXISTS  updateHai;
delimiter $$
CREATE PROCEDURE `updateHai`()
BEGIN
-- 
update secure_model_work_order set deal_flag_tboss=8 where deal_flag_tboss=0 and  insert_time>= DATE_ADD(date(now()),INTERVAL -3 DAY)
and (cust_name like '海%' or cust_name like '中%');
-- 
UPDATE secure_model_work_order t1 JOIN secure_yezhi_phone_user t2
ON t1.msisdn = t2.msisdn SET deal_flag_tboss=8 where t2.develop_staff_id='HNHKC45D' and t1.deal_flag_tboss=0;
--  
update secure_out_sen_recognition_result set deal_flag=3 where deal_flag=0 and open_date<=DATE_ADD(date(now()),INTERVAL -240 DAY) and insert_time>=DATE_ADD(date(now()),INTERVAL -1 DAY);
-- 
update secure_model_call_increase_result set deal_flag=3 where deal_flag=0 and open_date<=DATE_ADD(date(now()),INTERVAL -360 DAY) and insert_time>=DATE_ADD(date(now()),INTERVAL -1 DAY);
-- 
update secure_model_call_out_result_v2 set deal_flag=3 where deal_flag=0 and open_date<=DATE_ADD(date(now()),INTERVAL -360 DAY) and insert_time>=DATE_ADD(date(now()),INTERVAL -1 DAY);
commit;
END$$
delimiter ;
```

### 定时

```mysql
DROP EVENT IF EXISTS updateHaiEvent;
CREATE EVENT updateHaiEvent
ON SCHEDULE EVERY 1 MINUTE
DO
CALL updateHai;

show variables like 'event_scheduler';
```

### 定时器

```mysql
show variables like 'event_scheduler';
set global event_scheduler = on;
-- 关闭事件
ALTER EVENT event_name DISABLE;
-- 开启事件
ALTER EVENT event_name ENABLE;
-- 删除事件
DROP EVENT [IF EXISTS ] event_name;
```



### 查看语句

```
show create procedure updateHai;

show create EVENT updateHaiEvent;

```

### 查看所有定时任务

```
SHOW EVENTS;


run_same_develop_depart_trace.sh

run_old_user_model.sh
```

### 查看

```mysql
-- 查看调度器线程
show processlist; 
select * from information_schema.events;
select * from mysql.event;
```

### 定时器博客

[定时器创建](https://www.cnblogs.com/fanbi/p/9361204.html)

[定时器创建-学习](https://www.cnblogs.com/fanbi/p/9361204.html)

[存储函数使用-学习](https://blog.csdn.net/weixin_41381863/article/details/89311640)