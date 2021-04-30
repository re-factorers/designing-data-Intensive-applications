# JSON type in PostgreSQL

## json과 jsonb 타입

PostgreSQL에서는 JSON 형식을 필드로 선언해 저장할 수 있는데, 관련 타입은 json, json[], jsonb, jsonb[]으로 4가지이다. json과 jsonb의 차이점은 입력된 값 그대로를 저장할 것인지 최적화된 값을 저장할 것인지이다.

- json 타입 : 입력된 공백, 키 순서, 중복 등과 같은 모든 것을 그대로 저장한다. 저장 이후 json blob을 질의할 때마다 로드하고 구문을 분석하기 때문에 속도가 느린 단점이 있다.
- jsonb 타입 : json blob의 줄임말로 입력된 값을 질의에 최적화된 형태로 저장한다. 따라서 키의 순서, 중복 제거, 공백 제거 등이 발생해 초기 입력된 값의 형태와 다를 수 있다. 이미 저장 단계에서 최적화가 완료되었으므로 재분석이 필요하지 않아 질의 속도가 빠른 장점이 있다.

> 저장 이후 질의가 발생하지 않는다면 json 필드로, 잦은 질의가 발생한다고 예상이 되면 jsonb 필드로 저장하는 것이 좋다.

## 데이터 조회

json 데이터는 -> 와 ->> 두 개의 연산자를 이용하여 조회할 수 있다.

### ->로 키 조회하기

-> 연산자는 json의 키 값 유무를 조회할 수 있다. 먼저 아래와 같은 json 데이터가 DB에 저장되어 있다고 가정하자.

```
                 test_json
-----------------------------------------------
{"age": "21", "name": "LEE"}
{"age": "22", "name": "KIM"}
{"age": "15", "name": "PARK", "update": "True"}
```

위와 같은 형태일 때, update 키가 존재하는 데이터만 추출하는 쿼리는 아래와 같다.

```
select *
from test
where test_json -> 'update' is not null;
```

만약 해당 키에 해당하는 값을 추출하고 싶으면 select 절에도 동일하게 -> 연산자를 사용하면 된다.

```
select test_json -> 'update' as update
from test
where test_json -> 'update' is not null;
```

### ->> 로 값 조회하기

->> 연산자는 json의 키에 해당하는 값을 조회할 수 있고 텍스트 형식으로 반환이 된다. 위에서 DB에 저장되어 있다고 가정한 데이터에서 update 키의 값을 추출하는 쿼리는 아래와 같다.

```
select test_json ->> 'update'
from test
where test_json ->> 'update' is not null
```

쿼리의 결과를 보면 -> 연산자와 ->> 연산자의 결과가 동일한 것으로 보이지만 타입까지 비교를 하면 다르다는 것을 볼 수 있다.

```
select test_json -> 'update' as update,
test_json ->> 'update as update2'
from test
where test_json -> 'update' is not null;

update          update2
jsonb            text
----------------------
true             true
```

### json 필드 내 리스트 타입의 값 추출하기

먼저 아래와 같은 형식으로 json 데이터가 저장되어 있다고 가정하자. 여기서 친구의 순서는 가장 친한 순서로 저장되어 있다고 생각한다.

```
                 test_json
-----------------------------------------------
{"age": "21", "name": "LEE", "friend": ["KIM", "PARK"]}
{"age": "22", "name": "KIM", "friend": ["LEE", "PARK"]}
{"age": "15", "name": "PARK", "friend": ["KIM"], "update": "True"}
```

아래는 위 데이터에서 1번째 입력된 친구가 KIM인 케이스를 추출하는 예시이다.

```
select *
from test
where test_json -> 'friend' ->> 0 = 'KIM';
```

먼저 컬럼에서 -> 연산자를 사용해 friend의 키에 해당하는 값을 jsonb 형태로 추출하고 0번째에 해당하는 데이터를 ->> 연산자를 사용해 텍스트 타입으로 추출해 비교한다.

만약 리스트에 접근하는 방식은 ->> 연산자가 아닌 -> 연산자를 사용한다면 결과값은 jsonb 타입이므로 'KIM' 으로 비교하면 타입오류가 발생한다. 오류가 안나도록 하려면 아래와 같이 큰따옴표를 붙여서 조회하거나 (저장된 형태 그대로) ? 연산자를 사용해 'KIM' 그대로를 조회할 수 있다.

```
select *
from test
where test_json -> 'friend' -> 0 = '"KIM"';
```

또는

```
select *
from test
where test_json -> 'friend' -> 0 ? 'KIM';
```

> ? 연산자는 jsonb 타입에서 키가 존재하는지 비교하는 구문이다. 따라서 ->> 로 결과를 추출한 다음 ? 연산자를 사용하면 type case error를 볼 수 있다. (->> 의 결과 타입은 text이고 ?는 jsonb에 해당하는 연산자이기 때문)