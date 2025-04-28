### useQuery

#### isLoading

- 비동기 처리 성공 유무를 표시한다.
- 캐시된 데이터가 없고 패칭 상태인 경우이다.

#### isFetching

- isLoading의 상위 집합으로 아직 완료되지 않은 쿼리를 의미한다.
- 캐쉬된 데이터가 존재해도 불러오는 `true`이다.

#### 둘의 차이

캐쉬된 데이터있다?

```
loading = false
isfetching = true
```

##### 그래서 둘이 언제써?

- 데이터 새로 가져올 때 =>isLoading
- 데이터 처음 가져올 때 =>isfetching

#### isError

- 에러가 발생했다면 문제를 반환할 수 있다.
- 리엑트는 기본적으로 3번의 시도 후 에러를 반환하기 떄문에 성능상 느릴 수 도 있다.
- `error`를 이용해서 어떤 에러가 발생했는지도 확인할 수 있다.

### Stale

- 데이터가 오래됬다는 뜻 즉 만료됬다는 것을 의믜한다.
- 데이터는 캐시되어있다.
  - 삭제 된 것이 아닌 갱신이 필요함을 의미한다.
- 데이터 `prefetch`는 데이터가 `stale`일 때만 트리거 된다.

#### stale 과 gc의 차이

- `stale` : 리패치 시간
- `gc` : 가비지 컬렉터에 캐시를 유지할 시간.

##### 예시

`fresh` , `cache` = 캐시 데이터를 보여주고 `refetch` 하지 않는다.

`!fresh` , `cache` = 캐시 데이터를 보여주고 `refetch` 한다. 그 후 새 데이터를 표시한다. (isFetching)

`!fresh` , `!cache` = 캐시 데이터가 없기때문에
`refetch` 동안 아무것도 표시하지 않는다.
(isLoading)
