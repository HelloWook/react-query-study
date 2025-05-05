### InfiniteQuery

- 무한 스크롤이나 더보기 기능 구현을 하는데 사용된다.
- Query에 페이지네이션 방식이 추가되었다 생각하면 편하다.

### 차이점

- 쿼리 데이터를 포함하는 객체 형태로 반환
- `getNextPageParam`과 `getPreviousPageParam` 옵션이 추가되어 더 불러올 데이터가 있는지 판단하고 해당 정보를 가져오는 데 사용
- `hasNextPage` 불리언 값이 제공되며, `getNextPageParam`이 `null`이나 `undefined`가 아닌 값을 반환할 때 `true`
- `hasPreviousPage` 불리언 값도 제공
- `isFetchingNextPage`와 `isFetchingPreviousPage` 불리언 값이 제공되어 백그라운드 새로고침 상태와 더 불러오기 상태를 구분

### 기본적 사용법

```tsx
const fetchUrl = async ({ pageParam = 1 }) => {
  const response = await fetch(`${initialUrl}?page=${pageParam}`);
  return response.json();
};

export function InfinitePeople() {
  const { data, fetchNextPage, hasNextPage, isFetching, isLoading, isError } =
    useInfiniteQuery({
      queryKey: ["sw-people"],
      queryFn: fetchUrl,
      initialPageParam: 1,
      getNextPageParam: (lastPage) => {
        if (lastPage.next) {
          const nextPageUrl = new URL(lastPage.next);
          return nextPageUrl.searchParams.get("page");
        }
        return undefined;
      },
    });

  if (isLoading) {
    return <div className="loading">로딩 중</div>;
  }

  if (isError) {
    return <div>에러 중</div>;
  }

  return (
    <InfiniteScroll
      initialLoad={false}
      loadMore={() => {
        /*다음 데이터가 있으며 패칭 중이 아닐 시 데이터 불러오기*/
        if (!isFetching && hasNextPage) {
          fetchNextPage();
        }
      }}
      hasMore={hasNextPage}
      loader={
        <div key="loader" className="loading">
          로딩 중...
        </div>
      }
    >
      {data.pages.map((pageData, pageIndex) => {
        return pageData.results.map((person, personIndex) => {
          return (
            <Person
              key={`${pageIndex}-${personIndex}`}
              name={person.name}
              eyeColor={person.eye_color}
              hairColor={person.hair_color}
            />
          );
        });
      })}
    </InfiniteScroll>
  );
}
```

### 공문 권고 사항

```ts
/*다음 데이터가 있으며 패칭 중이 아닐 시 데이터 불러오기*/
if (!isFetching && hasNextPage) {
  fetchNextPage();
}
```

- 위 부분에서 `!isFetching`없이 진행 시 이미 불러오는 중인데 다시 불러오는 추가 요청의 불필요한 트래픽 발생
- 한 번의 진행 중인 패치만 있을 수 있는데 또 사용자가 스크롤 중 다시 똑같은 요청을 보낼 수 있게 되는 문제가 생긴다.
- 물론 `cancelRefetch` 속성이 기본적으로 `true`라 크게 문제는 없지만 차후에 문제가 발생할 수 도있으니 좀 더 선언적으로 명시할 필요가 있다
  ..
