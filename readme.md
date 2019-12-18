# SQL이란
##### Strutured Query Language(구조화된 질의어)

기본적인 쿼리문은 문장이 끝나면 반드시 ';'를 붙여줘야 한다.

사용할 데이터베이스를 미리 지정해준다.
###### ctrl + f9

내가 employees 데이터베이스를 사용하겠다.
```
USE employees;
```
* 기본적인 쿼리문을 사용해보자.
```
SELECT <FIELD명>, <FIELD 명>, ...
FROM 	 <TABLE 명>
```
* SELECT = 테이블에서 가져온다. 
```
SELECT *
FROM 	 employees;
```
' * ' = 전체를 의미한다.

* 존재하는 테이블 목록 보기
```
SHOW TABLES;
```

### SELECT, FROM

* 사원 테이블에서 사원들의 이름만 가져와 보자.
```
SELECT first_name, last_name
FROM 	 employees;
```
### WHERE 절

#### WHERE 조건 : 조건에 만족하는 데이터만 가져온다.

* 사원 테이블에서 이름이 'Elvis'인 사람의 정보를 가져와 보자.

```where
SELECT *
FROM 	 employees
WHERE  first_name = 'Elvis'
;
```
* 사원 테이블에서 성이 'Simmel'인 사람의 정보를 가져와 보자.
```
SELECT *
FROM 	 employees
WHERE  last_name = 'Simmel'
;
``` 
* 사원중에서 사번이 20000번 이하인 사원들의 정보를 출력해보자.
```
SELECT *
FROM 	 employees
WHERE  emp_no <= 20000
;
```
##### >= (크거나 같다), < (작다), <= (작거나 같다), <> (같지 않다), != (같지 않다)

#### AND 연산자 : 조건을 추가할때, 또는 A AND B의 조건을 다 만족할때

* 사원중에서 사번이 20000번 이상이고, 20100번 이하인 사원들의 이름과 성별을 출력해보자.
```
SELECT emp_no, first_name, gender
FROM	 employees
WHERE	 emp_no >= 20000
AND	 emp_no <= 20100
;
```
#### BETWEEN A AND B

##### A 이상 B 이하인 데이터를 가져오는 방법
```
SELECT emp_no, first_name, gender
FROM	 employees
WHERE  emp_no BETWEEN 20000 AND 20100
;
```
* 사원테이블에서 14000번을 초과하고 15000보다 작은 사원의 이름을 가져오시오.
```
SELECT emp_no, first_name, last_name
FROM 	 employees
WHERE  emp_no BETWEEN 14001 AND 14999
;
```
```
SELECT first_name, last_name
FROM 	 employees
WHERE  emp_no > 14000
AND 	 emp_no < 15000
;
```

### 날짜를 처리하는 함수
```
-- DATE_FORMAT(<field 명>, '내가 보고자하는 날짜 형식')
-- DATE_FORMAT(<field 명>, '%Y') : 연도를 출력
-- DATE_FORMAT(<field 명>, '%M') : 월을 출력
-- DATE_FORMAT(<field 명>, '%D') : 일을 출력
-- DATE_FORMAT(<field 명>, '%Y%M%D') : 20191217
-- DATE_FORMAT(<field 명>, '%Y-%M-%D') :2019-12-17
-- DATE_FORMAT(<field 명>, '%Y%M%D %H-%i-%p') : 20191217 14-12 pm (24)
-- DATE_FORMAT(<field 명>, '%Y%M%D %h-%i-%p') : 20191217 02-12 pm (12)
```

* 사원중에 입사한 일자가 '19850101' ~ '19860101'인 사원들의 정보를 출력해 보자.

```
SELECT DATE_FORMAT(hire_date, '%Y%M%D')
FROM 	 employees;
```
```
SELECT *
FROM 	 employees
WHERE  DATE_FORMAT(hire_date, '%Y%M%D') >= '19850101'
AND 	 DATE_FORMAT(hire_date, '%Y%M%D') <= '19860101'
;
```
```
SELECT *
FROM 	 employees
WHERE	 DATE_FORMAT(hire_date, '%Y%M%D') BETWEEN '19850101' AND '19860101'
;
```
```
SELECT *
FROM 	 employees
WHERE  hire_date BETWEEN '1985-01-01' AND '1986-01-01'
;
```
#### 나온 결과를 정렬을 해서 확인해보자.

#### ORDER BY <field명> ASC(1->10) or DESC(10->1)

* 사원중에 입사한 일자가 '19850101' ~ '19860101'인 사원들의 정보

* 최근에 입사한 입사자가 맨위로 오도록 출력해 보세요.

```
SELECT	*
FROM 		employees
WHERE	 	DATE_FORMAT(hire_date, '%Y%M%D') BETWEEN '19850101' AND '19860101'
ORDER BY hire_date DESC
;
```
```
SELECT 	*
FROM 	 	employees
WHERE  	hire_date BETWEEN '1985-01-01' AND '1986-01-01'
ORDER BY hire_date DESC
;
```
```
SELECT * FROM employees WHERE DATE_FORMAT(hire_date, '%Y%m%d') = '19860101';
```
* 입사년도가 1990년인 사원의 정보를 출력해 주세요.
```
SELECT *
FROM 	 employees
WHERE  DATE_FORMAT(hire_date, '%Y') = '1990'
;
```
### OR 연산자

#### 조건 or 조건: 둘중에 하나만 만족해도 출력되는 데이터

* 사원들 중에서 성이 'Genin'이거나 'Facello'인 사원의 정보를 출력해보자.
 
```
SELECT *
FROM 	 employees
WHERE  last_name = 'Facello'
OR  	 last_name = 'Genin'
;
```
### IN 연산자

#### OR와 동일한 결과를 나타낸다.

```
-- <field명> IN ('A값', 'B값')
```
A값 또는 B값을 만족하는 데이터를 가져온다.
```
SELECT *
FROM 	 employees
WHERE  last_name IN('Genin', 'Facello')
;
```
#### Alias(별칭)
```
-- <field명> as 사용하고 싶은 이름
-- <field명> 사용하고 싶은 이름
-- <field명> 이름에 공백이 있을겨우에는 ''로 묶어준다.
```
```
SELECT last_name AS '이 름', first_name AS 성
FROM	 employees
WHERE	 last_name IN ('Genin', 'Facello')
;
```
```
SELECT	hire_date AS 입사년도
FROM 		employees
WHERE	 	DATE_FORMAT(hire_date, '%Y%M%D') BETWEEN '19850101' AND '19860101'
ORDER BY hire_date DESC
;
```
```
SELECT	emp.hire_date
FROM 		employees emp
WHERE	 	DATE_FORMAT(hire_date, '%Y%M%D') BETWEEN '19850101' AND '19860101'
ORDER BY emp.hire_date DESC
;
```
### LIKE 연산자

```
-- <field명> LIKE 's' : s를 포함하는 데이터
-- <field명> LIKE 's%' : s로 시작하는 데이터
-- <field명> LIKE '%s' : s로 끝나는 데이터
-- <field명> LIKE '%s%' : s를 중간에 포함하는 데이터
-- <field명> LIKE '__s' : 3글자인데 2글자를 모르면 _로 채우고 조회한다.
-- wild card : %
```

* 사원중에 이름이 's'로 시작하는 사원의 정보를 출력해보자.

```
SELECT	*
FROM	 	board
WHERE		id = '%jisun%'
OR			title = 'icecream%'
;
```
```
SELECT	*
FROM	 	employees
WHERE	 	first_name LIKE 's%'
;
```
```
SELECT 	*
FROM 		employees
WHERE 	first_name LIKE 'Staff___'
;
```

* 사원중에 직위가 'Engineer'와 'Senior Engineer'인 사원의 사번을 출력해보세요.
```
SELECT	*
FROM		titles
WHERE		title IN('Engineer','Senior Engineer')
;
```

* 사원중에서 직무가 'Engineer'로 끝나는 사람과 직무가 'Staff'로 끝나는 사람의 사번을 출력해 주세요.
```
SELECT	*
FROM		titles
WHERE		title LIKE '%Engineer' 
OR 		title LIKE '%Staff'
;
```
```
-- SELECT	emp_no, title
-- FROM		titles
-- WHERE		title IN('Engineer', 'Staff')
-- AND		title = 'Engineer'
-- AND		title = 'Staff'
-- ;
```

#### NULL : 값이 없다
```
-- <field명> IS NULL : 값이 없을 경우
-- <field명> IS NOT NULL : 값이 NULL이 아닐 경우
```

* 사원중에서 salary가 NULL이 아닌 사원의 정보를 가져와 보자.

```
SELECT	*
FROM		salaries
WHERE 	salary IS NOT NULL
ORDER BY	salary DESC
;
```
```
SELECT	*
FROM 		salaries
WHERE 	salary IS NULL
;
```
* 직무가 'Development' 이거나 'Sales'인 사원들 중에서 1992년 01월 01일 이후에 부서배치가 된 사원의 사번을 구해봅시다.
```
SELECT	dept_no
FROM		departments
WHERE		dept_name IN('Development', 'Sales')
;
```
dept_no = d005, d007
```
SELECT 	emp_no
FROM		dept_emp
WHERE 	dept_no IN ('d005', 'd007')
;
```
```
SELECT	*
FROM		dept_emp
WHERE		dept_no IN ('d005','d007')
AND		DATE_FORMAT(from_date, '%Y%m%d') >= '19920101'
AND		DATE_FORMAT(to_date, '%Y%m%d') >= '99990101'
ORDER BY	from_date DESC
;
```

#### 집계함수: count(<field명>)
##### 필드의 해당하는 데이터 갯수를 출력한다.
##### ' * '가 들어가도 동일한 결과를 나타낸다
```
SELECT	COUNT(*)
FROM 	 	employees
WHERE		emp_no = '10003' 
;
```
```
SELECT	COUNT(*)
FROM 	 	dept_emp
WHERE		dept_no = 'd004' 
;
```
```
SELECT	COUNT(*) AS 사원 수
FROM 	 	dept_emp
WHERE		dept_no = 'd004' 
;
```
```
SELECT	COUNT(*) 사원수
FROM 	 	dept_emp
WHERE		dept_no IN('d004', 'd002') 
;
```
#### group by : 부서나 그룹별로 데이터를 출력할 때 사용
#### select <field명>, ...
#### from   <table명>
#### group by <~별로 사용할 field명>

* 입사년도 별로 사원의 수를 출력하는 쿼리문을 작성해보세요.
```
SELECT	DATE_FORMAT(hire_date, '%Y'), COUNT(*) AS 사원수
FROM		employees
GROUP BY	DATE_FORMAT(hire_date, '%Y')
;
```
* 1999년 입사자를 월별로 몇명이 입사했는지 출력해 보세요.
```
SELECT	DATE_FORMAT(hire_date, '%m'), COUNT(*)
FROM		employees
WHERE		DATE_FORMAT(hire_date, '%Y') ='1999'
GROUP BY	DATE_FORMAT(hire_date, '%m')
;
```
## Test

### 집계함수 : AVG(<field명>)
```
SELECT 	*
FROM 	hr_employees
;
```

* 사원들의 연봉의 평균을 구해봅시다.
```
SELECT	AVG(salary)
FROM		hr_employees
;
```
* 영업이나 마케팅 업무를 하는 사원을 대상으로 직무별 평균급여를 계산해 보세요.
직무 : job_ID
영업 : SA~ , 마케팅 : MK~
```
SELECT	job_ID, AVG(salary)
FROM		hr_employees
WHERE		job_ID LIKE 'SA%'
OR 		job_ID LIKE 'MK%'
GROUP BY job_ID
;
```
-- 2000년 이후 입사자 중에서 재고업무를 담당하는 사원들의 정보를 조회해 보세요.
-- 재고업무 : ST_MAN, ST_CLERK
```
SELECT	*
FROM 		hr_employees
WHERE 	job_id IN('ST_MAN','ST_CLERK')
AND 		DATE_FORMAT(hire_date, '%Y') >= '2000'
;
```
-- 1994년 6월부터 1998년 4월 기간 입사한 사원을 대상으로 부서별 인원수를 얻어보세요.
-- 결과는 인원수가 많은 순서로 출력하세요.
```
SELECT 	job_id, COUNT(*) 사원수
FROM		hr_employees
WHERE 	DATE_FORMAT(hire_date, '%Y%m') >='199406'
AND		DATE_FORMAT(hire_date, '%Y%m') <='199804'
GROUP BY	job_id
ORDER BY COUNT(*) DESC
;
```
```
SELECT 	job_id, COUNT(*)
FROM		hr_employees
WHERE 	DATE_FORMAT(hire_date, '%Y%m') BETWEEN '199406' AND '199804'
GROUP BY	job_id
ORDER BY COUNT(*) DESC
;
```
-- 급여가 5000$ 미만인 사원 중에서 직무가 'IT_PROG'인 사원들의 정보를 조회해 보자.
```
SELECT 	*
FROM		hr_employees
WHERE		job_id = 'IT_PROG'
AND 		salary < 5000
;
```
-- MOD(a,b) : a를 b로 나눈 나머지 값을 반환한다.
-- 	      b가 0이면, NULL을 반환한다.
```
SELECT MOD(12,5);
SELECT MOD(6,2);
SELECT MOD(2,0);
```
-- 짝수해와 홀수해에 입사한 사원의 정보를 얻어보자.
-- [결과]
-- 년도		/입사자수
-- 짝수년도/ 100
-- 홀수년도/ 50

-- 짝수년도에 입사한 사원의 정보를 출력해보자.
```
SELECT	*
FROM		hr_employees
WHERE		MOD(DATE_FORMAT(hire_date, '%Y'),2) = 0
;
```
-- 홀수년도에 입사한 사원의 정보를 출력해보자.
```
SELECT	*
FROM		hr_employees
WHERE		MOD(DATE_FORMAT(hire_date, '%Y'),2) = 1
;
```
```
SELECT	MOD(DATE_FORMAT(hire_date, '%Y'),2) AS 년도, COUNT(employee_id) AS 사원수
FROM		hr_employees
WHERE		MOD(DATE_FORMAT(hire_date, '%Y'),2) = 0
OR 		MOD(DATE_FORMAT(hire_date, '%Y'),2) = 1
GROUP BY	MOD(DATE_FORMAT(hire_date, '%Y'),2)
;
```
-- case문
-- CASE <대상이 되는 필드 or 값>
--   WHEN <대상이 되는 값이 가질 값> THEN <내용을 작성>
--   WHEN <대상이 되는 값이 가질 값> THEN <내용을 작성>
-- END as <표현하고자 하는 필드명을 적어준다.>
```
SELECT	CASE MOD(DATE_FORMAT(hire_date, '%Y'),2)
				WHEN 0 THEN '짝수년도'
				WHEN 1 THEN '홀수년도'
			END AS '년도',
			COUNT(employee_id) AS 사원수
FROM		hr_employees
GROUP BY	MOD(DATE_FORMAT(hire_date, '%Y'),2)
;
```
-- 집계함수 : SUM(<field명>)
-- 값을 합칠때 사용
```
SELECT	SUM(salary)
FROM 		hr_employees
;
```
-- 1998년 이후에 입사한 사람들을 대상으로 직무별 급여의 합을 구하시오.
-- 정렬은 급여가 많은 직무별로 해주세요.
```
SELECT 	job_id, SUM(salary) AS salary
FROM 		hr_employees
WHERE		DATE_FORMAT(hire_date, '%Y') >= '1998'
GROUP BY	job_id
ORDER BY SUM(salary) DESC
;
```
-- ROUND() 함수
-- 숫자를 지정한 자리수로 반올림하여 변환하는 함수
-- ROUND(123.32429382,2)
```
SELECT ROUND(123.32429382,2);
```
-- 문자함수중에 SUBSTR()/SUBSTRING()
-- SUBSTR('밥먹으러가자밥먹자!!!',2,3);
```
SELECT SUBSTR('밥먹으러가자밥먹자!!!',2,3);
SELECT SUBSTRING('medici-001',2,3);
```
-- 문자열을 붙이는 함수 CONCAT(str1, str2)
-- 필드명을 출력하거나 문자열을 출력할때 붙여서 출력할 수 있고,
-- 공백도 붙여서 출력할 수 있다.
-- CONCAT(str1,' ',str2)
```
SELECT CONCAT("이",' ','민지')
SELECT CONCAT("이",'민지')
```
-- 사원의 이름을 출력해보자.
```
SELECT CONCAT(first_name,' ',last_name)
FROM   hr_employees
WHERE  employee_id = 105
;
```

-- 현재 근무하는 사원의 재직기간을 나타내보시오.
```
-- SYSDATE() : 현재시간
-- FLOOR() : 소숫점을 없애주는 함수(정수로 만들어주는 함수)
-- YEAR('2019-12-18 13:07:07'); -- 연도
-- MONTH('2019-12-18 13:07:07'); -- 월
-- DAY('2019-12-18 13:07:07'); -- 일
-- HOUR('2019-12-18 13:07:07'); -- 시간
-- MINUTE('2019-12-18 13:07:07'); -- 분
-- SECOND('2019-12-18 13:07:07'); -- 초
```
```
SELECT YEAR('2019-12-18 13:07:07');
```

-- DATEDIFF(maxDate1, smallDate2)
```
SELECT DATEDIFF('2020-12-18 13:07:07', '2019-12-18 13:07:07');
```
-- TIMESTAMPDIFF(단위, smallDate1, maxDate2)
-- 단위
-- 	SECOND : 초
--		MINUTE : 분
-- 	HOUR	 : 시
-- 	DAY	 : 일
-- 	MONTH	 : 월
-- 	YEAR 	 : 년
```
SELECT TIMESTAMPDIFF(YEAR, '2019-12-18 13:07:07', '2022-12-18 13:07:07');
```
-- 결과 : 3
```
SELECT SYSDATE();
```
-- 현재 근무하는 사원의 재직기간을 나타내보시오.
```
SELECT	employee_id, FLOOR(TIMESTAMPDIFF(MONTH,hire_date,SYSDATE())/12) AS years
FROM 		hr_employees
;
```
```
SELECT	employee_id, FLOOR(DATEDIFF(SYSDATE(),hire_date)/365) AS years
FROM 		hr_employees
;
```
```
SELECT	employee_id, FLOOR(TIMESTAMPDIFF(DAY,hire_date,SYSDATE())/365) AS years
FROM 		hr_employees
;
```
```
SELECT 	*
FROM 		hr_employees
WHERE		employee_id = 100
;
```
```
SELECT 	*
FROM 		hr_job_history;
```
-- 1999년도에 입사한 사람들을 아래와 같이 출력해 봅시다.
-- [결과]
-- 반기(상,하) ㅣ직무	ㅣ입사자
-- 상반기		ㅣSA_REPㅣ	10
-- 하반기		ㅣSA_REPㅣ 20
```
SELECT 	*	
FROM 		hr_employees
WHERE 	DATE_FORMAT(hire_date, '%Y') = '1999'
;
```
```
SELECT 	*	
FROM 		hr_employees
WHERE 	YEAR(hire_date) = '1999'
;
```
```
SELECT 	TRUNCATE(MONTH(hire_date)/7,0)
FROM 		hr_employees
WHERE 	YEAR(hire_date) = '1999'
;
```
```
SELECT 	case TRUNCATE(MONTH(hire_date)/7,0)
				when 0 then '상반기'
				when 1 then '하반기'
			END AS '상하반기',
			job_id AS '직무',
			COUNT(employee_id) AS '사원수'	
FROM 		hr_employees
WHERE 	YEAR(hire_date) = '1999'
GROUP BY case TRUNCATE(MONTH(hire_date)/7,0)
				when 0 then '상반기'
				when 1 then '하반기'
			END,
			job_id
;
```
-- TRUNCATE() 함수 : 보여줄 소숫점 자리수까지 보여주는 함수
-- TRUNCATE(실수, 2) : 소숫점 둘째자리까지 보여준다.
-- TRUNCATE(실수, 0) : 정수로 보여준다.
```
SELECT TRUNCATE(10.2312345,2);
SELECT TRUNCATE(10.2312345,0);
```
-- TRIM() : 문자열에서 공백이나 특정 문자를 제거한 다음 값을 반환
```
SELECT TRIM(' test '); -- 바깥 공백제거
SELECT TRIM(LEADING FROM ' test '); -- 왼쪽 공백제거
SELECT TRIM(TRAILING FROM ' test '); -- 오른쪽 공백제거
SELECT TRIM(TRAILING 'x' FROM 'xxxtestxxx'); -- 오른쪽 끝 x값 제거
SELECT TRIM(TRAILING 'xyz' FROM 'xyztestxyz'); -- 오른쪽 끝 xyz제거
SELECT TRIM(BOTH 'x' FROM 'xxxtestxxx'); -- 양쪽 끝 x제거
LTRIM(str); -- 왼쪽 공백 제거
RTRIM(str); -- 오른쪽 공백 제거
```
-- 사원의 직무가 'IT_PROG'이면 '프로그래머'로 사원의 직무가 'ST_MAN'이면 '매니저'로
-- 'IT_PROG'나 'ST_MAN'이 아니면 '사원'으로 출력하도록 퀘리문을 작성해 보세요.
```
SELECT	*,
			CASE job_id
				WHEN 'IT_PROG' THEN '프로그래머'
				WHEN 'ST_MAN' THEN '매니저'
				ELSE '사원'
			END AS 직무
FROM 		hr_employees
;
```
-- if 함수 사용해보기
-- IF(조건, 참일때 실행되는 곳, 거짓일때 실행되는 곳)
-- if 와 case문의 차이점
-- 복잡하게 처리해야 하는 부분은 case로 하고
-- 단순하게 처리해야 하는 부분은 if()로 처리하는 것이 좋다.
```
SELECT 	*, if(job_id='IT_PROG','프로그래머',
					IF(job_id='ST_MAN','매니저','사원')) 직무
FROM 		hr_employees
;
```
-- 사원중에서 급여를 가장 많이 받는 사람의 급여는 얼마일까요?
-- MAX(<field명>) : field의 최대값
-- MIN(<field명>) : field의 최솟값
```
SELECT 	MAX(salary)
FROM 		hr_employees
;
```
-- 최고 급여 대비 본인연봉을 확인해 보자.
```
SELECT 	employee_id,
			salary,
			MAX(salary) OVER()
FROM 		hr_employees
ORDER BY salary DESC
;
```
-- 영업 직무를 가진 사원 중에서 가장 최근에 입사한 사원을 찾아보자.
-- [결과화면]
-- 사원번호 ㅣ 사원이름 ㅣ 직무
-- 영업직무 : SA%
```
SELECT 	employee_id,
			hire_date,
			MAX(hire_date)
FROM 		hr_employees
WHERE		job_id LIKE 'SA%'
;
```
```
SELECT 	employee_id,
			last_name,
			hire_date,
			job_id
FROM		hr_employees
WHERE		job_id LIKE 'SA%'
AND 		hire_date = (SELECT 	MAX(hire_date) FROM hr_employees)
;
```
-- rank() : 순위를 표현할 때 사용하는 함수
-- 영업부서에 속하는 사원들을 대상으로 급여가 적은 순위를 나타내 보세요.
```
SELECT 	employee_id,
			salary,
			RANK() over(ORDER BY salary ASC) AS salary_ranking
FROM		hr_employees
;
```
-- DENSE RANK() : 순위가 동률이라도 건너뛰지 않고 순서를 매겨준다.
```
SELECT 	employee_id,
			salary,
			dense_RANK() over(ORDER BY salary ASC) AS salary_ranking
FROM		hr_employees
;
```
-- 입사한지 3년 이상 되는 사원들의 연봉 상위 5명의 정보를 출력해주세요.
```
SELECT 	*
FROM		(SELECT 	salary,
						RANK() over(ORDER BY salary DESC) AS RANK
			FROM 		hr_employees
			WHERE		TIMESTAMPDIFF(MONTH, hire_date, SYSDATE()) >= 36
			) AS emp
WHERE 	RANK <= 5
;
```
-- 현재 사원들 중에서 입사일이 현재시점과 가장 가까운 순으로 순위를 매겨주세요.
```
SELECT 	employee_id,
			hire_date,
			RANK() over(ORDER BY hire_date DESC)
FROM		hr_employees
;
```
