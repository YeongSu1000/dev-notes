# SQL Bolt

## 00. SQL 소개


### SQL 이란
SQL(구조화 질의 언어)은 기술 전문가와 비전문가 모두 관계형 데이터베이스에서 데이터를 조회, 조작 및 변환할 수 있도록 설계된 언어입니다.  
SQL 데이터베이스는 그 단순성 덕분에 수백만 개의 웹사이트와 모바일 애플리케이션에 안전하고 확장 가능한 저장 공간을 제공합니다.

---

### 관계형 데이터베이스
SQL 구문을 배우기 전에 관계형 데이터베이스가 실제로 무엇인지에 대한 개념을 이해하는 것이 중요합니다.  
관계형 데이터베이스는 서로 관련된 (2차원) 테이블들의 모음입니다. 각 테이블은 엑셀 스프레드시트와 유사하게,  
고정된 수의 명명된 열(테이블의 속성)과 여러 개의 행으로 구성됩니다.

예를 들어, 차량등록국에 데이터베이스가 있다면, 그 데이터베이스에는 해당 주에 거주하는 사람들이 운전하는 모든 차량 정보가 담긴 표가 있을 수 있습니다.   
이 표에는 각 차량의 모델명, 차종, 바퀴 수, 문 개수 등의 정보가 저장되어야 할 수도 있습니다.

| ID | 제조사/모델   | #바퀴 | #문 | 유형    |
|----|----------|-----|----|-------|
| 1  | 포드 포커스   | 4   | 4  | 의자 가마 |
| 2  | 테슬라 로드스터 | 4   | 2  | 스포츠   |
| 3  | 카와카시 닌자  | 2   | 0  | 오토바이  |
| 4  | 맥라렌 포뮬러1 | 4   | 0  | 경주    |
| 5  | 테슬라 s    | 4   | 4  | 의자 가마 |

이러한 데이터베이스에는 해당 주에 등록된 모든 운전자 목록, 발급 가능한 운전면허 종류,   
심지어 각 운전자의 운전 위반 기록과 같은 정보를 포함하는 추가 관련 테이블이 있을 수 있습니다.

SQL을 배우는 목적은 "도로를 달리는 차량 중 바퀴가 네 개 미만인 차량은 어떤 종류가 있을까요?"  
또는 "테슬라는 몇 가지 모델의 자동차를 생산할까요?" 와 같이 데이터에 대한 구체적인 질문에 답하는 방법을 익혀  
향후 더 나은 의사결정을 내리는 데 도움을 주는 것입니다.

---
## 01. SELECT 쿼리 기초
```
특정 열을 선택하는 쿼리
SELECT column, another_column, …
FROM mytable;
```
```
모든 열을 선택하는 쿼리
SELECT * 
FROM mytable;
```

---
## 02. 제약 조건을 사용한 쿼리 (1부)

```
제약 조건을 포함한 선택 쿼리
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```
| 연산자 (Operator) | 조건 (Condition) | SQL 예시 (SQL Example) |
| :--- | :--- | :--- |
| `=`, `!=`, `<`, `<=`, `>`, `>=` | 일반적인 숫자 비교 연산자 | `col_name != 4` |
| `BETWEEN ... AND ...` | 두 값 범위 내에 값이 존재하는 경우 (양 끝값 포함) | `col_name BETWEEN 1.5 AND 10.5` |
| `NOT BETWEEN ... AND ...` | 두 값 범위 내에 값이 존재하지 않는 경우 (양 끝값 포함) | `col_name NOT BETWEEN 1 AND 10` |
| `IN (...)` | 지정한 리스트(목록)에 값이 존재하는 경우 | `col_name IN (2, 4, 6)` |
| `NOT IN (...)` | 지정한 리스트(목록)에 값이 존재하지 않는 경우 | `col_name NOT IN (1, 3, 5)` |

---
## 03. 제약 조건을 사용한 쿼리 (2부)

| 연산자 (Operator) | 조건 (Condition) | 예시 (Example) |
| :--- | :--- | :--- |
| `=` | 대소문자를 구분하는 정확한 문자열 비교 (`=`을 하나만 사용함에 주의) | `col_name = "abc"` |
| `!=` 또는 `<>` | 대소문자를 구분하는 정확한 불일치 비교 | `col_name != "abcd"` |
| `LIKE` | 대소문자를 구분하지 않는 문자열 비교 | `col_name LIKE "ABC"` |
| `NOT LIKE` | 대소문자를 구분하지 않는 불일치 비교 | `col_name NOT LIKE "ABCD"` |
| `%` | 0개 이상의 모든 문자열과 매칭 (`LIKE` 또는 `NOT LIKE`와 함께 사용) | `col_name LIKE "%AT%"`<br>("AT", "ATTIC", "CAT", "BATS" 등과 매칭) |
| `_` | 정확히 1개의 임의의 문자와 매칭 (`LIKE` 또는 `NOT LIKE`와 함께 사용) | `col_name LIKE "AN_"`<br>("AND"와는 매칭되지만, "AN"과는 매칭되지 않음) |
| `IN (...)` | 문자열이 지정한 리스트(목록) 안에 존재하는 경우 | `col_name IN ("A", "B", "C")` |
| `NOT IN (...)` | 문자열이 지정한 리스트(목록) 안에 존재하지 않는 경우 | `col_name NOT IN ("D", "E", "F")` |

---
## 04. 쿼리 결과 필터링 및 정렬

데이터베이스의 데이터 자체는 고유할 수 있지만, 특정 쿼리의 결과는 그렇지 않을 수 있습니다.  
예를 들어 영화 테이블의 경우, 같은 해에 여러 영화가 개봉될 수 있습니다.   
이러한 경우 SQL은 DISTINCT 키워드를 사용하여 중복된 열 값을 가진 행을 편리하게 제거하는 방법을 제공합니다.

```
결과가 고유한 쿼리 선택
SELECT DISTINCT column, another_column, …
FROM mytable
WHERE condition(s);
```
```
정렬된 결과를 포함하는 쿼리 선택
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC;
```

```
행 수를 제한한 선택 쿼리
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

## 05. JOIN을 사용한 다중 테이블 쿼리
