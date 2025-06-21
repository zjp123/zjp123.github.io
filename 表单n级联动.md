```typescript
interface ListNode {
  key: string;
  value: any;
  next: ListNode | null;
}

const useLinkLinst = (obj: Record<string, any>) => {
  if (typeof obj !== 'object' || obj === null || Array.isArray(obj)) {
    throw new Error('obj must be an object');
  }
  let linkList: any = null;
  const copyObj = { ...obj };

  const objToLinkList = (obj: Record<string, any>) => {
    // 将对象转换为链表
    let head: ListNode | null = null;
    let current: ListNode | null = null;

    // 遍历对象的键值对并构建链表
    Object.entries(obj).forEach(([key, value]) => {
      const newNode: ListNode = {
        key,
        value,
        next: null,
      };

      if (!head) {
        head = newNode;
        current = newNode;
      } else if (current) {
        current.next = newNode;
        current = newNode;
      }
    });
    return head;
  };
  linkList = objToLinkList(copyObj);
  const getLinkList = () => {
    return linkList;
  };

  const linkForeach = (currentLinkKey: string, callback: (linkKey: string, value: any) => void) => {
    // 从链表头开始查找指定的节点
    let current: ListNode | null = linkList;
    // 找到当前节点
    while (current && current.key !== currentLinkKey) {
      current = current.next;
    }
    // 从当前节点的下一个节点开始遍历
    if (current) {
      current = current.next;
    }
    // 遍历当前节点之后的所有节点
    while (current) {
      callback(current.key, current.value);
      current = current.next;
    }
  };

  const getLinkKey = (obj: Record<string, any>) => {
    return Object.keys(obj)[0];
  };

  const setLinkList = (obj: Record<string, any>) => {
    const copyObj = { ...obj };
    linkList = objToLinkList(copyObj);
    console.log('setlinklist', linkList);
  };

  return {
    getLinkKey,
    getLinkList,
    setLinkList,
    linkForeach,
  };
};

const create = (obj: Record<string, any>) => {
  if (typeof obj !== 'object' || obj === null || Array.isArray(obj)) {
    throw new Error('obj must be an object');
  }
  return useLinkLinst(obj);
};

export { useLinkLinst, create };
```
