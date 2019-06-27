# MySQL
* As a reminder, please make sure that you quit the **mysql** and then start **mysql80** using `net start mysql80`.
---

- :) 设有如下关系表S：
S(NO, NAME, SEX, AGE, CLASS)，主关键字是NO。
其中NO为学号，char(2)，学号不能为空，值是唯一的；NAME为姓名，char(10)，姓名的取值也唯一；SEX为性别，char(2)；AGE为年龄，int； CLASS为班号，char(5)。写出实现下列功能的SQL语句。
- 1.创建数据库test，在test中创建表S；
```SQL
 create table s(
       no char(2) not null,
       name char(10) not null,
       sex char(2),
       age int,
       class char(5),
       PRIMARY KEY (no),
       UNIQUE (no, name));
```

- 2.插入一个记录（25，“李明”，“男”，21，“95031”）； 
```SQL
 insert into s values(25,"李明","男",21,"95031");
```
- 3.再插入一个记录（10，“王丽”，“女”，20，“95101”）；
```SQL
 insert into s values(10,"王丽","女",20,"95101");
```
- 4.插入“95031”班学号为30，姓名为“郑和”的学生记录；
```SQL
insert into s (no, name) values(30,"郑和");
```
- 5.对表S，按学号升序建唯一索引（索引名为sno）； 对表S，按年龄降序建索引（索引名为sage）；
```SQL
create index sno on s (no asc);
create index sage on s (age desc);
```
- 6.向S表添加“入学时间（comedate）”列，其数据类型为日期型（datetime）；
```SQL
 alter table s add colunmn comedate datetime;
```
- 7.删除S表的sage索引；
```SQL
drop index sage on s;
```
- 8.将年龄的数据类型改为smallint；
```SQL
alter table s modify column age smallint;
```
- 9.删除学生姓名必须取唯一值的约束；（注意：MySQL与标准SQL语言有区别）
```SQL
 drop index no on s;
```
- 10.删除S表
```SQL
 drop table s;
```

---
### 创建数据库db_SPJ
- 1.确保当前操作的数据库为db_SPJ
- 2.在db_SPJ中创建以下四个关系（表）
    - 供应商表S(SNO,SNAME,STATUS,CITY)

    ```SQL
    create table S(
       SNO varchar(5) PRIMARY KEY,
       SNAME varchar(10) not null,
       STATUS int,
       CITY varchar(10) not null);
    ```  

    - 零件表P(PNO,PNAME,COLOR,WEIGHT) 

    ```SQL
    create table P(
       PNO varchar(5) PRIMARY KEY,
       PNAME varchar(10) not null,
       COLOR varchar(5) DEFAULT '红',
       WEIGHT int,
       UNIQUE (PNO));
    ```

    - 工程项目表J(JNO,JNAME,CITY)  

    ```SQL
    create table J(
       JNO varchar(5) PRIMARY KEY,
       JNAME varchar(10) not null,
       CITY varchar(10));
    ```

    - 供应情况表SPJ(SNO,PNO,JNO,QTY)	

    ```SQL
    create table SPJ(
       SNO varchar(5),
       PNO varchar(5),
       JNO varchar(5),
       QTY int not null,
       PRIMARY KEY (SNO,PNO,JNO),
       FOREIGN KEY SNO REFERENCES S(SNO),
       FOREIGN KEY PNO REFERENCES P(PNO),
       FOREIGN KEY JNO REFERENCES J(JNO));
    ```

    - [x] 每一张表都必须有主键。
    - [x] 需要使用外键的表必须使用外键。
    - [x] 根据需要适当采用唯一值、检查、非空和默认值约束。要求这四种约束在S、P和J表这三张表中至少使用一次。
    - [x] 根据主键、外键、唯一值、检查、非空和默认值六种约束的特性，设计适当的方案对S、P和J表中的这六种约束进行检验。方案自定。一种约束进行检验一次即可。

- 3.插入以下记录

```SQL
INSERT INTO S VALUES ('S1','精益',20,'天津'),
('S2','盛锡',10,'北京'),('S3','东方红',30,'北京'),
('S4','丰泰',20,' 天津'),('S5','为民',30,'上海');
```

```SQL
INSERT INTO P VALUES ('P1','螺母','红',12),('P2','螺栓','绿',17),
('P3','螺丝刀','蓝',14),('P4','螺丝刀','红',14),
('P5','凸轮','蓝',40),('P6','齿轮','红',30);
```

```SQL
 INSERT INTO J VALUES ('J1','三建','北京'),('J2','一汽','长春'),
 ('J3','弹簧厂','天津'),('J4','造船厂','天津'),
 ('J5','机车厂','唐山'),('J6','无线电厂','常州'),
 ('J7','半导体厂','南京');
```

```SQL
INSERT INTO SPJ VALUES ('S1','P1','J1',200),('S1','P1','J3',100),
('S1','P1','J4',700),('S1','P2','J2',100);

INSERT INTO SPJ VALUES ('S2','P3','J1',400),('S2','P3','J2',200),
('S2','P3','J4',500),('S2','P3','J5',400);

 INSERT INTO SPJ VALUES ('S2','P5','J1',400),('S2','P5','J2',100),
 ('S3','P1','J1',200),('S3','P3','J1',200);

 INSERT INTO SPJ VALUES ('S4','P5','J1',100),('S4','P6','J1',100),
 ('S4','P6','J4',200),('S5','P2','J4',100);

 INSERT INTO SPJ VALUES ('S5','P3','J1',200),('S5','P6','J2',200),
 ('S5','P6','J4',500);
```
---

### 查询数据库db_SPJ
表格名|符号|属性
:---:|:---:|:---:
供应商表|S|SNO,SNAME,STATUS,CITY
零件表|P|PNO,PNAME,COLOR,WEIGHT
工程项目表|J|JNO,JNAME,CITY
供应情况表|SPJ|SNO,PNO,JNO,QTY

#### 1--10
- 找出所有供应商的姓名和所在城市。
```SQL
select sname,city from s;
```
- 找出所有零件的名称、颜色和重量。
```SQL
select pname,color,weight from p;
```
- 找出使用了供应商S1所供应的零件的工程号码。
```SQL
select jno from spj where sno='S1';
```
- 找出工程J2使用的各种零件的名称和数量。
```SQL
select p.pname,spj.qty from p,spj where spj.jno='J2' and p.pno=spj.pno;
```
- 找出上海供应商供应的所有零件的零件号码。
```SQL
select distinct spj.pno from s,spj where s.city='上海' and s.sno=spj.sno;
```
- 找出使用了上海供应商供应的零件的工程名称。
```SQL
select distinct j.jname from s,j,spj 
where s.city='上海' and s.sno=spj.sno and spj.jno=j.jno;
```
- 找出供应工程J1零件的供应商号SNO。
```SQL
select distinct sno from spj where jno='J1';
```
- 找出供应工程J1零件P1的供应商号SNO。
```SQL
select distinct sno from spj where jno='J1' and pno='P1';
```
- 找出供应工程J1红色零件的供应商号SNO。
```SQL
select distinct sno from spj natural join p 
where color='红' and jno='J1';
```
- 找出没有使用天津供应商生产的红色零件的工程号JNO。
```SQL
select spj.jno from s,p,spj 
where not s.city='天津' and p.color='红' and
 s.sno=spj.sno and p.pno=spj.pno;
```

#### 11--20

- 求所有有关project 的信息。
```SQL
select * from j;
```
- 求在北京的所有project 的信息。
```SQL
select * from j where city='北京';
```
- 求为project（工程）J1 提供part（零件）的supplier（供应商）的号码。
```SQL
select distinct sno from spj where jno='J1';
```
- 求数量在300 到750 之间的发货。
```SQL
select * from spj where qty>300 and qty<750;
```
- ==求所有的零件颜色 / 城市对。注意：这里及以后所说的“所有”特指在数据库中。==
```SQL
select p.color,s.city from p,s;
```
- ==求所有的supplier-number / part-number / project-number 对。其中所指的供应商和工程在同一个城市。==
```SQL
select s.sno,p.pno,j.jno from s,p,j,spj 
where s.city=j.city and s.sno=spj.sno and
 p.pno=spj.pno and j.jno=spj.jno;
```
- 求所有的supplier-number / part-number / project-number 对。其中所指的供应商和工程不在同一个城市。
```SQL
select s.sno,p.pno,j.jno from s,p,j,spj 
where not s.city=j.city and s.sno=spj.sno and
 p.pno=spj.pno and j.jno=spj.jno;
```
- 求由北京供应商提供的零件的信息。
```SQL
select distinct p.* from s,p,spj 
where s.city='北京' and s.sno=spj.sno and spj.pno=p.pno;
```
- 求由北京供应商为北京工程供应的零件号。
```SQL
select distinct spj.pno from s,j,spj 
where s.city='北京' and j.city='北京' and
 s.sno=spj.sno and j.jno=spj.jno;
```
- 求满足下面要求的城市对，要求在第一个城市的供应商为第二个城市的工程供应零件。
```SQL
select distinct s.city,j.city from s,j,spj
 where s.sno=spj.sno and j.jno=spj.jno;
```


#### 21--30
- 求供应商为工程供应的零件的号码，要求供应商和工程在同一城市。
```SQL
select distinct spj.pno from s,j,spj 
where s.city=j.city and s.sno=spj.sno and j.jno=spj.jno;
```
- 求至少被一个不在同一城市的供应商供应零件的工程号。
```SQL
select distinct spj.jno from s,j,spj where not s.city=j.city and s.sno=spj.sno and j.jno=spj.jno;
```
- ==求由同一个供应商供应的零件号的对。==
```SQL

```
- 求所有由供应商S1 供应的工程号。
```SQL
select distinct jno from spj where sno='S1';
```
- 求供应商S1 供应的零件P1 的总量。
```SQL
select sum(qty) from spj where sno='S1' and pno='P1';
```
- 对每个供应给工程的零件，求零件号、工程号和相应的总量。
```SQL
select pno,jno,qty from spj;
```
- 求为单个工程供应的零件数量超过350 的零件号。:worried:
```SQL
select distinct pno from spj group by pno,jno having sum(qty)>350;
```
- 求由S1 供应的工程名称。
```SQL
select distinct j.jname from spj,j 
where spj.jno=j.jno and spj.sno='S1';
```
- 求由S1 供应的零件颜色。
```SQL
select distinct p.color from spj,p 
where spj.pno=p.pno and spj.sno='S1';
```
- 求供应给北京工程的零件号。
```SQL
select distinct spj.pno from spj,j 
where j.jno=spj.jno and j.city='北京';
```

#### 31--40
- 求使用了S1 供应的零件的工程号。
```SQL
select distinct jno from spj where sno='S1';
```
- 求status 比S1 低的供应商号码。
```SQL
select distinct sno from s where status<(select status from s where sno='S1');
```
- 求所在城市按字母排序为第一的工程号。
```SQL
select min(convert (city using gbk)) as answer from j;
```
- 求被供应零件P1 的平均数量大于供应给工程J1 的任意零件的最大数量的工程号。
```SQL
select jno from j where (select avg(qty)
from spj where spj.jno=j.jno and spj.pno='P1')
> (select max(qty) from spj where jno='J1');
```
- 求满足下面要求的供应商号码，该供应商供应给某个工程零件P1 的数量大于这个工程被供应的零件P1 的平均数量。
```SQL
select sno from spj where qty>
(select avg(qty) from spj where pno='P1') and pno='P1';
```
- 求没有被北京供应商供应过红色零件的工程号码。
```SQL
select jno from j where not exists
(select * from spj,s,p
where spj.jno=j.jno and spj.sno=s.sno and spj.pno=p.pno and s.city='北京'
and p.color='红');
```
- 求所用零件全被S1 供应的工程号码。
```SQL
 select jno from (select sno,jno from spj 
 group by jno having count(sno)=1 and sno='S1') as temp;
```
- 求所有北京工程都使用的零件号码。
```SQL
select pno from p
where not exists
(select * from j
where j.city='北京' and not exists
(select * from spj where spj.pno=p.pno and spj.jno=j.jno));
```
- 求对所有工程都提供了同一零件的供应商号码。
```SQL
select sno from s where exists
(select * from p where not exists
(select * from j where not exists
(select * from spj where spj.sno=s.sno and spj.pno=p.pno and spj.jno=j.jno)));
```
- 求使用了S1 提供的所有零件的工程号码。
```SQL

```


#### 41--55
- 求至少有一个供应商、零件或工程所在的城市。
```SQL
select city from s
union
select city from j;
```
- 求被北京供应商供应或被北京工程使用的零件号码。
```SQL
select spj.pno from spj,s where spj.sno=s.sno and s.city='北京'
union
select spj.pno from spj,j where spj.jno=j.jno and j.city='北京';
```
- 求所有supplier-number / part-number 对，其中指定的供应商不供应指定的零件。
```SQL
select s.sno,p.pno from s,p where not exists
(select * from spj where spj.sno=s.sno and spj.pno=p.pno);
```
- 向p表追加如下记录（P0,PN0,蓝）。
```SQL
insert into p(pno,pname,color) value ('P0','PN0','蓝');
```
- 把零件重量在15到20之间的零件信息追加到新的表p1中。
```SQL
create table p1 as(select * from p where weight <=20 and weight >=15);
```
- 向s表追加记录（s1, n2, ’上海’）能成功吗?为什么？
```
不可以，因为sno是主码，非空且唯一
```
- 把s、p、j三个表中的s#,p#,j#列进行交叉联接，把结果追加到spj1表中（如果只考虑下面表格中的原始数据，应该在spj1表中追加多少条记录？你是如何计算记录条数的？）。
```SQL
count(sno)*count(pno)*count(jno)-count(sno,pno,jno)
```
- 向spj表追加（s6,p1,j6,1000）本操作能正确执行吗？为什么？如果追加(s4,p1,j6,-10) 行吗？如果现在想强制追加这两条记录该怎么办？
```SQL
不行，存在外键约束
解除约束或更改约束
```
- 把s1供应商供应的零件为p1的所有项目对应的数量qty改为500。
```SQL
update spj
set qty=500
where sno='S1' and pno='P1';
```
- 把qty值大于等于1000的所有供应商城市更改为‘北京’ 。
```SQL
set SQL_SAFE_UPDATES=0;
update s
set s.city='北京'
where sno in (select sno from spj group by sno
having sum(qty) >= 1000);
```
- 把j1更改成j7，本操作能正确执行吗？为什么？如果改成j0呢？spj表中记录有何变化？为什么？
```SQL
改成j7不能，jno主码已经有了j7
j0可以
```
- 把零件重量低于15的增加3，高于15的增加2。
```SQL
set SQL_SAFE_UPDATES=0;
update p 
set weight = case
   when weight<15 then weight +3
   when weight>15 then weight +2
end;
```
- 删除为j7工程供应零件的所有供应商信息（如果建立外键时没有带级联删除选项，本操作能正确执行吗？为什么？）
```SQL
可以删除
```
- 删除p1表中所有记录。
```SQL
delete from p1
```
- 删除供应商和工程在同一个城市的供应商信息。
```SQL
delete from s
where sno in (
   select sno from spj,j where s.sno=spj.sno and
   spj.jno=j.jno and s.city=j.city
)
```

----

## 实验四
- 查看所有用户
```SQL
SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') 
AS query FROM mysql.user;
```
1. 创建U8，赋予U8在P表上的SELECT权限
```SQL
create user 'U8'%'localhost';

grant select on *.* to 'U8'@'localhost';
```
- 检查：
```SQL
show grants for 'U9'@'localhost';
```
or

```bash
mysql -u U8 -p
```
```SQL
show databases;

use ...;

show tables;

select * from ...;
```

2. 创建U9，赋予U9在SPJ表上qty的UPDATE权限
```SQL
create user 'U9'@'localhost';

grant update on db_spj.spj to 'U9'@'localhost';
```
- 检查
```SQL
show grants for 'U9'@'localhost';
```

3. 回收U9的所有权限
```SQL
 revoke all on *.* from 'U9'@'localhost';
```

4. 创建角色R2，使之拥有P表上的的SELECT和INSERT
```SQL
create role 'R2';

grant select, insert on db_spj.p to 'R2';
```
- 检查
```SQL
show grants for 'R2'@'localhost';
```

5. 将R2授予U3和U8，使他们具有角色R2所包含的全部权限
```SQL
grant 'R2' to 'U3'@'localhost','U8'@'localhost';
```

6. 增加R2对P表的DELETE权限，并验证U8是否具有了这个权限？
```SQL
grant delete on db_spj.p to 'R2';

show grants for 'U8'@'localhost' using 'R2';
```

7. 创建一个作用在SPJ表上的触发器，确保无论在什么时候，qty均应大于零，否则给出错误提示并回滚此操作。请测试该触发器
```SQL
DELIMITER ||

create trigger trg_spj_check before insert on spj 
for each row
begin
declare msg varchar(100);
if new.qty <=0
then
set msg=concat('您输入的qty值：',new.qty,' 非法，请输入大于0的数字！');
signal sqlstate 'HY000' SET MESSAGE_TEXT = msg;
end if;
end
||

DELIMITER ;
```
- 测试
```SQL
insert into spj values('S1','P1','J1',-2);
```

8. 查看SPJ上的触发器
```SQL
select * from information_schema.triggers 
where event_object_table='spj' \G;
```

9. 删除SPJ上的触发器
```SQL
drop trigger trg_spj_check;
```