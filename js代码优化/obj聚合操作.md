```typescript
代码简捷：

const updateExtraDataSource = (updater: (item: TableFormDataRow) => TableFormDataRow) => {
    const newDataSource = extraDataSource.map(updater);
    // setExtraDataSource(newDataSource);
    dispatch({ type: 'SET_EXTRA_DATA_SOURCE', payload: newDataSource });
  };

updateExtraDataSource((item: TableFormDataRow) => {
            item.bgCompanyList = arr || [];
            item.bgItem = null;
            item.bgCompany = null;
            item.bgItemList = [];
            item.tempItem = null;
            item.vendorTemplateList = [];
            item.assumeFinishTime = '';
            item.isExpress = 2;
            return item;
        })


// extraDataSourceItem.bgCompany = val;
        // extraDataSourceItem.vendorTemplateList = arr || [];
        // extraDataSourceItem.bgItem = null;
        // extraDataSourceItem.bgItemList = [];
        // extraDataSourceItem.tempItem = null;
        // extraDataSourceItem.assumeFinishTime = '';
        // extraDataSourceItem.isExpress = 2;

批量操作可以改成：
Object.assign(extraDataSourceItem, {
        bgItem: val,
        bgItemList: arr,
        tempItem: tempItem,
        isExpress: !tempItem?.expressFlag? 2 : null,
        assumeFinishTime: tempItem?.assumeFinishTime,
    });

```
