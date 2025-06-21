```typescript
import { useCallback, useEffect, useState } from 'react';

export const useTabState = <T extends string | number>(key: string, defaultTab: T) => {
  // 从localStorage获取存储的tab状态
  const getStoredTab = useCallback(() => {
    try {
      const stored = sessionStorage.getItem(`tabState_${key}`);
      if (stored) {
        const parsed = JSON.parse(stored);
        return parsed;
      }
      return defaultTab;
    } catch (error) {
      console.warn('获取存储的tab状态失败:', error);
      return defaultTab;
    }
  }, [key, defaultTab]);

  const [activeTab, setActiveTabState] = useState<T>(getStoredTab);

  const setActiveTab = useCallback(
    (newTab: T) => {
      try {
        setActiveTabState(newTab);
        sessionStorage.setItem(`tabState_${key}`, JSON.stringify(newTab));
      } catch (error) {
        console.warn('保存tab状态失败:', error);
        setActiveTabState(newTab);
      }
    },
    [key]
  );

  useEffect(() => {
    const storedTab = getStoredTab();
    if (storedTab !== activeTab) {
      setActiveTabState(storedTab);
    }
  }, [getStoredTab, activeTab]);

  // 清除存储的tab状态
  const clearTabState = useCallback(() => {
    try {
      sessionStorage.removeItem(`tabState_${key}`);
      setActiveTabState(defaultTab);
    } catch (error) {
      console.warn('清除tab状态失败:', error);
      setActiveTabState(defaultTab);
    }
  }, [key, defaultTab]);

  return [activeTab, setActiveTab, clearTabState] as const;
};

```
