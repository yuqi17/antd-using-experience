这两个Provider 都只对antd组件有效. 其中换皮肤改变的是颜色. 而根据设计不同可以去单独写样式调整padding和margin.
```jsx


import React, {useState} from 'react'
import moment from 'moment';
import { Button, ConfigProvider, Form, message, Space, Table, TimePicker, Tooltip } from 'antd';
import Modal from 'antd/es/modal/Modal';
import Card from 'antd/es/card/Card';
import Alert from 'antd/es/alert/Alert';
import Input from 'antd/es/input/Input';

import zhCN from 'antd/locale/zh_CN'
import enUS from 'antd/locale/en_US'

import { StyleProvider , px2remTransformer } from '@ant-design/cssinjs'

import './index.css'

const { RangePicker } = TimePicker;

export default function App() {
  const [flag, setFlag] = useState('en')
  const [visible, setVisible] = useState(false)
  const [messageApi, contextHolder] = message.useMessage();

  const lang = {
    en:enUS,
    zh:zhCN
  }[flag]
 
  return (
    <StyleProvider transformers={[px2remTransformer({
      rootValue:16
    })]}>
    <ConfigProvider
      locale={lang}
      theme={{
        token:{
          colorPrimary:'blue',
          colorText:'yellow',
          colorTextSecondary:'green',
          colorBgSpotlight:'red',
          colorBgBlur:'green',
          colorFill:'red',
          colorFillSecondary:'black',
          colorFillTertiary:'black',
          colorBorder:'black',           // picker 组件的外边框
          colorBorderSecondary:'yellow', // 表格等用于分界的线条颜色
          colorBgElevated:'black',       // modal框, 气泡
          colorBgMask:'red',             // 弹层遮罩
          colorTextTertiary:'purple',    // 图标颜色,比如modal层的关闭
          colorBgContainer:'red',        // 覆盖背景
          colorFillQuaternary:'red',     // 表头 行
        },
        components:{
          Button:{
            primaryColor:'red',
          }
        }
      }}
    >
      {contextHolder}
      <div className='px2rem' style={{height:64, background:'yellow'}}>asdfasdf</div>
      <Alert message='hahahahaha' style={{height:60}}/>
      <Form labelCol={{span:6}}>
        <Form.Item label='hahah' name="test" initialValue={[moment('12:30', 'HH:mm'), moment('13:30', 'HH:mm')]}>
          <RangePicker format='HH:mm' order={false} />
        </Form.Item>
        <Form.Item label='hahah' >
          <Input/>
        </Form.Item>
        <Form.Item>
          <Space>
            <Tooltip title="hahahah">
              <Button type='primary' onClick={() => messageApi.info('hahhha')}>test1</Button>
            </Tooltip>
            <Button type='primary' onClick={() => setVisible(true)}>test2</Button>
            <Button type='primary' onClick={() => setFlag('zh')}>zh</Button>
            <Button type='primary' onClick={() => setFlag('en')}>en</Button>
          </Space>
        </Form.Item>
      </Form>
      <Card title="hahaha">
        hahaha
      </Card>
      <Table
      bordered
        columns={
          [{title:'hahah'},{title:'hseeww'}]
        }
        dataSource={[{},{}]}
      />
      <Modal visible={visible} title='test' onCancel={()=> setVisible(false)}>
        safdasdf
      </Modal>
    </ConfigProvider>

    </StyleProvider>
  )
}

```
