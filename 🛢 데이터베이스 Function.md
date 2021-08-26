## 🛢 데이터베이스 Function

--------------------------------------------------------------------------------

###  Stored Function :  사용자가  정의한 함수로 사용할수 있으며 select 문에서  활용한다

프로시저와의 차이??

1. 입력 파라미터만 사용이 가능하다.
2. return 으로 하나의 값을 반환한다.
3. select 문에서 사용한다
4. 테이블에 의존하지 않는다? (개인적인 생각입니다.)



###### function 만들기 위한 설정

``` mysql
show global variables like 'log_bin_trust_function_creators';
```







위와 달리 OFF 로 설정이 되어있다면

```mysql
set global log_bin_trust_function_creators = 1;
```

위와 같이 입력하여 ON으로 변경해준다.

##### 생성방법

```mysql
DELIMITER %%
CREATE FUNCTION '함수명'(
	'파라미터명' '타입'
)
RETURNS '리턴타입'

BEGIN
'내용'
RETURN '리턴할 값 혹은 변수'

END $$
DELIMITER

## 호출부분
SELECT '함수명'('파라미터값')

```

위와 같이 생성할수 있다.



##### 예제 (나이를 입력하면 출생년도를 리턴하는 함수)

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









