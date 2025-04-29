### 만일 페이지네이션과 같은 함수라면

- 캐시 구분을 위해 식별자를 사용해야한다.

```js
const { data, isLoading } = useQuery({
  queryKey: ["comments", post.id],
  queryFn: () => fetchComments(post.id),
});
```

### prefetch를 이용해 데이터 미리 불러오기

```js
useEffect(() => {
  if (currentPage < maxPostPage) {
    const nextPage = currentPage + 1;
    queryClient.prefetchQuery({
      queryKey: ["posts", nextPage],
      queryFn: () => fetchPosts(nextPage),
    });
  }
}, [currentPage, queryClient]);
```

- `prefetch`를 이용해 다음 페이지의 데이터를 미리 가져올 수 있다.

### Mutation

- isError : 에러가 발생했을 떄
- isPending : 진행 중일떄
- isSuccess : 성공했을 시
- reset() : 삭제 상태를 초기화 해줘야하기 때문에 사용해야한다.
