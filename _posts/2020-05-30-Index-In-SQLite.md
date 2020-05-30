---
title: Index in SQLite
categories: [SQLite, Index]
tags: [SQLite, index]
---

## Index 란 무엇인가요?
------------------
인덱스(영어: index)는 데이터베이스 분야에 있어서 테이블에 대한 동작의 속도를 높여주는 자료 구조를 일컫는다. 
인덱스는 테이블 내의 1개의 컬럼, 혹은 여러 개의 컬럼을 이용하여 생성될 수 있다. 
고속의 검색 동작뿐만 아니라 레코드 접근과 관련 효율적인 순서 매김 동작에 대한 기초를 제공한다. 
인덱스를 저장하는 데 필요한 디스크 공간은 보통 테이블을 저장하는 데 필요한 디스크 공간보다 작다. 
(왜냐하면 보통 인덱스는 키-필드만 갖고 있고, 테이블의 다른 세부 항목들은 갖고 있지 않기 때문이다.) 
관계형 데이터베이스에서는 인덱스는 테이블 부분에 대한 하나의 사본이다. [\^myfootnote]

Index는 full index, partial index 로 나뉘어질 수 있다.

## Syntax
- - -   


*-CREATE INDEX*    

![CREATE-INDEX](https://www.sqlite.org/images/syntax/create-index-stmt.gif)

*-DROP INDEX*    

![DROP-INDEX](https://www.sqlite.org/images/syntax/drop-index-stmt.gif)


### - Full indexes vs. Partial indexes

간단하게 full index 와 partial index를 구분할 수 있는 방법은
WHERE clause 포함 여부이다.

WHERE clause가 포함될 경우, *Partial index*
포함되지 않을 경우 *Full index* 로 볼 수 있다.

Full index는 table에 있는 모든 행이 들어가 있는 반면,   
Partial index에는 해당 table의 subset만 있게 된다.   
(When used judiciously, partial indexes can result in smaller database files and improvements in both query and write performance.)

Partial Index가 존재한다고 해도, SQLite Query Planner는 모든 경우에 대해서 Partial Index를 사용하는 것이 아니니 주의해야 한다.
<https://www.sqlite.org/partialindex.html#queries_using_partial_indexes>


### - UNIQUE Indexes

CREATE 와 INDEX 사이에 UNIQUE keyword 가 붙을 경우, index key의 중복이 허용되지 않는다.
이 때, NULL value를 unique하게 볼 지 아닐 지는 DBMS 마다 다르다.


### - Indexes On Expressions

아래처럼 Index를 생성할 때, indexed-column 에 표현식이 들어올 수도 있다.


```
CREATE TABLE t2(x,y,z);
CREATE INDEX t2xy ON t2(x+y);
```

query의 WHERE clause 혹은 ORDER BY clause 에 위와 동일한 표현식(x+y)이 존재한다면,
SQLite query planer는 위에서 만들어진 Index를 사용하게 된다.


```
SELECT * FROM t2 WHERE x+y=22;
```


## References 
- - -   

[CREATE INDEX](https://www.sqlite.org/lang_createindex.html)   
[DROP INDEX](https://www.sqlite.org/lang_dropindex.html)   
[Indexes On Expressions](https://www.sqlite.org/expridx.html)   
[Partial Indexes](https://www.sqlite.org/partialindex.html)   



[\^myfootnote]: https://ko.wikipedia.org/wiki/%EC%9D%B8%EB%8D%B1%EC%8A%A4_(%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4)
