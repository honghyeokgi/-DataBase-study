## ๐ข ๋ฐ์ดํฐ๋ฒ ์ด์ค  ํ๋ก์์ (IF ๋ฌธ ์์ )

--------------------------------------------------------------------------------

##### ํ์ด๋ธ ๊ตฌ์ฑ

1. Areas

   region varchar(20), zone varchar(20), reg_code varchar(20)

![](https://github.com/honghyeokgi/-DataBase-study/blob/main/img/%EA%B7%B8%EB%A6%BC9.png?raw=true)

2. FARMINFO

   AREA  varchar(20), ZONE varchar(20), NAME varchar(10), FARM_ID varchar(20)

   ![](https://github.com/honghyeokgi/-DataBase-study/blob/main/img/%EA%B7%B8%EB%A6%BC10.png?raw=true)



ํ๋ก์์  ์์  ๋ชฉํ

1. ์๋ ฅ๊ฐ์ผ๋ก ์ง์ญ๊ณผ  NAME ์ ์๋ ฅ๋ฐ๋๋ค.
2. ์๋ ฅ๋ฐ์ ์ง์ญ์ด Areas์ ์๋๊ฒ๊ณผ ๋งค์นญํ์ฌ ์๋ค๋ฉด Areas.region,Areas.zone,Areas.reg_code ๋ฅผ ๋ณ์์ ์ ์ฅํ๋ค
3. ๋งค์นญ๋๋ ๊ฐ์ด ์๋ค๋ฉด ์๋ฌ ๋ฐฐ์ถ
4. ๋ณ์์ ์ ์ฅ๋ ๊ฐ์ FARMINFO ์ area,zone,name,Farm_id ๋ก insert ํ๋ค
5. Farm_id ๊ฒฝ์ฐ ์๋ ฅ๋ฐ์ Name + '_' + reg_code ํ ๊ฐ์ผ๋ก ์๋ ์๋ ฅํ๋๋ก ๊ตฌ์ฑํ๋ค.

##### ์์ฑ ์ฝ๋

```mysql
DELIMITER $$
CREATE PROCEDURE TEST_PR(

	IN USR_REGION VARCHAR(20), 	# ์๋ ฅ๋ฐ์ ํ๋ผ๋ฏธํฐ
	IN USR_NAME VARCHAR(10))	# ์๋ ฅ๋ฐ์ ํ๋ผ๋ฏธํฐ
	
BEGIN
	# ๋ณ์ ์ ์ธ ๋ฐ ๊ธฐ๋ณธ๊ฐ ์ค์ 
	DECLARE R_CODE VARCHAR(20) DEFAULT 0 ;
	DECLARE R_ZONE VARCHAR(20) DEFAULT 0 ;
	DECLARE USR_ID VARCHAR(20) DEFAULT 0;
	# ๋ณ์์ ๋งค์นญ๋๋ ๊ฐ ๋์
	SET R_CODE = (SELECT reg_code FROM Areas WHERE region = USR_REGION);
	SET R_ZONE = (SELECT zone FROM Areas WHERE region = USR_REGION);
	# ๋งค์นญ๋ ๊ฐ์ด ์๋๊ฒฝ์ฐ
	IF R_CODE = '' THEN
		SELECT 'reg_code is not exit';
	# ๋งค์นญ๋ ๊ฐ์ด ์๋๊ฒฝ์ฐ
	ELSEIF  R_CODE != '' THEN
		# ์ธ๋๋ฐ ์ถ๊ฐ ๋ฐ ์๋ ฅ๋ฐ์ NAM ๊ณผ R_CODE ๋ฌธ์ ํฉ์น๊ธฐ
		SET R_CODE = CONCAT ('_',R_CODE);
		SET USR_ID = CONCAT (USR_NAME,R_CODE);
		# ์ต์ข์ผ๋ก INSERT ์ฟผ๋ฆฌ
		INSERT INTO FARMINFO VALUES(USR_REGION,R_ZONE,USR_NAME,USR_ID);	
	# ์ธ ์ฌํญ ์ฒ๋ฆฌ	
	ELSE
		SELECT 'PR_ERROR 404';

	END IF;
	
END$$
DELIMITER ;
```



##### ํ์คํธ ์์ 

```mysql
CALL TEST_PR('๊ฒฝ๊ธฐ๋ ๊ณ ์์','ํ์คํธ');
```



๊ฒฐ๊ณผ 

![](https://github.com/honghyeokgi/-DataBase-study/blob/main/img/%EA%B7%B8%EB%A6%BC11.png?raw=true)

๋ค์๊ณผ ๊ฐ์ด Areas ์ ์๋ reg_code ์ ์๋ ฅ๋ฐ์ NAME ์ด ํฉ์ณ์ง ์ํ๋ก INSERT ๋๊ฒ ๋ฉ๋๋ค.