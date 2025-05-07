### 데이터를 미리 받아오기

#### prefetchQuery

- 데이터를 서버에서 미리 받아와 저장해놓는다. / 캐시에 저장된다.

#### setQueryData

- 프론트 측에서 쿼리 데이터를 받아온다 / 캐시에 추가한다.

#### palceholderData

- `useQuery` 실행 전 데이터를 제공한다. / 캐시에는 추가되지 않는다.

#### initialData

- `useQuery` 실행 전 데이터를 제공한다. / 캐시에는 추가된다.

### refetch

- refetch는 React Query에서 제공하는 함수로, 이미 가져온 데이터를 수동으로 다시 가져오는 기능

#### 설정

```ts
const { data } = useQuery({
  queryKey: ["todos"],
  queryFn: fetchTodos,
  refetchInterval: 5000, // 5초마다 자동 refetch
  refetchOnWindowFocus: true, // 창이 포커스될 때 refetch (기본값)
  refetchOnMount: true, // 컴포넌트 마운트 시 refetch (기본값)
  refetchOnReconnect: true, // 네트워크 재연결 시 refetch (기본값)
  staleTime: 1000 * 60 * 5, // 5분 동안은 데이터가 최신 상태로 간주됨
});
```

#### refetch vs invalidateQueries 차이

`refetc`

직접적인 실행: 특정 쿼리 인스턴스에 대한 즉시 재요청을 트리거
범위: 해당 useQuery 훅에서 반환된 특정 쿼리 인스턴스에만 영향
작동 방식: 쿼리 함수를 즉시 다시 호출하여 새 데이터를 가져온다.

`invalidateQueries`

간접적인 실행: 쿼리를 '오래된(stale)' 상태로 표시하고, React Query의 내부 시스템이 적절한 시점에 재요청을 결정
범위: 지정된 쿼리 키와 일치하는 모든 쿼리에 영향을 줌
작동 방식: 쿼리 캐시 상태를 변경하고, React Query의 일반적인 캐시 관리 규칙에 따라 재요청이 발생

### 낙관적 업데이트

서버에 요청을 보내기전 ui를 업데이트 하고 이후에 롤백하거나 반영하는 방법이다.

`setQueryData` : 서버 요청 없이 캐시 데이터를 즉시 변경한다.

### removeQueries

- 메서드는 쿼리 캐시에서 특정 쿼리를 완전히 제거하는 기능이다.
