### 刷题

1.一个6亿的表a，一个3亿的表b，通过外键tid关联，你如何最快的查询出满足条件的第50000到第50200中的这200条数据记录。

```mysql
-- 情况一：如果A表TID是自增长,并且是连续的,B表的ID为索引
select * from a,b where a.tid = b.id and a.tid>50000 limit 200; 
-- 情况二：如果A表的TID不是连续的,那么就需要使用覆盖索引.TID要么是主键,要么是辅助索引,B表ID也需要有索引。
select * from b , (select tid from a limit 50000,200) a where b.id = a .tid; 
```

2.Student(S,Sname,Sage,Ssex) 学生表 Course(C,Cname,T) 课程表 SC(S,C,score) 成绩表 Teacher(T,Tname) 教师表 查询没学过“叶平”老师课的同学的学号、姓名

```mysql
-- not in 使用
SELECT
	Student.S,
	Student.Sname
FROM
	Student
WHERE
	S NOT IN (
		SELECT DISTINCT
			(SC.S)
		FROM
			SC,
			Course,
			Teacher
		WHERE
			SC.C = Course.C
		AND Teacher.T = Course.T
		AND Teacher.Tname = ’叶平’
	);
```

3.随机取出小于10条数据

```mysql
SELECT
	*
FROM
	users
WHERE
	id >= (
		(SELECT MAX(id) FROM users) - (SELECT MIN(id) FROM users)
	) * RAND() + (SELECT MIN(id) FROM users)
LIMIT 10
-- 以下写法肯定不得分，效率不高
SELECT * FROM users order by rand() LIMIT 10
```

4.列出选课超过5门的学生

```mysql
select class from courses group by class having count(DISTINCT student) >= 5
```

5.查找昨天比今天温度高的日期

```mysql
-- 要点 DATEDIFF()
select w1.Id as Id from Weather w1,Weather w2
where datediff(w1.RecordDate,w2.RecordDate)=1 -- 表示当前日期减去昨天日期
and w1.Temperature > w2.Temperature
```

#### 学生-课程-教师-成绩

```mysql
问题及描述：
–1.学生表
Student(SID,Sname,Sage,Ssex) --SID 学生编号,Sname 学生姓名,Sage 出生年月,Ssex 学生性别
–2.课程表
Course(CID,Cname,TID) --CID --课程编号,Cname 课程名称,TID 教师编号
–3.教师表
Teacher(TID,Tname) --TID 教师编号,Tname 教师姓名
–4.成绩表
SC(SID,CID,score) --SID 学生编号,CID 课程编号,score 分数
*/
–创建测试数据
create table Student(SID varchar(10),Sname nvarchar(10),Sage datetime,Ssex nvarchar(10));
insert into Student values('01' , '赵雷' , '1990-01-01' , '男');
insert into Student values('02' , '钱电' , '1990-12-21' , '男');
insert into Student values('03' , '孙风' , '1990-05-20' , '男');
insert into Student values('04' , '李云' , '1990-08-06' , '男');
insert into Student values('05' , '周梅' , '1991-12-01' , '女');
insert into Student values('06' , '吴兰' , '1992-03-01' , '女');
insert into Student values('07' , '郑竹' , '1989-07-01' , '女');
insert into Student values('08' , '王菊' , '1990-01-20' , '女');
create table Course(CID varchar(10),Cname nvarchar(10),TID varchar(10));
insert into Course values('01' , '语文' , '02');
insert into Course values('02' , '数学' , '01');
insert into Course values('03' , '英语' , '03');
create table Teacher(TID varchar(10),Tname nvarchar(10));
insert into Teacher values('01' , '张三');
insert into Teacher values('02' , '李四');
insert into Teacher values('03' , '王五');
create table SC(SID varchar(10),CID varchar(10),score decimal(18,1));
insert into SC values('01' , '01' , 80);
insert into SC values('01' , '02' , 90);
insert into SC values('01' , '03' , 99);
insert into SC values('02' , '01' , 70);
insert into SC values('02' , '02' , 60);
insert into SC values('02' , '03' , 80);
insert into SC values('03' , '01' , 80);
insert into SC values('03' , '02' , 80);
insert into SC values('03' , '03' , 80);
insert into SC values('04' , '01' , 50);
insert into SC values('04' , '02' , 30);
insert into SC values('04' , '03' , 20);
insert into SC values('05' , '01' , 76);
insert into SC values('05' , '02' , 87);
insert into SC values('06' , '01' , 31);
insert into SC values('06' , '03' , 34);
insert into SC values('07' , '02' , 89);
insert into SC values('07' , '03' , 98);
-- 
```

