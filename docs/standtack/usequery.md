```js
export const getInterviewEmailTemplate = (data: {
  configType: 'ADD' | 'MODIFY';
  interviewChildStatus?: string;
  interviewStatus?: string;
  planId?: number;
  type?: 'ON_SITE' | 'VIDEO';
  arrangeType?: 'DIRECT' | 'INDIRECT' | 'CALL';
}) => {
  return queryOptions({
    queryKey: ['getInterviewEmailTemplate', cloneDeep(data)],
    queryFn: () => httpPost('xxxxxxxxxx', data),
  });
};
```

```js
当getInterviewEmailTemplate 中data包含得属性变化是，接口会重新执行const { data } = useQuery(
    getInterviewEmailTemplate({
      interviewChildStatus: nodeStatus,
      interviewStatus: node,
      planId,
      configType: xx,
      type: xxxx,
      arrangeType: xxxx,
    })
  );

```
