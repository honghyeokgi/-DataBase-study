## 🛢 데이터베이스  Trigger

--------------------------------------------------------------------------------

###  Trigger :  지정한 테이블에 변경사항( Insert,Update,Delete)이 있을 경우 실행되는 함수



Trigger 로 설정할수 있는 이벤트는

###### REPLACE 문 이벤트

1. AFTER INSERT
2. BEFORE INSERT
3. BEFORE DELETE
4. AFTER DERLETE

##### INSERT ..ON DUPLICATE KEY UPDATE  문 이벤트

1. BEFORE INSERT;
2. BEFORE UPDATE
3. AFTER UPDATE

###### 테이블에 하나의 이벤트에 대한 트리거는 한개만 생성 가능합니다 (MariDB 10.2.3 까지)



##### INSERT 트리거 함수

INSERT 의 경우 새로운 값이 들어 오는것이기 때문에 

NEW.컬럼 을 통해 해당 컬럼에 들어오는 값을 알수 있습니다.



##### UPDATE 트리거 함수

UPDATE 의 경우는 새로운 값과 기존 값이 있으므로

새로운값 : NEW.컬럼

기존  값 : OLD.컬럼



##### DELETE 트리거 함수

기존값만 존재하므로 OLD.컬럼을 사용





#### 트리거 생성 예제(테이블에 변동 사항이 있을경우 로그 테이블에 삽입,삭제,갱신 내용 기록)

```mysql
# 예시 로그 테이블

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
# INSERT 관련 트리거
DELIMITER $$
CREATE TRIGGER insert_log()
AFTER INSERT ON 'serial_port'FOR EACH ROW # serial_port 테이블은 변경사항이 일어나는 테이블
BEGIN

INSERT INTO serial_log VALUES('0',NEW.reg_port,NEW.serialNM,'INSERT',DATE_FORMAT(NOW(),'%Y%m%d'),DATE_FORMAT(NOW(),'%H%i%s'));

END $$
DELIMITER
```

```mysql
# UPDATE 관련 트리거
DELIMITER $$
CREATE TRIGGER insert_log()
AFTER UPDATE ON 'serial_port'FOR EACH ROW # serial_port 테이블은 변경사항이 일어나는 테이블
BEGIN

# 로그 기록 테이블의 경우 변경사항을 기록하는 것이기 때문에 INSERT 를 이용합니다.
INSERT INTO serial_log  VALUES('0',NEW.reg_port,OLD.serialNM,'UPDATE',DATE_FORMAT(NOW(),'%Y%m%d'),DATE_FORMAT(NOW(),'%H%i%s'));  

END $$
DELIMITER;

```

```mysql
# DELETE 관련 트리거
DELIMITER $$
CREATE TRIGGER delete_log()
AFTER DELETE ON 'serial_port' FOR EACH ROW BEGIN  

insert INTO serial_log VALUES('0',OLD.reg_port,OLD.serialNM,'DELETE',DATE_FORMAT(NOW(),'%Y%m%d'),DATE_FORMAT(NOW(),'%H%i%s'));

END $$
DELIMITER;

```

위 예제의 업데이트 관련 트리거는 serialNm 은 저는 변동을 하면 안되므로 OLD 로 지정하고 port 만 바뀌기에

port를 NEW 를 이용했습니다.



트리거 삭제 및 조회

```mysql
DROP TRIGGER '트리거 이름' # 트리거 삭제
SELECT TRIGGER FROM '데이터베이스명' # 트리거 조회
```

결과

![](https://github.com/honghyeokgi/-DataBase-study/blob/main/img/%EA%B7%B8%EB%A6%BC14.png?raw=true)

트리거를 활용하여 간단한 로그테이블을 만들수 있다





출처

- https://mariadb.com/kb/ko/trigger-overview/



