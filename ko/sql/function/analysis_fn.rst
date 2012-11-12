**************
집계/분석 함수
**************

**집계 함수**

집계 함수(aggregate functions)는 행들의 그룹에 기반하여 하나의 결과를 반환한다. **GROUP BY** 절을 포함하면 각 그룹마다 한 행의 집계 결과를 반환한다. **GROUP BY**
절을 생략하면 전체 행에 대해 한 행의 집계 결과를 반환한다. **HAVING** 절은 **GROUP BY** 절이 있는 질의에 조건을 추가할 때 사용한다.

대부분의 집계 함수는 **DISTINCT** 를 사용할 수 있다. **GROUP BY ... HAVING** 절에 대해서는 `GROUP BY ... HAVING 절 <#syntax_syntax_retreive_group_htm>`_ 을 참고한다.

**분석 함수**

분석 함수(analytic functions)는 행들의 결과에 기반하여 집계 값을 계산한다. 분석 함수는 **OVER** 절 뒤의 *query_partition_clause* 에 의해 지정된 그룹들(이 절이 생략되면 모든 행을 하나의 그룹으로 봄)을 기준으로 한 개 이상의 행을 반환할 수 있다는 점에서 집계 함수와 다르다.

분석 함수는 특정 행 집합에 대해 다양한 통계를 허용하기 위해 기존의 집계 함수들 일부에 **OVER** 라는 새로운 분석 절이 함께 사용된다. ::

	function_name ( [argument_list ] ) OVER (<analytic_clause>)
	 
	<analytic_clause>::=
		 [ <query_partition_clause> ] [ <order_by_clause> ]
		
	<query_partition_clause>::=
		PARTITION BY value_expr [, value_expr ]...
	 
	<order_by_clause>::=
		ORDER BY { expr | position | column_alias } [ ASC | DESC ]
			[, { expr | position | column_alias } [ ASC | DESC ] ] ...

*   <*query_partition_clause*> : 하나 이상의 *value_expr* 에 기반한 그룹들로, 질의 결과를 분할하기 위해 **PARTITION BY** 절을 사용한다.
*   <*order_by_clause*> : <*query_partition_clause*>에 의한 분할(partition) 내에서 데이터의 정렬 방식을 명시한다. 여러 개의 키로 정렬할 수 있다. <*query_partition_clause*>가 생략될 경우 전체 결과 셋 내에서 데이터를 정렬한다. 정렬된 순서에 의해 이전 값을 포함하여 누적한 레코드의 컬럼 값을 대상으로 함수를 적용하여 계산한다.

AVG 함수
========

.. function:: AVG ( [ { DISTINCT | DISTINCTROW } | UNIQUE | ALL ] expression )

	**AVG** 함수는 모든 행에 대한 연산식 값의 산술 평균을 구한다. 하나의 연산식 *expression* 만 인자로 지정되며, 연산식 앞에 **DISTINCT** 또는 **UNIQUE** 키워드를 포함시키면 연산식 값 중 중복을 제거한 후 평균을 구하고, 키워드가 생략되거나 **ALL** 인 경우에는 모든 값에 대해서 평균을 구한다.

	:param expression: 수치 값을 반환하는 임의의 연산식을 지정한다. 컬렉션 타입의 데이터를 반환하는 연산식은 지정될 수 없다.
	:param ALL: 모든 값에 대해 평균을 구하기 위해 사용되며, 기본값이다.
	:param DISTINCT, UNIQUE: 중복이 제거된 유일한 값에 대해서만 평균을 구하기 위해 사용된다.
	:rtype: DOUBLE

	다음은 *demodb* 에서 한국이 획득한 금메달의 평균 수를 반환하는 예제이다.

	.. code-block:: sql
	
		SELECT AVG(gold)
		FROM participant
		WHERE nation_code = 'KOR'; 
						 avg(gold)
		==========================
			 9.600000000000000e+00
	 
	다음은 *demodb* 에서 nation_code가 'AU'로 시작하는 국가에 대해 연도 별로 획득한 금메달 수와 해당 연도까지의 금메달 누적에 대한 평균 합계를 출력하는 예제이다.

	.. code-block:: sql

		SELECT host_year, nation_code, gold,
		AVG(gold) OVER (PARTITION BY nation_code ORDER BY host_year) avg_gold
		FROM participant WHERE nation_code like 'AU%';
		 
			host_year  nation_code                  gold               avg_gold
		=======================================================================
				 1988  'AUS'                           3  3.000000000000000e+00
				 1992  'AUS'                           7  5.000000000000000e+00
				 1996  'AUS'                           9  6.333333333333333e+00
				 2000  'AUS'                          16  8.750000000000000e+00
				 2004  'AUS'                          17  1.040000000000000e+01
				 1988  'AUT'                           1  1.000000000000000e+00
				 1992  'AUT'                           0  5.000000000000000e-01
				 1996  'AUT'                           0  3.333333333333333e-01
				 2000  'AUT'                           2  7.500000000000000e-01
				 2004  'AUT'                           2  1.000000000000000e+00

	다음은 위 예제에서 **OVER** 분석 절 이하의 "ORDER BY host_year" 절을 제거한 것으로, avg_gold의 값은 모든 연도의 금메달 평균으로 nation_code별로 각 연도에서 모두 같은 값을 가진다.

	.. code-block:: sql
	
		SELECT host_year, nation_code, gold, AVG(gold) OVER (PARTITION BY nation_code) avg_gold
		FROM participant WHERE nation_code LIKE 'AU%';
		 
			host_year  nation_code                  gold                  avg_gold
		==========================================================================
				 2004  'AUS'                          17     1.040000000000000e+01
				 2000  'AUS'                          16     1.040000000000000e+01
				 1996  'AUS'                           9     1.040000000000000e+01
				 1992  'AUS'                           7     1.040000000000000e+01
				 1988  'AUS'                           3     1.040000000000000e+01
				 2004  'AUT'                           2     1.000000000000000e+00
				 2000  'AUT'                           2     1.000000000000000e+00
				 1996  'AUT'                           0     1.000000000000000e+00
				 1992  'AUT'                           0     1.000000000000000e+00
				 1988  'AUT'                           1     1.000000000000000e+00

COUNT 함수
==========

.. function:: COUNT ( * | [ { DISTINCT | DISTINCTROW } | UNIQUE | ALL ] expression )

	**COUNT** 함수는 질의문이 반환하는 결과 행들의 개수를 반환한다. 별표(*)를 지정하면 조건을 만족하는 모든 행(**NULL** 값을 가지는 행 포함)의 개수를 반환하며, **DISTINCT** 또는 **UNIQUE** 키워드를 연산식 앞에 지정하면 중복을 제거한 후 유일한 값을 가지는 행(**NULL** 값을 가지는 행은 포함하지 않음)의 개수만 반환한다. 따라서, 반환되는 값은 항상 정수이며, **NULL** 은 반환되지 않는다.

	:param expression: 임의의 연산식이다.
	:param ALL: 주어진 expression의 모든 행의 개수를 구하기 위해 사용되며, 기본값이다.
	:param DISTINCT, UNIQUE: 중복이 제거된 유일한 값을 가지는 행의 개수를 구하기 위해 사용된다.
	:rtype: INT
	
	연산식 *expression* 은 수치형 또는 문자열 타입은 물론, 컬렉션 타입 칼럼과 오브젝트 도메인(사용자 정의 클래스 또는 멀티미디어 클래스)을 가지는 칼럼도 지정될 수 있다.

	다음은 *demodb* 에서 역대 올림픽 중에서 마스코트가 존재했었던 올림픽의 수를 반환하는 예제이다.

	.. code-block:: sql
	
		SELECT COUNT(*)
		FROM olympic
		WHERE mascot IS NOT NULL; 
			 count(*)
		=============
					9

	다음은 *demodb* 에서 nation_code가 'AUT'인 국가의 참가 선수의 종목(event)별 인원 수를 종목이 바뀔 때마다 누적하여 출력한 예제이다. 가장 마지막 줄에는 모든 인원 수가 출력된다.

	.. code-block:: sql
	
		SELECT nation_code, event,name, COUNT(*) OVER (ORDER BY event) co
		FROM athlete WHERE nation_code='AUT';
		   nation_code           event                 name                           co
		===============================================================================
		  'AUT'                 'Athletics'           'Kiesl Theresia'                2
		  'AUT'                 'Athletics'           'Graf Stephanie'                2
		  'AUT'                 'Equestrian'          'Boor Boris'                    6
		  'AUT'                 'Equestrian'          'Fruhmann Thomas'               6
		  'AUT'                 'Equestrian'          'Munzner Joerg'                 6
		  'AUT'                 'Equestrian'          'Simon Hugo'                    6
		  'AUT'                 'Judo'                'Heill Claudia'                 9
		  'AUT'                 'Judo'                'Seisenbacher Peter'            9
		  'AUT'                 'Judo'                'Hartl Roswitha'                9
		  'AUT'                 'Rowing'              'Jonke Arnold'                 11
		  'AUT'                 'Rowing'              'Zerbst Christoph'             11
		  'AUT'                 'Sailing'             'Hagara Roman'                 15
		  'AUT'                 'Sailing'             'Steinacher Hans Peter'        15
		  'AUT'                 'Sailing'             'Sieber Christoph'             15
		  'AUT'                 'Sailing'             'Geritzer Andreas'             15
		  'AUT'                 'Shooting'            'Waibel Wolfram Jr.'           17
		  'AUT'                 'Shooting'            'Planer Christian'             17
		  'AUT'                 'Swimming'            'Rogan Markus'                 18
 
DENSE_RANK 함수
===============

.. function:: DENSE_RANK() OVER ( [partition_by_clause] [order_by_clause] )

	**PARTITION BY** 절에 의한 칼럼 값의 그룹에서 값의 순위를 계산하여 **INTEGER** 로 출력하며, 분석 함수로만 사용된다. 공동 순위가 존재해도 그 다음 순위는 1을 더한다. 예를 들어, 13위에 해당하는 행이 3개여도 그 다음 행의 순위는 16위가 아니라 14위가 된다. 반면, :func:`RANK` 함수는 이와 달리 공동 순위의 개수만큼을 더해 다음 순위의 값을 계산한다.

	:rtype: INT

	다음은 역대 올림픽에서 연도별로 금메달을 많이 획득한 국가의 금메달 개수와 순위를 출력하는 예제이다. 공동 순위의 개수는 무시하고 다음 순위 값은 항상 1을 더한다.

	.. code-block:: sql
	
		SELECT host_year, nation_code, gold,
		DENSE_RANK() OVER (PARTITION BY host_year ORDER BY gold DESC) AS d_rank
		FROM participant;
		 
		host_year  nation_code                  gold       d_rank
		=============================================================
				 1988  'URS'                          55            1
				 1988  'GDR'                          37            2
				 1988  'USA'                          36            3
				 1988  'KOR'                          12            4
				 1988  'HUN'                          11            5
				 1988  'FRG'                          11            5
				 1988  'BUL'                          10            6
				 1988  'ROU'                           7            7
				 1988  'ITA'                           6            8
				 1988  'FRA'                           6            8
				 1988  'KEN'                           5            9
				 1988  'GBR'                           5            9
				 1988  'CHN'                           5            9
		...
				 1988  'CHI'                           0           14
				 1988  'ARG'                           0           14
				 1988  'JAM'                           0           14
				 1988  'SUI'                           0           14
				 1988  'SWE'                           0           14
				 1992  'EUN'                          45            1
				 1992  'USA'                          37            2
				 1992  'GER'                          33            3
		...
				 2000  'RSA'                           0           15
				 2000  'NGR'                           0           15
				 2000  'JAM'                           0           15
				 2000  'BRA'                           0           15
				 2004  'USA'                          36            1
				 2004  'CHN'                          32            2
				 2004  'RUS'                          27            3
				 2004  'AUS'                          17            4
				 2004  'JPN'                          16            5
				 2004  'GER'                          13            6
				 2004  'FRA'                          11            7
				 2004  'ITA'                          10            8
				 2004  'UKR'                           9            9
				 2004  'CUB'                           9            9
				 2004  'GBR'                           9            9
				 2004  'KOR'                           9            9
		...
				 2004  'EST'                           0           17
				 2004  'SLO'                           0           17
				 2004  'SCG'                           0           17
				 2004  'FIN'                           0           17
				 2004  'POR'                           0           17
				 2004  'MEX'                           0           17
				 2004  'LAT'                           0           17
				 2004  'PRK'                           0           17

GROUP_CONCAT 함수
=================

.. function:: GROUP_CONCAT([DISTINCT] {col | expression} [ORDER BY {col | unsigned_int} [ASC | DESC]] [SEPARATOR str_val])

	**GROUP_CONCAT** 함수는 그룹에서 **NULL** 이 아닌 값들을 연결하여 결과 문자열을 **VARCHAR** 타입으로 반환한다. 질의 결과 행이 없거나 **NULL** 값만 있으면 **NULL** 을 반환한다. 
	
	:param expression: 수치 또는 문자열을 반환하는 하나의 연산식
	:param str_val: 구분자로 쓰일 문자열
	:param DISTINCT: 결과에서 중복되는 값을 제거한다.
	:param ORDER BY: 결과 값의 순서를 지정한다.
	:param SEPARATOR: 결과 값 사이에 구분할 구분자를 지정한다. 생략하면 기본값인 쉼표(,)를 구분자로 사용한다.
	:rtype: STRING

	리턴 값의 최대 크기는 시스템 파라미터 **group_concat_max_len** 의 설정을 따른다. 기본값은 **1024** 바이트이며, 최소값은 4바이트, 최대값은 33,554,432바이트이다. 최대값을 초과하면 **NULL** 을 반환한다.

	중복되는 값을 제거하려면 **DISTINCT** 절을 사용하면 된다. 그룹 결과의 값 사이에 사용되는 기본 구분자는 쉼표(,)이며, 구분자를 명시적으로 표현하려면 **SEPARATOR** 절과 그 뒤에 구분자로 사용할 문자열을 추가한다. 구분자를 제거하려면 **SEPARATOR** 절 뒤에 빈 문자열(empty string)을 입력한다.

	결과 문자열에 문자형 데이터 타입이 아닌 다른 타입이 전달되면, 에러를 반환한다.

	**GROUP_CONCAT** 함수를 사용하려면 다음의 조건을 만족해야 한다.
	*   입력 인자로 하나의 표현식(또는 칼럼)만 허용한다.
	*   **ORDER BY** 를 이용한 정렬은 오직 인자로 사용되는 표현식(또는 칼럼)에 의해서만 가능하다.
	*   구분자로 사용되는 문자열은 문자형 타입만 허용하며, 다른 타입은 허용하지 않는다.

	.. code-block:: sql

		SELECT GROUP_CONCAT(s_name) FROM code;
		  group_concat(s_name)
		======================
		  'X,W,M,B,S,G'
		 
		SELECT GROUP_CONCAT(s_name ORDER BY s_name SEPARATOR ':') from code;
		  group_concat(s_name order by s_name separator ':')
		======================
		  'B:G:M:S:W:X'
		 
		CREATE TABLE t(i int);
		INSERT INTO t VALUES (4),(2),(3),(6),(1),(5);
		 
		SELECT GROUP_CONCAT(i*2+1 ORDER BY 1 SEPARATOR '') FROM t;
		  group_concat(i*2+1 order by 1 separator '')
		======================
		  '35791113'
  
MAX 함수
========

.. function:: MAX ( [ { DISTINCT | DISTINCTROW } | UNIQUE | ALL ] expression )

	**MAX** 함수는 모든 행에 대하여 연산식 값 중 최대 값을 구한다. 하나의 연산식 *expression* 만 인자로 지정된다. 문자열을 반환하는 연산식에 대해서는 사전 순서를 기준으로 뒤에 나오는 문자열이 최대 값이 되고, 수치를 반환하는 연산식에 대해서는 크기가 가장 큰 값이 최대 값이다.

	:param expression: 수치 또는 문자열을 반환하는 하나의 연산식을 지정한다. 컬렉션 타입의 데이터를 반환하는 연산식은 지정할 수 없다.
	:param ALL: 모든 값에 대해 최대 값을 구하기 위해 사용되며, 기본값이다.
	:param DISTINCT, UNIQUE: 중복이 제거된 유일한 값에 대해서 최대 값을 구하기 위해 사용된다.
	:rtype: expression의 타입

	다음은 올림픽 대회 중 한국이 획득한 최대 금메달의 수를 반환하는 예제이다.

	.. code-block:: sql
	
		SELECT MAX(gold) FROM participant WHERE nation_code = 'KOR';
			max(gold)
		=============
				   12

	다음은 역대 올림픽 대회 중 국가 코드와 연도 순대로 nation_code가 'AU'로 시작하는 국가가 획득한 금메달 수와 해당 국가의 역대 최대 금메달의 수를 같이 출력하는 예제이다.

	.. code-block:: sql
	
		SELECT host_year, nation_code, gold,
		MAX(gold) OVER (PARTITION BY nation_code) mx_gold
		FROM participant WHERE nation_code like 'AU%' ORDER BY nation_code, host_year;
		 
			host_year  nation_code                  gold      mx_gold
		=============================================================
				 1988  'AUS'                           3           17
				 1992  'AUS'                           7           17
				 1996  'AUS'                           9           17
				 2000  'AUS'                          16           17
				 2004  'AUS'                          17           17
				 1988  'AUT'                           1            2
				 1992  'AUT'                           0            2
				 1996  'AUT'                           0            2
				 2000  'AUT'                           2            2
				 2004  'AUT'                           2            2

MIN 함수
========

.. function:: MIN ( [ { DISTINCT | DISTINCTROW } | UNIQUE | ALL ] expression )

	**MIN** 함수는 모든 행에 대하여 연산식 값 중 최소?값을 구한다. 하나의 연산식 *expression* 만 인자로 지정된다. 문자열을 반환하는 연산식에 대해서는 사전 순서를 기준으로 앞에 나오는 문자열이 최소 값이 되고, 수치를 반환하는 연산식에 대해서는 크기가 가장 작은 값이 최소 값이다.

	:param expression: 수치 또는 문자열을 반환하는 하나의 연산식을 지정한다. 컬렉션 타입의 데이터를 반환하는 연산식은 지정할 수 없다.
	:param ALL: 모든 값에 대해 최소 값을 구하기 위해 사용되며, 기본값이다.
	:param DISTINCT, UNIQUE: 중복이 제거된 유일한 값에 대해서 최소 값을 구하기 위해 사용된다.
	:rtype: expression의 타입

	다음은 *demodb* 에서 올림픽 대회 중 한국이 획득한 최소 금메달의 수를 반환하는 예제이다.

	.. code-block:: sql
	
		SELECT MIN(gold) FROM participant WHERE nation_code = 'KOR';
			min(gold)
		=============
					7

	다음은 역대 올림픽 대회 중 국가 코드와 연도 순대로 nation_code가 'AU'로 시작하는 국가가 획득한 금메달 수와 해당 국가의 역대 최소 금메달의 수를 같이 출력하는 예제이다.

	.. code-block:: sql

		SELECT host_year, nation_code, gold,
		MIN(gold) OVER (PARTITION BY nation_code) mn_gold
		FROM participant WHERE nation_code like 'AU%' ORDER BY nation_code, host_year;
		 
			host_year  nation_code                  gold      mn_gold
		=============================================================
				 1988  'AUS'                           3            3
				 1992  'AUS'                           7            3
				 1996  'AUS'                           9            3
				 2000  'AUS'                          16            3
				 2004  'AUS'                          17            3
				 1988  'AUT'                           1            0
				 1992  'AUT'                           0            0
				 1996  'AUT'                           0            0
				 2000  'AUT'                           2            0
				 2004  'AUT'                           2            0

RANK 함수
=========

.. function:: RANK() OVER ( [partition_by_clause] [order_by_clause] )

	**PARTITION BY** 절에 의한 칼럼 값의 그룹에서 값의 순위를 계산하여 **INTEGER** 로 출력하며, 분석 함수로만 사용된다. 공동 순위가 존재하면 그 다음 순위는 공동 순위의 개수를 더한 숫자이다. 예를 들어, 13위에 해당하는 행이 3개이면 그 다음 행의 순위는 14위가 아니라 16위가 된다. 반면, :func:`DENSE_RANK` 함수는 이와 달리 순위에 1을 더해 다음 순위의 값을 계산한다.

	:rtype: INT
	
	다음은 역대 올림픽에서 연도별로 금메달을 많이 획득한 국가의 금메달 개수와 순위를 출력하는 예제이다. 공동 순위의 다음 순위 값은 공동 순위의 개수를 더한다.

	.. code-block:: sql
	
		SELECT host_year, nation_code, gold,
		RANK() OVER (PARTITION BY host_year ORDER BY gold DESC) AS g_rank
		FROM participant;
		 
			host_year  nation_code                  gold       g_rank
		=============================================================
				 1988  'URS'                          55            1
				 1988  'GDR'                          37            2
				 1988  'USA'                          36            3
				 1988  'KOR'                          12            4
				 1988  'HUN'                          11            5
				 1988  'FRG'                          11            5
				 1988  'BUL'                          10            7
				 1988  'ROU'                           7            8
				 1988  'ITA'                           6            9
				 1988  'FRA'                           6            9
				 1988  'KEN'                           5           11
				 1988  'GBR'                           5           11
				 1988  'CHN'                           5           11
		...
				 1988  'CHI'                           0           32
				 1988  'ARG'                           0           32
				 1988  'JAM'                           0           32
				 1988  'SUI'                           0           32
				 1988  'SWE'                           0           32
				 1992  'EUN'                          45            1
				 1992  'USA'                          37            2
				 1992  'GER'                          33            3
		...
				 2000  'RSA'                           0           52
				 2000  'NGR'                           0           52
				 2000  'JAM'                           0           52
				 2000  'BRA'                           0           52
				 2004  'USA'                          36            1
				 2004  'CHN'                          32            2
				 2004  'RUS'                          27            3
				 2004  'AUS'                          17            4
				 2004  'JPN'                          16            5
				 2004  'GER'                          13            6
				 2004  'FRA'                          11            7
				 2004  'ITA'                          10            8
				 2004  'UKR'                           9            9
				 2004  'CUB'                           9            9
				 2004  'GBR'                           9            9
				 2004  'KOR'                           9            9
		...
				 2004  'EST'                           0           57
				 2004  'SLO'                           0           57
				 2004  'SCG'                           0           57
				 2004  'FIN'                           0           57
				 2004  'POR'                           0           57
				 2004  'MEX'                           0           57
				 2004  'LAT'                           0           57
				 2004  'PRK'                           0           57

ROW_NUMBER 함수
===============

.. function:: ROW_NUMBER() OVER ( [partition_by_clause] [order_by_clause] )

	**PARTITION BY** 절에 의한 칼럼 값의 그룹에서 각 행에 고유한 일련번호를 1부터 순서대로 부여하여 **INTEGER** 로 출력하며, 분석 함수로만 사용된다.

	:rtype: INT

	다음은 역대 올림픽에서 연도별로 금메달을 많이 획득한 국가의 금메달 개수에 따라 일련번호를 출력하되, 금메달 개수가 같은 경우에는 nation_code의 알파벳 순서대로 출력하는 예제이다.

	.. code-block:: sql
	
		SELECT host_year, nation_code, gold,
		ROW_NUMBER() OVER (PARTITION BY host_year ORDER BY gold DESC) AS r_num
		FROM participant;
		 
			host_year  nation_code                  gold       r_num
		=============================================================
				 1988  'URS'                          55            1
				 1988  'GDR'                          37            2
				 1988  'USA'                          36            3
				 1988  'KOR'                          12            4
				 1988  'FRG'                          11            5
				 1988  'HUN'                          11            6
				 1988  'BUL'                          10            7
				 1988  'ROU'                           7            8
				 1988  'FRA'                           6            9
				 1988  'ITA'                           6           10
				 1988  'CHN'                           5           11
		...
				 1988  'YEM'                           0          152
				 1988  'YMD'                           0          153
				 1988  'ZAI'                           0          154
				 1988  'ZAM'                           0          155
				 1988  'ZIM'                           0          156
				 1992  'EUN'                          45            1
				 1992  'USA'                          37            2
				 1992  'GER'                          33            3
		...
				 2000  'VIN'                           0          194
				 2000  'YEM'                           0          195
				 2000  'ZAM'                           0          196
				 2000  'ZIM'                           0          197
				 2004  'USA'                          36            1
				 2004  'CHN'                          32            2
				 2004  'RUS'                          27            3
				 2004  'AUS'                          17            4
				 2004  'JPN'                          16            5
				 2004  'GER'                          13            6
				 2004  'FRA'                          11            7
				 2004  'ITA'                          10            8
				 2004  'CUB'                           9            9
				 2004  'GBR'                           9           10
				 2004  'KOR'                           9           11
		...
				 2004  'UGA'                           0          195
				 2004  'URU'                           0          196
				 2004  'VAN'                           0          197
				 2004  'VEN'                           0          198
				 2004  'VIE'                           0          199
				 2004  'VIN'                           0          200
				 2004  'YEM'                           0          201
				 2004  'ZAM'                           0          202

STDDEV 함수, STDDEV_POP 함수
============================

.. function:: STDDEV( [ { DISTINCT | DISTINCTROW } | UNIQUE | ALL] expression )
.. function:: STDDEV_POP( [ { DISTINCT | DISTINCTROW } | UNIQUE | ALL] expression )

	**STDDEV** 함수와 **STDDEV_POP** 함수는 동일하며, 모든 행에 대한 연산식 값들에 대한 표준편차, 즉 모표준편차를 반환한다. **STDDEV_POP** 함수가 SQL:1999 표준이다. 하나의 연산식 *expression* 만 인자로 지정되며, 연산식 앞에 **DISTINCT** 또는 **UNIQUE** 키워드를 포함시키면 연산식 값 중 중복을 제거한 후, 표본 표준편차를 구하고, 키워드가 생략되거나 **ALL** 인 경우에는 모든 값에 대해 표본 표준편차를 구한다.

	:param expression: 수치를 반환하는 하나의 연산식을 지정한다.
	:param ALL: 모든 값에 대해 표준 편차를 구하기 위해 사용되며, 기본값이다.
	:param DISTINCT, UNIQUE: 중복이 제거된 유일한 값에 대해서만 표준 편차를 구하기 위해 사용된다.
	:rtype: DOUBLE
	
	리턴 값은 :func:`VAR_POP` 리턴 값의 제곱근과 같으며 **DOUBLE** 타입이다. 결과 계산에 사용할 행이 없으면 **NULL** 을 반환한다.

	다음은 함수에 적용된 공식이다.
	
	.. math:: STDDEV_POP = [ (1/N) * SUM( { xI - AVG(x) }^2) ]^1/2

	.. warning:: CUBRID 2008 R3.1 이하 버전에서 **STDDEV** 함수는 :func:`STDDEV_SAMP` 와 같은 기능을 수행했다.

	다음은 전체 과목에 대해 전체 학생의 모 표준 편차를 출력하는 예제이다.

	.. code-block:: sql
		
		CREATE TABLE student (name VARCHAR(32), subjects_id INT, score DOUBLE);
		INSERT INTO student VALUES
		('Jane',1, 78),
		('Jane',2, 50),
		('Jane',3, 60),
		('Bruce', 1, 63),
		('Bruce', 2, 50),
		('Bruce', 3, 80),
		('Lee', 1, 85),
		('Lee', 2, 88),
		('Lee', 3, 93),
		('Wane', 1, 32),
		('Wane', 2, 42),
		('Wane', 3, 99),
		('Sara', 1, 17),
		('Sara', 2, 55),
		('Sara', 3, 43);
		 
		SELECT STDDEV_POP(score) FROM student;
		 
				 stddev_pop(score)
		==========================
			 2.329711474744362e+01

	다음은 각 과목(subjects_id)별로 전체 학생의 점수와 모 표준편차를 함께 출력하는 예제이다.
	
	.. code-block:: sql	

		SELECT subjects_id, name, score, STDDEV_POP(score) OVER(PARTITION BY subjects_id) std_pop FROM student ORDER BY subjects_id, name;
		 
		  subjects_id  name                                     score                   std_pop
		=======================================================================================
					1  'Bruce'                  6.300000000000000e+01     2.632869157402243e+01
					1  'Jane'                   7.800000000000000e+01     2.632869157402243e+01
					1  'Lee'                    8.500000000000000e+01     2.632869157402243e+01
					1  'Sara'                   1.700000000000000e+01     2.632869157402243e+01
					1  'Wane'                   3.200000000000000e+01     2.632869157402243e+01
					2  'Bruce'                  5.000000000000000e+01     1.604992211819110e+01
					2  'Jane'                   5.000000000000000e+01     1.604992211819110e+01
					2  'Lee'                    8.800000000000000e+01     1.604992211819110e+01
					2  'Sara'                   5.500000000000000e+01     1.604992211819110e+01
					2  'Wane'                   4.200000000000000e+01     1.604992211819110e+01
					3  'Bruce'                  8.000000000000000e+01     2.085185843036539e+01
					3  'Jane'                   6.000000000000000e+01     2.085185843036539e+01
					3  'Lee'                    9.300000000000000e+01     2.085185843036539e+01
					3  'Sara'                   4.300000000000000e+01     2.085185843036539e+01
					3  'Wane'                   9.900000000000000e+01     2.085185843036539e+01

STDDEV_SAMP 함수
================

.. function:: STDDEV_SAMP( [ { DISTINCT | DISTINCTROW } | UNIQUE | ALL] expression )

	**STDDEV_SAMP** 함수는 표본 표준편차를 구한다. 하나의 연산식 *expression* 만 인자로 지정되며, 연산식 앞에 **DISTINCT** 또는 **UNIQUE** 키워드를 포함시키면 연산식 값 중 중복을 제거한 후, 표본 표준편차를 구하고, 키워드가 생략되거나 **ALL** 인 경우에는 모든 값에 대해 표본 표준편차를 구한다.

	:param expression: 수치를 반환하는 하나의 연산식을 지정한다.
	:param ALL: 모든 값에 대해 표준 편차를 구하기 위해 사용되며, 기본값이다.
	:param DISTINCT, UNIQUE: 중복이 제거된 유일한 값에 대해서만 표준 편차를 구하기 위해 사용된다.
	:rtype: DOUBLE
	
	리턴 값은 :func:`VAR_SAMP` 리턴 값의 제곱근과 같으며 **DOUBLE** 타입이다. 결과 계산에 사용할 행이 없으면 **NULL** 을 반환한다.

	다음은 함수에 적용된 공식이다.

	.. math:: STDDEV_SAMP = [ { 1 / (N-1) } * SUM( { xI - mean(x) }^2) ]^1/2

	다음은 전체 과목에 대해 전체 학생의 표본 표준 편차를 출력하는 예제이다.

	.. code-block:: sql
	
		CREATE TABLE student (name VARCHAR(32), subjects_id INT, score DOUBLE);
		INSERT INTO student VALUES
		('Jane',1, 78),
		('Jane',2, 50),
		('Jane',3, 60),
		('Bruce', 1, 63),
		('Bruce', 2, 50),
		('Bruce', 3, 80),
		('Lee', 1, 85),
		('Lee', 2, 88),
		('Lee', 3, 93),
		('Wane', 1, 32),
		('Wane', 2, 42),
		('Wane', 3, 99),
		('Sara', 1, 17),
		('Sara', 2, 55),
		('Sara', 3, 43);
		 
		SELECT STDDEV_SAMP(score) FROM student;
		 
				stddev_samp(score)
		==========================
			 2.411480477888654e+01

	다음은 각 과목(subjects_id)별로 전체 학생의 점수와 표본 표준편차를 함께 출력하는 예제이다.

	.. code-block:: sql
	
		SELECT subjects_id, name, score, STDDEV_SAMP(score) OVER(PARTITION BY subjects_id) std_samp FROM student ORDER BY subjects_id, name;
		 
		  subjects_id  name                                     score                  std_samp
		=======================================================================================
					1  'Bruce'                  6.300000000000000e+01     2.943637205907005e+01
					1  'Jane'                   7.800000000000000e+01     2.943637205907005e+01
					1  'Lee'                    8.500000000000000e+01     2.943637205907005e+01
					1  'Sara'                   1.700000000000000e+01     2.943637205907005e+01
					1  'Wane'                   3.200000000000000e+01     2.943637205907005e+01
					2  'Bruce'                  5.000000000000000e+01     1.794435844492636e+01
					2  'Jane'                   5.000000000000000e+01     1.794435844492636e+01
					2  'Lee'                    8.800000000000000e+01     1.794435844492636e+01
					2  'Sara'                   5.500000000000000e+01     1.794435844492636e+01
					2  'Wane'                   4.200000000000000e+01     1.794435844492636e+01
					3  'Bruce'                  8.000000000000000e+01     2.331308645374953e+01
					3  'Jane'                   6.000000000000000e+01     2.331308645374953e+01
					3  'Lee'                    9.300000000000000e+01     2.331308645374953e+01
					3  'Sara'                   4.300000000000000e+01     2.331308645374953e+01
					3  'Wane'                   9.900000000000000e+01     2.331308645374953e+01

SUM 함수
========

.. function:: SUM ( [ { DISTINCT | DISTINCTROW } | UNIQUE | ALL ] expression )

	**SUM** 함수는 모든 행에 대한 연산식 값들의 합계를 반환한다. 하나의 연산식 *expression* 만 인자로 지정되며, 연산식 앞에 **DISTINCT** 또는 **UNIQUE** 키워드를 포함시키면 연산식 값 중 중복을 제거한 후 합계를 구하고, 키워드가 생략되거나 **ALL** 인 경우에는 모든 값에 대해 합계를 구한다. 단일 값 수식을 **SUM** 함수의 입력으로 사용할 수 있다.

	:param expression: 수치를 반환하는 하나의 연산식을 지정한다.
	:param ALL: 모든 값에 대해 합계를 구하기 위해 사용되며, 기본으로 지정된다.
	:param DISTINCT, UNIQUE: 중복이 제거된 유일한 값에 대해서만 합계를 구하기 위해 사용된다.
	:rtype: expression의 타입

	다음은 *demodb* 에서 역대 올림픽에서 획득한 금메달 수의 합계를 기준으로 10위권 국가와 금메달 총 수를 출력하는 예제이다.

	.. code-block:: sql
		
		SELECT nation_code, SUM(gold) FROM participant GROUP BY nation_code
		ORDER BY SUM(gold) DESC
		FOR ORDERBY_NUM() BETWEEN 1 AND 10 ;
		 
		=== <Result of SELECT Command in Line 1> ===
		 
		  nation_code             sum(gold)
		===================================
		  'USA'                         190
		  'CHN'                          97
		  'RUS'                          85
		  'GER'                          79
		  'URS'                          55
		  'FRA'                          53
		  'AUS'                          52
		  'ITA'                          48
		  'KOR'                          48
		  'EUN'                          45

	다음은 *demodb* 에서 nation_code가 'AU'로 시작하는 국가에 대해 연도별로 획득한 금메달 수와 해당 연도까지의 금메달 누적 합계를 출력하는 예제이다.

	.. code-block:: sql
	
		SELECT host_year, nation_code, gold,
		SUM(gold) OVER (PARTITION BY nation_code ORDER BY host_year) sum_gold
		FROM participant WHERE nation_code LIKE 'AU%';
		 
			host_year  nation_code                  gold     sum_gold
		=============================================================
				 1988  'AUS'                           3            3
				 1992  'AUS'                           7           10
				 1996  'AUS'                           9           19
				 2000  'AUS'                          16           35
				 2004  'AUS'                          17           52
				 1988  'AUT'                           1            1
				 1992  'AUT'                           0            1
				 1996  'AUT'                           0            1
				 2000  'AUT'                           2            3
				 2004  'AUT'                           2            5

	다음은 위 예제에서 **OVER** 함수 이하의 "ORDER BY host_year" 절을 제거한 것으로, sum_gold의 값은 모든 연도의 금메달 합계로 각 연도에서 모두 같은 값을 가진다.

	.. code-block:: sql
	
		SELECT host_year, nation_code, gold, SUM(gold) OVER (PARTITION BY nation_code) sum_gold
		FROM participant WHERE nation_code LIKE 'AU%';
			host_year  nation_code                  gold     sum_gold
		=============================================================
				 2004  'AUS'                          17           52
				 2000  'AUS'                          16           52
				 1996  'AUS'                           9           52
				 1992  'AUS'                           7           52
				 1988  'AUS'                           3           52
				 2004  'AUT'                           2            5
				 2000  'AUT'                           2            5
				 1996  'AUT'                           0            5
				 1992  'AUT'                           0            5
				 1988  'AUT'                           1            5

VAR_POP 함수, VARIANCE 함수
===========================

.. function:: VAR_POP( [ DISTINCT | UNIQUE | ALL] expression )
.. function:: VARIANCE( [ DISTINCT | UNIQUE | ALL] expression )

	**VAR_POP** 함수와 **VARIANCE** 함수는 동일하며, 모든 행에 대한 연산식 값들에 대한 분산, 즉 모분산을 반환한다. 분모는 모든 행의 개수이다. 하나의 연산식 *expression* 만 인자로 지정되며, 연산식 앞에 **DISTINCT** 또는 **UNIQUE** 키워드를 포함시키면 연산식 값 중 중복을 제거한 후, 모분산을?구하고, 키워드가 생략되거나 **ALL** 인 경우에는 모든 값에 대해 모분산을 구한다.

	:param expression: 수치를 반환하는 하나의 연산식을 지정한다.
	:param ALL: 모든 값에 대해 모분산을 구하기 위해 사용되며, 기본값이다.
	:param DISTINCT, UNIQUE: 중복이 제거된 유일한 값에 대해서만 모분산을 구하기 위해 사용된다.
	:rtype: DOUBLE
	
	리턴 값은 **DOUBLE** 타입이며, 결과 계산에 사용할 행이 없으면 **NULL** 을 반환한다.

	다음은 함수에 적용된 공식이다.

	.. math:: VAR_POP = (1/N) * SUM( { xI - AVG(x) }^2 )

	.. warning:: CUBRID 2008 R3.1 이하 버전에서 **VARIANCE** 함수는 :func:`VAR_SAMP` 와 같은 기능을 수행했다.

	다음은 전체 과목에 대해 전체 학생의 모 분산을 출력하는 예제이다.

	.. code-block:: sql
	
		CREATE TABLE student (name VARCHAR(32), subjects_id INT, score DOUBLE);
		INSERT INTO student VALUES
		('Jane',1, 78),
		('Jane',2, 50),
		('Jane',3, 60),
		('Bruce', 1, 63),
		('Bruce', 2, 50),
		('Bruce', 3, 80),
		('Lee', 1, 85),
		('Lee', 2, 88),
		('Lee', 3, 93),
		('Wane', 1, 32),
		('Wane', 2, 42),
		('Wane', 3, 99),
		('Sara', 1, 17),
		('Sara', 2, 55),
		('Sara', 3, 43);
		 
		SELECT VAR_POP(score) FROM student;
		 
					var_pop(score)
		==========================
			 5.427555555555550e+02

	다음은 각 과목(subjects_id)별로 전체 학생의 점수와 모 분산을 함께 출력하는 예제이다.

	.. code-block:: sql
	
		SELECT subjects_id, name, score, VAR_POP(score) OVER(PARTITION BY subjects_id) v_pop
		FROM student ORDER BY subjects_id, name;
		 
		  subjects_id  name                                     score                     v_pop
		=======================================================================================
					1  'Bruce'                  6.300000000000000e+01     6.931999999999998e+02
					1  'Jane'                   7.800000000000000e+01     6.931999999999998e+02
					1  'Lee'                    8.500000000000000e+01     6.931999999999998e+02
					1  'Sara'                   1.700000000000000e+01     6.931999999999998e+02
					1  'Wane'                   3.200000000000000e+01     6.931999999999998e+02
					2  'Bruce'                  5.000000000000000e+01     2.575999999999999e+02
					2  'Jane'                   5.000000000000000e+01     2.575999999999999e+02
					2  'Lee'                    8.800000000000000e+01     2.575999999999999e+02
					2  'Sara'                   5.500000000000000e+01     2.575999999999999e+02
					2  'Wane'                   4.200000000000000e+01     2.575999999999999e+02
					3  'Bruce'                  8.000000000000000e+01     4.348000000000002e+02
					3  'Jane'                   6.000000000000000e+01     4.348000000000002e+02
					3  'Lee'                    9.300000000000000e+01     4.348000000000002e+02
					3  'Sara'                   4.300000000000000e+01     4.348000000000002e+02
					3  'Wane'                   9.900000000000000e+01     4.348000000000002e+02

VAR_SAMP 함수
=============

.. function:: VAR_SAMP( [ DISTINCT | UNIQUE | ALL] expression )

	**VAR_SAMP** 함수는 표본 분산을 반환한다. 분모는 모든 행의 개수 - 1이다. 하나의 연산식 *expression* 만 인자로 지정되며, 연산식 앞에 **DISTINCT** 또는 **UNIQUE** 키워드를 포함시키면 연산식 값 중 중복을 제거한 후, 표본 분산을?구하고, 키워드가 생략되거나 **ALL** 인 경우에는 모든 값에 대해 표본 분산을 구한다.

	:param expression: 수치를 반환하는 하나의 연산식을 지정한다.
	:param ALL: 모든 값에 대해 표본 분산을 구하기 위해 사용되며, 기본값이다.
	:param DISTINCT, UNIQUE: 중복이 제거된 유일한 값에 대해서만 표본 분산을 구하기 위해 사용된다.
	:rtype: DOUBLE
	
	리턴 값은 **DOUBLE** 타입이며, 결과 계산에 사용할 행이 없으면 **NULL** 을 반환한다.

	다음은 함수에 적용된 공식이다.

	.. math:: VAR_SAMP = { 1 / (N-1) } * SUM( { xI - mean(x) }^2 )

	다음은 전체 과목에 대해 전체 학생의 표본 분산을 출력하는 예제이다.
	
	.. code-block:: sql
	
		CREATE TABLE student (name VARCHAR(32), subjects_id INT, score DOUBLE);
		INSERT INTO student VALUES
		('Jane',1, 78),
		('Jane',2, 50),
		('Jane',3, 60),
		('Bruce', 1, 63),
		('Bruce', 2, 50),
		('Bruce', 3, 80),
		('Lee', 1, 85),
		('Lee', 2, 88),
		('Lee', 3, 93),
		('Wane', 1, 32),
		('Wane', 2, 42),
		('Wane', 3, 99),
		('Sara', 1, 17),
		('Sara', 2, 55),
		('Sara', 3, 43);
		 
		SELECT VAR_SAMP(score) FROM student;
				   var_samp(score)
		==========================
			 5.815238095238092e+02

	다음은 각 과목(subjects_id)별로 전체 학생의 점수와 표본 분산을 함께 출력하는 예제이다.

	.. code-block:: sql
	
		SELECT subjects_id, name, score, VAR_SAMP(score) OVER(PARTITION BY subjects_id) v_samp
		FROM student ORDER BY subjects_id, name;
		 
		  subjects_id  name                                     score                    v_samp
		=======================================================================================
					1  'Bruce'                  6.300000000000000e+01     8.665000000000000e+02
					1  'Jane'                   7.800000000000000e+01     8.665000000000000e+02
					1  'Lee'                    8.500000000000000e+01     8.665000000000000e+02
					1  'Sara'                   1.700000000000000e+01     8.665000000000000e+02
					1  'Wane'                   3.200000000000000e+01     8.665000000000000e+02
					2  'Bruce'                  5.000000000000000e+01     3.220000000000000e+02
					2  'Jane'                   5.000000000000000e+01     3.220000000000000e+02
					2  'Lee'                    8.800000000000000e+01     3.220000000000000e+02
					2  'Sara'                   5.500000000000000e+01     3.220000000000000e+02
					2  'Wane'                   4.200000000000000e+01     3.220000000000000e+02
					3  'Bruce'                  8.000000000000000e+01     5.435000000000000e+02
					3  'Jane'                   6.000000000000000e+01     5.435000000000000e+02
					3  'Lee'                    9.300000000000000e+01     5.435000000000000e+02
					3  'Sara'                   4.300000000000000e+01     5.435000000000000e+02
					3  'Wane'                   9.900000000000000e+01     5.435000000000000e+02