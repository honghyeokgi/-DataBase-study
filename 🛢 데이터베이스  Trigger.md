## ๐ข ๋ฐ์ดํฐ๋ฒ ์ด์ค  Trigger

--------------------------------------------------------------------------------

###  Trigger :  ์ง์ ํ ํ์ด๋ธ์ ๋ณ๊ฒฝ์ฌํญ( Insert,Update,Delete)์ด ์์ ๊ฒฝ์ฐ ์คํ๋๋ ํจ์



Trigger ๋ก ์ค์ ํ ์ ์๋ ์ด๋ฒคํธ๋

###### REPLACE ๋ฌธ ์ด๋ฒคํธ

1. AFTER INSERT
2. BEFORE INSERT
3. BEFORE DELETE
4. AFTER DERLETE

##### INSERT ..ON DUPLICATE KEY UPDATE  ๋ฌธ ์ด๋ฒคํธ

1. BEFORE INSERT;
2. BEFORE UPDATE
3. AFTER UPDATE

###### ํ์ด๋ธ์ ํ๋์ ์ด๋ฒคํธ์ ๋ํ ํธ๋ฆฌ๊ฑฐ๋ ํ๊ฐ๋ง ์์ฑ ๊ฐ๋ฅํฉ๋๋ค (MariDB 10.2.3 ๊น์ง)



##### INSERT ํธ๋ฆฌ๊ฑฐ ํจ์

INSERT ์ ๊ฒฝ์ฐ ์๋ก์ด ๊ฐ์ด ๋ค์ด ์ค๋๊ฒ์ด๊ธฐ ๋๋ฌธ์ 

NEW.์ปฌ๋ผ ์ ํตํด ํด๋น ์ปฌ๋ผ์ ๋ค์ด์ค๋ ๊ฐ์ ์์ ์์ต๋๋ค.



##### UPDATE ํธ๋ฆฌ๊ฑฐ ํจ์

UPDATE ์ ๊ฒฝ์ฐ๋ ์๋ก์ด ๊ฐ๊ณผ ๊ธฐ์กด ๊ฐ์ด ์์ผ๋ฏ๋ก

์๋ก์ด๊ฐ : NEW.์ปฌ๋ผ

๊ธฐ์กด  ๊ฐ : OLD.์ปฌ๋ผ



##### DELETE ํธ๋ฆฌ๊ฑฐ ํจ์

๊ธฐ์กด๊ฐ๋ง ์กด์ฌํ๋ฏ๋ก OLD.์ปฌ๋ผ์ ์ฌ์ฉ





#### ํธ๋ฆฌ๊ฑฐ ์์ฑ ์์ (ํ์ด๋ธ์ ๋ณ๋ ์ฌํญ์ด ์์๊ฒฝ์ฐ ๋ก๊ทธ ํ์ด๋ธ์ ์ฝ์,์ญ์ ,๊ฐฑ์  ๋ด์ฉ ๊ธฐ๋ก)

```mysql
# ์์ ๋ก๊ทธ ํ์ด๋ธ

CREATE TABLE `serial_log` (
	`idx` INT(11) NOT NULL AUTO_INCREMENT,
	`port_log` VARCHAR(50) NULL DEFAULT NULL COLLATE 'utf8_general_ci',
	`serial_log` VARCHAR(50) NULL DEFAULT NULL COLLATE 'utf8_general_ci',
	`command` VARCHAR(50) NULL DEFAULT NULL COLLATE 'utf8_general_ci',
	`reg_date` DATE NULL DEFAULT NULL,
	`reg_Time` TIME NULL DEFAULT NULL,
	PRIMARY KEY (`idx`) USING BTREE
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB
AUTO_INCREMENT=1
;
```



```mysql
# INSERT ๊ด๋ จ ํธ๋ฆฌ๊ฑฐ
DELIMITER $$
CREATE TRIGGER insert_log()
AFTER INSERT ON 'serial_port'FOR EACH ROW # serial_port ํ์ด๋ธ์ ๋ณ๊ฒฝ์ฌํญ์ด ์ผ์ด๋๋ ํ์ด๋ธ
BEGIN

INSERT INTO serial_log VALUES('0',NEW.reg_port,NEW.serialNM,'INSERT',DATE_FORMAT(NOW(),'%Y%m%d'),DATE_FORMAT(NOW(),'%H%i%s'));

END $$
DELIMITER
```

```mysql
# UPDATE ๊ด๋ จ ํธ๋ฆฌ๊ฑฐ
DELIMITER $$
CREATE TRIGGER insert_log()
AFTER UPDATE ON 'serial_port'FOR EACH ROW # serial_port ํ์ด๋ธ์ ๋ณ๊ฒฝ์ฌํญ์ด ์ผ์ด๋๋ ํ์ด๋ธ
BEGIN

# ๋ก๊ทธ ๊ธฐ๋ก ํ์ด๋ธ์ ๊ฒฝ์ฐ ๋ณ๊ฒฝ์ฌํญ์ ๊ธฐ๋กํ๋ ๊ฒ์ด๊ธฐ ๋๋ฌธ์ INSERT ๋ฅผ ์ด์ฉํฉ๋๋ค.
INSERT INTO serial_log  VALUES('0',NEW.reg_port,OLD.serialNM,'UPDATE',DATE_FORMAT(NOW(),'%Y%m%d'),DATE_FORMAT(NOW(),'%H%i%s'));  

END $$
DELIMITER;

```

```mysql
# DELETE ๊ด๋ จ ํธ๋ฆฌ๊ฑฐ
DELIMITER $$
CREATE TRIGGER delete_log()
AFTER DELETE ON 'serial_port' FOR EACH ROW BEGIN  

insert INTO serial_log VALUES('0',OLD.reg_port,OLD.serialNM,'DELETE',DATE_FORMAT(NOW(),'%Y%m%d'),DATE_FORMAT(NOW(),'%H%i%s'));

END $$
DELIMITER;

```

์ ์์ ์ ์๋ฐ์ดํธ ๊ด๋ จ ํธ๋ฆฌ๊ฑฐ๋ serialNm ์ ์ ๋ ๋ณ๋์ ํ๋ฉด ์๋๋ฏ๋ก OLD ๋ก ์ง์ ํ๊ณ  port ๋ง ๋ฐ๋๊ธฐ์

port๋ฅผ NEW ๋ฅผ ์ด์ฉํ์ต๋๋ค.



ํธ๋ฆฌ๊ฑฐ ์ญ์  ๋ฐ ์กฐํ

```mysql
DROP TRIGGER 'ํธ๋ฆฌ๊ฑฐ ์ด๋ฆ' # ํธ๋ฆฌ๊ฑฐ ์ญ์ 
SELECT TRIGGER FROM '๋ฐ์ดํฐ๋ฒ ์ด์ค๋ช' # ํธ๋ฆฌ๊ฑฐ ์กฐํ
```

๊ฒฐ๊ณผ

![](https://github.com/honghyeokgi/-DataBase-study/blob/main/img/%EA%B7%B8%EB%A6%BC14.png?raw=true)

ํธ๋ฆฌ๊ฑฐ๋ฅผ ํ์ฉํ์ฌ ๊ฐ๋จํ ๋ก๊ทธํ์ด๋ธ์ ๋ง๋ค์ ์๋ค





์ถ์ฒ

- https://mariadb.com/kb/ko/trigger-overview/



