### useIsFetching

- 현재 상태가 데이터를 로딩 중인지 아닌지 확인한다.
- suspense로 처리할랬는데 이런 방법도 있다니 ..다만 패칭은 캐싱이 있어도 나오는 화면이니.. 로딩은 suspense로 ..

```ts
export function Loading() {
  //  use React Query `useIsFetching` to determine whether or not to display
  const isFetching = useIsFetching();
  const display = isFetching ? "inherit" : "none";

  return (
    <Spinner
      thickness="4px"
      speed="0.65s"
      emptyColor="olive.200"
      color="olive.800"
      role="status"
      position="fixed"
      zIndex="9999"
      top="50%"
      left="50%"
      transform="translate(-50%, -50%)"
      display={display}
    >
      <Text display="none">Loading...</Text>
    </Spinner>
  );
}
```

### 전역적으로 에러 핸들링

```ts
export const queryClient = new QueryClient({
  queryCache: new QueryCache({
    onError: (error) => {
      errorHandler(error.message);
    },
  }),
});
```
