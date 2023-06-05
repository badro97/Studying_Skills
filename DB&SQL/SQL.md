# SQL

데이터베이스에 쿼리를 보내 원하는 데이터만을 가져올 수 있게 해주는 데이터베이스 용 프로그래밍 언어  

SQL(Structured Query Language)은 데이터베이스 언어의 기준으로 주로 관계형 데이터베이스에서 사용 됩니다.  
MySQL, Oracle, SQLite, PostgreSQL등 다양한 데이터베이스에서 사용할 수 있습니다.  

## SQL 종류  

- Data Definition Language
- Data Manipulation Language
- Data Control Language
- Data Query Language
- Transaction Control Language

**Data Definition Language (DDL)**  
DDL은 데이터베이스의 테이블과 같은 오브젝트를 정의할 때 사용되는 언어를 가리킵니다. 예를 들어 테이블을 만들 때 사용하는 CREATE 이나 테이블을 제거할 때 사용되는 DROP등이 있습니다.

**Data Manipulation Language (DML)**  
DML은 데이터베이스에 데이터를 변경할 때 사용되는 언어를 가리킵니다. INSERT처럼 새로운 레코드를 추가하거나, 데이터를 삭제하는 DELETE, 변경하는 UPDATE 등의 문법이 여기에 포함됩니다.

**Data Control Language (DCL)**  
DCL은 데이터베이스에 대한 접근 권한과 관련된 문법입니다. 어느 유저가 데이터베이스에 접근할 수 있는지에 대한 권한을 설정하거나 없애는 역할이죠. 권한을 주는 GRANT나 권한을 가져가는 REVOKE등이 포함됩니다.

**Data Query Language (DQL)**  
DQL은 정해진 스키마 내에서 쿼리를 할 수 있는 언어입니다. 여기에 포함된 문법은 SELECT 등이 있습니다. 물론 이렇게 따로 언어가 분류되지만 DQL을 DML의 일부분으로 말하곤 합니다.

**Transaction Control Language (TCL)**  
TCL은 DML을 거친 데이터의 변경사항을 수정할 수 있습니다. 예를 들어 COMMIT처럼 DML이 작업한 내용을 데이터베이스에 기록하거나 ROLLBACK처럼 커밋했던 내용을 다시 취소하는 문법들이 있습니다.  
