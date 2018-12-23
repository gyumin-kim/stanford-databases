# Introduction

## 관계형 데이터베이스는 왜 널리 사용되는가?

Database Management System(DBMS)는 다음을 제공한다.

> 효율적이고, 믿을 수 있고, 편리하며, 안전한 멀티유저 저장소, 그리고 대용량 영구 데이터에 대한 접근

- Massive

  메모리로는 전부 다룰 수 없는 많은 양의 데이터를 다루기 위함

- Persistent

  DBMS에 의해 관리되는 데이터는 영구적이다. 즉 데이터베이스에 있는 데이터가 그 데이터를 실행하는 프로그램보다 더 수명이 길다. 같은 데이터베이스를 여러 프로그램에서 사용한다.

- Safe

  데이터들이 일관된 상태로 유지되며, 어떤 failure가 발생하더라도 데이터가 손실되거나 덮어씌워지지 않도록 해야 한다. failure의 종류에는 hardware, software, power, (malicious)users 등이 있다.

- Multi-user

  여러 프로그램이나 사용자가 데이터베이스를 사용할 수 있다.

  Concurrency control. 

- Convenient

  Physical Data independence: 디스크에 물리적으로 어떤 구조로 데이터가 저장되어 있는지는 프로그램이 데이터의 구조를 생각하는 방법과는 독립적이다(무관하다). 즉 물리적으로 어떻게 데이터가 저장되어있는지, 그 방법이 바뀐다 해도 프로그램이 데이터를 사용하는 방법은 바뀌지 않는다.

  High-level query language(declarative): 데이터베이스에서 무엇을 원하는지만 써주면 된다. 데이터를 꺼내기 위해 어떤 알고리즘이 필요한지는 쓸 필요 없다.

- Effiecient

  데이터베이스 시스템에서는 성능이 매우 중요하다. 초당 1,000회의 쿼리/업데이트를 수행한다.

- Reliable

  99.99999% 신뢰할 수 있어야 한다.



## Key concepts

- Data model

  데이터가 어떻게 구조화되는지 설명한 것.

  예를 들어, 관계형 데이터베이스에서는 데이터와 데이터베이스가 일련의 레코드 집합으로 간주된다. 또는 XML은 계층형 구조로 데이터를 저장한다. 혹은 그래프 데이터 모델도 있다.

  즉 데이터 모델은 데이터베이스에 저장될 데이터의 일반적인 형태를 말해준다.

- Schema versus data

  프로그래밍 언어로 치면 스키마는 타입, 데이터는 변수라고 생각할 수 있다. 스키마는 데이터베이스의 구조를 세우고(학번, 학점, 학교 등), 데이터는 스키마 내부에 존재하는 실제 데이터다. 대개 스키마는 초반부에 정의되어 잘 바뀌지 않는다.

- Data definition language (DDL)

  데이터베이스의 스키마를 정의하기 위해 사용한다.

- Data manipulation or query language (DML)

  스키마가 정의되고 데이터가 load되면, 데이터를 querying하고 수정하는 것이 가능한데, 이를 DML을 통해 수행한다.



## Key people that are involved in a database system

- DBMS implementer

  DBMS를 실행하는 사람. 즉 시스템을 빌드하는 사람

- Database designer

  데이터베이스의 스키마를 만드는 사람. 애플리케이션에 복잡한 데이터들이 연관될 경우, 매우 어려운 일이 된다.

- Database application developer

  스키마가 정의되면, 해당 데이터베이스를 사용하는 프로그램을 개발하는 사람.

- Database administrator

  데이터를 load하고, 데이터가 원활하게 실행될 수 있도록 관리하는 사람. 대용량 데이터베이스 애플리케이션에서 매우 중요한 역할을 담당한다. 튜닝 매개변수가 데이터베이스 시스템의 성능에 있어 매우 중요한 변수가 된다.

이 강좌에서는 데이터베이스의 design과 manipulation을 주로 다루며, administration에 대해서도 일부 다룬다.