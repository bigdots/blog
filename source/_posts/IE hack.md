---
title: IE hack整理
date: 2016-01-12 15:40:43
tags:
description:
categories: [js]
---

### CSS属性前缀法 ###


hack       | browser               |使用
-----      | ---                   |---
[*+><]     | IE7                   |用于属性值前
\9         | IE6/IE7/IE8/IE9/IE10  |用于属性值前
\0         | IE8/IE9/IE10          |用于属性值前
\9\0       | IE9/IE10              |用于属性值前

测试用例：

```css
＃color{
	color:#000; /* 所有识别 */
	color:#000\9; /* IE识别 */
	*color:#000; /* IE7识别 */
}
```

### 条件注释法 ###

1. 只在IE下生效

	```html
	<!--[if IE]>
	这段文字只在IE浏览器显示
	<![endif]-->
	```

2. 只在IE6下生效

	```html
	<!--[if IE 6]>
	这段文字只在IE6浏览器显示
	<![endif]-->
	```

3. 只在IE6以上版本生效

	```html
	<!--[if gte IE 6]>
	这段文字只在IE6以上(包括)版本IE浏览器显示
	<![endif]-->
	```

4. 非IE浏览器生效

	```html
	<!--[if !IE]>
	这段文字只在非IE浏览器显示
	<![endif]-->
	```

### 选择器前缀法 ###

阅读了相关文章，觉得基本用不上，不做记录。