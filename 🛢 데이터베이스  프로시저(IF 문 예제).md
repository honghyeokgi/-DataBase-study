## 🛢 데이터베이스  프로시저(IF 문 예제)

--------------------------------------------------------------------------------

##### 테이블 구성

1. Areas

   region varchar(20), zone varchar(20), reg_code varchar(20)





2. FARMINFO

   AREA  varchar(20), ZONE varchar(20), NAME varchar(10), FARM_ID varchar(20),











프로시저 예제 목표

1. 입력값으로 지역과  NAME 을 입력받는다.
2. 입력받은 지역이 Areas에 있는것과 매칭하여 있다면 Areas.region,Areas.zone,Areas.reg_code 를 변수에 저장한다
3. 매칭되는 값이 없다면 에러 배출
4. 변수에 저장된 값을 FARMINFO 에 area,zone,name,Farm_id 로 insert 한다
5. Farm_id 경우 입력받은 Name + '_' + reg_code 한 값으로 자동 입력하도록 구성한다.

##### 작성 코드

```mysql
DELIMITER $$
CREATE PROCEDURE TEST_PR(

	IN USR_REGION VARCHAR(20), 	# 입력받을 파라미터
	IN USR_NAME VARCHAR(10))	# 입력받을 파라미터
	
BEGIN
	# 변수 선언 및 기본값 설정
	DECLARE R_CODE VARCHAR(20) DEFAULT 0 ;
	DECLARE R_ZONE VARCHAR(20) DEFAULT 0 ;
	DECLARE USR_ID VARCHAR(20) DEFAULT 0;
	# 변수에 매칭되는 값 대입
	SET R_CODE = (SELECT reg_code FROM Areas WHERE region = USR_REGION);
	SET R_ZONE = (SELECT zone FROM Areas WHERE region = USR_REGION);
	# 매칭된 값이 없는경우
	IF R_CODE = '' THEN
		SELECT 'reg_code is not exit';
	# 매칭된 값이 있는경우
	ELSEIF  R_CODE != '' THEN
		# 언더바 추가 및 입력받은 NAM 과 R_CODE 문자 합치기
		SET R_CODE = CONCAT ('_',R_CODE);
		SET USR_ID = CONCAT (USR_NAME,R_CODE);
		# 최종으로 INSERT 쿼리
		INSERT INTO FARMINFO VALUES(USR_REGION,R_ZONE,USR_NAME,USR_ID);	
	# 외 사항 처리	
	ELSE
		SELECT 'PR_ERROR 404';

	END IF;
	
END$$
DELIMITER ;
```



##### 테스트 예제

```mysql
CALL TEST_PR('경기도 고양시','테스트');
```



결과 

