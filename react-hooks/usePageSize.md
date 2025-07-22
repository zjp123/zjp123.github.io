```typescript
import { useCallback, useEffect, useState } from 'react';

/**
 * 页码大小持久化钩子
 * @param key 存储键名，用于区分不同页面的页码大小
 * @param defaultPageSize 默认页码大小
 * @returns [pageSize, setPageSize] 页码大小和设置函数
 */
export const usePageSize = (key: string, defaultPageSize: number = 100) => {
  // 从localStorage获取存储的页码大小
  const getStoredPageSize = useCallback(() => {
    try {
      const stored = localStorage.getItem(`pageSize_${key}`);
      return stored ? parseInt(stored, 10) : defaultPageSize;
    } catch (error) {
      console.warn('获取存储的页码大小失败:', error);
      return defaultPageSize;
    }
  }, [key, defaultPageSize]);

  // 初始化页码大小状态
  const [pageSize, setPageSizeState] = useState<number>(getStoredPageSize);

  // 设置页码大小并持久化到localStorage
  const setPageSize = useCallback(
    (newPageSize: number) => {
      try {
        setPageSizeState(newPageSize);
        localStorage.setItem(`pageSize_${key}`, newPageSize.toString());
      } catch (error) {
        console.warn('保存页码大小失败:', error);
        setPageSizeState(newPageSize);
      }
    },
    [key]
  );

  // 组件挂载时从localStorage恢复页码大小
  useEffect(() => {
    const storedPageSize = getStoredPageSize();
    if (storedPageSize !== pageSize) {
      setPageSizeState(storedPageSize);
    }
  }, [getStoredPageSize, pageSize]);

  // 清除存储的页码大小
  const clearPageSize = useCallback(() => {
    try {
      localStorage.removeItem(`pageSize_${key}`);
      setPageSizeState(defaultPageSize);
    } catch (error) {
      console.warn('清除页码大小失败:', error);
      setPageSizeState(defaultPageSize);
    }
  }, [key, defaultPageSize]);

  return {
    pageSize,
    setPageSize,
    clearPageSize,
  };
};

export default usePageSize;

```
