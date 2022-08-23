
# <span style="font-size: 60px; background-color: rgb(0, 0, 0);">
	<span style="color: rgb(255, 255, 255);">我是</span>
	<span style="color: rgb(0, 0, 0); background-color: rgb(255, 153, 0);">一级大标题</span>
</span>


哔哔：做什么（目标），怎么做（分解），做到什么程度（验收）
---

华丽的分割线（以下正文）
---
正文内容


---
目录页
---
[TOC]



---
TODO
---
- [ ] 已知未做事项1
- [ ] 已知未做事项2
- [x] 已做完的事项3
- [y] 不知道怎么分，总之已经存在的feature



---
tags
---
#CA #mk


## 1. 二级级标题
#标签1

### 1.1. 三级标题
#标签2


#### 1.1.1. 格式
##### 1. 加粗
正文长这样
**我变粗了**

##### 2. 斜体
正文长这样
*我变斜了*


##### 3. 加粗斜体
正文长这样
***我变又粗又斜了***

##### 4. 删除线
正文长这样
~~我被删除了~~

##### 5. <a style="color:red;">颜色</a>
> 原生Obsidian不支持，找到插件再说

```html

<a> `a标签原生蓝色` </a>
<span style="background-color: green; color: red" > `span标签设置背景绿色字体红色` </span>

<div style="font-size: 60px; background-color: rgb(0, 0, 0);">
	<span style="color: rgb(255, 255, 255);">P站</span>
	<span style="color: rgb(0, 0, 0); background-color: rgb(255, 153, 0);">配色</span>
</div>

```

<a> a标签原生蓝色 </a>

<span style="background-color: green; color: red" > span标签设置背景绿色 + 字体红色 </span>

<div style="font-size: 60px; background-color: rgb(0, 0, 0);">
	<span style="color: rgb(255, 255, 255);">P站</span>
	<span style="color: rgb(0, 0, 0); background-color: rgb(255, 153, 0);">配色</span>
</div>


#### 1.1.2. 内容

##### 1. 引用
> 这里是引用

##### 2. 时间戳
> 需要安装插件

##### 3. 有序列表
1. 有序列表项1
2. 有序列表项2
3. 有序列表项3

##### 4. 无序列表

\-开头加空格

- 无序列表项1
- 无序列表项2
- 无序列表项3

\*开头加空格

* 无序列表项4
* 无序列表项5
* 无序列表项6



##### 5. 代码块
```shell
# 我是shell脚本代码块
```

```text
log: 我就是文本1
log: 我就是文本2
```

```python
print("hello python")
```

```java
System.out.print("我是最好的语言，这个没意见吧");
```

##### 6. 表格
| 序列 |主题   |标题|
|:---:|:---  |:---|
| 1 | 大主题1  | 标题1 |
| 2 | 大主题2  | 标题1 |
| 3 | 大主题3  | 标题1 |



##### 7. 网址链接
[baidu](https://www.baidu.com)



##### 8. 本地图片
![我的指纹](C:\Users\liangjianhui\Desktop\tmpProject\9372captureIssue\com.focaltech.fingerprinttest\focalBmp\finger.bmp)

##### 9. 网络图片
![李小龙](https://bkimg.cdn.bcebos.com/pic/4a36acaf2edda3cc7cd91fbdcca62e01213fb80e3454?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2U5Mg==,g_7,xp_5,yp_5/format,f_auto)

##### 10. 网络视频
链接不太友好，嵌入视频直接播放，注意！！！形象！！！
[李小龙视频demo](https://baike.baidu.com/item/%E6%9D%8E%E5%B0%8F%E9%BE%99/32914?fr=aladdin)
[二舅](https://www.bilibili.com/video/BV1MN4y177PB?spm_id_from=333.337.search-card.all.click)

这里为了不自动播放，在网址中间加了空格，所以网址解释不出来（复制html代码是可以解释的），注意里面增加了两条线
<div style="backgroud-color=green">
	<hr style="color: gold"/>
	<iframe src="https://www.bilibili.com/video/BV1MN4y177PB?spm_id_from=333.337.search-card.all.click" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="1024" height="768"></iframe>
	<hr style="color: gold"/>
</div>



```html
<iframe src="" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>


<iframe src="https://www.bilibili.com/video/BV1MN4y177PB?spm_id_from=333.337.search-card.all.click" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>


<div style="backgroud-color=lightgreen">
	<hr style="color: red"/>
	<iframe src="https://www.bilibili.com/video/BV1MN4y177PB?spm_id_from=333.337.search-card.all.click" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" width="1024" height="768"></iframe>
	<hr style="color: red"/>
</div>
```



##### 9. 笔记链接

| 序列 |方式   |说明|
|:---:|:---  |:---|
| 1 | [[Obsidian_demo]]  | 这是demo，也就是我自己，Ctrl加鼠标悬浮可预览，单击可见分栏打开 |
| 2 | [[Obsidian_demo#5 a style color red 颜色 a]]  | 想看颜色到这一节 |
| 3 | [[Obsidian_demo#10 网络视频 \|这里看视频]]  | 在这里可以看视频 |
| 3 | [[Obsidian_demo#10 注释^注释可以隐身]]  | 注释可以隐身 |


##### 10. 注释
```markdown
打开预览模式只能看到这里
%% 打开编辑模式才能看得到我 %% 
```

打开预览模式只能看到这里
%% 打开编辑模式才能看得到我 %%  


































