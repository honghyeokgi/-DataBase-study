## π’ λ°μ΄ν°λ² μ΄μ€ Function

--------------------------------------------------------------------------------

###  Stored Function :  μ¬μ©μκ°  μ μν ν¨μλ‘ μ¬μ©ν μ μμΌλ©° select λ¬Έμμ  νμ©νλ€

νλ‘μμ μμ μ°¨μ΄??

1. μλ ₯ νλΌλ―Έν°λ§ μ¬μ©μ΄ κ°λ₯νλ€.
2. return μΌλ‘ νλμ κ°μ λ°ννλ€.
3. select λ¬Έμμ μ¬μ©νλ€
4. νμ΄λΈμ μμ‘΄νμ§ μλλ€? (κ°μΈμ μΈ μκ°μλλ€.)



###### function λ§λ€κΈ° μν μ€μ 

``` mysql
show global variables like 'log_bin_trust_function_creators';
```

![](https://github.com/honghyeokgi/-DataBase-study/blob/main/img/%EA%B7%B8%EB%A6%BC12.png?raw=true)

μμ λ¬λ¦¬ OFF λ‘ μ€μ μ΄ λμ΄μλ€λ©΄

```mysql
set global log_bin_trust_function_creators = 1;
```

μμ κ°μ΄ μλ ₯νμ¬ ONμΌλ‘ λ³κ²½ν΄μ€λ€.

##### μμ±λ°©λ²

```mysql
DELIMITER %%
CREATE FUNCTION 'ν¨μλͺ'(
	'νλΌλ―Έν°λͺ' 'νμ'
)
RETURNS 'λ¦¬ν΄νμ'

BEGIN
'λ΄μ©'
RETURN 'λ¦¬ν΄ν  κ° νΉμ λ³μ'

END $$
DELIMITER

## νΈμΆλΆλΆ
SELECT 'ν¨μλͺ'('νλΌλ―Έν°κ°')

```

μμ κ°μ΄ μμ±ν μ μλ€.



##### μμ  (λμ΄λ₯Ό μλ ₯νλ©΄ μΆμλλλ₯Ό λ¦¬ν΄νλ ν¨μ)

```mysql
DELIMITER $$
CREATE DEFINER=`root`@`desktop-135kilo` FUNCTION `getyear`(
	`AGE` INT
)
RETURNS int(11)
LANGUAGE SQL
NOT DETERMINISTIC
CONTAINS SQL
SQL SECURITY DEFINER
COMMENT ''

BEGIN
DECLARE YEARS INT;
SET YEARS = date_format(CURDATE(),'%Y')-AGE + 1;
RETURN YEARS;
END $$
DELIMITER;
```

##### result

![](https://github.com/honghyeokgi/-DataBase-study/blob/main/img/%EA%B7%B8%EB%A6%BC13.png?raw=true)

μ μ΄λ―Έμ§μ κ°μ΄ κ²°κ³Όκ° μΆλ ₯ λλκ²μ λ³Όμ μλ€.

μμ§ Stored Function κΈ°λ₯μ μ΄λ»κ² ν¨μ¨μ μΌλ‘ μ¬μ©ν μ μλμ§ μκ°ν΄ λ΄μΌκ² λ€...

κ°μΈμ μΌλ‘ SP κ° λ ν¨μ¨μ μ΄λΌκ³  μκ°νλ€.





