# SQL_BASIC 5주차 정규 과제 

📌SQL_BASIC 정규과제는 매주 정해진 분량의 `초보자를 위한 BigQuery(SQL) 입문` 강의를 듣고 간단한 문제를 풀면서 학습하는 것입니다. 이번주는 아래의 **SQL_Basic_5th_TIL**에 나열된 분량을 수강하고 `학습 목표`에 맞게 공부하시면 됩니다.

**5주차 과제는 문제 풀이를 중심으로**, 강의에서 제시된 예제 문제 중 **3 문제 이상을 선택하여 직접 풀어본 뒤**, 강의 영상의 풀이와 비교해 **틀린 부분, 맞은 부분, 새롭게 배운 개념**을 구체적으로 정리해주세요. (적어도 4문제는 정리해야 합니다.) 완성된 과제는 Gihub에 업로드하고, 링크를 스프레드시트 'SQL' 시트에 입력해 제출해주세요.

**👀(수행 인증샷은 필수입니다.)** 
<img width="1625" height="872" alt="image" src="https://github.com/user-attachments/assets/c2d2fa9f-8754-4136-bb68-a8055593861e" />



## SQL_BASIC_5th

### 섹션 5. 데이터 탐색 - 변환

### 4-4. 날짜 및 시간 데이터 이해하기(2) (EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME)

### 4-5. 시간 데이터 연습문제 1~2번

### 4-5. 시간 데이터 연습문제 3~5번

### 4-6. 조건문 (CASE WHEN, IF)

### 4-7. 조건문 연습 문제

### 4-8. 정리

### 4-9. BigQuery 공식 문서 확인하는 법

(강의에서 연습문제가 많아서 따로 프로그래머스 문제 과제는 없습니다.)



## 🏁 강의 수강 (Study Schedule)

| 주차  | 공부 범위              | 완료 여부 |
| ----- | ---------------------- | --------- |
| 1주차 | 섹션 **1-1** ~ **2-2** | ✅         |
| 2주차 | 섹션 **2-3** ~ **2-5** | ✅         |
| 3주차 | 섹션 **2-6** ~ **3-3** | ✅         |
| 4주차 | 섹션 **3-4** ~ **4-4** | ✅         |
| 5주차 | 섹션 **4-4** ~ **4-9** | ✅         |
| 6주차 | 섹션 **5-1** ~ **5-7** | 🍽️         |
| 7주차 | 섹션 **6-1** ~ **6-6** | 🍽️         |

<br>



<!-- 여기까진 그대로 둬 주세요-->

---

# 4-4. 날짜 및 시간 데이터 이해하기(2) (EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME)

~~~
✅ 학습 목표 :
* 날짜 및 시간 데이터에 대해서 더 자세히 설명할 수 있다. 
* CURRENT_TIME, EXTRACT, DATETIME_TRUNC, PARSE_DATETIME, FROMAT_DATETIME 을 설명할 수 있다. 
~~~

이번 강의를 들으면서 날짜 및 시간 데이터는 단순히 연도, 월, 일만 보는 것이 아니라 **타임존을 함께 고려해야 정확한 분석이 가능하다**는 점이 중요하다고 느꼈습니다. 

특히 강의에서 `CURRENT_DATETIME`, `CURRENT_DATE`를 예시로 설명하면서 타임존을 넣지 않으면 UTC 기준으로 계산되어 한국 시간과 9시간 차이가 날 수 있다고 설명한 부분이 인상적이었습니다. 

현재 시각 계열 함수도 같은 맥락으로 이해하였고, 실무에서는 `Asia/Seoul` 같은 타임존을 명시하는 습관이 중요하다고 생각했습니다.

또한 `EXTRACT` 함수는 날짜·시간 데이터에서 필요한 부분만 뽑아내는 함수라고 이해했습니다. 

예를 들어 연도만 필요하면 `YEAR`, 월만 필요하면 `MONTH`, 시간대 분석이 필요하면 `HOUR`, 요일 분석이 필요하면 `DAYOFWEEK`를 추출할 수 있습니다. 저는 이 함수가 월별 주문 수, 시간대별 방문 수, 요일별 수요 분석처럼 **집계의 기준을 만드는 데 매우 유용하다**고 느꼈습니다.

`DATETIME_TRUNC`는 날짜·시간 데이터를 특정 단위로 잘라서 기준 시점으로 맞춰주는 함수라고 이해했습니다. 

예를 들어 시간 단위로 자르면 분과 초가 0으로 정리되고, 월 단위로 자르면 해당 월의 시작 시점 형태로 맞춰집니다. 강의에서는 `EXTRACT`와 `DATETIME_TRUNC`를 모두 소개했는데, 저는 둘의 차이를 다음과 같이 정리했습니다.

| 함수 | 역할 | 예시 |
|------|------|------|
| `EXTRACT` | 특정 부분만 숫자처럼 추출 | 연도, 월, 시, 요일 추출 |
| `DATETIME_TRUNC` | 날짜/시간 자체를 특정 단위 기준으로 자름 | 시간 단위, 월 단위, 연 단위 정리 |

`PARSE_DATETIME`은 문자열을 `DATETIME` 타입으로 바꾸는 함수이고, `FORMAT_DATETIME`은 반대로 `DATETIME` 값을 원하는 문자열 형식으로 바꾸는 함수라고 이해했습니다. 

처음에는 두 함수가 헷갈렸지만, **PARSE는 문자열 → 날짜형**, **FORMAT은 날짜형 → 문자열**이라고 정리하니 훨씬 쉽게 구분할 수 있었습니다. 

특히 `%Y`, `%m`, `%d`, `%H`, `%M`, `%S` 같은 포맷 문자는 외우기보다 필요할 때 공식 문서에서 찾아보는 방식이 더 효율적이라고 느꼈습니다.

추가로 `DATETIME_DIFF`, `LAST_DAY` 같은 함수도 함께 배웠는데, 날짜 간 차이를 계산하거나 월의 마지막 날짜를 구할 때 유용하다고 이해했습니다. 

이번 파트를 통해 시간 데이터는 겉보기 형식만 보고 판단하면 안 되고, **저장된 타입과 타임존을 먼저 검증한 뒤 함수를 적용해야 한다**는 점을 다시 한 번 배웠습니다.



# 4-6. 조건문(CASE WHEN, IF)

~~~
✅ 학습 목표 :
* 조건문 함수의 기능을 이해하고, 설명할 수 있다. 
~~~

조건문 함수는 **기존 컬럼의 값을 기준으로 새로운 분류 기준을 만드는 데 사용하는 도구**라고 이해했습니다. 

데이터를 분석하다 보면 원본 값을 그대로 쓰기보다, 여러 범주를 묶거나 해석하기 쉬운 이름으로 바꿔야 하는 경우가 많습니다. 

예를 들어 요일 번호를 주중/주말로 바꾸거나, 타입명을 한글 설명으로 바꾸거나, 점수 구간에 따라 등급을 나누는 작업이 이에 해당한다고 생각했습니다.

강의에서는 조건문으로 `CASE WHEN`과 `IF`를 설명하였습니다. 

저는 두 함수의 차이를 아래처럼 정리했습니다.

| 함수 | 적합한 상황 | 특징 |
|------|-------------|------|
| `CASE WHEN` | 조건이 여러 개일 때 | 여러 분기를 순서대로 처리할 수 있습니다. |
| `IF` | 조건이 하나일 때 | 문법이 짧고 단순하여 빠르게 작성할 수 있습니다. |

`CASE WHEN`은 `WHEN 조건 THEN 결과` 형식으로 여러 조건을 나열하고, 마지막에 `ELSE`로 나머지 값을 처리하는 구조라고 이해했습니다. 

반면 `IF`는 `IF(조건, 참일 때 값, 거짓일 때 값)`처럼 한 줄로 작성할 수 있어서 단일 조건일 때 가독성이 좋다고 느꼈습니다.

이번 강의에서 특히 중요하다고 느낀 부분은 **CASE WHEN은 순서가 매우 중요하다**는 점이었습니다. 

예를 들어 `total >= 50`을 먼저 쓰고 그다음 `total >= 100`을 쓰면, 100 이상인 값도 먼저 50 이상 조건에 걸려 버려서 원하는 결과가 나오지 않을 수 있습니다. 

따라서 조건이 겹칠 수 있는 경우에는 더 큰 범위를 먼저 쓰거나, 구간을 명확히 나누어서 작성해야 한다고 이해했습니다.

또한 조건문은 단순히 결과를 보기 좋게 만드는 기능이 아니라, 실제 분석을 위한 전처리의 역할도 한다고 느꼈습니다. 

원본 데이터를 주중/주말, 저학년/고학년, 낮/밤처럼 새로운 기준으로 묶을 수 있기 때문에, 이후의 `GROUP BY`나 집계 분석과 자연스럽게 연결될 수 있다고 생각했습니다. 


 # 4-5. 시간 데이터 연습문제 & 4-7. 조건문 연습 문제

~~~
✅ 학습 목표 :
* 4-5, 4-7 각각에서 두 문제 이상 (최소 4문제) 푼 내용 정리하기
~~~

이번 주차는 문제 풀이가 중심이었기 때문에, 제가 직접 풀어보면서 어디서 헷갈렸는지와 강의 풀이를 비교하며 새롭게 배운 점을 중심으로 아래와 같이 정리하였습니다.


## 1. 시간 데이터 연습문제 1번  
### 2023년 1월에 포획된 포켓몬 수 구하기

처음에는 `catch_date` 컬럼만 보고 바로 2023년 1월 데이터를 세면 되겠다고 생각했습니다. 하지만 강의에서는 컬럼 이름만 믿지 말고 실제 저장 타입을 먼저 확인해야 한다고 설명했습니다. 

직접 확인해 보니 `catch_date_time`은 타임스탬프 기반 데이터였고, `catch_date`는 제가 예상한 기준과 다를 수 있는 값이었습니다. 이 부분이 제가 처음에 놓친 부분이었습니다.

강의 풀이와 비교해 보니, 먼저 `catch_date`와 `DATETIME(catch_date_time, 'Asia/Seoul')`를 비교하여 날짜가 완전히 일치하는지 검증하는 과정이 있었습니다. 

저는 바로 집계부터 하려고 했는데, 강의에서는 **집계 전에 데이터 정의를 검증하는 과정**을 먼저 거쳤다는 점이 가장 큰 차이였습니다.

최종적으로는 `DATETIME(catch_date_time, 'Asia/Seoul')`에서 `EXTRACT(YEAR ...)`, `EXTRACT(MONTH ...)`를 사용해 2023년 1월 데이터를 필터링하는 방식이 더 정확하다고 이해했습니다. 

이 문제를 통해 실무에서는 “컬럼명이 date라고 해서 무조건 내가 생각하는 기준의 날짜는 아니다”라는 점을 배웠습니다.

```sql
SELECT COUNT(DISTINCT id) AS cnt
FROM basic.trainer_pokemon
WHERE EXTRACT(YEAR FROM DATETIME(catch_date_time, 'Asia/Seoul')) = 2023
  AND EXTRACT(MONTH FROM DATETIME(catch_date_time, 'Asia/Seoul')) = 1;
```

### 새롭게 배운 점
- 컬럼 이름보다 **실제 타입과 생성 기준**을 먼저 봐야 합니다.
- 타임존이 포함된 시간 데이터는 한국 시간으로 바꾼 뒤 분석해야 할 수 있습니다.
- 집계 전에 데이터 검증 쿼리를 작성하는 습관이 중요하다고 느꼈습니다.

---

## 2. 시간 데이터 연습문제 2번  
### 오전 6시부터 오후 6시 사이에 일어난 배틀 수 구하기

이 문제는 처음에 `battle_datetime`에서 시간만 뽑아서 `>= 6 AND <= 18` 조건을 쓰면 된다고 생각했습니다. 

이 접근 자체는 맞았지만, 강의에서는 먼저 `battle_datetime`과 `battle_timestamp`를 한국 시간 기준으로 바꾼 값이 같은지 검증하는 과정을 보여주었습니다. 

저는 바로 문제를 풀려고 했고, 강의는 먼저 데이터 신뢰성을 확인했다는 점이 차이였습니다.

또한 강의 풀이를 보면서 `EXTRACT(HOUR FROM battle_datetime) >= 6 AND ... <= 18`처럼 길게 쓰는 방법 외에도 `BETWEEN 6 AND 18`을 사용하면 더 간단하게 작성할 수 있다는 점을 배웠습니다. 

같은 의미이더라도 가독성이 더 좋은 문법을 선택하는 것이 중요하다고 느꼈습니다.

```sql
SELECT COUNT(DISTINCT id) AS battle_cnt
FROM basic.battle
WHERE EXTRACT(HOUR FROM battle_datetime) BETWEEN 6 AND 18;
```

### 새롭게 배운 점
- 시간 조건을 걸기 전에 날짜/시간 컬럼의 기준이 같은지 검증하는 것이 필요합니다.
- 반복되는 비교식은 `BETWEEN`으로 줄일 수 있어 가독성이 좋아집니다.
- `EXTRACT(HOUR ...)`는 시간대 분석에서 매우 자주 쓰일 것 같다고 느꼈습니다.

---

## 3. 시간 데이터 연습문제 5번  
### 트레이너가 처음 포획한 날짜와 마지막으로 포획한 날짜의 간격 구하기

이 문제는 처음 읽었을 때 무엇을 먼저 계산해야 할지 조금 헷갈렸습니다. 강의를 보고 나서 정리해 보니, 핵심은 **트레이너별 최소 날짜와 최대 날짜를 먼저 구한 뒤 두 값의 차이를 계산하는 것**이었습니다. 

즉, `MIN`과 `MAX`를 먼저 구하고 그다음 `DATETIME_DIFF`를 적용해야 했습니다.

저는 처음에 바로 `DATETIME_DIFF`부터 생각했는데, 강의 풀이와 비교해 보니 그게 아니라 집계 순서를 먼저 정리하는 것이 중요했습니다. 

또한 `DATETIME_DIFF(큰 값, 작은 값, DAY)` 순서로 넣어야 양수가 나온다는 점도 다시 확인할 수 있었습니다.

```sql
SELECT
  trainer_id,
  min_catch_time,
  max_catch_time,
  DATETIME_DIFF(max_catch_time, min_catch_time, DAY) AS diff_day
FROM (
  SELECT
    trainer_id,
    MIN(DATETIME(catch_date_time, 'Asia/Seoul')) AS min_catch_time,
    MAX(DATETIME(catch_date_time, 'Asia/Seoul')) AS max_catch_time
  FROM basic.trainer_pokemon
  GROUP BY trainer_id
)
ORDER BY diff_day DESC;
```

### 새롭게 배운 점
- “처음”은 `MIN`, “마지막”은 `MAX`라는 점을 더 확실히 이해했습니다.
- 날짜 차이 함수는 **첫 번째 인자와 두 번째 인자의 순서**가 중요합니다.
- 결과를 믿기 전에 직접 날짜 예시를 대입해 검증하는 습관이 필요하다고 느꼈습니다.

---

## 4. 조건문 연습문제 2번  
### 타입1에 따라 한글 설명 컬럼 만들기

이 문제는 조건이 여러 개였기 때문에 `IF`보다 `CASE WHEN`이 더 적절하다고 판단했습니다. `Water`는 물, `Fire`는 불, `Electric`은 전기로 바꾸고 나머지는 기타로 처리하면 되었습니다. 

저는 처음에 여러 번 `IF`를 중첩해서 쓰는 방법도 떠올렸지만, 강의 풀이와 비교해 보니 `CASE WHEN`이 훨씬 읽기 쉽고 수정하기도 편하다고 느꼈습니다.

```sql
SELECT
  id,
  korean_name,
  name,
  type1,
  CASE
    WHEN type1 = 'Water' THEN '물'
    WHEN type1 = 'Fire' THEN '불'
    WHEN type1 = 'Electric' THEN '전기'
    ELSE '기타'
  END AS type1_korean
FROM basic.pokemon;
```

### 강의 풀이와 비교한 점
- 제 생각과 마찬가지로 여러 조건이 있을 때는 `CASE WHEN`을 사용하는 것이 맞았습니다.
- 다만 강의에서는 `ELSE`를 꼭 넣어 나머지 값을 모두 처리한다는 점을 더 강조했습니다.
- 저는 특정 타입만 바꾸는 데 집중했는데, 강의에서는 **빠지는 값 없이 전체 데이터를 어떻게 처리할 것인지**를 함께 생각하는 것이 중요하다고 설명했습니다.

### 새롭게 배운 점
- 조건이 여러 개면 `CASE WHEN`이 더 적합합니다.
- `ELSE`를 넣지 않으면 예기치 않은 NULL이 생길 수 있으므로 주의해야 합니다.
- 조건문은 단순 치환이 아니라 분석을 위한 전처리라는 점을 이해했습니다.

---

## 5. 조건문 연습문제 3번  
### 포켓몬 총점 기준으로 Low / Medium / High 나누기

이 문제는 구간을 나누는 문제였기 때문에 `CASE WHEN`을 사용하였습니다. 처음에는 분류 조건만 생각했는데, 강의 풀이를 보며 **조건의 순서와 SELECT 별칭을 WHERE에서 바로 쓸 수 없는 점**을 함께 배웠습니다.

예를 들어 `total >= 50`을 먼저 쓰고 `total >= 100`을 뒤에 쓰면, 100 이상도 먼저 50 이상에 걸릴 수 있습니다. 

강의에서는 이런 이유로 조건의 순서가 중요하다고 설명했고, 저는 이 부분이 매우 실무적이라고 느꼈습니다. 

또한 `AS total_grade`로 만든 별칭은 같은 SELECT문 안의 WHERE에서 바로 쓸 수 없어서, 확인이 필요할 때는 서브쿼리로 한 번 감싸야 한다는 점도 새롭게 배웠습니다.

```sql
SELECT
  id,
  korean_name,
  name,
  total,
  CASE
    WHEN total >= 501 THEN 'High'
    WHEN total BETWEEN 301 AND 500 THEN 'Medium'
    ELSE 'Low'
  END AS total_grade
FROM basic.pokemon;
```

### 강의 풀이와 비교한 점
- 큰 구간부터 먼저 쓰는 방식이 더 안전하다는 점을 배웠습니다.
- 저는 결과 확인을 위해 WHERE에 별칭을 바로 쓰려 했는데, 강의 풀이에서는 서브쿼리로 해결하였습니다.
- 단순히 “정답 쿼리 작성”이 아니라, **결과를 검증하기 위한 구조까지 생각해야 한다**는 점을 느꼈습니다.

### 새롭게 배운 점
- `CASE WHEN`은 순서가 중요합니다.
- SELECT에서 만든 별칭은 같은 단계의 WHERE에서 바로 사용할 수 없습니다.
- 확인용 쿼리를 따로 만들거나 서브쿼리를 사용하는 습관이 필요하다고 생각했습니다.



<br>

<br>

---

# 확인문제

## 문제 1

> **🧚Q. 광윤이는 카페 주문 로그 데이터(order_log)를 분석하여, '오전(0시-11시)'과 '오후(12시-23시)'의 주문 건수를 집계하려고 합니다. 광윤이가 작성한 다음 SQL 쿼리 중 문법적으로 틀렸거나 의도한 결과가 나오지 않는 것을 모두 골라보세요. (복수 선택 가능)**

~~~sql
1. SELECT 
   IF(EXTRACT(HOUR FROM order_time) < 12, '오전', '오후') AS time_type,
   COUNT(*)
   FROM order_log
   GROUP BY time_type;

2. SELECT 
   DATETIME_TRUNC(order_time, HOUR) AS truncated_hour,
   COUNT(*)
   FROM order_log
   WHERE order_time BETWEEN '2021-01-01' AND '2021-12-31'
   GROUP BY order_time;

3. SELECT 
   FORMAT_DATETIME(order_time, '%H') AS order_hour,
   COUNT(*)
   FROM order_log
   GROUP BY 1;

4. SELECT 
    CASE 
      WHEN EXTRACT(HOUR FROM order_time) BETWEEN 0 AND 11 THEN '오전'
      ELSE '오후'
    AS time_group,
    COUNT(*)
   FROM order_log
   GROUP BY time_group;
~~~

<!-- 틀린쿼리에 대한 오류의 원인도 같이 작성해주세요. 문제에서 제공된 order_time 컬럼은 DATETIME type의 데이터를 가지고 있다고 가정합니다. -->

~~~
틀린 쿼리는 2번, 3번, 4번 입니다. 

먼저 1번 쿼리는 `EXTRACT(HOUR FROM order_time)`으로 시간만 추출한 뒤, 12보다 작으면 오전 아니면 오후로 분류하고 `GROUP BY time_type`으로 집계하고 있으므로 의도한 결과에 맞는 쿼리입니다.

2번 쿼리는 `DATETIME_TRUNC(order_time, HOUR)`를 사용하고 있어서 시간 단위로 자른 값은 만들고 있지만, 문제에서 원하는 것은 '오전/오후' 집계입니다. 즉 이 쿼리는 오전과 오후로 나누는 쿼리가 아니라 시간 단위로 자른 값과 관련된 집계에 더 가깝습니다. 또한 `GROUP BY order_time`으로 원본 시간값을 기준으로 묶고 있어서, `truncated_hour` 기준 집계와도 맞지 않습니다. 따라서 문법 자체보다도 의도한 결과가 나오지 않는 쿼리라고 볼 수 있습니다.

3번 쿼리는 `FORMAT_DATETIME` 함수의 인자 순서가 잘못되었습니다. `FORMAT_DATETIME`은 `FORMAT_DATETIME('%H', order_time)`처럼 포맷 문자열이 먼저 와야 하는데, 현재는 `order_time`이 먼저 들어가 있어 문법적으로 맞지 않습니다. 또한 설령 인자 순서를 고치더라도 결과는 `00`부터 `23`까지의 시간 문자열이므로, 오전/오후 집계를 바로 구하는 쿼리는 아닙니다.

4번 쿼리는 `CASE WHEN` 문법에서 `END`가 빠졌습니다. `CASE ... WHEN ... THEN ... ELSE ... END AS time_group` 형태로 작성해야 하는데, 현재는 `END` 없이 바로 `AS time_group`이 나와 있어 문법 오류가 발생합니다.

따라서 정답은 2번, 3번, 4번이라고 정리할 수 있습니다.
~~~



## 문제 2

> **🧚Q. 예운이는 포켓몬 타입에 따라 설명을 부여하는 쿼리를 작성했습니다. type 1 컬럼의 값에 따라 조건을 분기했으며, 다음 SQL 쿼리를 실행했습니다.**

~~~sql
SELECT name,
       CASE 
         WHEN type1 = 'Fire' THEN 'Hot'
         WHEN type1 = 'Water' THEN 'Cool'
         ELSE 'Normal'
       END AS type_description
FROM pokemon;
~~~

> **다음 중 type_description의 결과가 'Normal'로 출력될 포켓몬은?**

| **name**   | **type1** |
| ---------- | --------- |
| Pikachu    | Electric  |
| Charmander | Fire      |
| Squirtle   | Water     |
| Bulbasaur  | Grass     |

<!-- 근거와 함께 답을 작성해주세요 -->

~~~
`Normal`로 출력될 포켓몬은 **Pikachu**와 **Bulbasaur**입니다.

그 이유는 `CASE WHEN`에서 `type1 = 'Fire'`이면 `Hot`, `type1 = 'Water'`이면 `Cool`로 처리하고, 그 외의 모든 값은 `ELSE 'Normal'`로 처리하기 때문입니다.  
따라서

- Pikachu : `Electric` → Fire도 Water도 아니므로 `Normal`
- Charmander : `Fire` → `Hot`
- Squirtle : `Water` → `Cool`
- Bulbasaur : `Grass` → Fire도 Water도 아니므로 `Normal`

이라고 해석할 수 있습니다.

즉, 정답은 **Pikachu, Bulbasaur**입니다.
~~~



<br>

### 🎉 수고하셨습니다.
