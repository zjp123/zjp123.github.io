```ts
reducer ---- 状态聚合

使用reducer之前
//   const [companyList, setCompanyList] = useState<Vender[]>([]);
//   const [vendorTemplateList, setVendorTemplateList] = useState<BgTemplate[]>([]);
//   const [bgItemList, setBgItemList] = useState<BgItem[]>([]);
//   const [isCanOperationExpress, setisCanOperationExpress] = useState(true);
//   const [extraDataSource, setExtraDataSource] = useState<TableFormDataRow[]>([]);

在需要的地方执行对应的setstate，这样比较散乱


优化后：

const initialState: State = {
  companyList: [],
  vendorTemplateList: [],
  bgItemList: [],
  isCanOperationExpress: true,
  extraDataSource: [],
};

const reducer = (state: State, action: Action): State => {
  switch (action.type) {
    case 'SET_COMPANY_LIST':
      return { ...state, companyList: action.payload };
    case 'SET_TEMPLATE_LIST':
      return { ...state, vendorTemplateList: action.payload };
    case 'SET_BG_ITEM_LIST':
      return { ...state, bgItemList: action.payload };
    case 'SET_CAN_OPERATION_EXPRESS':
      return { ...state, isCanOperationExpress: action.payload };
    case 'SET_EXTRA_DATA_SOURCE':
      return { ...state, extraDataSource: action.payload };
    case 'UPDATE_EXTRA_DATA_SOURCE':
      const newExtraDataSource = [...state.extraDataSource];
      newExtraDataSource[action.payload.index] = {
        ...newExtraDataSource[action.payload.index],
        ...action.payload.data,
      };
      return { ...state, extraDataSource: newExtraDataSource };
    case 'RESET_STATE':
      return initialState;
    default:
      return state;
  }
};


// 使用
const [state, dispatch] = useReducer(reducer, initialState);
  const { companyList, vendorTemplateList, bgItemList, isCanOperationExpress, extraDataSource } = state;

// 在需要的地方执行dispatch




```
