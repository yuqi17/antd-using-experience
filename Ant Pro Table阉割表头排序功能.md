
ant pro 和 antd 其实是一脉相承的东西, 改主题色的时候也会被影响到. ant pro 是对antd 组件根据业务需要进行的二次封装, 为了减少页面编写的时间, 灵活性在没有充分熟悉之前可能是比较低的, 适合做样式要求不高的后台

下面编写了一个阉割版的pro talbe 组件:
```jsx


import React, {useState} from 'react'
import { Button, Table } from 'antd';
import { ProTable,  } from '@ant-design/pro-components';
import './index.less'

function CastratedProTable({ columns, onChange=()=>{}}){
  return (
    <ProTable
      className='castrated-pro-table'
      columns={columns}
      columnsState={{onChange:(map)=>{
        const newArr = columns.map(item => ({...item, ...map[item.dataIndex]})).filter(item => item.show).sort((a, b)=> {
          if(a.order === b.order)
            return 0 
          else if(a.order > b.order)
            return 1
          else
            return -1
        });
        onChange(newArr);
      }}}
      search={false}
      options={{
        search: false,
        reload: false,
        density: false
      }}
    />
  )
}

export default function App() {

  const columnsConfig = [
    { title: 'hahah', dataIndex: 'a' },
    { title: 'world', dataIndex: 'b' },
    { title: 'asdf', dataIndex: 'c' },
    { title: 'aaa', dataIndex: 'd' },
  ];

  const [columns, setColumns] = useState(columnsConfig)
 
  return (
      <div>
            <div className='flex-box'>
              <Button>
                HELLO
              </Button>

              <CastratedProTable columns={columnsConfig} onChange={columns => setColumns(columns)}/>
            </div>
            
            <Table
              columns={columns}
            />
      </div>
  )
}

```

样式部分:
```less
.castrated-pro-table {
  .ant-pro-card-body {
    padding: 0;
    // background-color: red;

    .ant-pro-table-list-toolbar {
      // background-color: green;

      .ant-pro-table-list-toolbar-container {
        display: inline;
        .ant-pro-table-list-toolbar-right{
          display: inline;
        }
      }
    }
  }

  .ant-table-wrapper {
    display: none;
  }
}

.flex-box{
  display: flex;
  align-items: center;
  justify-content: space-between;
}
```
