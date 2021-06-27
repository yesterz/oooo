d³ÆÎªÍâ¼ü¡£ÎÒÃÇÍ¨¹ıÖ÷±íµÄÖ÷¼üºÍ´Ó±íµÄÍâ¼üÀ´ÃèÊöÖ÷Íâ¼ü¹ØÏµ£¬¾ÍÕâÈÃ±íÓë±íÖ®¼ä²úÉúÁË¹ØÏµ¡£

- Íâ¼üÌØµã£º
  - ´Ó±íÍâ¼üµÄÖµÊÇ¶ÔÖ÷±íÖ÷¼üµÄÒıÓÃ¡£
  - ´Ó±íÍâ¼üÀàĞÍ£¬±ØĞëÓëÖ÷±íÖ÷¼üÀàĞÍÒ»ÖÂ¡£

- ÉùÃ÷Íâ¼üÔ¼Êø         

```mysql
Óï·¨£º
alter table ´Ó±í add [constraint][Íâ¼üÃû³Æ] foreign key (´Ó±íÍâ¼ü×Ö¶ÎÃû) references Ö÷±í (Ö÷±íµÄÖ÷¼ü);

[Íâ¼üÃû³Æ]ÓÃÓÚÉ¾³ıÍâ¼üÔ¼ÊøµÄ£¬Ò»°ã½¨Òé¡°_fk¡±½áÎ²
alter table ´Ó±í drop foreign key Íâ¼üÃû³Æ
```

![1568278660359](mysql02.assets/1568278660359.png)

- Ê¹ÓÃÍâ¼üÄ¿µÄ£º

  - ±£Ö¤Êı¾İÍêÕûĞÔ

    - Òª¿¼ÂÇ´Ó±íÌí¼ÓÊı¾İ

    - Òª¿¼ÂÇÖ÷¼üÉ¾³ıÊı¾İ

      ![1568278670264](mysql02.assets/1568278670264.png)



```sql
# Àà±ğ±í
create table category(
	cid int primary key auto_increment,
	cname varchar(32)
);

# ÉÌÆ·±í
create table product(
	pid int primary key auto_increment,
	pname varchar(32),
	price int,
	category_id int
);

# Ìí¼ÓÍâ¼ü
alter table product 
add constraint fk_product_category foreign key (category_id) references category(cid);

# 1 Ïò·ÖÀà±íÌí¼ÓÊı¾İ
insert into category values(1,'µçÄÔ');
insert into category values(2,'ÊÖ»ú');

# 2 ÏòÉÌÆ·±íÌí¼ÓÊı¾İ
# ³É¹¦
insert into product values(null, '»ªÎªÈÙÒ«9', 2000, 2);
# Ê§°Ü: ÒòÎªÓĞÁËÍâ¼üÔ¼Êø, Íâ¼üµÄÖµ ±ØĞëÊÇÖ÷±í´æÔÚµÄ
insert into product values(null, 'ÁªÏëy7000P', 5000, 3);

# 3 É¾³ı·ÖÀà±íµÄÊı¾İ
# ³É¹¦
delete from category where cid = 1;
# Ê§°Ü : ÒòÎª´æÔÚÍâ¼üÔ¼Êø, ±ØĞëÉ¾³ı´Ó±í ÉÌÆ·±íµÄÒÀÀµ¹ØÏµ, ²Å¿ÉÒÔÉ¾³ıÖ÷±íµÄÊı¾İ
delete from category where cid = 2;
```



## 2 ±íÓë±íÖ®¼äµÄ¹ØÏµ

Êµ¼Ê¿ª·¢ÖĞ£¬Ò»¸öÏîÄ¿Í¨³£ĞèÒªºÜ¶àÕÅ±í²ÅÄÜÍê³É¡£ÀıÈç£ºÒ»¸öÉÌ³ÇÏîÄ¿¾ÍĞèÒª·ÖÀà±í(category)¡¢ÉÌÆ·±í(products)¡¢¶©µ¥±í(orders)µÈ¶àÕÅ±í¡£ÇÒÕâĞ©±íµÄÊı¾İÖ®¼ä´æÔÚÒ»¶¨µÄ¹ØÏµ£¬½ÓÏÂÀ´ÎÒÃÇ½«ÔÚµ¥±íµÄ»ù´¡ÉÏ£¬Ò»ÆğÑ§Ï°¶à±í·½ÃæµÄÖªÊ¶¡£

![1568278690486](mysql02.assets/1568278690486.png)

- Ò»¶Ô¶à¹ØÏµ£º

  - ³£¼ûÊµÀı£º¿Í»§ºÍ¶©µ¥£¬·ÖÀàºÍÉÌÆ·£¬²¿ÃÅºÍÔ±¹¤, Ê¡·İºÍ³ÇÊĞ

  - Ò»¶Ô¶à½¨±íÔ­Ôò£ºÔÚ´Ó±í(¶à·½)´´½¨Ò»¸ö×Ö¶Î£¬×Ö¶Î×÷ÎªÍâ¼üÖ¸ÏòÖ÷±í(Ò»·½)µÄÖ÷¼ü.

    ![1568278701813](mysql02.assets/1568278701813.png)

- ¶à¶Ô¶à¹ØÏµ£º

  - ³£¼ûÊµÀı£ºÑ§ÉúºÍ¿Î³Ì¡¢ÓÃ»§ºÍ½ÇÉ«, ÑİÔ±ºÍµçÓ°, ÉÌÆ·ºÍ¶©µ¥

  - ¶à¶Ô¶à¹ØÏµ½¨±íÔ­Ôò£ºĞèÒª´´½¨µÚÈıÕÅ±í,ÖĞ¼ä±íÖĞÖÁÉÙÁ½¸ö×Ö¶Î£¬ÕâÁ½¸ö×Ö¶Î·Ö±ğ×÷ÎªÍâ¼üÖ¸Ïò¸÷×ÔÒ»·½µÄÖ÷¼ü.

    ![1568278738451](mysql02.assets/1568278738451.png)

- Ò»¶ÔÒ»¹ØÏµ£º

  - ÔÚÊµ¼ÊµÄ¿ª·¢ÖĞÓ¦ÓÃ²»¶à.ÒòÎªÒ»¶ÔÒ»¿ÉÒÔ´´½¨³ÉÒ»ÕÅ±í.

![1568278822725](mysql02.assets/1568278822725.png)



## 3 Ò»¶Ô¶à²Ù×÷

### ·ÖÎö

![1568447027989](mysql02.assets/1568447027989.png)

- category·ÖÀà±í£¬ÎªÒ»·½£¬Ò²¾ÍÊÇÖ÷±í£¬±ØĞëÌá¹©Ö÷¼ücid
- productsÉÌÆ·±í£¬Îª¶à·½£¬Ò²¾ÍÊÇ´Ó±í£¬±ØĞëÌá¹©Íâ¼ücategory_id

### ÊµÏÖ£º·ÖÀàºÍÉÌÆ·

```mysql
#´´½¨·ÖÀà±í
create table category(
  cid varchar(32) PRIMARY KEY ,
  cname varchar(100) -- ·ÖÀàÃû³Æ
);

# ÉÌÆ·±í
CREATE TABLE `products` (
  `pid` varchar(32) PRIMARY KEY  ,
  `name` VARCHAR(40) ,
  `price` DOUBLE 
);

#Ìí¼ÓÍâ¼ü×Ö¶Î
alter table products add column category_id varchar(32);

#Ìí¼ÓÔ¼Êø
alter table products add constraint product_fk foreign key (category_id) references category (cid);
```

### ²Ù×÷

```mysql
#1 Ïò·ÖÀà±íÖĞÌí¼ÓÊı¾İ
INSERT INTO category (cid ,cname) VALUES('c001','·ş×°');

#2 ÏòÉÌÆ·±íÌí¼ÓÆÕÍ¨Êı¾İ,Ã»ÓĞÍâ¼üÊı¾İ£¬Ä¬ÈÏÎªnull
INSERT INTO products (pid,pname) VALUES('p001','ÉÌÆ·Ãû³Æ');

#3 ÏòÉÌÆ·±íÌí¼ÓÆÕÍ¨Êı¾İ£¬º¬ÓĞÍâ¼üĞÅÏ¢(category±íÖĞ´æÔÚÕâÌõÊı¾İ)
INSERT INTO products (pid ,pname ,category_id) VALUES('p002','ÉÌÆ·Ãû³Æ2','c001');

#4 ÏòÉÌÆ·±íÌí¼ÓÆÕÍ¨Êı¾İ£¬º¬ÓĞÍâ¼üĞÅÏ¢(category±íÖĞ²»´æÔÚÕâÌõÊı¾İ) -- Ê§°Ü,Òì³£
INSERT INTO products (pid ,pname ,category_id) VALUES('p003','ÉÌÆ·Ãû³Æ2','c999');

#5 É¾³ıÖ¸¶¨·ÖÀà(·ÖÀà±»ÉÌÆ·Ê¹ÓÃ) -- Ö´ĞĞÒì³£
DELETE FROM category WHERE cid = 'c001';
```

## 4 ¶à¶Ô¶à²Ù×÷

### ·ÖÎö

![1568447052768](mysql02.assets/1568447052768.png)

- ÉÌÆ·ºÍ¶©µ¥¶à¶Ô¶à¹ØÏµ£¬½«²ğ·Ö³ÉÁ½¸öÒ»¶Ô¶à¡£
- productsÉÌÆ·±í£¬ÎªÆäÖĞÒ»¸öÒ»¶Ô¶àµÄÖ÷±í£¬ĞèÒªÌá¹©Ö÷¼üpid
- orders ¶©µ¥±í£¬ÎªÁíÒ»¸öÒ»¶Ô¶àµÄÖ÷±í£¬ĞèÒªÌá¹©Ö÷¼üoid
- orderitemÖĞ¼ä±í£¬ÎªÁíÍâÌí¼ÓµÄµÚÈıÕÅ±í£¬ĞèÒªÌá¹©Á½¸öÍâ¼üoidºÍpid, ÓÃÀ´Î¬»¤ÉÌÆ·Óë·ÖÀàµÄ¹ØÏµ

### ÊµÏÖ£º¶©µ¥ºÍÉÌÆ·

```mysql
#ÉÌÆ·±í[ÒÑ´æÔÚ]

#¶©µ¥±í
create table `orders`(
  `oid` varchar(32) PRIMARY KEY ,
  `totalprice` double 	#×Ü¼Æ
);

#¶©µ¥Ïî±í
create table orderitem(
  oid varchar(50),-- ¶©µ¥id
  pid varchar(50)-- ÉÌÆ·id
);

#¶©µ¥±íºÍ¶©µ¥Ïî±íµÄÖ÷Íâ¼ü¹ØÏµ
alter table `orderitem` add constraint orderitem_orders_fk foreign key (oid) references orders(oid);

#ÉÌÆ·±íºÍ¶©µ¥Ïî±íµÄÖ÷Íâ¼ü¹ØÏµ
alter table `orderitem` add constraint orderitem_product_fk foreign key (pid) references products(pid);

#ÁªºÏÖ÷¼ü£¨¿ÉÊ¡ÂÔ£©
alter table `orderitem` add primary key (oid,pid);
```



add constraint ±ğÃû foreign key (µ±Ç°±íµÄ¹ØÁª×Ö¶Î) references products(Ö÷±íµÄ¹ØÁª×Ö¶Î);

### ²Ù×÷

```mysql
#1 ÏòÉÌÆ·±íÖĞÌí¼ÓÊı¾İ
insert into products values('p004', '´¸×ÓÊÖ»ú', 2999, 'c002');

#2 Ïò¶©µ¥±íÖĞÌí¼ÓÊı¾İ
INSERT into orders values('o001', 5000);
INSERT into orders values('o002', 8000);

#3ÏòÖĞ¼ä±íÌí¼ÓÊı¾İ(Êı¾İ´æÔÚ)
insert into orderitem(oid,pid) values('o001', 'p001');
insert into orderitem(oid,pid) values('o001', 'p002');
insert into orderitem(oid,pid) values('o002', 'p001');

#4É¾³ıÖĞ¼ä±íµÄÊı¾İ(Êı¾İ´æÔÚ)
delete from orderitem where oid='o001' and pid='p001';

#5ÏòÖĞ¼ä±íÌí¼ÓÊı¾İ(Êı¾İ²»´æÔÚ) -- Ö´ĞĞÒì³£
insert into orderitem(oid,pid) values('o002', 'p099'); -- ÉÌÆ·±ípidÊı¾İ²»´æÔÚ
insert into orderitem(oid,pid) values('o099', 'p001'); -- ¶©µ¥±íoidÊı¾İ²»´æÔÚ

#6É¾³ıÉÌÆ·±íµÄÊı¾İ -- Ö´ĞĞÒì³£
delete from products where pid='p001';
```



# µÚ¶şÕÂ ¶à±í¹ØÏµÊµÕ½

## 1 ÊµÕ½1£ºÊ¡ºÍÊĞ

- ·½°¸1£º¶àÕÅ±í£¬Ò»¶Ô¶à

![1568447090081](mysql02.assets/1568447090081.png)

```sql
-- ´´½¨Ê¡·İ±í
create table province(
	pid int PRIMARY KEY,
	pname varchar(32), -- Ê¡·İÃû³Æ
	description varchar(100) -- ÃèÊö
);

-- ´´½¨³ÇÊĞ±í
create table city (
	cid int PRIMARY KEY,
	cname varchar(32), -- ³ÇÊĞÃû³Æ
	description varchar(100), -- ÃèÊö
	province_id int,
	CONSTRAINT city_province_fk foreign key(province_id) references province(pid)
);
```



- ·½°¸2£ºÒ»ÕÅ±í£¬×Ô¹ØÁªÒ»¶Ô¶à(À©Õ¹)

![1568447101586](mysql02.assets/1568447101586.png)

```sql
create table area (
	id int PRIMARY key AUTO_INCREMENT,
	`name` varchar(32),
	description varchar(100),
	parent_id int,
	CONSTRAINT area_area_fk FOREIGN KEY(parent_id) REFERENCES area(id)
);

INSERT into area values(null, 'ÁÉÄşÊ¡', 'ÕâÊÇÒ»¸öÊ¡·İ', null);
INSERT into area values(null, '´óÁ¬ÊĞ', 'ÕâÊÇÒ»¸ö³ÇÊĞ', 1);
INSERT into area values(null, 'ÉòÑôÊĞ', 'ÕâÊÇÒ»¸ö³ÇÊĞ', 1);
INSERT into area values(null, 'ºÓ±±Ê¡', 'ÕâÊÇÒ»¸öÊ¡·İ', null);
INSERT into area values(null, 'Ê¯¼Ò×¯ÊĞ', 'ÕâÊÇÒ»¸ö³ÇÊĞ', 4);
INSERT into area values(null, '±£¶¨ÊĞ', 'ÕâÊÇÒ»¸ö³ÇÊĞ', 4);
```



## 2 ÊµÕ½2£ºÓÃ»§ºÍ½ÇÉ«

- ¶à¶Ô¶à¹ØÏµ

  ![1568447112656](mysql02.assets/1568447112656.png)



```sql
-- ÓÃ»§±í
create table `user` (
	uid varchar(32) PRIMARY KEY,
	username varchar(32),
	`password` varchar(32)
);

-- ½ÇÉ«±í
create table role (
	rid varchar(32) PRIMARY KEY,
	rname varchar(32)
);

-- ÖĞ¼ä±í
create table user_role(
	user_id varchar(32),
	role_id varchar(32),
	CONSTRAINT user_role_pk PRIMARY KEY(user_id,role_id),
	CONSTRAINT user_id_fk FOREIGN KEY(user_id) REFERENCES `user`(uid),
	CONSTRAINT role_id_fk FOREIGN KEY(role_id) REFERENCES role(rid)
);

```



# µÚÈıÕÂ ¶à±í²éÑ¯ ÖØµãÖØµãÖØµã

**Ìá¹©±í½á¹¹ÈçÏÂ£º**

![1568447127701](mysql02.assets/1568447127701.png)



```mysql
# ·ÖÀà±í
CREATE TABLE category (
  cid VARCHAR(32) PRIMARY KEY ,
  cname VARCHAR(50)
);

#ÉÌÆ·±í
CREATE TABLE products(
  pid VARCHAR(32) PRIMARY KEY ,
  pname VARCHAR(50),
  price INT,
  flag VARCHAR(2), #ÊÇ·ñÉÏ¼Ü±ê¼ÇÎª£º1±íÊ¾ÉÏ¼Ü¡¢0±íÊ¾ÏÂ¼Ü
  category_id VARCHAR(32),
  CONSTRAINT products_category_fk FOREIGN KEY (category_id) REFERENCES category (cid)
);
```

## 1 ³õÊ¼»¯Êı¾İ

```mysql
#·ÖÀà
INSERT INTO category(cid,cname) VALUES('c001','¼Òµç');
INSERT INTO category(cid,cname) VALUES('c002','·şÊÎ');
INSERT INTO category(cid,cname) VALUES('c003','»¯×±Æ·');
#ÉÌÆ·
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p001','ÁªÏë',5000,'1','c001');
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p002','º£¶û',3000,'1','c001');
INSERT INTO products(pid, pname,price,flag,category_id) VALUES('p003','À×Éñ',5000,'1','c001');

INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p004','JACK JONES',800,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p005','ÕæÎ¬Ë¹',200,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p006','»¨»¨¹«×Ó',440,'1','c002');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p007','¾¢°Ô',2000,'1','c002');

INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p008','ÏãÄÎ¶ù',800,'1','c003');
INSERT INTO products (pid, pname,price,flag,category_id) VALUES('p009','ÏàÒË±¾²İ',200,'1','c003');
```

## 2 ¶à±í²éÑ¯

#### ½»²æÁ¬½Ó²éÑ¯(¿ª·¢ÖĞ²»Ê¹ÓÃ-µÃµ½µÄÊÇÁ½¸ö±íµÄ³Ë»ı) [ÁË½â]

- Óï·¨£º`select * from A,B;`

```sql
select * from category,products ORDER BY cid, pid;
```

- Ğ§¹û: category±íÖĞÃ¿Ìõ¼ÇÂ¼,  Óë products±íÖĞÃ¿Ìõ¼ÇÂ¼·Ö±ğÁ¬½Ó

![1568447147187](mysql02.assets/1568447147187.png)

```sql
-- ÉÏ¿Î´úÂë
-- Á½ÕÅ±íÅÅÁĞ×éºÏµÄ½á¹û ³ÆÖ®Îª µÑ¿¨¶û»ı
-- ÎÊÌâ: ´æÔÚºÜ¶à´íÎóµÄÊı¾İ
select * from products, category
where products.category_id=category.cid;

-- ÎÊÌâ: Ì«³¤ ½â¾ö: ±ğÃû
select * from products as p, category as c
where p.category_id=c.cid;

select * from products  p, category  c
where p.category_id=c.cid;

select p.pname, c.cname from products  p, category  c
where p.category_id=c.cid;
```



#### ÏÂÃæÍ¨¹ıÒ»ÕÅÍ¼ËµÃ÷³£ÓÃµÄ¶à±í²éÑ¯·½Ê½:

![1568447157611](mysql02.assets/1568447157611.png)



#### ÄÚÁ¬½Ó²éÑ¯(Ê¹ÓÃµÄ¹Ø¼ü×Ö inner join  -- inner¿ÉÒÔÊ¡ÂÔ)

- ÒşÊ½ÄÚÁ¬½Ó£º`select * from A,B where Ìõ¼ş;`

- ÏÔÊ¾ÄÚÁ¬½Ó£º`select * from A inner join B on Ìõ¼ş;`

```mysql
-- ²éÑ¯ÄÇĞ©·ÖÀàµÄÉÌÆ·ÒÑ¾­ÉÏ¼Ü
-- ÒşÊ½ÄÚÁ¬½Ó
SELECT DISTINCT
	c.cname
FROM
	category c,
	products p
WHERE
	c.cid = p.category_id;

-- ÏÔÊ¾ÄÚÁ¬½Ó
SELECT DISTINCT
	c.cname
FROM
	category c INNER JOIN products p 
ON 
	c.cid = p.category_id;
```

- Ğ§¹û

![1568447182198](mysql02.assets/1568447182198.png)



#### ÍâÁ¬½Ó²éÑ¯(Ê¹ÓÃµÄ¹Ø¼ü×Ö outer join -- outer¿ÉÒÔÊ¡ÂÔ)

- ×óÍâÁ¬½Ó£ºleft outer join
  - `select * from A left outer join B on Ìõ¼ş;`
- ÓÒÍâÁ¬½Ó£ºright outer join
  - `select * from A right outer join B on Ìõ¼ş;`

```mysql
#2.²éÑ¯ËùÓĞ·ÖÀàÉÌÆ·µÄ¸öÊı
#×óÍâÁ¬½Ó
INSERT INTO category(cid,cname) VALUES('c004','Éİ³ŞÆ·');

SELECT cname,COUNT(category_id) 
FROM category c LEFT OUTER JOIN products p 
ON c.cid = p.category_id 
GROUP BY cname;
```

- Ğ§¹û

  ![1568447202862](mysql02.assets/1568447202862.png)



## 3 ×Ó²éÑ¯

```http
ĞèÇó1:²éÑ¯¹éÊôÓÚ»¯×±Æ·µÄÉÌÆ·
ĞèÇó2:²éÑ¯¹éÊôÓÚ»¯×±Æ·ºÍ¼ÒµçµÄÉÌÆ·
```



**×Ó²éÑ¯**£ºÒ»ÌõselectÓï¾ä½á¹û×÷ÎªÁíÒ»ÌõselectÓï·¨Ò»²¿·Ö£¨²éÑ¯Ìõ¼ş£¬²éÑ¯½á¹û£¬±íµÈ£©¡£
**Óï·¨**£º`select ....²éÑ¯×Ö¶Î ... from ... ±í.. where ... ²éÑ¯Ìõ¼ş`

```mysql
-- ×Ó²éÑ¯, ²éÑ¯¡°»¯×±Æ··ÖÀàÉÏ¼ÜÉÌÆ·ÏêÇé

-- ÄÚÁ¬½Ó
select 
	p.*
from 
	category c, products p
WHERE
	c.cid = p.category_id and c.cname = '»¯×±Æ·'


-- ×Ó²éÑ¯ µÚÒ»ÖÖ(×÷Îª²éÑ¯Ìõ¼şÖµÊ¹ÓÃ)
select 
	* 
from
	products p
where
	p.category_id = (SELECT cid from category where cname='»¯×±Æ·') -- 'c003'

-- SELECT cid from category where cname='»¯×±Æ·';



-- ×Ó²éÑ¯ µÚ¶şÖÖ(×÷Îª Ò»ÕÅ±í Ê¹ÓÃ)
select 
	p.*
FROM
	products p, (select * from category where cname='»¯×±Æ·') c
WHERE
	p.category_id = c.cid;


-- select * from category where cname='»¯×±Æ·'
```

- Ğ§¹û

  ![1568447224297](mysql02.assets/1568447224297.png)



**×Ó²éÑ¯Á·Ï°£º**

```mysql
#²éÑ¯¡°»¯×±Æ·¡±ºÍ¡°¼Òµç¡±Á½¸ö·ÖÀàÉÏ¼ÜÉÌÆ·ÏêÇé
select
	*
from 
	products 
WHERE
	category_id in (select cid from category where cname='¼Òµç' or cname='»¯×±Æ·');  -- 'c001', 'c003'


-- select cid from category where cname='¼Òµç' or cname='»¯×±Æ·';
-- select cid from category where cname in ('¼Òµç', '»¯×±Æ·');
```

- Ğ§¹û

  ![1568447275984](mysql02.assets/1568447275984.png)



¸Ò±È»áÖØÒª

»ı¼«

# µÚËÄÕÂ Ë÷Òı

## 1 Ë÷Òı¸ÅÊö

MySQL¹Ù·½¶ÔË÷ÒıµÄ¶¨ÒåÎª£ºË÷Òı£¨index£©ÊÇ°ïÖúMySQL¸ßĞ§»ñÈ¡Êı¾İµÄÊı¾İ½á¹¹£¨ÓĞĞò£©¡£ÔÚÊı¾İÖ®Íâ£¬Êı¾İ¿âÏµÍ³»¹Î¬»¤ÕßÂú×ãÌØ¶¨²éÕÒËã·¨µÄÊı¾İ½á¹¹£¬ÕâĞ©Êı¾İ½á¹¹ÒÔÄ³ÖÖ·½Ê½ÒıÓÃ£¨Ö¸Ïò£©Êı¾İ£¬ ÕâÑù¾Í¿ÉÒÔÔÚÕâĞ©Êı¾İ½á¹¹ÉÏÊµÏÖ¸ß¼¶²éÕÒËã·¨£¬ÕâÖÖÊı¾İ½á¹¹¾ÍÊÇË÷Òı¡£ÈçÏÂÃæµÄ==Ê¾ÒâÍ¼==ËùÊ¾ : 

![461877-20160721092805029-903699213](../../../%E6%8C%81%E4%B9%85%E5%B1%82%E9%83%A8%E5%88%86/day03_mysql03/%E7%AC%94%E8%AE%B0/mysql03.assets/461877-20160721092805029-903699213.gif)

![1568448725971](mysql02.assets/1568448725971.png)

×ó±ßÊÇÊı¾İ±í£¬Ò»¹²ÓĞÁ½ÁĞÆßÌõ¼ÇÂ¼£¬×î×ó±ßµÄÊÇÊı¾İ¼ÇÂ¼µÄÎïÀíµØÖ·£¨×¢ÒâÂß¼­ÉÏÏàÁÚµÄ¼ÇÂ¼ÔÚ´ÅÅÌÉÏÒ²²¢²»ÊÇÒ»¶¨ÎïÀíÏàÁÚµÄ£©¡£ÎªÁË¼Ó¿ìCol2µÄ²éÕÒ£¬¿ÉÒÔÎ¬»¤Ò»¸öÓÒ±ßËùÊ¾µÄ¶ş²æ²éÕÒÊ÷£¬Ã¿¸ö½Úµã·Ö±ğ°üº¬Ë÷Òı¼üÖµºÍÒ»¸öÖ¸Ïò¶ÔÓ¦Êı¾İ¼ÇÂ¼ÎïÀíµØÖ·µÄÖ¸Õë£¬ÕâÑù¾Í¿ÉÒÔÔËÓÃ¶ş²æ²éÕÒ¿ìËÙ»ñÈ¡µ½ÏàÓ¦Êı¾İ¡£

Ò»°ãÀ´ËµË÷Òı±¾ÉíÒ²ºÜ´ó£¬²»¿ÉÄÜÈ«²¿´æ´¢ÔÚÄÚ´æÖĞ£¬Òò´ËË÷ÒıÍùÍùÒÔË÷ÒıÎÄ¼şµÄĞÎÊ½´æ´¢ÔÚ´ÅÅÌÉÏ¡£Ë÷ÒıÊÇÊı¾İ¿âÖĞÓÃÀ´Ìá¸ßĞÔÄÜµÄ×î³£ÓÃµÄ¹¤¾ß¡£



## 2 Ë÷ÒıÓÅÊÆÁÓÊÆ

ÓÅÊÆ

1£© ÀàËÆÓÚÊé¼®µÄÄ¿Â¼Ë÷Òı£¬Ìá¸ßÊı¾İ¼ìË÷µÄĞ§ÂÊ£¬½µµÍÊı¾İ¿âµÄIO³É±¾¡£

2£© Í¨¹ıË÷ÒıÁĞ¶ÔÊı¾İ½øĞĞÅÅĞò£¬½µµÍÊı¾İÅÅĞòµÄ³É±¾£¬½µµÍCPUµÄÏûºÄ¡£

ÁÓÊÆ

1£© Êµ¼ÊÉÏË÷ÒıÒ²ÊÇÒ»ÕÅ±í£¬¸Ã±íÖĞ±£´æÁËÖ÷¼üÓëË÷Òı×Ö¶Î£¬²¢Ö¸ÏòÊµÌåÀàµÄ¼ÇÂ¼£¬ËùÒÔË÷ÒıÁĞÒ²ÊÇÒªÕ¼ÓÃ¿Õ¼äµÄ¡£

2£© ËäÈ»Ë÷Òı´ó´óÌá¸ßÁË²éÑ¯Ğ§ÂÊ£¬Í¬Ê±È´Ò²½µµÍ¸üĞÂ±íµÄËÙ¶È£¬Èç¶Ô±í½øĞĞINSERT¡¢UPDATE¡¢DELETE¡£ÒòÎª¸üĞÂ±íÊ±£¬MySQL ²»½öÒª±£´æÊı¾İ£¬»¹Òª±£´æÒ»ÏÂË÷ÒıÎÄ¼şÃ¿´Î¸üĞÂÌí¼ÓÁËË÷ÒıÁĞµÄ×Ö¶Î£¬¶¼»áµ÷ÕûÒòÎª¸üĞÂËù´øÀ´µÄ¼üÖµ±ä»¯ºóµÄË÷ÒıĞÅÏ¢¡£



## 3 Ë÷Òı½á¹¹

Ë÷ÒıÊÇÔÚMySQLµÄ´æ´¢ÒıÇæ²ãÖĞÊµÏÖµÄ£¬¶ø²»ÊÇÔÚ·şÎñÆ÷²ãÊµÏÖµÄ¡£ËùÒÔÃ¿ÖÖ´æ´¢ÒıÇæµÄË÷Òı¶¼²»Ò»¶¨ÍêÈ«ÏàÍ¬£¬Ò²²»ÊÇËùÓĞµÄ´æ´¢ÒıÇæ¶¼Ö§³ÖËùÓĞµÄË÷ÒıÀàĞÍµÄ¡£MySQLÄ¿Ç°Ìá¹©ÁËÒÔÏÂ4ÖÖË÷Òı£º

- BTREE Ë÷Òı £º ×î³£¼ûµÄË÷ÒıÀàĞÍ£¬´ó²¿·ÖË÷Òı¶¼Ö§³Ö B Ê÷Ë÷Òı¡£
- HASH Ë÷Òı£ºÖ»ÓĞMemoryÒıÇæÖ§³Ö £¬ Ê¹ÓÃ³¡¾°¼òµ¥ ¡£
- R-tree Ë÷Òı£¨¿Õ¼äË÷Òı£©£º¿Õ¼äË÷ÒıÊÇMyISAMÒıÇæµÄÒ»¸öÌØÊâË÷ÒıÀàĞÍ£¬Ö÷ÒªÓÃÓÚµØÀí¿Õ¼äÊı¾İÀàĞÍ£¬Í¨³£Ê¹ÓÃ½ÏÉÙ£¬²»×öÌØ±ğ½éÉÜ¡£
- Full-text £¨È«ÎÄË÷Òı£© £ºÈ«ÎÄË÷ÒıÒ²ÊÇMyISAMµÄÒ»¸öÌØÊâË÷ÒıÀàĞÍ£¬Ö÷ÒªÓÃÓÚÈ«ÎÄË÷Òı£¬InnoDB´ÓMysql5.6°æ±¾¿ªÊ¼Ö§³ÖÈ«ÎÄË÷Òı¡£

MyISAM¡¢InnoDB¡¢MemoryÈıÖÖ´æ´¢ÒıÇæ¶Ô¸÷ÖÖË÷ÒıÀàĞÍµÄÖ§³Ö

| Ë÷Òı        | InnoDBÒıÇæ      | MyISAMÒıÇæ | MemoryÒıÇæ |
| ----------- | --------------- | ---------- | ---------- |
| BTREEË÷Òı   | Ö§³Ö            | Ö§³Ö       | Ö§³Ö       |
| HASH Ë÷Òı   | ²»Ö§³Ö          | ²»Ö§³Ö     | Ö§³Ö       |
| R-tree Ë÷Òı | ²»Ö§³Ö          | Ö§³Ö       | ²»Ö§³Ö     |
| Full-text   | 5.6°æ±¾Ö®ºóÖ§³Ö | Ö§³Ö       | ²»Ö§³Ö     |

ÎÒÃÇÆ½³£ËùËµµÄË÷Òı£¬Èç¹ûÃ»ÓĞÌØ±ğÖ¸Ã÷£¬¶¼ÊÇÖ¸B+Ê÷£¨¶àÂ·ËÑË÷Ê÷£¬²¢²»Ò»¶¨ÊÇ¶ş²æµÄ£©½á¹¹×éÖ¯µÄË÷Òı¡£ÆäÖĞ¾Û¼¯Ë÷Òı¡¢¸´ºÏË÷Òı¡¢Ç°×ºË÷Òı¡¢Î¨Ò»Ë÷ÒıÄ¬ÈÏ¶¼ÊÇÊ¹ÓÃ B+tree Ë÷Òı£¬Í³³ÆÎª Ë÷Òı¡£

## 4 Ë÷Òı·ÖÀà

1£© µ¥ÖµË÷Òı £º¼´Ò»¸öË÷ÒıÖ»°üº¬µ¥¸öÁĞ£¬Ò»¸ö±í¿ÉÒÔÓĞ¶à¸öµ¥ÁĞË÷Òı

2£© Î¨Ò»Ë÷Òı £ºË÷ÒıÁĞµÄÖµ±ØĞëÎ¨Ò»£¬µ«ÔÊĞíÓĞ¿ÕÖµ

3£© ¸´ºÏË÷Òı £º¼´Ò»¸öË÷Òı°üº¬¶à¸öÁĞ

## 5 Ë÷ÒıÓï·¨

Ë÷ÒıÔÚ´´½¨±íµÄÊ±ºò£¬¿ÉÒÔÍ¬Ê±´´½¨£¬ Ò²¿ÉÒÔËæÊ±Ôö¼ÓĞÂµÄË÷Òı¡£

×¼±¸»·¾³:

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


insert into `city` (`city_id`, `city_name`, `country_id`) values(1,'Î÷°²',1);
insert into `city` (`city_id`, `city_name`, `country_id`) values(2,'NewYork',2);
insert into `city` (`city_id`, `city_name`, `country_id`) values(3,'±±¾©',1);
insert into `city` (`city_id`, `city_name`, `country_id`) values(4,'ÉÏº£',1);

insert into `country` (`country_id`, `country_name`) values(1,'China');
insert into `country` (`country_id`, `country_name`) values(2,'America');
insert into `country` (`country_id`, `country_name`) values(3,'Japan');
insert into `country` (`country_id`, `country_name`) values(4,'UK');
```



### 5.1 ´´½¨Ë÷Òı

Óï·¨ £º 	

```sql
CREATE 	[UNIQUE|FULLTEXT|SPATIAL]  INDEX index_name 
[USING  index_type]
ON tbl_name(index_col_name,...)


index_col_name : column_name[(length)][ASC | DESC]
```

Ê¾Àı £º Îªcity±íÖĞµÄcity_name×Ö¶Î´´½¨Ë÷Òı £»

![1568449039856](mysql02.assets/1568449039856.png)	

### 5.2 ²é¿´Ë÷Òı

Óï·¨£º 

```
show index  from  table_name;
```

Ê¾Àı£º²é¿´city±íÖĞµÄË÷ÒıĞÅÏ¢£»

![1568449069501](mysql02.assets/1568449069501.png)

![1568449085745](mysql02.assets/1568449085745.png)

### 5.3 É¾³ıË÷Òı

Óï·¨ £º

```
DROP  INDEX  index_name  ON  tbl_name;
```

Ê¾Àı £º ÏëÒªÉ¾³ıcity±íÉÏµÄË÷Òıidx_city_name£¬¿ÉÒÔ²Ù×÷ÈçÏÂ£º

![1568449102956](mysql02.assets/1568449102956.png)

### 5.4 ALTERÃüÁî

```
1). alter  table  tb_name  add  primary  key(column_list); 

	¸ÃÓï¾äÌí¼ÓÒ»¸öÖ÷¼ü£¬ÕâÒâÎ¶×ÅË÷ÒıÖµ±ØĞëÊÇÎ¨Ò»µÄ£¬ÇÒ²»ÄÜÎªNULL
	
2). alter  table  tb_name  add  unique index_name(column_list);
	
	ÕâÌõÓï¾ä´´½¨Ë÷ÒıµÄÖµ±ØĞëÊÇÎ¨Ò»µÄ£¨³ıÁËNULLÍâ£¬NULL¿ÉÄÜ»á³öÏÖ¶à´Î£©
	
3). alter  table  tb_name  add  index index_name(column_list);

	Ìí¼ÓÆÕÍ¨Ë÷Òı£¬ Ë÷ÒıÖµ¿ÉÒÔ³öÏÖ¶à´Î¡£
```

# µÚÎåÕÂ JDBC ÖØµãÖØµãÖØµã

## 1 JDBC¸ÅÊö

JDBC£¨Java DataBase Connectivity,javaÊı¾İ¿âÁ¬½Ó£©ÊÇÒ»ÖÖÓÃÓÚÖ´ĞĞSQLÓï¾äµÄJava API¡£JDBCÊÇJava·ÃÎÊÊı¾İ¿âµÄ±ê×¼¹æ·¶£¬¿ÉÒÔÎª²»Í¬µÄ¹ØÏµĞÍÊı¾İ¿âÌá¹©Í³Ò»·ÃÎÊ£¬ËüÓÉÒ»×éÓÃJavaÓïÑÔ±àĞ´µÄ½Ó¿ÚºÍÀà×é³É¡£

JDBCĞèÒªÁ¬½ÓÇı¶¯£¬Çı¶¯ÊÇÁ½¸öÉè±¸Òª½øĞĞÍ¨ĞÅ£¬Âú×ãÒ»¶¨Í¨ĞÅÊı¾İ¸ñÊ½£¬Êı¾İ¸ñÊ½ÓÉÉè±¸Ìá¹©ÉÌ¹æ¶¨£¬Éè±¸Ìá¹©ÉÌÎªÉè±¸Ìá¹©Çı¶¯Èí¼ş£¬Í¨¹ıÈí¼ş¿ÉÒÔÓë¸ÃÉè±¸½øĞĞÍ¨ĞÅ¡£½ñÌìÎÒÃÇÊ¹ÓÃµÄÊÇmysqlµÄÇı¶¯`mysql-connector-java-5.1.37-bin.jar`

![1568450502628](mysql02.assets/1568450502628.png)

**JDBC¹æ·¶£¨ÕÆÎÕËÄ¸öºËĞÄ¶ÔÏó£©£º**

- DriverManager:ÓÃÓÚ×¢²áÇı¶¯
- Connection: ±íÊ¾ÓëÊı¾İ¿â´´½¨µÄÁ¬½Ó
- Statement: ²Ù×÷Êı¾İ¿âsqlÓï¾äµÄ¶ÔÏó
- ResultSet: ½á¹û¼¯»òÒ»ÕÅĞéÄâ±í

## 2 JDBCÔ­Àí

JavaÌá¹©·ÃÎÊÊı¾İ¿â¹æ·¶³ÆÎªJDBC£¬¶øÉú²ú³§ÉÌÌá¹©¹æ·¶µÄÊµÏÖÀà³ÆÎªÇı¶¯¡£

![1568450524007](mysql02.assets/1568450524007.png)

JDBCÊÇ½Ó¿Ú£¬Çı¶¯ÊÇ½Ó¿ÚµÄÊµÏÖ£¬Ã»ÓĞÇı¶¯½«ÎŞ·¨Íê³ÉÊı¾İ¿âÁ¬½Ó£¬´Ó¶ø²»ÄÜ²Ù×÷Êı¾İ¿â£¡Ã¿¸öÊı¾İ¿â³§ÉÌ¶¼ĞèÒªÌá¹©×Ô¼ºµÄÇı¶¯£¬ÓÃÀ´Á¬½Ó×Ô¼º¹«Ë¾µÄÊı¾İ¿â£¬Ò²¾ÍÊÇËµÇı¶¯Ò»°ã¶¼ÓÉÊı¾İ¿âÉú³É³§ÉÌÌá¹©¡£

## 3 JDBCÈëÃÅ°¸Àı

### ×¼±¸Êı¾İ

Ö®Ç°ÎÒÃÇÑ§Ï°ÁËsqlÓï¾äµÄÊ¹ÓÃ£¬²¢´´½¨µÄ·ÖÀà±ícategory£¬½ñÌìÎÒÃÇ½«Ê¹ÓÃJDBC¶Ô·ÖÀà±í½øĞĞÔöÉ¾¸Ä²é²Ù×÷¡£

```sql
-- ´´½¨±í
create table users(
	uid int primary key auto_increment,
	username varchar(32) not null unique,
	password varchar(32) not null,
	nickname varchar(32)
);

-- ²åÈëÊı¾İ
insert into users values(null,'zhangsan','123','ÕÅÈı'), (null,'lisi','123','ÀîËÄ'), (null,'wangwu','123','ÍõÎå');
```

### µ¼ÈëÇı¶¯jar°ü

´´½¨libÄ¿Â¼£¬´æ·ÅmysqlµÄÇı¶¯mysql-connector-java-5.1.37-bin.jar

Ñ¡ÖĞmysqlµÄjar°ü£¬ÓÒ¼üÑ¡Ôñ¡° Add as Library... Íê³Éjarµ¼Èë

![1568450779709](mysql02.assets/1568450779709.png)

![1568451270317](mysql02.assets/1568451270317.png)



### ¿ª·¢²½Öè

1. ×¢²áÇı¶¯.
2. »ñµÃÁ¬½Ó.
3. »ñµÃÖ´ĞĞsqlÓï¾äµÄ¶ÔÏó
4. Ö´ĞĞsqlÓï¾ä£¬²¢·µ»Ø½á¹û
5. ´¦Àí½á¹û
6. ÊÍ·Å×ÊÔ´.

### °¸ÀıÊµÏÖ

```java
public static void main(String[] args) throws Exception {
        /**
         * 1.×¢²áÇı¶¯
         * 2.»ñµÃÁ¬½Ó
         * 3.»ñÈ¡Ö´ĞĞsqlÓï¾äµÄ¶ÔÏó
         * 4.Ö´ĞĞsqlÓï¾ä, ²¢·µ»Ø½á¹û
         * 5.´¦Àí½á¹û
         * 6.ÊÍ·Å×ÊÔ´
         */

        //1.×¢²áÇı¶¯,  ¾ÍÊÇ°Ñ Driver.class¡¡ÎÄ¼ş¡¡¼ÓÔØµ½ÄÚ´æ
        Class.forName("com.mysql.jdbc.Driver");

        /**
         *   2.»ñµÃÁ¬½Ó
         *   ²ÎÊı url : ĞèÒªÁ¬½ÓÊı¾İ¿âµÄµØÖ·  jdbc:mysql://IPµØÖ·:¶Ë¿ÚºÅ/ÒªÁ¬½ÓµÄÊı¾İ¿âÃû³Æ
         *   ²ÎÊıuser : Á¬½ÓÊı¾İ¿â Ê¹ÓÃµÄÓÃ»§Ãû
         *   ²ÎÊıpassword: Á¬½ÓÊı¾İ¿â Ê¹ÓÃµÄÃÜÂë
         */
        String url = "jdbc:mysql://localhost:3306/day03_db";
        Connection conn = DriverManager.getConnection(url, "root", "root");
        System.out.println("conn = " + conn);

        //3.»ñÈ¡Ö´ĞĞsqlÓï¾äµÄ¶ÔÏó
        String sql = "select * from users";
        PreparedStatement stmt = conn.prepareStatement(sql);

        //4.Ö´ĞĞsqlÓï¾ä, ²¢·µ»Ø½á¹û
        ResultSet rs = stmt.executeQuery();
        System.out.println("rs = " + rs);

        //5.´¦Àí½á¹û
        while (rs.next()) {
            //int uid = rs.getInt(1); // Í¨¹ıÁĞµÄÎ»ÖÃ »ñÈ¡Öµ
            //int uid2 = rs.getInt("cid"); //Í¨¹ıÁĞµÄÃû³Æ »ñÈ¡Öµ
            int uid = rs.getInt("uid");
            String username = rs.getString("username");
            String password = rs.getString("password");
            String nickname = rs.getString("nickname");

            System.out.println("uid = " + uid + ", username = " + username + ", password = " + password + ", nickname = " + nickname);
        }

        //6.ÊÍ·Å×ÊÔ´
        rs.close();
        stmt.close();
        conn.close();
    }
```

## 4 APIÏê½â

### APIÏê½â£º×¢²áÇı¶¯

`DriverManager.registerDriver(new com.mysql.jdbc.Driver());`²»½¨ÒéÊ¹ÓÃ£¬Ô­ÒòÓĞ2¸ö£º

- µ¼ÖÂÇı¶¯±»×¢²á2´Î
- Ç¿ÁÒÒÀÀµÊı¾İ¿âµÄÇı¶¯jar

½â¾ö°ì·¨£º

- `Class.forName("com.mysql.jdbc.Driver");`

### APIÏê½â£º»ñµÃÁ´½Ó

`static Connection getConnection(String url, String user, String password)`:ÊÔÍ¼½¨Á¢µ½¸ø¶¨Êı¾İ¿â URL µÄÁ¬½Ó¡£

- ²ÎÊıËµÃ÷£º
  - url ĞèÒªÁ¬½ÓÊı¾İ¿âµÄÎ»ÖÃ£¨ÍøÖ·£©
  - userÓÃ»§Ãû
  - password ÃÜÂë
- ÀıÈç£º`getConnection("jdbc:mysql://localhost:3306/day03_db", "root", "root");`

> À©Õ¹£º
>
> URL: SUN¹«Ë¾ÓëÊı¾İ¿â³§ÉÌÖ®¼äµÄÒ»ÖÖĞ­Òé¡£
>
> ```
> jdbc:mysql://localhost:3306/day03_db
> ```
>
> Ğ­Òé×ÓĞ­Òé IP :¶Ë¿ÚºÅÊı¾İ¿â	mysqlÊı¾İ¿â: jdbc:mysql://localhost:3306/day04 »òÕß jdbc:mysql:///day04£¨Ä¬ÈÏ±¾»úÁ¬½Ó£©	oracleÊı¾İ¿â: jdbc:oracle:thin:@localhost:1521:sid

### APIÏê½â£ºjava.sql.Connection½Ó¿Ú£ºÒ»¸öÁ¬½Ó

½Ó¿ÚµÄÊµÏÖÔÚÊı¾İ¿âÇı¶¯ÖĞ¡£ËùÓĞÓëÊı¾İ¿â½»»¥¶¼ÊÇ»ùÓÚÁ¬½Ó¶ÔÏóµÄ¡£

- `Statement createStatement();`//´´½¨²Ù×÷sqlÓï¾äµÄ¶ÔÏó

### APIÏê½â£ºjava.sql.PreparedStatement½Ó¿Ú: ²Ù×÷sqlÓï¾ä£¬²¢·µ»ØÏàÓ¦½á¹û

```
String sql = "Ä³SQLÓï¾ä";
»ñÈ¡StatementÓï¾äÖ´ĞĞÆ½Ì¨£ºPreparedStatement stmt = conn.prepareStatement(sql);
```

³£ÓÃ·½·¨£º

- `int executeUpdate();`--Ö´ĞĞinsert update deleteÓï¾ä.

- `ResultSet executeQuery();` --Ö´ĞĞselectÓï¾ä.

- `boolean execute();` --½öµ±Ö´ĞĞselect²¢ÇÒÓĞ½á¹ûÊ±²Å·µ»Øtrue£¬Ö´ĞĞÆäËûµÄÓï¾ä·µ»Øfalse.

### APIÏê½â£º´¦Àí½á¹û¼¯£¨×¢£ºÖ´ĞĞinsert¡¢update¡¢deleteÎŞĞè´¦Àí£©

ResultSetÊµ¼ÊÉÏ¾ÍÊÇÒ»ÕÅ¶şÎ¬µÄ±í¸ñ£¬ÎÒÃÇ¿ÉÒÔµ÷ÓÃÆä`boolean next()`·½·¨Ö¸ÏòÄ³ĞĞ¼ÇÂ¼£¬µ±µÚÒ»´Îµ÷ÓÃ`next()`·½·¨Ê±£¬±ãÖ¸ÏòµÚÒ»ĞĞ¼ÇÂ¼µÄÎ»ÖÃ£¬ÕâÊ±¾Í¿ÉÒÔÊ¹ÓÃResultSetÌá¹©µÄ`getXXX(int col)`·½·¨À´»ñÈ¡Ö¸¶¨ÁĞµÄÊı¾İ£º(ÓëÊı×éË÷Òı´Ó0¿ªÊ¼²»Í¬£¬ÕâÀïË÷Òı´Ó1¿ªÊ¼)

```
rs.next();//Ö¸ÏòµÚÒ»ĞĞ
rs.getInt(1);//»ñÈ¡µÚÒ»ĞĞµÚÒ»ÁĞµÄÊı¾İ
```

³£ÓÃ·½·¨£º

- `Object getObject(int index)` / `Object getObject(String name)` »ñµÃÈÎÒâ¶ÔÏó
- `String getString(int index)` / `String getString(String name)` »ñµÃ×Ö·û´®

- `int getInt(int index)` / `int getInt(String name)` »ñµÃÕûĞÎ
- `double getDouble(int index)` / `double getDouble(String name)`»ñµÃË«¾«¶È¸¡µãĞÍ

### APIÏê½â£ºÊÍ·Å×ÊÔ´

ÓëIOÁ÷Ò»Ñù£¬Ê¹ÓÃºóµÄ¶«Î÷¶¼ĞèÒª¹Ø±Õ£¡¹Ø±ÕµÄË³ĞòÊÇÏÈµÃµ½µÄºó¹Ø±Õ£¬ºóµÃµ½µÄÏÈ¹Ø±Õ¡£

```
rs.close();
stmt.close();
con.close();
```

## 5 JDBCÔöÉ¾¸Ä²é²Ù×÷

### 5.1 ²éÑ¯Êı¾İ

```java
    //²éÑ¯²Ù×÷
    @Test
    public void jdbcTest() throws SQLException {
        //1.Í¨¹ı¹¤¾ßÀà »ñÈ¡ Connection¶ÔÏó
        Connection conn = JDBCUtils.getConnection();
        //2.´´½¨ ÓÃÓÚÖ´ĞĞsqlÓï¾äµÄ¶ÔÏó PreparedStatement , Í¬Ê±Ö¸¶¨sqlÓï¾ä
        String sql = "select * from users where username=? and password=?";
        PreparedStatement pstat = conn.prepareStatement(sql);
        //3.ÎªsqlÓï¾äÖĞµÄ Ã¿¸ö? ºÅ  ¸³Öµ
        pstat.setString(1,"admin");
        pstat.setString(2,"123");
        //4.Ö´ĞĞsqlÓï¾ä
        ResultSet rs = pstat.executeQuery();
        //5.´¦Àí Ö´ĞĞsqlÓï¾äºóµÄ ½á¹û
        while(rs.next()){
            int uid = rs.getInt("uid");
            String username = rs.getString("username");
            String password = rs.getString("password");
            System.out.println(uid + "== " +  username + " === " + password);
        }
        //6.ÊÍ·Å×ÊÔ´
        JDBCUtils.close(rs, pstat, conn);
    }
```

### 5.2 ²åÈëÊı¾İ

```java
    //²åÈë²Ù×÷
    @Test
    public void jdbcTest2() throws SQLException {
        /*
        1.Í¨¹ı¹¤¾ßÀà »ñÈ¡ Connection¶ÔÏó
        2.´´½¨ ÓÃÓÚÖ´ĞĞsqlÓï¾äµÄ¶ÔÏó PreparedStatement , Í¬Ê±Ö¸¶¨sqlÓï¾ä
        3.ÎªsqlÓï¾äÖĞµÄ Ã¿¸ö? ºÅ  ¸³Öµ
        4.Ö´ĞĞsqlÓï¾ä
        5.´¦Àí Ö´ĞĞsqlÓï¾äºóµÄ ½á¹û
        6.ÊÍ·Å×ÊÔ´
         */
        Connection conn = JDBCUtils.getConnection();
        String sql = "insert into users(username, password) values(?, ?)";
        PreparedStatement pstat = conn.prepareStatement(sql);
        //ÎªsqlÓï¾äÖĞµÄ Ã¿¸ö? ºÅ  ¸³Öµ
        pstat.setString(1, "lisi");
        pstat.setString(2,"123");
        //Ö´ĞĞsqlÓï¾ä
        int line = pstat.executeUpdate();
        System.out.println("line = " + line);
        //ÊÍ·Å×ÊÔ´
        JDBCUtils.close(null, pstat, conn);
    }
```

### 5.3 ¸üĞÂ

```java
    //¸üĞÂ²Ù×÷
    @Test
    public void jdbcTest3() throws SQLException {
        /*
        1.Í¨¹ı¹¤¾ßÀà »ñÈ¡ Connection¶ÔÏó
        2.´´½¨ ÓÃÓÚÖ´ĞĞsqlÓï¾äµÄ¶ÔÏó PreparedStatement , Í¬Ê±Ö¸¶¨sqlÓï¾ä
        3.ÎªsqlÓï¾äÖĞµÄ Ã¿¸ö? ºÅ  ¸³Öµ
        4.Ö´ĞĞsqlÓï¾ä
        5.´¦Àí Ö´ĞĞsqlÓï¾äºóµÄ ½á¹û
        6.ÊÍ·Å×ÊÔ´
         */
        Connection conn = JDBCUtils.getConnection();
        String sql = "update users set password=? where uid=?";
        PreparedStatement pstat = conn.prepareStatement(sql);
        //ÎªsqlÓï¾äÖĞµÄ Ã¿¸ö? ºÅ  ¸³Öµ
        pstat.setString(1,"abcdef");
        pstat.setInt(2,3);
        //Ö´ĞĞsqlÓï¾ä
        int line = pstat.executeUpdate();
        System.out.println("line = " + line);

        JDBCUtils.close(null, pstat, conn);
    }
```

### 5.4 Í¨¹ıid²éÑ¯ÏêÇé

```java
    //¸ù¾İID ½øĞĞ²éÑ¯
    @Test
    public void jdbcTest4() throws SQLException {
        /*
        1.Í¨¹ı¹¤¾ßÀà »ñÈ¡ Connection¶ÔÏó
        2.´´½¨ ÓÃÓÚÖ´ĞĞsqlÓï¾äµÄ¶ÔÏó PreparedStatement , Í¬Ê±Ö¸¶¨sqlÓï¾ä
        3.ÎªsqlÓï¾äÖĞµÄ Ã¿¸ö? ºÅ  ¸³Öµ
        4.Ö´ĞĞsqlÓï¾ä
        5.´¦Àí Ö´ĞĞsqlÓï¾äºóµÄ ½á¹û
        6.ÊÍ·Å×ÊÔ´
         */
        //1.Í¨¹ı¹¤¾ßÀà »ñÈ¡ Connection¶ÔÏó
        Connection conn = JDBCUtils.getConnection();
        //2.´´½¨ ÓÃÓÚÖ´ĞĞsqlÓï¾äµÄ¶ÔÏó PreparedStatement , Í¬Ê±Ö¸¶¨sqlÓï¾ä
        String sql = "select * from users where uid=?";
        PreparedStatement pstat = conn.prepareStatement(sql);
        //3.ÎªsqlÓï¾äÖĞµÄ Ã¿¸ö? ºÅ  ¸³Öµ
        pstat.setInt(1,3);
        //4.Ö´ĞĞsqlÓï¾ä
        ResultSet rs = pstat.executeQuery();
        //5.´¦Àí Ö´ĞĞsqlÓï¾äºóµÄ ½á¹û
        while(rs.next()){
            int uid = rs.getInt("uid");
            String username = rs.getString("username");
            String password = rs.getString("password");
            System.out.println(uid + "== " +  username + " === " + password);
        }
        //6.ÊÍ·Å×ÊÔ´
        JDBCUtils.close(rs, pstat, conn);

    }
```

 

# µÚÁùÕÂ JDBC¹¤¾ßÀà

¡°»ñµÃÊı¾İ¿âÁ¬½Ó²Ù×÷£¬½«ÔÚÒÔºóµÄÔöÉ¾¸Ä²éËùÓĞ¹¦ÄÜÖĞ¶¼´æÔÚ£¬¿ÉÒÔ·â×°¹¤¾ßÀàJDBCUtils¡£Ìá¹©»ñÈ¡Á¬½Ó¶ÔÏóµÄ·½·¨£¬´Ó¶ø´ïµ½´úÂëµÄÖØ¸´ÀûÓÃ¡£

¸Ã¹¤¾ßÀàÌá¹©·½·¨£º`public static Connection getConnection()`¡£´úÂëÈçÏÂ£º

- jdbc.propertiesÎÄ¼ş

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/day03_db
jdbc.user=root
jdbc.password=root
```

- JDBC¹¤¾ßÀà

```java
public class JDBCUtils2 {
    private static String driver;
    private static String url;
    private static String user;
    private static String password;

    static {
        try {
            //Ê¹ÓÃÀà¼ÓÔØÆ÷, ¶ÁÈ¡ÅäÖÃÎÄ¼ş
            InputStream is = JDBCUtils2.class.getClassLoader().getResourceAsStream("jdbc.properties");
            Properties prop = new Properties();
            prop.load(is);

            driver = prop.getProperty("jdbc.driver");
            url = prop.getProperty("jdbc.url");
            user = prop.getProperty("jdbc.user");
            password = prop.getProperty("jdbc.password");

            //×¢²áÇı¶¯
            Class.forName(driver);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    /**
     * ·µ»ØÁ¬½Ó¶ÔÏó Connection
     */
    public static Connection getConnection() throws SQLException {
       Connection conn = DriverManager.getConnection(url, user, password);
       return conn;
    }

    /**
     * ÊÍ·Å×ÊÔ´
     */
    public static void close(ResultSet rs, Statement stat, Connection conn) throws SQLException {
        if (rs != null) {
            rs.close();
        }
        if (stat != null) {
            stat.close();
        }
        //¿´ConnectionÀ´×ÔÄÄÀï, Èç¹ûConnectionÊÇ´ÓÁ¬½Ó³ØÀïÃæ»ñµÃµÄ, close()·½·¨ÆäÊµÊÇ¹é»¹; Èç¹ûConnectionÊÇ´´½¨µÄ, ¾ÍÊÇÏú»Ù
        if (conn != null) {
            conn.close();
        }
    }
}
```

- Ê¹ÓÃJDBC¹¤¾ßÀà Íê³É²éÑ¯

```java
	@Test
    public void testJDBC6() throws ClassNotFoundException, Exception {
        //1.×¢²áÇı¶¯,  ¾ÍÊÇ°Ñ Driver.class¡¡ÎÄ¼ş¡¡¼ÓÔØµ½ÄÚ´æ
        Connection conn = JDBCUtils.getConnection();

        //3.»ñÈ¡Ö´ĞĞsqlÓï¾äµÄ¶ÔÏó
        String sql = "select * from users";
        PreparedStatement stmt = conn.prepareStatement(sql);

        //4.Ö´ĞĞsqlÓï¾ä, ²¢·µ»Ø½á¹û
        ResultSet rs = stmt.executeQuery();

        //5.´¦Àí½á¹û
        while (rs.next()) {
            int uid = rs.getInt("uid");
            String username = rs.getString("username");
            String password = rs.getString("password");
            String nickname = rs.getString("nickname");

            System.out.println("uid = " + uid + ", username = " + username + ", password = " + password + ", nickname = " + nickname);
        }

        //6.ÊÍ·Å×ÊÔ´
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



# µÚÒ»ÕÂ ¶à±í²Ù×÷

## 1 Íâ¼üÔ¼Êø

![1568278643243](mysql02.assets/1568278643243.png)

![1569199629993](mysql02.assets/1569199629993.png)

```sql
# Àà±ğ±í
create table category(
	cid int primary key auto_increment,
	cname varchar(32)
);

# ÉÌÆ·±í
create table product(
	pid int primary key auto_increment,
	pname varchar(32),
	price int,
	category_id int
);

# 1 Ïò·ÖÀà±íÌí¼ÓÊı¾İ
insert into category values(1,'µçÄÔ');
insert into category values(2,'ÊÖ»ú');

# 2 ÏòÉÌÆ·±íÌí¼ÓÊı¾İ
insert into product values(null, '»ªÎªÈÙÒ«9', 2000, 2);
# ²åÈë3ºÅÀà±ğ ÊÇ·ñºÏÀí?
insert into product values(null, 'ÁªÏëy7000P', 5000, 3);

# 3 É¾³ı·ÖÀà±íµÄÊı¾İ
delete from category where cid = 1;
# É¾³ı2ºÅÀà±ğ ÊÇ·ñºÏÀí?
delete from category where cid = 2;
```



ÏÖÔÚÎÒÃÇÓĞÁ½ÕÅ±í·ÖÀà±í¡±ºÍ¡°ÉÌÆ·±í£¬ÎªÁË±íÃ÷ÉÌÆ·ÊôÓÚÄÄ¸ö·ÖÀà£¬Í¨³£Çé¿öÏÂ£¬ÎÒÃÇ½«ÔÚÉÌÆ·±íÉÏÌí¼ÓÒ»ÁĞ£¬ÓÃÓÚ´æ·Å·ÖÀàcidµÄĞÅÏ¢£¬´ËÁĞ³ÆÎª£ºÍâ¼ü

![1568278632282](mysql02.assets/1568278632282.png)

![1568278643243](mysql02.assets/1568278643243.png)

ÎõÎ

