## π’ λ°μ΄ν°λ² μ΄μ€  SP Handler

--------------------------------------------------------------------------------

###  Handler : SP μμ  μΏΌλ¦¬λ¬Έμ΄ μμ°¨μ μΌλ‘ μ€νλλ©΄μ μ€λ₯κ°  λ°μν  κ²½μ° μ·¨ν  μ‘°μΉλ₯Ό μ€μ ν μ μλλ‘ ν΄μ€λ€.

Handler κ° μ μλμ΄ μμ§ μμΌλ©΄ μλ¬ λ°μμμΉμμ SPλ μ’λ£κ° λλ©° μ§μ  μΏΌλ¦¬λ€μ COMMIT μ΄ μ§νλλ€.

```mysql
# μλ¬κ° λ°μμ SPλ₯Ό μ’λ£λ₯Ό μν νΈλ€λ¬

DECLARE EXIT HANDLER FRO SQLEXCEPTION
BEGIN 
#  μλ¬ μ²λ¦¬
END;
```



```mysql
#μλ¬ μ²λ¦¬ κ΅¬λ¬Έ ν , λ€μ μ§νμ κ³μνλ λ°©λ²

DECLARE CONTINUE HANDLER FOR SQLEXCEPTION
BEGIN
# μ§νν  μΏΌλ¦¬ 
END;
```



μμ μμ λ₯Ό ν΅ν΄ μ΄λμ λ λμΉλ₯Ό μ±μ μλ―μ΄ νΈλ€λ¬λ μλμ κ°λ€

```mysql
DECLARE [HANDLER_ACTION] HANDLER FOR [CONDITION_VAL] [STATEMENT]
```

SQLWARNING : SQLSTATE κ°μ΄ '01' λ‘ μμνλ κ²½μ°

NOT FOUND : SQLSTATE κ°μ΄ '02'λ‘ μμ

SQLEXCEPTION : '00', '01', '02' μ μΈλ‘ μμ

SP μμλ μ λ μλμ κ°μ΄ μ¬μ©ν©λλ€

```mysql
DELIMITER $$

CREATE PROCEDURE [PROCEDURE_NAME](
# μΈμΌλ‘ λ°μ λ³μλ€
)
BEGIN
DECLARE ERR INT DEFAULT 0;
DECLARE CONTINUE HANDLER FOR SQLEXCEPTION SET ERR = -1;

START TRANSACTION

# μΏΌλ¦¬ μ§μ λ±λ± λ΄μ©

# SQLEXCEPTION μ΄ λ°μνλ©΄ ERRμ κ°μ΄ -1λ‘ λ³κ²½λμ΄  ν΄λΉ IF λ¬Έμ κ±Έλ € ROLLBACK μ μ§ν
IF ERR < 0 THEN
	ROLLBACK;
# λ¬Έμ κ° μλκ²½μ° ν΄λΉ SP μμ μ€νλ QUERY λ¬Έμ COMMIT μ§ν
ELSE
	COMMIT;
END IF;
END $$
DELIMITER ;

```

