当tabs中，children也用到了tabs 嵌套的情况下，那么这种写法  只针对当面tabs了
.topTabsStyle {
        > :global(.ant-tabs-nav) {
          margin-bottom: 0;
          :global(.ant-tabs-tab) {
            padding: 18px 0;
          }
        }
      }
