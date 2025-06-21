```typescript

/** 滚动事件处理函数 */
  const handleScroll = (event: Event) => {
    const distance = calculateDistanceToParent();
    const target = event.target as HTMLDivElement;
    if (distance?.top && target.scrollTop >= distance.top) {
      setIsNeedAddClassName(true);
    } else {
      setIsNeedAddClassName(false);
    }
  };

  /** 计算子元素相对于父元素的距离 */
  const calculateDistanceToParent = () => {
    const parentElement = scrollContainerRef.current; // styles.body 父元素
    const childElement = targetContainerRef.current; // 目标Container子元素

    if (parentElement && childElement) {
      const parentRect = parentElement.getBoundingClientRect();
      const childRect = childElement.getBoundingClientRect();

      const distance = {
        top: childRect.top - parentRect.top, // 子元素顶部相对于父元素顶部的距离
        offsetTop: (childElement as HTMLElement).offsetTop, // 子元素相对于父元素的offsetTop
      };

      console.log('Container相对于styles.body的距离:', distance);
      return distance;
    }

    console.warn('无法获取父元素或子元素的引用');
    return null;
  };

  /** 添加滚动事件监听器 */
  useEffect(() => {
    const scrollContainer = scrollContainerRef.current;
    if (scrollContainer) {
      scrollContainer.addEventListener('scroll', handleScroll);

      // 清理函数，组件卸载时移除事件监听器
      return () => {
        scrollContainer.removeEventListener('scroll', handleScroll);
      };
    }
  }, []);
```
