# 第十一课 前端技术入门之UI库

进入Web App时代以后，默认的页面组件早已不够用了。现代页面充斥着各种复杂的控件，如对话框，范围选择器，下拉菜单等。这些控件HTML都没有为我们提供（未来可能会渐渐提供，例如HTML 5.2增加了`<dialog>`标签来支持native dialog）。因此我们必须使用第三方UI库。

目前为大家介绍一个基于styled-components的UI库，Grommet V2。我们的demo code使用这个库为大家生成一个包含了一种控件的对话框。

Grommet库的V2版使用Styled-Components替代了SASS（一种CSS的预处理器，用来生成CSS，但增加了很多扩展语法）。Grommet V2被NetFlix和Uber等公司所使用。
Grommet V2还增加了布局元素，支持Box，Grid，Layer和Stack等常用布局。
```javascript
import React, { Component } from 'react';
import { Box, Layer, Grommet, Button, Heading, FormField, TextInput } from 'grommet';

export default class extends Component {
  constructor(props) {
    super(props);
    this.onClick = this.onClick.bind(this);
    this.onChange = this.onChange.bind(this);
    this.onCancel = this.onCancel.bind(this);
    this.onCreate = this.onCreate.bind(this);
    this.state = {
      isOpen: false,
      name: ''
    }
  }

  onClick() {
    this.setState({ isOpen: true });
  }

  onChange(e) {
    this.setState({ name: e.target.value });
  }

  onCancel() {
    this.setState({ isOpen: false });
  }

  onCreate() {
    alert(`Student ${this.state.name} is created.`);
    this.onCancel();
  }

  render() {
    return (
      <Grommet plain full>
        <Button primary label='Open Dialog' onClick={this.onClick} />
        {this.state.isOpen &&
        <Layer>
          <Box pad='medium' gap='small' width='medium'>
            <Heading level={3} margin='none'>
              Create a new student
            </Heading>
            <FormField label='name'>
              <TextInput
                value={this.state.name}
                onChange={this.onChange} />
            </FormField>
            <Box
              as='footer'
              gap='small'
              direction='row'
              align='center'
              justify='end'
              pad={{ top: 'medium', bottom: 'small' }}
            >
              <Button primary label='Create' onClick={this.onCreate} />
              <Button default label='Cancel' onClick={this.onCancel} />
            </Box>
          </Box>
        </Layer>}
      </Grommet>
    );
  }
}
```

## 参考资料
1. [Grommet V2官网](https://v2.grommet.io/)
