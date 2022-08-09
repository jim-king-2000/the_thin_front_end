# 第十一课 前端技术入门之UI库

进入Web App时代以后，默认的页面组件早已不够用了。现代页面充斥着各种复杂的控件，如对话框，范围选择器，下拉菜单等。这些控件HTML都没有为我们提供（未来可能会渐渐提供，例如HTML 5.2增加了`<dialog>`标签来支持native dialog）。因此我们必须使用第三方UI库。

目前为大家介绍一个基于styled-components的UI库，Grommet V2。我们的demo code使用这个库为大家生成一个包含了一种控件的对话框。

Grommet库的V2版使用Styled-Components替代了SASS（一种CSS的预处理器，用来生成CSS，但增加了很多扩展语法）。Grommet V2被NetFlix和Uber等公司所使用。
Grommet V2还增加了布局元素，支持Box，Grid，Layer和Stack等常用布局。

使用Grommet V2，先需要安装。

    npm i grommet
    
然后在`index.js`里填上如下代码：
``` javascript
import { Grommet, Box, Button, Heading, Text, Accordion, AccordionPanel, CheckBoxGroup, TextInput, List, Clock } from 'grommet';
import { grommet } from 'grommet/themes';

export default () => (
  <Grommet full theme={grommet}>
    <Box>
      <Clock />
      <Heading>Grommet V2</Heading>
      <Text>这是一个Grommet V2的测试网页。</Text>
      <Button label='测试' />
      <Accordion>
        <AccordionPanel label='Panel 1'>
          <Box pad='medium' background='light-2'>
            <Text>One</Text>
          </Box>
        </AccordionPanel>
        <AccordionPanel label='Panel 2'>
          <Box pad='medium' background='light-2'>
            <Text>Two</Text>
          </Box>
        </AccordionPanel>
      </Accordion>
      <Box pad='medium'>
        <CheckBoxGroup options={['Maui', 'Kauai', 'Oahu']} />
      </Box>
      <TextInput placeholder='type here' />
      <List
        primaryKey="name"
        secondaryKey="percent"
        data={[
          { name: 'Alan', percent: 20 },
          { name: 'Bryan', percent: 30 },
          { name: 'Chris', percent: 40 },
          { name: 'Eric', percent: 80 },
        ]}
      />
    </Box>
  </Grommet>
);
```

## 参考资料
1. [Grommet V2官网](https://v2.grommet.io/)
