# Maya-xgen制作总结

## 1. Maya版本

&emsp; <font color=red>尽可能保持从制作到最终渲染xgen的Maya版本一致</font>，<font color=red>包括maya的小版本</font>（例如2018.6和2018.7），版本的问题也会导致最后渲染的时候，出现xgen拉丝的情况

## 2. xgen创建

### a. 指定xgen路径

&emsp; 可以指定相对路径或者绝对路径，如果是相对路径需要在渲染的时候指定一下相对路径的位置，建议是使用绝对路径问题比较少，绝对路径只是不利于迁移数据需要重新指定而已

&emsp; ![](../../../images/mayas/hairs/h1_output_1.png)

&emsp; ![](../../../images/mayas/hairs/h1_output_2.png)

### b. xgen的生长面

&emsp; 不要直接在模型上面制作，尽量复制模型并裁剪出需要生长xgen的部分面来制作，<font color=red>生长面需要UV展好并合理，后续的制作环节不能再去修改或者改变生长面</font>
&emsp;![](../../../images/mayas/hairs/h1_output_3.png)

### c. 制作xgen的节点一定要使用烘焙

&emsp;制作过程中的所有节点，<font color=red>不要使用live模式</font>，一定要使用baked模式，把效果烘焙成文件再读取回来

&emsp;![](../../../images/mayas/hairs/h1_output_4.png)

### d. xgen的表达式问题

&emsp; 不要在表达式窗口使用`${DESC}`变量，<font color=red>xgen存在bug在表达式窗口无法正常读取到`${DESC}`</font>变量指向的description节点，所以在表达式窗口不要使用`${DESC}`变量

&emsp;![](../../../images/mayas/hairs/h1_output_5.png)

在其他的窗口属性就可以正常的使用

&emsp;![](../../../images/mayas/hairs/h1_output_6.png)

### e. 使用Guide Check节点检查

&emsp; 该节点可以检查出xgen的引导曲线是否重叠或者两者位置比较靠近，<font color=red>在xgen制作完成之后必须要使用给节点进行xgen的检查</font>，检查出有问题的引导曲线，也要删去或者修改，直到check没问题

&emsp;![](../../../images/mayas/hairs/h1_output_7.png)

## 3. xgen绑定

&emsp;<font color=red>直接对生长面蒙皮或者使用blendShape都可以</font>
&emsp;如果要绑定xgen的引导线，建议是绑定xgen的创建的hair system的输出曲线，绑定输出曲线，动画做完后，再把输出曲线导出abc让xgen使用；如果是直接绑定用xgen导线，可能会造成一些问题
&emsp;![](../../../images/mayas/hairs/h1_output_8.png)

## 4. xgen的解算

&emsp; 使用Create Hair System创建nhair系统，通过对nhair系统来解算，解算完成之后把xgen的输出曲线导出abc，然后再指定回xgen读取
&emsp;![](../../../images/mayas/hairs/h1_output_9.png)

导出，<font color=red>导出的时候一定要和原来解算的帧率一致</font>
&emsp;![](../../../images/mayas/hairs/h1_output_10.png)

指定xgen读取动画abc，<font color=red>不要使用live Mode</font>
&emsp;![](../../../images/mayas/hairs/h1_output_11.png)

## 5. xgen的渲染

&emsp;需要导出xgen批量渲染abc，每一个渲染文件都要导出

&emsp;![](../../../images/mayas/hairs/h1_output_12.png)

<font color=red>快速导出技巧：</font>能帮助快速导出gen批量渲染abc
关闭实时更新和隐藏xgen、隐藏xgen引导线、不显示所有的物体

&emsp;![](../../../images/mayas/hairs/h1_output_13.png)

&emsp;![](../../../images/mayas/hairs/h1_output_14.png)

&emsp;![](../../../images/mayas/hairs/h1_output_15.png)

<font color=red>交互式xgen也需要在渲染前导出缓存</font>

&emsp;![](../../../images/mayas/hairs/h1_output_16.png)

## 6. 无法正常读取.xgen文件

&emsp; 大部分无法正常读取.xgen文件，<font color=red>可以检查一下打开文件的过程是否存在报错的语句</font>，如果报一些插件的错误，可能是在制作的时候，该电脑maya安装了某个插件，但在其他电脑打开时，因为没有配置该插件就无法正常读取.xgen文件；例如：redshift插件等

&emsp; <font color=red>大部分无法正常读取.xgen文件</font>都是因为maya<font color=red>打开过程的某些插件或者脚本或文件无法正常读取加载</font>，从而导致无法正常读取到.xgen文件，所以一定<font color=red>要分析打开过程的报错语句</font>

## 参考
- [XGen 学习路径](https://help.autodesk.com/view/MAYAUL/2018/CHS/?guid=GUID-C0470142-600B-4615-8110-EC779934DF5F)

- [渲染 XGen 基本体](https://help.autodesk.com/view/MAYAUL/2018/CHS/?guid=GUID-1A93B16B-7B6C-4FF8-9855-F0BE74E68DFF)

- [maya xgen 毛发制作](https://www.bilibili.com/video/av25143777/?p=9&vd_source=4655f6297e93c05206bd3f385bdc5342)

- [Maya2018 Xgen交互式](https://www.bilibili.com/video/av43898231/?from=search&seid=8274814087944584002&vd_source=4655f6297e93c05206bd3f385bdc5342)

- [Maya2018 Xgen毛发教学](https://www.bilibili.com/video/av44158323/?vd_source=4655f6297e93c05206bd3f385bdc5342)

- [XGen - Arnold for Maya](https://help.autodesk.com/view/ARNOL/CHS/?guid=arnold_for_maya_shapes_shapes_xgen_html)