# Querying Relational Databases

## Steps in creating and using a (relational) database

1. 스키마 디자인하기 (DDL 이용하여 create)

   스키마는 릴레이션의 구조와 그 릴레이션들 각각의 속성으로 구성되어 있다.

2. 초기 데이터를 데이터베이스에 load하기

   데이터가 load되면, 릴레이션이 튜플들을 갖게 된다.

3. 반복: 쿼리 수행 & 수정

   데이터베이스가 존재하는 한 계속된다.



## Ad-hoc queries in high-level language

- Stanford와 MIT에 지원한 학생들 중 GPA가 3.7보다 높은 모든 학생
- CA에 위치해 있으면서 지원자가 500명 미만인 모든 엔지니어링 학부
- accept rate가 지난 5년간 가장 높은 대학

- 일부는 쉽고, 어떤 것은 어렵다.
- 일부는 DBMS가 효율적으로 실행하기에 쉽고, 어떤 것은 어렵다.
- "Query language"는 데이터를 수정하는 데에도 사용된다.



## Queries return relations ("compositional", "closed")

- Compositionality: 이전 쿼리의 결과에 대해서도 쿼리를 수행할 수 있음



## Query Languages

- Relational Algebra

  formal language  
  이름으로 찾는다. 이론적인 근거가 탄탄하다.

  ![equation](https://latex.codecogs.com/svg.latex?%5CPi_%7BID%7D%5Csigma_%7BGPA%3E3.7%20%5Cwedge%20C.name%3D%22stanford%22%7D%28Student%20%5Cbowtie%20Apply%29)

- SQL

  actual/implemented language  
  실제 배포된 데이터베이스 애플리케이션에서 실행하게 된다.
  relational algebra를 기반으로 한다.

  ```sql
  Select Student.id
  From Student, Apply
  Where Student.ID=Apply.ID
  And GPA>3.7 and college='stanford'
  ```

SQL을 배우기 전 relational algebra를 먼저 배우는 것을 권장한다.













