# SQL Bolt

## 00. SQL 소개

### SQL 이란

SQL(Structured Query Language)은 기술 전문가와 비전문가 모두 관계형 데이터베이스에서 데이터를 조회, 조작 및 변환할 수 있도록 설계된 언어입니다.  
SQL 데이터베이스는 그 단순성 덕분에 수백만 개의 웹사이트와 모바일 애플리케이션에 안전하고 확장 가능한 저장소를 제공합니다.

---

### 관계형 데이터베이스

SQL 구문을 배우기 전에 관계형 데이터베이스가 실제로 무엇인지에 대한 개념을 이해하는 것이 중요합니다. 관계형 데이터베이스는 서로 관련된 (2차원) 테이블들의 모음입니다. 각 테이블은 엑셀 스프레드시트와 유사하게, 이름이 지정된 정해진 수의 열(테이블의 속성 또는 특성)과 수많은 행(데이터)으로 구성됩니다.

예를 들어, 차량등록국에 데이터베이스가 있다면, 그 데이터베이스에는 해당 주에 거주하는 사람들이 운전하는 모든 차량 정보가 담긴 표가 있을 수 있습니다. 이 표에는 각 차량의 모델명, 차종, 바퀴 수, 문 개수 등의 정보가 저장되어야 할 수도 있습니다.

| ID | 제조사/모델   | #바퀴 | #문 | 유형    |
|----|----------|-----|----|-------|
| 1  | 포드 포커스   | 4   | 4  | 의자 가마 |
| 2  | 테슬라 로드스터 | 4   | 2  | 스포츠   |
| 3  | 카와카시 닌자  | 2   | 0  | 오토바이  |
| 4  | 맥라렌 포뮬러1 | 4   | 0  | 경주    |
| 5  | 테슬라 s    | 4   | 4  | 의자 가마 |

이러한 데이터베이스에는 해당 주에 등록된 모든 운전자 목록, 발급 가능한 운전면허 종류, 심지어 각 운전자의 운전 위반 기록과 같은 정보를 포함하는 추가 관련 테이블이 있을 수 있습니다.

SQL을 배우는 목적은 "도로를 달리는 차량 중 바퀴가 네 개 미만인 차량은 어떤 종류가 있을까요?" 또는 "테슬라는 몇 가지 모델의 자동차를 생산할까요?" 와 같이 데이터에 대한 구체적인 질문에 답하는 방법을 익혀 향후 더 나은 의사결정을 내리는 데 도움을 주는 것입니다.

---

## 01. SELECT 쿼리 기초

SQL 데이터베이스에서 데이터를 조회하려면 SELECT 문을 작성해야 하며, 이를 구어체로 쿼리라고 부르곤 합니다. 쿼리 자체는 우리가 찾고자 하는 데이터가 무엇인지, 데이터베이스의 어느 위치에서 찾을지, 그리고 선택적으로 데이터를 반환하기 전에 어떻게 변환할지를 선언하는 문장입니다. 하지만 쿼리는 특정 구문을 가지고 있으며, 이어지는 실습에서 배울 내용이 바로 이것입니다.

SQL의 테이블은 하나의 엔티티(예: 개) 유형으로 생각할 수 있고, 테이블의 각 행은 해당 유형의 특정 인스턴스(예: 퍼그, 비글, 다른 색상의 퍼그 등)로 생각할 수 있습니다. 이는 열이 해당 엔티티의 모든 인스턴스가 공유하는 공통 속성(예: 털 색상, 꼬리 길이 등)을 나타냄을 의미합니다.

데이터 테이블이 주어졌을 때 우리가 작성할 수 있는 가장 기본적인 쿼리는, 모든 행(인스턴스)에서 테이블의 몇 개 열(속성)을 선택하여 조회하는 쿼리입니다.

```
특정 열을 조회하는 SELECT 쿼리
SELECT column, another_column, …
FROM mytable;
```

```
모든 열을 조회하는 SELECT 쿼리
SELECT * 
FROM mytable;
```

---

## 02. 제약 조건을 사용한 쿼리 (1부)

이제 테이블에서 특정 열의 데이터를 선택하는 방법을 배웠습니다. 하지만 1억 개의 데이터 행이 있는 테이블이 있다면, 모든 행을 읽는 것은 비효율적이며 심지어 불가능할 수도 있습니다.

특정 결과가 반환되지 않도록 필터링하려면 쿼리에 WHERE 절을 사용해야 합니다. 이 절은 데이터의 각 행에 적용되어 특정 컬럼 값을 검사하고, 해당 행을 결과에 포함할지 여부를 결정합니다.

```
제약 조건을 포함한 SELECT 쿼리
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```

여러 개의 AND 또는 OR 논리 키워드를 연결하여 더 복잡한 조건절을 구성할 수 있습니다 (예: num_wheels >= 4 AND doors <= 2). 아래는 수치 데이터(정수 또는 부동 소수점)에 사용할 수 있는 유용한 연산자들입니다.

| 연산자 (Operator)                  | 조건 (Condition)                    | SQL 예시 (SQL Example)            |
|:--------------------------------|:----------------------------------|:--------------------------------|
| `=`, `!=`, `<`, `<=`, `>`, `>=` | 일반적인 숫자 비교 연산자                    | `col_name != 4`                 |
| `BETWEEN ... AND ...`           | 두 값 범위 내에 값이 존재하는 경우 (양 끝값 포함)    | `col_name BETWEEN 1.5 AND 10.5` |
| `NOT BETWEEN ... AND ...`       | 두 값 범위 내에 값이 존재하지 않는 경우 (양 끝값 포함) | `col_name NOT BETWEEN 1 AND 10` |
| `IN (...)`                      | 지정한 리스트(목록)에 값이 존재하는 경우           | `col_name IN (2, 4, 6)`         |
| `NOT IN (...)`                  | 지정한 리스트(목록)에 값이 존재하지 않는 경우        | `col_name NOT IN (1, 3, 5)`     |

---

## 03. 제약 조건을 사용한 쿼리 (2부)

| 연산자 (Operator) | 조건 (Condition)                                   | 예시 (Example)                                                   |
|:---------------|:-------------------------------------------------|:---------------------------------------------------------------|
| `=`            | 대소문자를 구분하는 정확한 문자열 비교 (`=`을 하나만 사용함에 주의)         | `col_name = "abc"`                                             |
| `!=` 또는 `<>`   | 대소문자를 구분하는 정확한 불일치 비교                            | `col_name != "abcd"`                                           |
| `LIKE`         | 대소문자를 구분하지 않는 문자열 비교                             | `col_name LIKE "ABC"`                                          |
| `NOT LIKE`     | 대소문자를 구분하지 않는 불일치 비교                             | `col_name NOT LIKE "ABCD"`                                     |
| `%`            | 0개 이상의 모든 문자열과 매칭 (`LIKE` 또는 `NOT LIKE`와 함께 사용)  | `col_name LIKE "%AT%"`<br>("AT", "ATTIC", "CAT", "BATS" 등과 매칭) |
| `_`            | 정확히 1개의 임의의 문자와 매칭 (`LIKE` 또는 `NOT LIKE`와 함께 사용) | `col_name LIKE "AN_"`<br>("AND"와는 매칭되지만, "AN"과는 매칭되지 않음)       |
| `IN (...)`     | 문자열이 지정한 리스트(목록) 안에 존재하는 경우                      | `col_name IN ("A", "B", "C")`                                  |
| `NOT IN (...)` | 문자열이 지정한 리스트(목록) 안에 존재하지 않는 경우                   | `col_name NOT IN ("D", "E", "F")`                              |

---

## 04. 쿼리 결과 필터링 및 정렬

데이터베이스에 저장된 데이터는 고유할 수 있지만, 특정 쿼리의 실행 결과는 그렇지 않을 수 있습니다. 예를 들어 영화 테이블의 경우, 같은 해에 여러 영화가 개봉될 수 있습니다. 이러한 경우 SQL은 DISTINCT 키워드를 사용하여 중복된 열 값을 가진 행을 편리하게 제거하는 방법을 제공합니다.

```
중복을 제거한 결과를 조회하는 SELECT 쿼리
SELECT DISTINCT column, another_column, …
FROM mytable
WHERE condition(s);
```
---
결과 정렬하기

지난 몇 개 레슨에서 보았던 깔끔하게 정렬된 테이블과 달리, 실제 데이터베이스의 대부분 데이터는 특정 컬럼 순서 없이 추가됩니다. 그 결과 테이블의 크기가 수천 행, 수백만 행으로 커짐에 따라 쿼리 결과를 읽고 이해하는 것이 어려워질 수 있습니다.

이를 돕기 위해 SQL은 ORDER BY 절을 사용하여 지정된 컬럼을 기준으로 결과를 오름차순(ASC) 또는 내림차순(DESC)으로 정렬하는 방법을 제공합니다.
```
정렬된 결과를 조회하는 SELECT 쿼리
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC;
```
---
결과 개수 제한하기

ORDER BY 절과 함께 흔히 사용되는 또 다른 구문은 LIMIT과 OFFSET 절입니다. 이는 관심 있는 결과의 일부(부분 집합)만 데이터베이스에 요청하는 유용한 최적화 방법입니다. LIMIT은 반환할 행의 개수를 제한하며, 선택 사항인 OFFSET은 몇 번째 행부터 조회를 시작할지 지정합니다.

```
개수가 제한된 결과를 조회하는 SELECT 쿼리
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

---

## 05. JOIN을 사용한 다중 테이블 쿼리

### 데이터베이스 정규화

데이터베이스 정규화는 단일 테이블 내의 중복 데이터를 최소화하고 데이터베이스 내부의 데이터가 서로 독립적으로 확장될 수 있도록 해주기 때문에 유용합니다(예: 자동차 엔진 종류는 각 자동차 모델과 독립적으로 확장될 수 있음).  하지만 정규화의 단점으로는 데이터베이스의 여러 부분에서 데이터를 찾아야 하므로 쿼리가 다소 복잡해지고, 대규모 테이블을 많이 다룰 경우 성능 문제가 발생할 수 있다는 점입니다.

정규화된 데이터베이스에서 여러 테이블에 걸쳐 데이터가 분산되어 있는 엔티티에 대한 정보를 조회하려면, 모든 데이터를 결합하여 필요한 정보만 정확하게 추출할 수 있는 쿼리를 작성하는 방법을 배워야 합니다.

---

### JOIN을 이용한 다중 테이블 쿼리

단일 엔티티에 대한 정보를 공유하는 테이블들은 데이터베이스 전체에서 해당 엔티티를 고유하게 식별할 수 있는 기본키를 가져야 합니다. 흔히 사용되는 기본키 타입 중 하나는 자동 증가 정수(공간 효율적이기 때문)이지만, 고유하기만 하다면 문자열이나 해시값이 될 수도 있습니다.

쿼리에서 JOIN 절을 사용하면 이 고유한 키를 바탕으로 두 개 이상의 서로 다른 테이블에 있는 행 데이터를 결합할 수 있습니다. 가장 먼저 소개할 조인 방식은 INNER JOIN입니다.

```
여러 테이블에 대해 INNER JOIN을 사용하는 SELECT 쿼리
SELECT column, another_table_column, …
FROM mytable
INNER JOIN another_table 
    ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

INNER JOIN은 ON 조건절에 정의된 대로 첫 번째 테이블과 두 번째 테이블에서 동일한 키를 가진 행들을 매칭하여, 두 테이블의 컬럼을 결합한 결과 행을 생성하는 과정입니다. 테이블이 조인된 후에는 이전에 배운 다른 절(WHERE, ORDER BY 등)들이 적용됩니다.

---

## 06. 외부 조인

데이터를 어떻게 분석하느냐에 따라 지난 레슨에서 배운 INNER JOIN만으로는 부족할 수 있습니다.
결과 테이블에 두 테이블 모두에 존재하는 데이터만 포함되기 때문입니다.

데이터가 서로 다른 단계에서 입력되는 등 두 테이블의 데이터가 비대칭인 경우, 필요한 데이터가 결과에서 누락되지 않도록  
LEFT JOIN, RIGHT JOIN, 또는 FULL JOIN을 대신 사용해야 합니다.

```
여러 테이블에 대한 LEFT/RIGHT/FULL JOIN을 사용한 SELECT 쿼리
SELECT column, another_column, …
FROM mytable
INNER/LEFT/RIGHT/FULL JOIN another_table 
    ON mytable.id = another_table.matching_id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

INNER JOIN과 마찬가지로 이 세 가지 새로운 조인 역시 데이터를 결합할 기준 컬럼을 지정해야 합니다.  
테이블 A를 테이블 B에 조인할 때:

* LEFT JOIN: B에서 일치하는 행을 찾았는지 여부와 상관없이 A의 모든 행을 포함합니다.

* RIGHT JOIN: 반대로 작동하여, A에서 일치하는 행이 있는지와 상관없이 B의 모든 행을 유지합니다.

* FULL JOIN: 상대 테이블에 일치하는 행이 존재하는지 여부와 관계없이 두 테이블의 모든 행을 유지합니다.

이 새로운 조인들을 사용할 때는 결과에 생성되는 NULL값과 제약 조건을 처리하기 위한 추가 로직을 작성해야 하는 경우가 많습니다  
(이에 대해서는 다음 레슨에서 자세히 다룹니다).
---

## 07. NULL에 대한 간단한 설명

지난 레슨에서 약속했듯이, SQL 데이터베이스에서의 NULL 값에 대해 간단히 알아보겠습니다.  
데이터베이스에서 NULL 값의 가능성을 줄이는 것은 언제나 좋습니다. NULL 값은 쿼리를 작성할 때, 제약 조건을 설정할 때(특정 함수는 null 값에 대해 다르게 동작함),  
그리고 결과를 처리할 때 특별한 주의가 필요하기 때문입니다.

데이터베이스에서 NULL 값 대신 사용할 수 있는 대안은 숫자 데이터에는 0, 텍스트 데이터에는 빈 문자열('')과 같이 데이터 타입에  
적절한 기본값을 사용하는 것입니다. 하지만 기본값을 설정했을 때 추후 분석(예: 숫자 데이터의 평균을 구할 때)이  
왜곡될 수 있다면, 미완성된 데이터를 저장해야 하는 데이터베이스의 특성상 NULL 값을 사용하는 것이 적절할 수 있습니다.

또한 지난 레슨에서 비대칭 데이터를 가진 두 테이블을 외부 조인할 때 보았듯이,  
때로는 NULL 값을 피하는 것이 불가능할 수도 있습니다. 이러한 경우 WHERE 절에서 IS NULL 또는 IS NOT NULL 제약 조건을  
사용하여 컬럼의 NULL 여부를 검사할 수 있습니다.

```
NULL 값에 대한 제약 조건을 포함하는 SELECT 쿼리
SELECT column, another_column, …
FROM mytable
WHERE column IS/IS NOT NULL
AND/OR another_condition
AND/OR …;
```

---

## 08. 표현식을 활용한 쿼리

SQL에서는 단순히 테이블의 컬럼 데이터를 그대로 조회하고 참조하는 것 외에도, 표현식을 사용하여 컬럼 값에 더 복잡한 로직을 작성할 수 있습니다. 아래의 물리학 관련 예시처럼, 표현식은 기본 산술 연산과 함께 수학
및 문자열 함수를 사용하여 쿼리가 실행되는 시점에 값을 변환할 수 있습니다.

```
표현식을 사용한 예시 쿼리
SELECT particle_speed / 2.0 AS half_particle_speed
FROM physics_data
WHERE ABS(particle_position) * 10.0 > 500;
```

각 데이터베이스는 쿼리에서 사용할 수 있는 고유한 수학, 문자열, 날짜 함수들을 지원하며, 이는 각 데이터베이스의 공식 문서에서 확인할 수 있습니다.

표현식을 사용하면 시간을 절약하고 결과 데이터의 추가적인 후처리를 줄일 수 있지만, 쿼리가 읽기 어려워질 수도 있습니다. 따라서 쿼리의 SELECT 절에서 표현식을 사용할 때는 AS 키워드를 사용하여 직관적인 별칭(
alias)을 지정해 주는 것이 좋습니다.

```
표현식 별칭을 사용한 쿼리 선택
SELECT col_expression AS expr_description, …
FROM mytable;
```

표현식 외에도 일반 컬럼이나 테이블에도 별칭을 지정할 수 있으며, 이는 출력 결과를 쉽게 알아볼 수 있게 만들고 복잡한 쿼리를 단순화하는 데 도움을 줍니다.

```
열 이름과 테이블 이름에 별칭을 모두 사용한 쿼리 예시
SELECT column AS better_column_name, …
FROM a_long_widgets_table_name AS mywidgets
INNER JOIN widget_sales
  ON mywidgets.id = widget_sales.widget_id;
  ```

---

## 09. 집계 함수를 활용한 쿼리 (Part 1)

지난 레슨에서 소개한 단순 수식 외에도, SQL은 여러 행의 데이터를 그룹화하여 요약된 정보를 제공하는 집계 함수의 사용을 지원합니다. 여러분이 다뤄온 Pixar 데이터베이스를 사용하면, 집계 함수를 통해 "
Pixar가 제작한 영화는 총 몇 편인가?", "연도별로 가장 높은 수익을 올린 Pixar 영화는 무엇인가?"와 같은 질문에 답할 수 있습니다.

```
모든 행에 대한 집계 함수를 포함하는 SELECT 쿼리
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression;
```

별도의 그룹화를 지정하지 않으면, 각 집계 함수는 결과 행의 전체 집합을 대상으로 실행되어 단 하나의 값만 반환합니다. 그리고 일반 표현식과 마찬가지로 집계 함수에 별칭을 부여하면 결과를 훨씬 쉽게 읽고 처리할 수
있습니다.

---
일반적인 집계 함수

| Function                | Description                                                                                                           |
|:------------------------|-----------------------------------------------------------------------------------------------------------------------|
| COUNT(*), COUNT(column) | 컬럼명을 지정하지 않으면 그룹 내 전체 행의 개수를 셉니다. 컬럼명을 지정하면 해당 컬럼에서 NULL이 아닌 값의 행 개수만 셉니다.                                            |
| MIN(column)             | 그룹 내 모든 행 중에서 지정된 컬럼의 가장 작은 숫자 값을 찾습니다.                                                                               |
| MAX(column)             | 그룹 내 모든 행 중에서 지정된 컬럼의 가장 큰 숫자 값을 찾습니다.                                                                                |
| AVG(column)             | 그룹 내 모든 행 중에서 지정된 컬럼의 숫자 값 평균을 구합니다.                                                                                  |
| SUM(column)             | 그룹 내 모든 행 중에서 지정된 컬럼의 모든 숫자 값의 합을 구합니다.                                                                               |
| Docs                    | [MySQL][MySQLLink], [Postgres][PostgresLink], [SQLite][SQLiteLink], [Microsoft SQL Server][Microsoft SQL Server Link] |

[MySQLLink]:https://dev.mysql.com/doc/refman/5.6/en/group-by-functions.html

[PostgresLink]:https://www.postgresql.org/docs/9.4/functions-aggregate.html

[SQLiteLink]:https://www.sqlite.org/lang_aggfunc.html

[Microsoft SQL Server Link]:https://learn.microsoft.com/en-us/sql/t-sql/functions/aggregate-functions-transact-sql?view=sql-server-ver17&redirectedfrom=MSDN

---

그룹화된 집계 함수

전체 행을 대상으로 집계하는 것 외에도, 특정 데이터 그룹별로 집계 함수를 적용할 수도 있습니다(예: 코미디 영화 vs 액션 영화의 박스오피스 매출 비교). 이렇게 하면 GROUP BY 절에 의해 정의된 고유한
그룹의 개수만큼 결과가 생성됩니다.

```
그룹에 대한 집계 함수를 사용하는 SELECT 쿼리
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression
GROUP BY column;
```

이 GROUP BY절은 지정된 열에 동일한 값을 가진 행들을 그룹화하는 방식으로 작동합니다.

---

## 10. 집계 함수를 활용한 쿼리 (Part 2)

쿼리가 제법 복잡해지고 있지만, 이제 SELECT 쿼리의 중요한 구성 요소는 거의 다 다루었습니다. 한 가지 눈치채셨을 수도 있는 점은, GROUP BY 절이 (그룹화할 행들을 필터링하는) WHERE 절 이후에
실행된다면, 그룹화된 행들 자체는 정확히 어떻게 필터링하느냐는 것입니다.

다행히도 SQL에서는 GROUP BY 절과 함께 전용으로 사용되어, 결과 집합에서 그룹화된 행들을 필터링할 수 있도록 HAVING 절이라는 추가적인 구문을 제공합니다.

```
HAVING 제약 조건을 포함하는 SELECT 쿼리
SELECT group_by_column, AGG_FUNC(column_expression) AS aggregate_result_alias, …
FROM mytable
WHERE condition
GROUP BY column
HAVING group_condition;
```

HAVING 절의 제약 조건은 WHERE 절의 제약 조건과 동일한 방식으로 작성되며, 그룹화된 행에 적용됩니다. 우리가 다루는 예제 수준에서는 그다지 유용해 보이지 않을 수 있지만, 서로 다른 속성을 가진 수백만
행의 데이터를 떠올려보면, 데이터를 빠르게 이해하기 위해 이러한 추가 제약 조건을 적용하는 것이 필요한 경우가 많습니다.

## 11. 쿼리 실행 순서

쿼리의 모든 구성 요소를 파악했으므로, 이제 완성된 쿼리 안에서 각 요소들이 어떻게 맞물려 동작하는지 살펴보겠습니다.

```
전체 SELECT 쿼리
SELECT DISTINCT column, AGG_FUNC(column_or_expression), …
FROM mytable
    JOIN another_table
      ON mytable.column = another_table.column
    WHERE constraint_expression
    GROUP BY column
    HAVING constraint_expression
    ORDER BY column ASC/DESC
    LIMIT count OFFSET COUNT;
```

모든 쿼리는 데이터베이스에서 필요한 데이터를 찾는 것부터 시작하여, 가능한 한 빠르게 처리하고 이해할 수 있는 형태로 데이터를 필터링해 나갑니다. 쿼리의 각 부분은 순차적으로 실행되기 때문에, 어떤 결과를 어디서
접근할 수 있는지 이해하려면 실행 순서를 아는 것이 중요합니다.

---
쿼리 실행 순서

+ FROM 및 JOIN
    + 조회 대상이 되는 전체 작업 데이터 집합을 결정하기 위해 FROM 절과 이어지는 JOIN들이 가장 먼저 실행됩니다. 이 절에 포함된 서브쿼리도 이때 함께 실행되며, 조인되는 테이블들의 모든 컬럼과 행을
      포함하는 임시 테이블이 내부적으로 생성될 수 있습니다.
+ WHERE
    + 전체 작업 데이터 집합이 준비되면, 개별 행에 대해 1차 WHERE 제약 조건이 적용되고 조건에 맞지 않는 행들은 버려집니다. 각 제약 조건은 FROM 절에서 요청한 테이블의 컬럼에만 직접 접근할 수
      있습니다. SELECT 절에 정의된 별칭(Alias)은 아직 실행되지 않은 쿼리 부분의 수식에 의존할 수 있으므로 대부분의 데이터베이스에서 접근할 수 없습니다.
+ GROUP BY
    + WHERE 제약 조건을 적용하고 남은 행들은 GROUP BY 절에 지정된 컬럼의 공통 값을 기준으로 그룹화됩니다. 그룹화 결과 해당 컬럼의 고유한 값 개수만큼만 행이 남게 됩니다. 이는 쿼리에 집계 함수가
      있을 때만 사용해야 함을 의미합니다.
+ HAVING
    + 쿼리에 GROUP BY 절이 있는 경우, 그룹화된 행들에 HAVING 절의 제약 조건이 적용되며 조건에 맞지 않는 그룹화된 행들은 버려집니다. WHERE 절과 마찬가지로, 대부분의 데이터베이스에서 이 단계
      역시 별칭에 접근할 수 없습니다.
+ SELECT
    + SELECT 절에 있는 모든 표현식이 최종적으로 계산됩니다.
+ DISTINCT
    + 남은 행들 중에서 DISTINCT로 지정된 컬럼의 중복된 값을 가진 행들이 제거됩니다.
+ ORDER BY
    + ORDER BY 절에 정렬 순서가 지정되어 있다면, 데이터가 오름차순(ASC) 또는 내림차순(DESC)으로 정렬됩니다. SELECT 절의 모든 수식이 이미 계산된 상태이므로, 이 절에서는 별칭(Alias)
      을 참조할 수 있습니다.
+ LIMIT
    + 마지막으로 LIMIT과 OFFSET에 의해 지정된 범위를 벗어나는 행들이 버려지며, 쿼리가 반환할 최종 행 집합만 남게 됩니다.

FROM / JOIN ➔ WHERE ➔ GROUP BY ➔ HAVING ➔ SELECT ➔ DISTINCT ➔ ORDER BY ➔ LIMIT

---
결론

모든 쿼리가 위에서 나열한 모든 절을 가질 필요는 없습니다. 하지만 SQL이 유연한 이유 중 하나는 개발자와 데이터 분석가가 별도의 코드를 작성하지 않고도 위 절들을 사용하는 것만으로 데이터를 신속하게 다룰 수 있게
해주기 때문입니다.

## 12. 행 삽입

이제 SQL 스키마와 새로운 데이터를 추가하는 방법에 대해 배울 차례입니다.

---
스키마(Schema) 란?

앞서 우리는 데이터베이스의 테이블을 2차원 형태의 행과 열의 집합으로 설명했으며, 여기서 열은 속성을, 행은 테이블 내 엔티티의 인스턴스를 나타냅니다. SQL에서 데이터베이스 스키마는 각 테이블의 구조와 테이블의 각
컬럼이 포함할 수 있는 데이터 타입을 설명하는 것입니다.

```
예를 들어,  
Movies 테이블에서 Year 컬럼의 값은 정수여야 하고, Title 컬럼의 값은 문자열이어야 합니다.
```

이러한 고정된 구조 덕분에 데이터베이스는 수백만, 심지어 수십억 개의 행을 저장하더라도 효율적이고 일관성을 유지할 수 있습니다.

---

새로운 데이터 삽입하기

데이터베이스에 데이터를 삽입할 때는 INSERT 문을 사용해야 합니다. 이는 어떤 테이블에 데이터를 쓸지, 어떤 데이터 컬럼을 채울지, 그리고 삽입할 하나 이상의 데이터 행을 선언합니다. 일반적으로 삽입하는 각
데이터 행에는 테이블의 모든 해당 컬럼에 매칭되는 값이 포함되어야 합니다. 순서대로 나열하기만 하면 한 번에 여러 행을 삽입할 수도 있습니다.

```
모든 열에 값을 포함하는 삽입문
INSERT INTO mytable
VALUES (value_or_expr, another_value_or_expr, …),
       (value_or_expr_2, another_value_or_expr_2, …),
       …;
```

데이터가 전부 있지 않고 테이블에 기본값을 지원하는 컬럼이 있는 경우, 가지고 있는 데이터의 컬럼만 명시적으로 지정하여 해당 행을 삽입할 수 있습니다.

```
특정 열을 포함하는 삽입문
INSERT INTO mytable
(column, another_column, …)
VALUES (value_or_expr, another_value_or_expr, …),
      (value_or_expr_2, another_value_or_expr_2, …),
      …;
```

이 경우, 입력하는 값의 개수는 지정된 컬럼의 개수와 일치해야 합니다. 작성하기에 코드가 더 길어지기는 하지만, 이런 방식으로 값을 삽입하면 상위 호환성을 가질 수 있다는 장점이 있습니다. 예를 들어, 테이블에
기본값을 가진 새로운 컬럼이 추가되더라도 기존에 작성해 둔 하드코딩된 INSERT 문을 수정할 필요가 없습니다.

또한, 삽입하는 값에 수학 및 문자열 수식을 사용할 수 있습니다. 이는 삽입되는 모든 데이터가 특정 형식으로 지정되도록 보장할 때 유용할 수 있습니다.

```
예시: 표현식을 사용한 INSERT 문
INSERT INTO boxoffice
(movie_id, rating, sales_in_millions)
VALUES (1, 9.9, 283742034 / 1000000);
```

## 13. 행 업데이트

새로운 데이터를 추가하는 것 외에도, 기존 데이터를 수정하는 것은 흔히 이루어지는 작업입니다. 이는 UPDATE 문을 사용하여 수행할 수 있습니다. INSERT 문과 유사하게, 수정할 정확한 테이블, 컬럼, 그리고
행을 지정해야 합니다. 또한 수정하려는 데이터는 테이블 스키마의 컬럼 데이터 타입과 일치해야 합니다.

```
값을 사용하여 문장을 업데이트합니다.
UPDATE mytable
SET column = value_or_expr, 
    other_column = another_value_or_expr, 
    …
WHERE condition;
```

이 구문은 여러 개의 컬럼/값 쌍을 받아서 WHERE 절의 제약 조건을 만족하는 모든 행에 해당 변경 사항을 적용하는 방식으로 동작합니다.

---
주의할 점

SQL을 다루는 대부분의 사람들은 언젠가 한 번쯤 데이터 수정에서 실수하게 됩니다. 운영 데이터베이스에서 잘못된 행들을 수정하거나, 실수로 WHERE 절을 누락하여 (모든 행에 수정이 적용되는) 문제를 일으키는
것이든, UPDATE 문을 작성할 때는 각별히 주의해야 합니다.

유용한 팁 하나는, 먼저 제약 조건을 작성한 뒤 이를 SELECT 쿼리로 테스트하여 정확한 행을 수정하려 하는지 확인한 다음, 수정할 컬럼/값 쌍을 작성하는 것입니다.

## 14. 행 삭제

데이터베이스의 테이블에서 데이터를 삭제해야 할 때는 DELETE 문을 사용할 수 있습니다. DELETE 문은 데이터를 작업할 테이블을 지정하고, WHERE 절을 통해 삭제할 테이블의 행을 정의합니다.

```
조건을 포함한 삭제문
DELETE FROM mytable
WHERE condition;
```

WHERE 제약 조건을 생략하기로 결정하면 모든 행이 삭제되는데, 이는 (의도한 것이라면) 테이블을 완전히 비우는 빠르고 쉬운 방법입니다.

---
각별한 주의사항

지난 레슨의 UPDATE 문과 마찬가지로, 먼저 SELECT 쿼리로 제약 조건을 실행하여 정확한 행을 삭제하는 것인지 확인하는 것이 좋습니다. 적절한 백업이나 테스트 데이터베이스가 없다면 데이터를 되돌릴 수 없이
삭제해 버리기 너무나 쉬우므로, 항상 DELETE 문은 두 번 읽고 한 번 실행하세요.

## 15. 테이블 생성

데이터베이스에 저장할 새로운 엔티티와 관계가 생기면 CREATE TABLE 문을 사용하여 새 데이터베이스 테이블을 생성할 수 있습니다.

```
테이블 생성 구문 (선택적 테이블 제약 조건 및 기본값 포함)
CREATE TABLE IF NOT EXISTS mytable (
    column DataType TableConstraint DEFAULT default_value,
    another_column DataType TableConstraint DEFAULT default_value,
    …
);
```

새 테이블의 구조는 일련의 컬럼들을 정의하는 테이블 스키마에 의해 결정됩니다. 각 컬럼은 이름, 해당 컬럼에 허용되는 데이터 타입, 삽입되는 값에 대한 선택적 테이블 제약
조건, 그리고 선택적 기본값을 가집니다.

동일한 이름의 테이블이 이미 존재하는 경우 SQL 구현체는 일반적으로 에러를 발생시킵니다. 따라서 이미 테이블이 존재하는 경우 에러를 방지하고 생성 과정을 건너뛰려면 IF NOT EXISTS 절을 사용할 수
있습니다.

---
테이블 데이터 타입

데이터베이스마다 지원하는 데이터 타입이 다르지만, 일반적인 타입은 숫자, 문자열, 그리고 날짜, 불리언, 바이너리 데이터와 같은 기타 다양한 형태를 지원합니다. 실제 코드에서 사용할 수 있는 몇 가지 예시입니다.

| 데이터 타입                                               | 설명                                                                                                                                                                                                                                  |
|:-----------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| INTEGER , BOOLEAN                                    | 정수 타입은 수량이나 나이와 같은 정수 값을 저장할 수 있습니다. 일부 DB 구현체에서 불리언 값은 단순 0 또는 1의 정수 값으로 표현됩니다.                                                                                                                                                    |
| FLOAT , DOUBLE , REAL                                | 부동 소수점 데이터 타입은 측정값이나 소수점 값과 같이 더 정밀한 숫자 데이터를 저장할 수 있습니다. 해당 값에 필요한 부동 소수점 정밀도에 따라 다른 타입을 사용할 수 있습니다.                                                                                                                                |
| CHARACTER(num_chars) ,<br/>VARCHAR(num_chars) , TEXT | 텍스트 기반 데이터 타입은 모든 종류의 로케일 문자열과 텍스트를 저장할 수 있습니다. 각 타입 간의 차이는 일반적으로 이 컬럼들을 다룰 때 데이터베이스의 내부적인 효율성 차이입니다. <br> <br>CHARACTER와 VARCHAR(가변 길이 문자) 타입 모두 저장할 수 있는 최대 문자 수를 지정합니다(길이가 넘어가면 잘릴 수 있음). 따라서 대용량 테이블에서 저장 및 조회 시 더 효율적일 수 있습니다. |
| DATE , DATETIME                                      | SQL은 시계열 데이터 및 이벤트 데이터를 추적하기 위해 날짜와 타임스탬프도 저장할 수 있습니다. 특히 여러 타임존에 걸쳐 데이터를 다룰 때 처리하기 까다로울 수 있습니다.                                                                                                                                    |
| BLOB                                                 | 마지막으로, SQL은 데이터베이스 내에 바이너리 데이터를 BLOB 형태로 직접 저장할 수 있습니다. 이 값들은 데이터베이스 입장에서 내부 구조를 알 수 없는 형태인 경우가 많으므로, 다시 조회하려면 올바른 메타데이터와 함께 저장해야 합니다.                                                                                              |
| Docs                    | [MySQL][MySQLLink], [Postgres][PostgresLink], [SQLite][SQLiteLink], [Microsoft SQL Server][Microsoft SQL Server Link]                                                                                                               |

[MySQLLink]:https://dev.mysql.com/doc/refman/5.6/en/group-by-functions.html
[PostgresLink]:https://www.postgresql.org/docs/9.4/datatype.html
[SQLiteLink]:https://www.sqlite.org/datatype3.html
[Microsoft SQL Server Link]:https://learn.microsoft.com/en-us/sql/t-sql/data-types/data-types-transact-sql?view=sql-server-ver17&redirectedfrom=MSDN

---
테이블 제약 조건

이번 레슨에서 테이블 제약 조건을 너무 깊게 다루지는 않지만, 각 컬럼에는 해당 컬럼에 삽입될 수 있는 값을 제한하는 추가적인 제약 조건을 설정할 수 있습니다. 전체 목록은 아니지만 유용하게 쓰이는 몇 가지 공통 제약 조건입니다.

| 제약 조건 | 설명                                                                                                   |
|:------|------------------------------------------------------------------------------------------------------|
| PRIMARY KEY  | 이 컬럼의 값이 고유하며, 각 값이 이 테이블의 단일 행을 식별하는 데 사용됨을 의미합니다.                                                  |
| AUTOINCREMENT | 정수 값의 경우, 행이 삽입될 때마다 값이 자동으로 채워지고 1씩 증가함을 의미합니다. 모든 데이터베이스에서 지원되지는 않습니다.                             |
| UNIQUE| 이 컬럼의 값이 고유해야 하므로, 테이블의 다른 행에 동일한 값을 가진 행을 삽입할 수 없음을 의미합니다. 테이블 행의 키일 필요는 없다는 점에서 PRIMARY KEY와 다릅니다. |
| NOT NULL| 삽입되는 값이 NULL일 수 없음을 의미합니다.                                                                           |
| CHECK (expression)| 더 복잡한 수식을 실행하여 삽입된 값이 유효한지 테스트할 수 있습니다. 예를 들어 값이 양수인지, 특정 크기보다 큰지, 특정 접두사로 시작하는지 등을 검사할 수 있습니다.      |
| FOREIGN KEY| 이 컬럼의 각 값이 다른 테이블 컬럼의 값과 일치하는지 확인하는 일관성 검사입니다.<br><br> 예를 들어 ID별로 모든 직원을 나열하는 테이블과 급여 정보를 나열하는 또 다른 테이블이 있는 경우, FOREIGN KEY를 통해 급여 테이블의 모든 행이 마스터 직원 목록의 유효한 직원과 일치하도록 보장할 수 있습니다.                                              |

---
예시

지금까지 레슨에서 사용해 온 Movies 테이블의 스키마 예시입니다.
```
영화 테이블 스키마
CREATE TABLE movies (
    id INTEGER PRIMARY KEY,
    title TEXT,
    director TEXT,
    year INTEGER, 
    length_minutes INTEGER
);
```

## 16. 테이블 수정
시간이 지나 데이터가 변경됨에 따라, SQL은 ALTER TABLE 문을 사용하여 컬럼 및 테이블 제약 조건을 추가, 제거 또는 수정함으로써 해당 테이블과 데이터베이스 스키마를 업데이트할 수 있는 방법을 제공합니다.

---
칼럼 추가하기

새 컬럼을 추가하는 구문은 CREATE TABLE 문에서 테이블을 만들 때 컬럼을 정의하는 문법과 유사합니다. 기존 행과 새 행 모두에 적용될 컬럼의 데이터 타입과 함께 필요한 테이블 제약 조건, 기본값을 지정해야 합니다. 표준 기능은 아니지만 MySQL과 같은 일부 데이터베이스에서는 FIRST 또는 AFTER 절을 사용하여 새 컬럼을 어디에 삽입할지 위치를 지정할 수도 있습니다.
```
테이블에 새 열을 추가하는 방법
ALTER TABLE mytable
ADD column DataType OptionalTableConstraint 
    DEFAULT default_value;
```
---
컬럼 제거하기

컬럼을 삭제하는 것은 삭제할 컬럼을 지정하기만 하면 되므로 매우 간단합니다. 하지만 일부 데이터베이스(SQLite 포함)는 이 기능을 지원하지 않습니다. 이러한 경우에는 새 테이블을 생성한 뒤 데이터를 이전해야 할 수도 있습니다.
```
테이블에서 열을 제거하도록 수정합니다.
ALTER TABLE mytable
DROP column_to_be_deleted;
```
---
테이블 이름 변경

테이블 자체의 이름을 변경해야 하는 경우 RENAME TO 절을 사용하여 변경할 수 있습니다.
```
테이블 이름 변경
ALTER TABLE mytable
RENAME TO new_table_name;
```
---
기타 변경 사항

각 데이터베이스 구현체마다 테이블을 수정하는 서로 다른 방식을 지원하므로, 작업을 진행하기 전에 사용 중인 데이터베이스의 공식 문서를 확인하는 것이 가장 좋습니다: [MySQL][MySQLLink], [Postgres][PostgresLink], [SQLite][SQLiteLink], [Microsoft SQL Server][Microsoft SQL Server Link]

[MySQLLink]:https://dev.mysql.com/doc/refman/9.7/en/alter-table.html
[PostgresLink]:https://www.postgresql.org/docs/9.4/sql-altertable.html
[SQLiteLink]:https://www.sqlite.org/lang_altertable.html
[Microsoft SQL Server Link]:https://learn.microsoft.com/en-us/sql/t-sql/statements/alter-table-transact-sql?view=sql-server-ver17&redirectedfrom=MSDN

## 17. 테이블 삭제
드문 경우지만, 모든 데이터와 메타데이터를 포함한 테이블 전체를 삭제하고 싶을 때가 있습니다. 이를 위해 DROP TABLE 문을 사용할 수 있으며, 이는 데이터베이스에서 테이블 스키마 자체까지 완전히 삭제한다는 점에서 DELETE 문과 다릅니다.

```
테이블 삭제 명령
DROP TABLE IF EXISTS mytable;
```
CREATE TABLE 문과 마찬가지로, 지정한 테이블이 존재하지 않으면 데이터베이스가 에러를 발생시킬 수 있으므로, 이 에러를 방지하기 위해 IF EXISTS 절을 사용할 수 있습니다.

또한, 삭제하려는 테이블의 컬럼에 의존하는 다른 테이블이 있는 경우(예: FOREIGN KEY 의존성), 의존성이 있는 행들을 제거하기 위해 모든 관련 테이블을 먼저 업데이트하거나, 그 테이블들을 완전히 삭제해야 합니다.