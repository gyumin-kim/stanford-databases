# The Relational Model

Relational Model은 DBMS의 근간이다. 근래 상업적인 데이터베이스 시스템에 널리 퍼져있다. 

주요 개념(construct)은 **relation**(릴레이션; 때때로 **table**이라고도 불림)이다. 데이터베이스는 릴레이션의 집합으로 구성되어 있으며, 각각의 릴레이션은 이름을 갖는다. 
각 릴레이션은 **attribute**(속성), 혹은 **column**(컬럼)의 집합을 갖는다.
각 **tuple**(혹은 **row**)은 각각의 attribute에 대한 값을 갖는다.
각 attribute는 **type**(혹은 **domain**)을 갖는다. 대부분의 RDB에서는 'enumerated domain'이라는 개념이 있는데, 예를 들어 'college' table의 'state' 컬럼은 50개 주의 약어를 도메인으로 가질 것이다.

`Student` 테이블

|  ID  | name  | GPA  | Photo |
| :--: | :---: | :--: | :---: |
| 123  |  Amy  | 3.9  |   😄   |
| 234  |  Bob  | 3.4  | NULL  |
| 345  | Craig | NULL |   😐   |
| ...  |  ...  | ...  |  ...  |

`College` 테이블

|   name   | state | enrollment |
| :------: | :---: | :--------: |
| Stanford |  CA   |   15,000   |
| Berkeley |  CA   |   36,000   |
|   MIT    |  MA   |   10,000   |
|   ...    |  ...  |    ...     |

데이터베이스의 **Schema**(스키마)는 릴레이션의 구조다. 릴레이션의 이름, 릴레이션이 가진 속성, 속성의 타입 등을 포함한다.
**Instance**(인스턴스)는 특정 시점에 실제 존재하는 내용을 의미한다.

**NULL**은 RDB에서 꽤 중요한데, 특정 값이 unknown이거나 undefined라는 의미를 나타내는 특별한 값이다. NULL값은 유용하지만, NULL값을 갖는 릴레이션들에 대해 쿼리를 수행할 때 주의해야 한다.
**Key**는 모든 값이 unique한 속성이다. 예를 들어 `student` 릴레이션에서, ID는 key라고 할 수 있다. `college` 릴레이션에서 name 속성이 key라고 할 수도 있지만, 전세계에 걸쳐 이름이 같은 대학이 있을 수 있으므로 key가 아닐 수도 있다. 따라서 이 경우에는 name과 state를 묶어 하나의 key라고 하면 될 것이다. key는 여러 용도로 사용되는데, 그 중 하나는 특정 tuple을 인식하기 위해 필요하다. 그리고 RDB는 효율성을 위해 특별한 index를 만들거나 특정한 방법으로 데이터베이스를 저장하는 경향이 있는데, 이 때 key에 근거해 tuple을 찾는 것이 빠른 방법이다. 마지막으로, 어떤 릴레이션에서 다른 릴레이션의 tuple을 참조하고자 할 때, RDB에는 포인터의 개념이 없으므로, 참조하는 릴레이션의 unique key를 바탕으로 참조하게 된다.



## Creating relations (tables) in SQL

```sql
Create Table Student(ID, name, GPA, photo)
```

```sql
Create Table college(name string, state char(2), enrollment integer)
```

