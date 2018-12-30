# Select, Project, Join

## Part 1

- 가장 단순한 쿼리: 릴레이션 이름
  filter, slice, combine하기 위해 연산자를 사용하자.
  연산자를 사용한 결과로 릴레이션이 생성된다.

- Select 연산자: 특정 행(row)을 선택
  Selection 연산자인 Sigma(시그마) 기호 (𝛔)와, 특정 행을 선택할 조건을 적는다.

  - *Students with GPA > 3.7*
    $$
    \sigma_{GPA\gt3.7} Student
    $$
    Student 릴레이션의 부분집합이 리턴된다.

  - *Students with GPA > 3.7 and HS < 1000*
    $$
    \sigma_{GPA\gt3.7} \wedge_{HS\lt1000} Student
    $$

  - *Applications to Stanford CS major*
    $$
    \sigma_{cName=`stanford` \wedge major=`cs`} Apply
    $$
    Apply 릴레이션의 부분집합이 리턴된다.



  즉, 
  $$
  \sigma_{condition} Relation
  $$
  과 같이 적는다.

- Project 연산자: 특정 열(column)을 선택
  Projection 연산자인 Pi(파이) 기호(𝚷)와, 추출하고자 하는 칼럼의 이름을 적는다.

  - *ID and decision of all applications*
    $$
    \Pi_{sID, decision} Apply
    $$


  즉, 
  $$
  \Pi_{A_1, A_2, ..., A_n} Relation
  $$
  과 같이 적는다.



- 행과 열을 둘다 선택하려면?

  - *ID and name of students with GPA>3.7*
    $$
    \Pi_{sID, sName}(\sigma_{GPA\gt3.7} Student)
    $$




### Q1

A. 두 개의 Selection 연산자를 조합하는 것은 유용하다. (참/거짓)
$$
ex)\quad \sigma_{state=`CA`}(\sigma_{enr\gt8000} College)
$$
B. 두 개의 Projection 연산자를 조합하는 것은 유용하다. (참/거짓)
$$
ex) \quad \pi_{GPA}(\pi_{sID,GPA,HS}Student)
$$
정답: <u>A, B 모두 거짓</u>
설명: 어떤 행에서 두 개의 selection 연산자는, 각 두 조건을 'and'로 묶은 한 개의 selection 연산자로 항상 대체될 수 있다. 만약 어떤 행에 두 개의 projection 연산자가 있다면, 두번째(outer) projection의 속성들은 첫번째(inner) projection의 부분집합일 것이다. 그러므로, 첫번째 projection을 지워도 결과는 동일하다.
(Two selection operators in a row can always be replaced by a single selection operator whose condition is the "and" of the two selection conditions. If there are two projection operators in a row, the attribute list of the second (outer) projection must be a subset of the attribute list of the first (inner) projection. Thus, the first projection can be removed without changing the result of the expression.)





## Part 2

Duplicates
: List of application majors and decisions(Y/N)

The semantics of relational algebra says that duplicates are always eliminated.

- SQL: multisets, bags(중복이 제거되지 않는다)
- Relational Algebra: sets(중복이 제거된다)



- Cross-product: 2개의 릴레이션을 결합
  (a.k.a. Cartesian product)
  : 2개의 릴레이션을 결합한다. 결과값의 스키마는 두 릴레이션의 스키마의 union과 같고, 결과 테이블은 두 릴레이션의 튜블의 모든 조합과 같다. 

$$
ex) \quad Student\times Apply
$$

Student

| sID  | sName | GPA  | HS   |
| ---- | ----- | ---- | ---- |
|      |       |      |      |
|      |       |      |      |
|      |       |      |      |
|      |       |      |      |

Apply

| sID  | cName | major | dec  |
| ---- | ----- | ----- | ---- |
|      |       |       |      |
|      |       |       |      |
|      |       |       |      |
|      |       |       |      |

일 때, 결과 릴레이션은 8개의 속성(attribute)을 가진다. 두 릴레이션 모두 sID라는 같은 이름의 속성이 있는데, 결과 릴레이션에서는 각각 Student.sID, Apply.sID로 표기된다.

*Names and GPAs of students with HS>1000 who applied to CS and were rejected*
$$
\Pi_{sName, GPA}(\sigma_{Student.sID = Apply.sID \hspace{0.2cm} \wedge \hspace{0.2cm} HS\gt1000 \hspace{0.2cm} \wedge \hspace{0.2cm} major=`CS` \hspace{0.2cm} \wedge \hspace{0.2cm} dec = `R`}(Student \times Apply))
$$


### Q2

다음 수식들 중 HS>1000, CS에 지원, reject된 학생들의 name과 GPA를 리턴하는 것이 아닌 것은?
$$
\pi_{sName, GPA}(\sigma_{Student.sID=Apply.sID}(\sigma_{HS\gt1000}(Student) \times \sigma_{major=`CS` \wedge dec=`R`}(Apply)))
$$

$$
\pi_{sName, GPA}(\sigma_{Student.sID=Apply.sID \wedge HS \gt 1000 \wedge major=`CS` \wedge dec=`R`}(Student \times \pi_{sID, major, dec}Apply))
$$

$$
\sigma_{Student.sID=Apply.sID}(\pi_{sName, GPA}(\sigma_{HS \gt 1000}Student \times \sigma_{major=`CS` \wedge dec=`R`}Apply))
$$



정답: <u>3번</u>
설명: 처음 2개의 수식은 영상에 나온 쿼리와 동일하다. 3번째 수식은 유효하지 않은데, sID 속성이 조건 Student.sID = Apply.sID가 적용된 식의 결과에 포함되어 있지 않기 때문이다.
(The first two expressions are equivalent to the expression given in the video for this query. The third expression is invalid because the sID attributes are not included in the result of the expression to which condition Student.sID=Apply.sID is applied.Submit)





## Part 3

#### Natural Join (⨝)

- 이름이 같은 모든 속성에 동일성을 부여한다.
- 중복된 속성들은 하나만 남기고 제거한다.

Names and GPAs of students with HS>1000 who applied to CS at college with enr>20,000 and were rejected
$$
\Pi_{sName, GPA}(\pi_{HS\gt1000\hspace{0.2cm}\wedge\hspace{0.2cm}major=`CS`\hspace{0.2cm}\wedge\hspace{0.2cm}dec=`R`\hspace{0.2cm}\wedge\hspace{0.2cm}enr > 20,000}(Student\bowtie(Apply\bowtie College)))
$$
Natural join이므로 'Student.sID=Apply.sID' 조건이 필요 없다.

natural join이 엄청난 힘을 추가로 가져다 주는 것은 아니다. cross-product로 natural join을 다시 쓸 수 있기는 하다.
$$
Exp_1 \bowtie Exp_2
\equiv \Pi_{schema(E_1) \cup schema(E_2)}(\sigma_{E_1A_1=E_2A_1 \wedge E_1A_2=E_2A_2\wedge...}(Exp_1 \times Exp_2))
$$


### Q3

$$
\pi_{sName, cName}(\sigma_{HS \gt enr}(\sigma_{state=`CA`}College \Join Student \Join \sigma_{major=`CS`}Apply))
$$

위 문장의 결과를 올바르게 묘사한 것은?

1. All student-college name pairs, where the student is applying to major in CS at the college, the college is in California, and the college is smaller than some high school
2. Students paired with all California colleges to which the student applied to major in CS, where at least one of those colleges is smaller than the student's high school
3. Students paired with all colleges smaller than the student's high school to which the student applied to major in CS, where at least one of those colleges is in California
4. Students paired with all California colleges smaller than the student's high school to which the student applied to major in CS

정답: <u>4</u>
설명: inner natural join이 학생들과 학생들이 지원한 대학(단 캘리포니아의 CS 전공에 한해)을 연결시킨다. outer selection 조건은 HS가 대학보다 큰 경우를 제외하고 모두 필터링하며, 마지막 projection이 학생과 대학 이름을 유지한다.
(The inner natural join connects students with the colleges to which they've applied, allowing only California colleges and CS-major applicaitons. The outer selection condition filters out all applications except those where the high school is bigger than the college, and the final projection keeps the student and college names.)



### Q4

Which of the following expressions finds the IDs of all students such that some college bears the student's name?
$$
\pi_{sID}(College \Join Student)
$$

$$
\pi_{sID}(\sigma_{cName=sName}(College \times Student))
$$

$$
\pi_{sID}(\pi_{cName}College \Join \pi_{cName}(\sigma_{sName=cName}Student))
$$

$$
\pi_{sID}(\sigma_{cName=sName}(\pi_{sID}Student \times College \times Student)
$$



정답: <u>2번</u>
설명: 1번은 데이터베이스에 있는 모든 학생들의 ID를 리턴한다. 3번은 cName이 Student의 속성이 아니므로 유효하지 않다. 4번은 Student의 모든 ID들을 리턴한다.





## Part 4

### Theta join

$$
Exp_1 \bowtie_{\theta} Exp_2 = \sigma_\theta(Exp_1 \times Exp_2)
$$

중간의 theta 기호는 condition을 나타낸다. 

- 많은 DBMS에서 theta join을 릴레이션 조합 시 기본 연산으로 실행한다. 기본 연산이란, 2개의 릴레이션의 모든 튜플들을 조합하고, theta condition을 통과한 조합만 유지하는 것이다.
- "join"이라는 용어는 대개 theta join을 의미한다.





## 요약

릴레이션들의 집합에 대한 Query(수식)은 그 결과로 릴레이션을 만든다.

- 가장 단순한 쿼리: 릴레이션 이름
- filter, slice, combine하기 위해 연산자를 사용
- 지금까지 배운 연산자: select, project, cross-product, natural join, theta join