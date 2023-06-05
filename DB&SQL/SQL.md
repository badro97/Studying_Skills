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



### SQL Query 연습 (chinook.db - SQLite)

1. AlbumId 가 31 인 앨범의 Title 을 구합니다.  
```
SELECT Title  
FROM albums a  
WHERE AlbumId = 31;  
```

2. 아티스트 이름에 'the' 가 들어간 앨범의 AlbumId를 전부 조회합니다.
```
SELECT a.AlbumId
FROM albums a
JOIN artists ar ON a.ArtistId = ar.ArtistId 
WHERE ar.Name LIKE '%the%';
```

3. invoices 테이블에서 BillingCity 가 Stuttgart, Oslo, Redmond 인 InvoiceId 를 InvoiceId 에 따라 오름차순으로 전부 조회합니다.
```
SELECT i.InvoiceId
FROM invoices i
WHERE BillingCity = 'Stuttgart'
OR BillingCity = 'Oslo'
OR BillingCity = 'Redmond'
ORDER BY InvoiceId;
```

4. tracks 테이블에서 트랙 Name 이 'The' 로 시작하는 trackId들을 전부 조회합니다.
```
SELECT t.TrackId
FROM tracks t
WHERE Name LIKE 'The%';
```

5. customers 테이블에서 Email 이 'gmail.com' 인 CustomerId를 전부 조회합니다.
```
SELECT c.CustomerId
FROM customers c
WHERE c.Email LIKE '%gmail.com'; 
```

6. CustomerId 가 29, 30, 63 인 고객들의 주문금액이 $1.00 이상 $3.00 이하인 주문 (invoice)의 Id를 전부 조회합니다.
```
SELECT i.InvoiceId
FROM customers c
JOIN invoices i ON c.CustomerId = i.CustomerId
WHERE c.CustomerId IN (29, 30, 63)
AND i.Total >= 1 AND i.Total <= 3;
```

7. 고객의 주문 금액인 Total을 invoice 테이블에서 찾으세요
장르 (genre) 가 'Soundtrack' 인 트랙 중 트랙의 길이 (Milliseconds) 가 300,000 이상 400,000 이하인 트랙들의 Id 들을 전부 조회합니다.
```
SELECT t.TrackId
FROM genres g 
JOIN tracks t ON g.GenreId = t.GenreId
WHERE g.Name = 'Soundtrack'
AND t.Milliseconds >= 300000
AND t.Milliseconds < 400000;
```

8. 각 나라 (country) 별로 고객 (customer) 수를 구해봅니다.
컬럼은 하나만 구성하세요 (컬럼 이름은 자신이 지어주어도 상관없습니다)
| The_Num_of_customers_X_Country |
```
SELECT COUNT(*) AS 'The_Num_of_customers_X_Country'
FROM customers c 
GROUP BY Country;
```

9. 총 구매한 비용이 가장 많은 고객 (customer) 5 명의 고객 (customer)의 CustomerId를 조회합니다.

고객 구매 내역은 invoices 테이블에 있습니다.
고객의 주문금액은 invoice 테이블에서 Total을 사용
특정 고객이 여러개의 invoice를 가지고 있을 수 있음
```
SELECT c.CustomerId
FROM customers c
JOIN invoices i ON c.CustomerId = i.CustomerId
GROUP BY c.CustomerId
HAVING SUM(i.Total)
ORDER BY SUM(i.Total) DESC
LIMIT 5; 
```

10. 각 장르 (genre) 마다 트랙을 구매한 고객의 id 의 수를 구해봅니다.

고객의 수를 계산할 때 고객의 id (customer_id) 는 중복되지 않아야 합니다
조회한 결과는 장르 이름이 표시되는 'genre_name' 칼럼과 구매한 고객수를 표시하는 'The_Number_of_customer_ID' 칼럼이 있어야 합니다.
- | genre_name | The Number of customer_ID |
```
SELECT g.Name AS 'genre_name', COUNT(DISTINCT i.CustomerId) AS 'The Number of customer_ID'
FROM invoice_items ii 
JOIN invoices i ON ii.InvoiceId = i.InvoiceId 
JOIN tracks t ON ii.TrackId = t.TrackId 
JOIN genres g ON t.GenreId = g.GenreId
GROUP BY g.Name;
```

11. customers 테이블에서 각 고객의 'CustomerId' 칼럼과 고객의 도시와 나라를 대문자로 합친 문자열 칼럼을 조회해야 합니다.
도시와 나라 사이에는 한 칸을 띄웁니다. 예시 도시가 'Seoul' 이고 나라가 'South Korea' 인 경우에는 'SEOUL SOUTH KOREA' 로 합칩니다.

| CustomerId | 새로운 컬럼 |
```
SELECT CustomerId, UPPER(City||' '||Country) AS 'City_Country'
FROM customers;
```

12. 새로운 customer 아이디를 만들어봅니다. 새로운 아이디는 customer의 FirstName 의 첫 4 글자와 LastName 의 첫 2 글자를 합친 소문자입니다.

예시 FirstName 이 'Mark' 이고 LastName 이 'Zonzales' 인 경우에는 'markzo' 가 됩니다.

| 새로운 컬럼 |
```
SELECT LOWER((SUBSTRING(FirstName, 1, 4) || SUBSTRING(LastName, 1, 2))) AS 'New_Id'
FROM customers;
```

13. 직원 (employee) 중에서 회사에서 2020년 1월 1일 기준으로 7년 넘게 (>) 근무한 직원들의 EmployeeId 를 조회해야 합니다. 조회된 결과는 LastName 을 기준으로 오름차순으로 정렬합니다.
```
SELECT EmployeeId
FROM employees
WHERE HireDate < Datetime('2013-01-01 00:00:00')
ORDER BY LastName;
```

14. 새로운 고객 주문 번호를 만들어 봅니다. 새로운 주문 번호는 각 고객의 FirstName과 LastName과 InvoiceId 를 합쳐서 만듭니다. 각 고객의 새로운 주문 번호를 다음 기준으로 순차적으로 오름차순 정렬합니다
```
SELECT FirstName || LastName || i.InvoiceId AS 'new_number' 
FROM customers c
JOIN invoices i ON c.CustomerId = i.CustomerId
ORDER BY FirstName, LastName, i.InvoiceId;
```

FirstName, LastName, InvoiceId 순으로 오름차순 정렬하세요
예를 들어 FirstName 이 'Sponge', LastName 이 'Bob' InvoiceId 가 '24' 인 경우 'SpongeBob24' 가 됩니다.

15. 서브쿼리를 이용해서 앨범타이틀이 'Unplugged' 이거나 'Outbreak' 인 Track의 Name을 모두 출력하세요
```
SELECT t.Name
FROM tracks t
WHERE AlbumId IN (
  SELECT AlbumId 
  FROM albums a
  WHERE Title = 'Unplugged' OR Title = 'Outbreak'
  );
```
