
- 可拖拽行参考: [参考](https://ant.design/components/table-cn#components-table-demo-drag-sorting) 注意数据源的rowKey 一定要对应上
- 可变宽表头, 将计算后的表头设置给Table 组件的columns 属性, components 设置给 components 属性, 注意一定要在useEffect 把动态表头设置好, 只有设置了width的列才能改变宽度

```jsx
import useATRH from '@minko-fe/use-antd-resizable-header';
import "@minko-fe/use-antd-resizable-header/dist/style.css";

const { components, resizableColumns } = useATRH({
    columns: useMemo(() => columns, [columns]),
    minConstraints: 50
});
```
