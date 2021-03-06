#### 一、分析
1. `Chrome Network`面板显示**HTTP**请求有**20**条左右;
2. `bg.jpg`背景图片加载为`23.85s`左右, _参考解决方案3_；
3. 其他头像的图片都是在 `40K` 左右,并且都是`png`, _参考解决方案2_；
4. 外部资源大概有`660KB`

#### 二、解决方案
1. **减少http请求：**
	- 尽量把`js`和`css`内联到`HTML`，用`gulp-inlinesource`来实现；
	- `CSS精灵`: 在该项目中还是有点**问题**，故不采用；
	- 使用`Base64` [(编码BASE64)](http://tool.css-js.com/base64.html)：小的图片使用`Base64`,尝试用了二维码的Base64，感觉加载速度上并没有太多地变化；
	- 图片置灰 [使用CSS将图片转换成黑白](http://www.zhangxinxu.com/wordpress/2012/08/小tip-使用css将图片转换成黑白的/)
2. **压缩：**
	- JavaScript/CSS压缩：用`gulp-inlinesource`；
	- 图片压缩：`PNG` `jpg` `webp`，用了`gulp`的图片压缩`gulp-imagemin`插件，可以使得`saved 283.96KB - 66.4%`
	压缩率达到了 `66.4%`，之前的图片大小为`417KB`，一共有`10`张图片，然后压缩之后变成了之后`140KB`；
	- 参考资料和工具：
		* [智图工具](http://zhitu.isux.us/)
		* [图片格式那点事儿](http://ued.taobao.org/blog/2010/12/jpg_png/)
3. 不要用`CSS3`代码去`高斯模糊`图片，图片还是那么大，`兼容性`又不好
所以对首页背景图片的，我们采用PS处理高斯模糊图片，从`180K` 变成了 `57K`;

#### 三、结果
1. **外部资源(主要是图片)**
`660KB左右` &#8594; `417KB` &#8594; `140KB`

2. **加载速度**
`23.83s` &#8594; `14.00s左右` &#8594; `5.99s`

3. **HTTP 请求**
`20` &#8594; `13` &#8594; `11`