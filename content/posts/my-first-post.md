d��Ϊ���������ͨ������������ʹӱ������������������ϵ�������ñ����֮������˹�ϵ��

- ����ص㣺
  - �ӱ������ֵ�Ƕ��������������á�
  - �ӱ�������ͣ�������������������һ�¡�

- �������Լ��         

```mysql
�﷨��
alter table �ӱ� add [constraint][�������] foreign key (�ӱ�����ֶ���) references ���� (���������);

[�������]����ɾ�����Լ���ģ�һ�㽨�顰_fk����β
alter table �ӱ� drop foreign key �������
```

![1568278660359](mysql02.assets/1568278660359.png)

- ʹ�����Ŀ�ģ�

  - ��֤����������

    - Ҫ���Ǵӱ��������

    - Ҫ��������ɾ������

      ![1568278670264](mysql02.assets/1568278670264.png)



```sql
# ����
create table category(
	cid int primary key auto_increment,
	cname varchar(32)
);

# ��Ʒ��
create table product(
	pid int primary key auto_increment,
	pname varchar(32),
	price int,
	category_id int
);

# ������
alter table product 
add constraint fk_product_category foreign key (category_id) references category(cid);

# 1 �������������
insert into category values(1,'����');
insert into category values(2,'�ֻ�');

# 2 ����Ʒ���������
# �ɹ�
insert into product values(null, '��Ϊ��ҫ9', 2000, 2);
# ʧ��: ��Ϊ�������Լ��, �����ֵ ������������ڵ�
insert into product values(null, '����y7000P', 5000, 3);

# 3 ɾ������������
# �ɹ�
delete from category where cid = 1;
# ʧ�� : ��Ϊ�������Լ��, ����ɾ���ӱ� ��Ʒ���������ϵ, �ſ���ɾ�����������
delete from category where cid = 2;
```



## 2 �����֮��Ĺ�ϵ

ʵ�ʿ����У�һ����Ŀͨ����Ҫ�ܶ��ű������ɡ����磺һ���̳���Ŀ����Ҫ�����(category)����Ʒ��(products)��������(orders)�ȶ��ű�����Щ�������֮�����һ���Ĺ�ϵ�����������ǽ��ڵ���Ļ����ϣ�һ��ѧϰ������֪ʶ��

![1568278690486](mysql02.assets/1568278690486.png)

- һ�Զ��ϵ��

  - ����ʵ�����ͻ��Ͷ������������Ʒ�����ź�Ա��, ʡ�ݺͳ���

  - һ�Զཨ��ԭ���ڴӱ�(�෽)����һ���ֶΣ��ֶ���Ϊ���ָ������(һ��)������.

    ![1568278701813](mysql02.assets/1568278701813.png)

- ��Զ��ϵ��

  - ����ʵ����ѧ���Ϳγ̡��û��ͽ�ɫ, ��Ա�͵�Ӱ, ��Ʒ�Ͷ���

  - ��Զ��ϵ����ԭ����Ҫ���������ű�,�м�������������ֶΣ��������ֶηֱ���Ϊ���ָ�����һ��������.

    ![1568278738451](mysql02.assets/1568278738451.png)

- һ��һ��ϵ��

  - ��ʵ�ʵĿ�����Ӧ�ò���.��Ϊһ��һ���Դ�����һ�ű�.

![1568278822725](mysql02.assets/1568278822725.png)



## 3 һ�Զ����

### ����

![1568447027989](mysql02.assets/1568447027989.png)

- category�����Ϊһ����Ҳ�������������ṩ����cid
- products��Ʒ��Ϊ�෽��Ҳ���Ǵӱ������ṩ���category_id

### ʵ�֣��������Ʒ

```mysql
#���������
create table category(
  cid varchar(32) PRIMARY KEY ,
  cname varchar(100) -- ��������
);

# ��Ʒ��
CREATE TABLE `products` (
  `pid` varchar(32) PRIMARY KEY  ,
  `name` VARCHAR(40) ,
  `price` DOUBLE 
);

#�������ֶ�
alter table products add column category_id varchar(32);

#���Լ��
alter table products add constraint product_fk foreign key (category_id) references category (cid);
```

### ����

```mysql
#1 ���������������
INSERT INTO category (cid ,cname) VALUES('c001','��װ');

#2 ����Ʒ�������ͨ����,û��������ݣ�Ĭ��Ϊnull
INSERT INTO products (pid,pname) VALUES('p001','��Ʒ����');

#3 ����Ʒ�������ͨ���ݣ����������Ϣ(category���д�����������)
INSERT INTO products (pid ,pname ,category_id) VALUES('p002','��Ʒ����2','c001');

#4 ����Ʒ�������ͨ���ݣ����������Ϣ(category���в�������������) -- ʧ��,�쳣
INSERT INTO products (pid ,pname ,category_id) VALUES('p003','��Ʒ����2','c999');

#5 ɾ��ָ������(���౻��Ʒʹ��) -- ִ���쳣
DELETE FROM category WHERE cid = 'c001';
```

## 4 ��Զ����

### ����

![1568447052768](mysql02.assets/1568447052768.png)

- ��Ʒ�Ͷ�����Զ��ϵ������ֳ�����һ�Զࡣ
- products��Ʒ��Ϊ����һ��һ�Զ��������Ҫ�ṩ����pid
- orders ������Ϊ��һ��һ�Զ��������Ҫ�ṩ����oid
- orderitem�м��Ϊ������ӵĵ����ű���Ҫ�ṩ�������oid��pid, ����ά����Ʒ�����Ĺ�ϵ

### ʵ�֣���������Ʒ

```mysql
#��Ʒ��[�Ѵ���]

#������
create table `orders`(
  `oid` varchar(32) PRIMARY KEY ,
  `totalprice` double 	#�ܼ�
);

#�������
create table orderitem(
  oid varchar(50),-- ����id
  pid varchar(50)-- ��Ʒid
);

#������Ͷ��������������ϵ
alter table `orderitem` add constraint orderitem_orders_fk foreign key (oid) references orders(oid);

#��Ʒ��Ͷ��������������ϵ
alter table `orderitem` add constraint orderitem_product_fk foreign key (pid) references products(pid);

#������������ʡ�ԣ�
alter table `orderitem` add primary key (oid,pid);
```



add constraint ���� foreign key (��ǰ��Ĺ����ֶ�) references products(����Ĺ����ֶ�);

### ����

```mysql
#1 ����Ʒ�����������
insert into products values('p004', '�����ֻ�', 2999, 'c002');

#2 �򶩵������������
INSERT into orders values('o001', 5000);
INSERT into orders values('o002', 8000);

#3���м���������(���ݴ���)
insert into orderitem(oid,pid) values('o001', 'p001');
insert into orderitem(oid,pid) values('o001', 'p002');
insert into orderitem(oid,pid) values('o002', 'p001');

#4ɾ���м�������(���ݴ���)
delete from orderitem where oid='o001' and pid='p001';

#5���м���������(���ݲ�����) -- ִ���쳣
insert into orderitem(oid,pid) values('o002', 'p099'); -- ��Ʒ��pid���ݲ�����
insert into orderitem(oid,pid) values('o099', 'p001'); -- ������oid���ݲ�����

#6ɾ����Ʒ������� -- ִ���쳣
delete from products where pid='p001';
```



# �ڶ��� ����ϵʵս

## 1 ʵս1��ʡ����

- ����1�����ű�һ�Զ�

![1568447090081](mysql02.assets/1568447090081.png)

```sql
-- ����ʡ�ݱ�
create table province(
	pid int PRIMARY KEY,
	pname varchar(32), -- ʡ������
	description varchar(100) -- ����
);

-- �������б�
create table city (
	cid int PRIMARY KEY,
	cname varchar(32), -- ��������
	description varchar(100), -- ����
	province_id int,
	CONSTRAINT city_province_fk foreign key(province_id) references province(pid)
);
```



- ����2��һ�ű��Թ���һ�Զ�(��չ)

![1568447101586](mysql02.assets/1568447101586.png)

```sql
create table area (
	id int PRIMARY key AUTO_INCREMENT,
	`name` varchar(32),
	description varchar(100),
	parent_id int,
	CONSTRAINT area_area_fk FOREIGN KEY(parent_id) REFERENCES area(id)
);

INSERT into area values(null, '����ʡ', '����һ��ʡ��', null);
INSERT into area values(null, '������', '����һ������', 1);
INSERT into area values(null, '������', '����һ������', 1);
INSERT into area values(null, '�ӱ�ʡ', '����һ��ʡ��', null);
INSERT into area values(null, 'ʯ��ׯ��', '����һ������', 4);
INSERT into area values(null, '������', '����һ������', 4);
```



## 2 ʵս2���û��ͽ�ɫ

- ��Զ��ϵ

  ![1568447112656](mysql02.assets/1568447112656.png)



```sql
-- �û���
create table `user` (
	uid varchar(32) PRIMARY KEY,
	username varchar(32),
	`password` varchar(32)
);

-- ��ɫ��
create table role (
	rid varchar(32) PRIMARY KEY,
	rname varchar(32)
);

-- �м��
create table user_role(
	user_id varchar(32),
	role_id varchar(32),
	CONSTRAINT user_role_pk PRIMARY KEY(user_id,role_id),
	CONSTRAINT user_id_fk FOREIGN KEY(user_id) REFERENCES `user`(uid),
	CONSTRAINT role_id_fk FOREIGN KEY(role_id) REFERENCES role(rid)
);

```



# ������ ����ѯ �ص��ص��ص�

**�ṩ��ṹ���£�**

![1568447127701](mysql02.assets/1568447127701.png)



```mysql
# �����
CREATE TABLE category (
  cid VARCHAR(32) PRIMARY KEY ,
  cname VARCHAR(50)
);

#��Ʒ��
CREATE TABLE products(
  pid VARCHAR(32) PRIMARY KEY ,
  pname VARCHAR(50),
  price INT,
  flag VARCHAR(2), #�Ƿ��ϼܱ��Ϊ��1��ʾ�ϼܡ�0��ʾ�¼�
  category_id VARCHAR(32),
  CONSTRAINT products_category_fk FOREIGN KEY (category_id) REFERENCES category (cid)
);
```

## 1 ��ʼ������

```mysql
#����
INSERT INTO category(cid,cname) VALUES('c001','�ҵ�');
INSERT INTO category(cid,cname) VALUES('c002','����');
INSERT INTO category(cid,cname) VALUES('c003','��ױƷ');
#��Ʒ
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p001','����',5000,'1','c001');
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p002','����',3000,'1','c001');
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p003','����',5000,'1','c001');

INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p004','JACK JONES',800,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p005','��ά˹',200,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p006','��������',440,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p007','����',2000,'1','c002');

INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p008','���ζ�',800,'1','c003');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p009','���˱���',200,'1','c003');
```

## 2 ����ѯ

#### �������Ӳ�ѯ(�����в�ʹ��-�õ�����������ĳ˻�) [�˽�]

- �﷨��`select * from A,B;`

```sql
select * from category,products ORDER BY cid, pid;
```

- Ч��: category����ÿ����¼,  �� products����ÿ����¼�ֱ�����

![1568447147187](mysql02.assets/1568447147187.png)

```sql
-- �Ͽδ���
-- ���ű�������ϵĽ�� ��֮Ϊ �ѿ�����
-- ����: ���ںܶ���������
select * from products, category
where products.category_id=category.cid;

-- ����: ̫�� ���: ����
select * from products as p, category as c
where p.category_id=c.cid;

select * from products  p, category  c
where p.category_id=c.cid;

select p.pname, c.cname from products  p, category  c
where p.category_id=c.cid;
```



#### ����ͨ��һ��ͼ˵�����õĶ���ѯ��ʽ:

![1568447157611](mysql02.assets/1568447157611.png)



#### �����Ӳ�ѯ(ʹ�õĹؼ��� inner join  -- inner����ʡ��)

- ��ʽ�����ӣ�`select * from A,B where ����;`

- ��ʾ�����ӣ�`select * from A inner join B on ����;`

```mysql
-- ��ѯ��Щ�������Ʒ�Ѿ��ϼ�
-- ��ʽ������
SELECT DISTINCT
	c.cname
FROM
	category c,
	products p
WHERE
	c.cid = p.category_id;

-- ��ʾ������
SELECT DISTINCT
	c.cname
FROM
	category c INNER JOIN products p 
ON 
	c.cid = p.category_id;
```

- Ч��

![1568447182198](mysql02.assets/1568447182198.png)



#### �����Ӳ�ѯ(ʹ�õĹؼ��� outer join -- outer����ʡ��)

- �������ӣ�left outer join
  - `select * from A left outer join B on ����;`
- �������ӣ�right outer join
  - `select * from A right outer join B on ����;`

```mysql
#2.��ѯ���з�����Ʒ�ĸ���
#��������
INSERT INTO category(cid,cname) VALUES('c004','�ݳ�Ʒ');

SELECT cname,COUNT(category_id) 
FROM category c LEFT OUTER JOIN products p 
ON c.cid = p.category_id 
GROUP BY cname;
```

- Ч��

  ![1568447202862](mysql02.assets/1568447202862.png)



## 3 �Ӳ�ѯ

```http
����1:��ѯ�����ڻ�ױƷ����Ʒ
����2:��ѯ�����ڻ�ױƷ�ͼҵ����Ʒ
```



**�Ӳ�ѯ**��һ��select�������Ϊ��һ��select�﷨һ���֣���ѯ��������ѯ�������ȣ���
**�﷨**��`select ....��ѯ�ֶ� ... from ... ��.. where ... ��ѯ����`

```mysql
-- �Ӳ�ѯ, ��ѯ����ױƷ�����ϼ���Ʒ����

-- ������
select 
	p.*
from 
	category c, products p
WHERE
	c.cid = p.category_id and c.cname = '��ױƷ'


-- �Ӳ�ѯ ��һ��(��Ϊ��ѯ����ֵʹ��)
select 
	* 
from
	products p
where
	p.category_id = (SELECT cid from category where cname='��ױƷ') -- 'c003'

-- SELECT cid from category where cname='��ױƷ';



-- �Ӳ�ѯ �ڶ���(��Ϊ һ�ű� ʹ��)
select 
	p.*
FROM
	products p, (select * from category where cname='��ױƷ') c
WHERE
	p.category_id = c.cid;


-- select * from category where cname='��ױƷ'
```

- Ч��

  ![1568447224297](mysql02.assets/1568447224297.png)



**�Ӳ�ѯ��ϰ��**

```mysql
#��ѯ����ױƷ���͡��ҵ硱���������ϼ���Ʒ����
select
	*
from 
	products 
WHERE
	category_id in (select cid from category where cname='�ҵ�' or cname='��ױƷ');  -- 'c001', 'c003'


-- select cid from category where cname='�ҵ�' or cname='��ױƷ';
-- select cid from category where cname in ('�ҵ�', '��ױƷ');
```

- Ч��

  ![1568447275984](mysql02.assets/1568447275984.png)



�ұȻ���Ҫ

����

# ������ ����

## 1 ��������

MySQL�ٷ��������Ķ���Ϊ��������index���ǰ���MySQL��Ч��ȡ���ݵ����ݽṹ�����򣩡�������֮�⣬���ݿ�ϵͳ��ά���������ض������㷨�����ݽṹ����Щ���ݽṹ��ĳ�ַ�ʽ���ã�ָ�����ݣ� �����Ϳ�������Щ���ݽṹ��ʵ�ָ߼������㷨���������ݽṹ�����������������==ʾ��ͼ==��ʾ : 

![461877-20160721092805029-903699213](../../../%E6%8C%81%E4%B9%85%E5%B1%82%E9%83%A8%E5%88%86/day03_mysql03/%E7%AC%94%E8%AE%B0/mysql03.assets/461877-20160721092805029-903699213.gif)

![1568448725971](mysql02.assets/1568448725971.png)

��������ݱ�һ��������������¼������ߵ������ݼ�¼�������ַ��ע���߼������ڵļ�¼�ڴ�����Ҳ������һ���������ڵģ���Ϊ�˼ӿ�Col2�Ĳ��ң�����ά��һ���ұ���ʾ�Ķ����������ÿ���ڵ�ֱ����������ֵ��һ��ָ���Ӧ���ݼ�¼�����ַ��ָ�룬�����Ϳ������ö�����ҿ��ٻ�ȡ����Ӧ���ݡ�

һ����˵��������Ҳ�ܴ󣬲�����ȫ���洢���ڴ��У�������������������ļ�����ʽ�洢�ڴ����ϡ����������ݿ�������������ܵ���õĹ��ߡ�



## 2 ������������

����

1�� �������鼮��Ŀ¼������������ݼ�����Ч�ʣ��������ݿ��IO�ɱ���

2�� ͨ�������ж����ݽ������򣬽�����������ĳɱ�������CPU�����ġ�

����

1�� ʵ��������Ҳ��һ�ű��ñ��б����������������ֶΣ���ָ��ʵ����ļ�¼������������Ҳ��Ҫռ�ÿռ�ġ�

2�� ��Ȼ�����������˲�ѯЧ�ʣ�ͬʱȴҲ���͸��±���ٶȣ���Ա����INSERT��UPDATE��DELETE����Ϊ���±�ʱ��MySQL ����Ҫ�������ݣ���Ҫ����һ�������ļ�ÿ�θ�������������е��ֶΣ����������Ϊ�����������ļ�ֵ�仯���������Ϣ��



## 3 �����ṹ

��������MySQL�Ĵ洢�������ʵ�ֵģ��������ڷ�������ʵ�ֵġ�����ÿ�ִ洢�������������һ����ȫ��ͬ��Ҳ�������еĴ洢���涼֧�����е��������͵ġ�MySQLĿǰ�ṩ������4��������

- BTREE ���� �� ������������ͣ��󲿷�������֧�� B ��������
- HASH ������ֻ��Memory����֧�� �� ʹ�ó����� ��
- R-tree �������ռ����������ռ�������MyISAM�����һ�������������ͣ���Ҫ���ڵ���ռ��������ͣ�ͨ��ʹ�ý��٣������ر���ܡ�
- Full-text ��ȫ�������� ��ȫ������Ҳ��MyISAM��һ�������������ͣ���Ҫ����ȫ��������InnoDB��Mysql5.6�汾��ʼ֧��ȫ��������

MyISAM��InnoDB��Memory���ִ洢����Ը����������͵�֧��

| ����        | InnoDB����      | MyISAM���� | Memory���� |
| ----------- | --------------- | ---------- | ---------- |
| BTREE����   | ֧��            | ֧��       | ֧��       |
| HASH ����   | ��֧��          | ��֧��     | ֧��       |
| R-tree ���� | ��֧��          | ֧��       | ��֧��     |
| Full-text   | 5.6�汾֮��֧�� | ֧��       | ��֧��     |

����ƽ����˵�����������û���ر�ָ��������ָB+������·������������һ���Ƕ���ģ��ṹ��֯�����������оۼ�����������������ǰ׺������Ψһ����Ĭ�϶���ʹ�� B+tree ������ͳ��Ϊ ������

## 4 ��������

1�� ��ֵ���� ����һ������ֻ���������У�һ��������ж����������

2�� Ψһ���� �������е�ֵ����Ψһ���������п�ֵ

3�� �������� ����һ���������������

## 5 �����﷨

�����ڴ������ʱ�򣬿���ͬʱ������ Ҳ������ʱ�����µ�������

׼������:

```sql
CREATE TABLE `city` (
  `city_id` int(11) NOT NULL AUTO_INCREMENT,
  `city_name` varchar(50) NOT NULL,
  `country_id` int(11) NOT NULL,
  PRIMARY KEY (`city_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `country` (
  `country_id` int(11) NOT NULL AUTO_INCREMENT,
  `country_name` varchar(100) NOT NULL,
  PRIMARY KEY (`country_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;


insert into `city` (`city_id`, `city_name`, `country_id`) values(1,'����',1);
insert into `city` (`city_id`, `city_name`, `country_id`) values(2,'NewYork',2);
insert into `city` (`city_id`, `city_name`, `country_id`) values(3,'����',1);
insert into `city` (`city_id`, `city_name`, `country_id`) values(4,'�Ϻ�',1);

insert into `country` (`country_id`, `country_name`) values(1,'China');
insert into `country` (`country_id`, `country_name`) values(2,'America');
insert into `country` (`country_id`, `country_name`) values(3,'Japan');
insert into `country` (`country_id`, `country_name`) values(4,'UK');
```



### 5.1 ��������

�﷨ �� 	

```sql
CREATE 	[UNIQUE|FULLTEXT|SPATIAL]  INDEX index_name 
[USING  index_type]
ON tbl_name(index_col_name,...)


index_col_name : column_name[(length)][ASC | DESC]
```

ʾ�� �� Ϊcity���е�city_name�ֶδ������� ��

![1568449039856](mysql02.assets/1568449039856.png)	

### 5.2 �鿴����

�﷨�� 

```
show index  from  table_name;
```

ʾ�����鿴city���е�������Ϣ��

![1568449069501](mysql02.assets/1568449069501.png)

![1568449085745](mysql02.assets/1568449085745.png)

### 5.3 ɾ������

�﷨ ��

```
DROP  INDEX  index_name  ON  tbl_name;
```

ʾ�� �� ��Ҫɾ��city���ϵ�����idx_city_name�����Բ������£�

![1568449102956](mysql02.assets/1568449102956.png)

### 5.4 ALTER����

```
1). alter  table  tb_name  add  primary  key(column_list); 

	��������һ������������ζ������ֵ������Ψһ�ģ��Ҳ���ΪNULL
	
2). alter  table  tb_name  add  unique index_name(column_list);
	
	������䴴��������ֵ������Ψһ�ģ�����NULL�⣬NULL���ܻ���ֶ�Σ�
	
3). alter  table  tb_name  add  index index_name(column_list);

	�����ͨ������ ����ֵ���Գ��ֶ�Ρ�
```

# ������ JDBC �ص��ص��ص�

## 1 JDBC����

JDBC��Java DataBase Connectivity,java���ݿ����ӣ���һ������ִ��SQL����Java API��JDBC��Java�������ݿ�ı�׼�淶������Ϊ��ͬ�Ĺ�ϵ�����ݿ��ṩͳһ���ʣ�����һ����Java���Ա�д�Ľӿں�����ɡ�

JDBC��Ҫ���������������������豸Ҫ����ͨ�ţ�����һ��ͨ�����ݸ�ʽ�����ݸ�ʽ���豸�ṩ�̹涨���豸�ṩ��Ϊ�豸�ṩ���������ͨ�������������豸����ͨ�š���������ʹ�õ���mysql������`mysql-connector-java-5.1.37-bin.jar`

![1568450502628](mysql02.assets/1568450502628.png)

**JDBC�淶�������ĸ����Ķ��󣩣�**

- DriverManager:����ע������
- Connection: ��ʾ�����ݿⴴ��������
- Statement: �������ݿ�sql���Ķ���
- ResultSet: �������һ�������

## 2 JDBCԭ��

Java�ṩ�������ݿ�淶��ΪJDBC�������������ṩ�淶��ʵ�����Ϊ������

![1568450524007](mysql02.assets/1568450524007.png)

JDBC�ǽӿڣ������ǽӿڵ�ʵ�֣�û���������޷�������ݿ����ӣ��Ӷ����ܲ������ݿ⣡ÿ�����ݿ⳧�̶���Ҫ�ṩ�Լ������������������Լ���˾�����ݿ⣬Ҳ����˵����һ�㶼�����ݿ����ɳ����ṩ��

## 3 JDBC���Ű���

### ׼������

֮ǰ����ѧϰ��sql����ʹ�ã��������ķ����category���������ǽ�ʹ��JDBC�Է���������ɾ�Ĳ������

```sql
-- ������
create table users(
	uid int primary key auto_increment,
	username varchar(32) not null unique,
	password varchar(32) not null,
	nickname varchar(32)
);

-- ��������
insert into users values(null,'zhangsan','123','����'), (null,'lisi','123','����'), (null,'wangwu','123','����');
```

### ��������jar��

����libĿ¼�����mysql������mysql-connector-java-5.1.37-bin.jar

ѡ��mysql��jar�����Ҽ�ѡ�� Add as Library... ���jar����

![1568450779709](mysql02.assets/1568450779709.png)

![1568451270317](mysql02.assets/1568451270317.png)



### ��������

1. ע������.
2. �������.
3. ���ִ��sql���Ķ���
4. ִ��sql��䣬�����ؽ��
5. ������
6. �ͷ���Դ.

### ����ʵ��

```java
public static void main(String[] args) throws Exception {
        /**
         * 1.ע������
         * 2.�������
         * 3.��ȡִ��sql���Ķ���
         * 4.ִ��sql���, �����ؽ��
         * 5.������
         * 6.�ͷ���Դ
         */

        //1.ע������,  ���ǰ� Driver.class���ļ������ص��ڴ�
        Class.forName("com.mysql.jdbc.Driver");

        /**
         *   2.�������
         *   ���� url : ��Ҫ�������ݿ�ĵ�ַ  jdbc:mysql://IP��ַ:�˿ں�/Ҫ���ӵ����ݿ�����
         *   ����user : �������ݿ� ʹ�õ��û���
         *   ����password: �������ݿ� ʹ�õ�����
         */
        String url = "jdbc:mysql://localhost:3306/day03_db";
        Connection conn = DriverManager.getConnection(url, "root", "root");
        System.out.println("conn = " + conn);

        //3.��ȡִ��sql���Ķ���
        String sql = "select * from users";
        PreparedStatement stmt = conn.prepareStatement(sql);

        //4.ִ��sql���, �����ؽ��
        ResultSet rs = stmt.executeQuery();
        System.out.println("rs = " + rs);

        //5.������
        while (rs.next()) {
            //int uid = rs.getInt(1); // ͨ���е�λ�� ��ȡֵ
            //int uid2 = rs.getInt("cid"); //ͨ���е����� ��ȡֵ
            int uid = rs.getInt("uid");
            String username = rs.getString("username");
            String password = rs.getString("password");
            String nickname = rs.getString("nickname");

            System.out.println("uid = " + uid + ", username = " + username + ", password = " + password + ", nickname = " + nickname);
        }

        //6.�ͷ���Դ
        rs.close();
        stmt.close();
        conn.close();
    }
```

## 4 API���

### API��⣺ע������

`DriverManager.registerDriver(new com.mysql.jdbc.Driver());`������ʹ�ã�ԭ����2����

- ����������ע��2��
- ǿ���������ݿ������jar

����취��

- `Class.forName("com.mysql.jdbc.Driver");`

### API��⣺�������

`static Connection getConnection(String url, String user, String password)`:��ͼ�������������ݿ� URL �����ӡ�

- ����˵����
  - url ��Ҫ�������ݿ��λ�ã���ַ��
  - user�û���
  - password ����
- ���磺`getConnection("jdbc:mysql://localhost:3306/day03_db", "root", "root");`

> ��չ��
>
> URL: SUN��˾�����ݿ⳧��֮���һ��Э�顣
>
> ```
> jdbc:mysql://localhost:3306/day03_db
> ```
>
> Э����Э�� IP :�˿ں����ݿ�	mysql���ݿ�: jdbc:mysql://localhost:3306/day04 ���� jdbc:mysql:///day04��Ĭ�ϱ������ӣ�	oracle���ݿ�: jdbc:oracle:thin:@localhost:1521:sid

### API��⣺java.sql.Connection�ӿڣ�һ������

�ӿڵ�ʵ�������ݿ������С����������ݿ⽻�����ǻ������Ӷ���ġ�

- `Statement createStatement();`//��������sql���Ķ���

### API��⣺java.sql.PreparedStatement�ӿ�: ����sql��䣬��������Ӧ���

```
String sql = "ĳSQL���";
��ȡStatement���ִ��ƽ̨��PreparedStatement stmt = conn.prepareStatement(sql);
```

���÷�����

- `int executeUpdate();`--ִ��insert update delete���.

- `ResultSet executeQuery();` --ִ��select���.

- `boolean execute();` --����ִ��select�����н��ʱ�ŷ���true��ִ����������䷵��false.

### API��⣺����������ע��ִ��insert��update��delete���账��

ResultSetʵ���Ͼ���һ�Ŷ�ά�ı�����ǿ��Ե�����`boolean next()`����ָ��ĳ�м�¼������һ�ε���`next()`����ʱ����ָ���һ�м�¼��λ�ã���ʱ�Ϳ���ʹ��ResultSet�ṩ��`getXXX(int col)`��������ȡָ���е����ݣ�(������������0��ʼ��ͬ������������1��ʼ)

```
rs.next();//ָ���һ��
rs.getInt(1);//��ȡ��һ�е�һ�е�����
```

���÷�����

- `Object getObject(int index)` / `Object getObject(String name)` ����������
- `String getString(int index)` / `String getString(String name)` ����ַ���

- `int getInt(int index)` / `int getInt(String name)` �������
- `double getDouble(int index)` / `double getDouble(String name)`���˫���ȸ�����

### API��⣺�ͷ���Դ

��IO��һ����ʹ�ú�Ķ�������Ҫ�رգ��رյ�˳�����ȵõ��ĺ�رգ���õ����ȹرա�

```
rs.close();
stmt.close();
con.close();
```

## 5 JDBC��ɾ�Ĳ����

### 5.1 ��ѯ����

```java
    //��ѯ����
    @Test
    public void jdbcTest() throws SQLException {
        //1.ͨ�������� ��ȡ Connection����
        Connection conn = JDBCUtils.getConnection();
        //2.���� ����ִ��sql���Ķ��� PreparedStatement , ͬʱָ��sql���
        String sql = "select * from users where username=? and password=?";
        PreparedStatement pstat = conn.prepareStatement(sql);
        //3.Ϊsql����е� ÿ��? ��  ��ֵ
        pstat.setString(1,"admin");
        pstat.setString(2,"123");
        //4.ִ��sql���
        ResultSet rs = pstat.executeQuery();
        //5.���� ִ��sql����� ���
        while(rs.next()){
            int uid = rs.getInt("uid");
            String username = rs.getString("username");
            String password = rs.getString("password");
            System.out.println(uid + "== " +  username + " === " + password);
        }
        //6.�ͷ���Դ
        JDBCUtils.close(rs, pstat, conn);
    }
```

### 5.2 ��������

```java
    //�������
    @Test
    public void jdbcTest2() throws SQLException {
        /*
        1.ͨ�������� ��ȡ Connection����
        2.���� ����ִ��sql���Ķ��� PreparedStatement , ͬʱָ��sql���
        3.Ϊsql����е� ÿ��? ��  ��ֵ
        4.ִ��sql���
        5.���� ִ��sql����� ���
        6.�ͷ���Դ
         */
        Connection conn = JDBCUtils.getConnection();
        String sql = "insert into users(username, password) values(?, ?)";
        PreparedStatement pstat = conn.prepareStatement(sql);
        //Ϊsql����е� ÿ��? ��  ��ֵ
        pstat.setString(1, "lisi");
        pstat.setString(2,"123");
        //ִ��sql���
        int line = pstat.executeUpdate();
        System.out.println("line = " + line);
        //�ͷ���Դ
        JDBCUtils.close(null, pstat, conn);
    }
```

### 5.3 ����

```java
    //���²���
    @Test
    public void jdbcTest3() throws SQLException {
        /*
        1.ͨ�������� ��ȡ Connection����
        2.���� ����ִ��sql���Ķ��� PreparedStatement , ͬʱָ��sql���
        3.Ϊsql����е� ÿ��? ��  ��ֵ
        4.ִ��sql���
        5.���� ִ��sql����� ���
        6.�ͷ���Դ
         */
        Connection conn = JDBCUtils.getConnection();
        String sql = "update users set password=? where uid=?";
        PreparedStatement pstat = conn.prepareStatement(sql);
        //Ϊsql����е� ÿ��? ��  ��ֵ
        pstat.setString(1,"abcdef");
        pstat.setInt(2,3);
        //ִ��sql���
        int line = pstat.executeUpdate();
        System.out.println("line = " + line);

        JDBCUtils.close(null, pstat, conn);
    }
```

### 5.4 ͨ��id��ѯ����

```java
    //����ID ���в�ѯ
    @Test
    public void jdbcTest4() throws SQLException {
        /*
        1.ͨ�������� ��ȡ Connection����
        2.���� ����ִ��sql���Ķ��� PreparedStatement , ͬʱָ��sql���
        3.Ϊsql����е� ÿ��? ��  ��ֵ
        4.ִ��sql���
        5.���� ִ��sql����� ���
        6.�ͷ���Դ
         */
        //1.ͨ�������� ��ȡ Connection����
        Connection conn = JDBCUtils.getConnection();
        //2.���� ����ִ��sql���Ķ��� PreparedStatement , ͬʱָ��sql���
        String sql = "select * from users where uid=?";
        PreparedStatement pstat = conn.prepareStatement(sql);
        //3.Ϊsql����е� ÿ��? ��  ��ֵ
        pstat.setInt(1,3);
        //4.ִ��sql���
        ResultSet rs = pstat.executeQuery();
        //5.���� ִ��sql����� ���
        while(rs.next()){
            int uid = rs.getInt("uid");
            String username = rs.getString("username");
            String password = rs.getString("password");
            System.out.println(uid + "== " +  username + " === " + password);
        }
        //6.�ͷ���Դ
        JDBCUtils.close(rs, pstat, conn);

    }
```

 

# ������ JDBC������

��������ݿ����Ӳ����������Ժ����ɾ�Ĳ����й����ж����ڣ����Է�װ������JDBCUtils���ṩ��ȡ���Ӷ���ķ������Ӷ��ﵽ������ظ����á�

�ù������ṩ������`public static Connection getConnection()`���������£�

- jdbc.properties�ļ�

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/day03_db
jdbc.user=root
jdbc.password=root
```

- JDBC������

```java
public class JDBCUtils2 {
    private static String driver;
    private static String url;
    private static String user;
    private static String password;

    static {
        try {
            //ʹ���������, ��ȡ�����ļ�
            InputStream is = JDBCUtils2.class.getClassLoader().getResourceAsStream("jdbc.properties");
            Properties prop = new Properties();
            prop.load(is);

            driver = prop.getProperty("jdbc.driver");
            url = prop.getProperty("jdbc.url");
            user = prop.getProperty("jdbc.user");
            password = prop.getProperty("jdbc.password");

            //ע������
            Class.forName(driver);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    /**
     * �������Ӷ��� Connection
     */
    public static Connection getConnection() throws SQLException {
       Connection conn = DriverManager.getConnection(url, user, password);
       return conn;
    }

    /**
     * �ͷ���Դ
     */
    public static void close(ResultSet rs, Statement stat, Connection conn) throws SQLException {
        if (rs != null) {
            rs.close();
        }
        if (stat != null) {
            stat.close();
        }
        //��Connection��������, ���Connection�Ǵ����ӳ������õ�, close()������ʵ�ǹ黹; ���Connection�Ǵ�����, ��������
        if (conn != null) {
            conn.close();
        }
    }
}
```

- ʹ��JDBC������ ��ɲ�ѯ

```java
	@Test
    public void testJDBC6() throws ClassNotFoundException, Exception {
        //1.ע������,  ���ǰ� Driver.class���ļ������ص��ڴ�
        Connection conn = JDBCUtils.getConnection();

        //3.��ȡִ��sql���Ķ���
        String sql = "select * from users";
        PreparedStatement stmt = conn.prepareStatement(sql);

        //4.ִ��sql���, �����ؽ��
        ResultSet rs = stmt.executeQuery();

        //5.������
        while (rs.next()) {
            int uid = rs.getInt("uid");
            String username = rs.getString("username");
            String password = rs.getString("password");
            String nickname = rs.getString("nickname");

            System.out.println("uid = " + uid + ", username = " + username + ", password = " + password + ", nickname = " + nickname);
        }

        //6.�ͷ���Դ
        JDBCUtils.close(rs, stmt, conn);
    }
```

 y--
oTOC]---
title: "My First Post"
date: 2021-06-27T10:04:18+08:00
draft:ft: true
---
C]



# ��һ�� ������

## 1 ���Լ��

![1568278643243](mysql02.assets/1568278643243.png)

![1569199629993](mysql02.assets/1569199629993.png)

```sql
# ����
create table category(
	cid int primary key auto_increment,
	cname varchar(32)
);

# ��Ʒ��
create table product(
	pid int primary key auto_increment,
	pname varchar(32),
	price int,
	category_id int
);

# 1 �������������
insert into category values(1,'����');
insert into category values(2,'�ֻ�');

# 2 ����Ʒ���������
insert into product values(null, '��Ϊ��ҫ9', 2000, 2);
# ����3����� �Ƿ����?
insert into product values(null, '����y7000P', 5000, 3);

# 3 ɾ������������
delete from category where cid = 1;
# ɾ��2����� �Ƿ����?
delete from category where cid = 2;
```



�������������ű������͡���Ʒ��Ϊ�˱�����Ʒ�����ĸ����࣬ͨ������£����ǽ�����Ʒ�������һ�У����ڴ�ŷ���cid����Ϣ�����г�Ϊ�����

![1568278632282](mysql02.assets/1568278632282.png)

![1568278643243](mysql02.assets/1568278643243.png)

���

