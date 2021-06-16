# Group by

## 그룹화

DB에 있는 데이터를  컬럼 별로 묶어서 보여준다.

group by를 사용하기 위해선 `SELECT문`을 사용해야한다.

**값에 따라서 데이터를 그룹 짓고, 중복된 열은 제거된다.**

```mysql
select userID, amount from buytbl order by userID;
```

으로 묶게 되면 아이디끼리 모이긴하지만 amount 와 같이 나머지 컬럼의 값들이 달라 묶이지는 않는다.

![1-1](.\1-1.PNG)

이를 해결하기위해 amount 에대해 계산을 해주면 묶이게 된다.

### 계산 함수:![1-2](.\1-2.PNG)

```mysql
select userID, sum(amount) from buytbl group by userID;
```

![1-2](.\1-3.PNG)

# Having

## 조건식

HAVING도 WHERE과 같이 조건식이다.

하지만 WHERE 은 그룹화 전 조건을 거는 것이고,HAVING은 그룹화 한 경과에 조건을 거는 것이다.

## 사용법

#### 컬럼 그룹화

```mysql
SE	LECT 컬럼 FROM 테이블 GROUP BY 그룹화할 컬럼;
```

#### 조건 처리 후에 컬럼 그룹화

```mysql
SELECT 컬럼 FROM 테이블 WHERE 조건식 GROUP BY 그룹화할 컬럼;
```

#### 컬럼 그룹화 후에 조건 처리

```mysql
SELECT 컬럼 FROM 테이블 GROUP BY 그룹화할 컬럼 HAVING 조건식;
```

#### 조건 처리 후에 컬럼 그룹화 후에 조건 처리

```mysql
SELECT 컬럼 FROM 테이블 WHERE 조건식 GROUP BY 그룹화할 컬럼 HAVING 조건식;
```

#### ORDER BY가 존재하는 경우

```
SELECT 컬럼 FROM 테이블 [WHERE 조건식] GROUP BY 그룹화할 컬럼 [HAVING 조건식] ORDER BY 컬럼1 [, 컬럼2, 컬럼3 ...];
```

[]:상관x



### 테이블 : pika

| **idx** | **type** | **name** |
| ------- | -------- | -------- |
| 1       | 1        | 피카츄   |
| 2       | 1        | 라이츄   |
| 3       | 2        | 파이리   |
| 4       | 2        | 꼬북이   |
| 5       | 3        | 버터플   |
| 6       | 3        | 야도란   |
| 7       | 4        | 피죤투   |

### type 그룹화하여 name 갯수 조회 (컬럼 그룹화)

#### 예제

```
SELECT type, COUNT(name) AS cnt FROM pika GROUP BY type;
```

#### 결과

| **type** | **cnt** |
| -------- | ------- |
| 1        | 2       |
| 2        | 2       |
| 3        | 2       |
| 4        | 1       |





### type 1 초과인, type 그룹화하여 name 갯수 조회 (조건 처리 후 컬럼 그룹화)

#### 예제

```
SELECT type, COUNT(name) AS cnt FROM pika WHERE type > 1 GROUP BY type;
```

#### 결과

| **type** | **cnt** |
| -------- | ------- |
| 2        | 2       |
| 3        | 2       |
| 4        | 1       |





### type 그룹화하여 name 갯수를 가져온 후, 그 중에 갯수가 2개 이상인 데이터 조회 (조건 처리 후에 컬럼 그룹화 후에 조건 처리)

#### 예제

```
SELECT type, COUNT(name) AS cnt FROM pika GROUP BY type HAVING cnt >= 2;
```

#### 결과

| **type** | **cnt** |
| -------- | ------- |
| 1        | 2       |
| 2        | 2       |
| 3        | 2       |





### type 1 초과인, type 그룹화하여 name 갯수를 가져온 후, 그 중에 갯수가 2개 이상인 데이터 조회 (조건 처리 후에 컬럼 그룹화 후에 조건 처리)

#### 예제

```
SELECT type, COUNT(name) AS cnt FROM pika WHERE type > 1 GROUP BY type HAVING cnt >= 2;
```

#### 결과

| **type** | **cnt** |
| -------- | ------- |
| 2        | 2       |
| 3        | 2       |





### type 1 초과인, type 그룹화하여 name 갯수를 가져온 후, 그 중에 갯수가 2개 이상인 데이터를 type 내림차순 정렬로 조회 (내림차순 정렬)

#### 예제

```
SELECT type, COUNT(name) AS cnt FROM pika WHERE type > 1 GROUP BY type HAVING cnt >= 2 ORDER BY type DESC;
```

#### 결과

| **type** | **cnt** |
| -------- | ------- |
| 3        | 2       |
| 2        | 2       |