## 🛢 데이터베이스  SP Handler

--------------------------------------------------------------------------------

###  Handler : SP 에서  쿼리문이 순차적으로 실행되면서 오류가  발생할 경우 취할 조치를 설정할수 있도록 해준다.

Handler 가 정의되어 있지 않으면 에러 발생위치에서 SP는 종료가 되며 직전 쿼리들은 COMMIT 이 진행된다.

```mysql
# 에러가 발생시 SP를 종료를 위한 핸들러

DECLARE EXIT HANDLER FRO SQLEXCEPTION
BEGIN 
#  에러 처리
END;
```



```mysql
#에러 처리 구문 후 , 다음 진행을 계속하는 방법

DECLARE CONTINUE HANDLER FOR SQLEXCEPTION
BEGIN
# 진행할 쿼리 
END;
```



위의 예제를 통해 어느정도 눈치를 챌수 있듯이 핸들러는 아래와 같다

```mysql
DECLARE [HANDLER_ACTION] HANDLER FOR [CONDITION_VAL] [STATEMENT]
```

SQLWARNING : SQLSTATE 값이 '01' 로 시작하는 경우

NOT FOUND : SQLSTATE 값이 '02'로 시작

SQLEXCEPTION : '00', '01', '02' 제외로 시작

SP 에서는 저는 아래와 같이 사용합니다

```mysql
DELIMITER $$

CREATE PROCEDURE [PROCEDURE_NAME](
# 인으로 받을 변수들
)
BEGIN
DECLARE ERR INT DEFAULT 0;
DECLARE CONTINUE HANDLER FOR SQLEXCEPTION SET ERR = -1;

START TRANSACTION

# 쿼리 질의 등등 내용

# SQLEXCEPTION 이 발생하면 ERR의 값이 -1로 변경되어  해당 IF 문에 걸려 ROLLBACK 을 진행
IF ERR < 0 THEN
	ROLLBACK;
# 문제가 없는경우 해당 SP 에서 실행된 QUERY 문을 COMMIT 진행
ELSE
	COMMIT;
END IF;
END $$
DELIMITER ;

```

