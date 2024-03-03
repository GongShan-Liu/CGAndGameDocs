# 004-DG节点

**Maya节点包含4个部分：属性、插头、数据块、数据句柄**

&emsp;

**节点的属性和插头**
- 节点的属性定义了节点与DG其余部分的数据接口。节点在评估过程中相互传递的唯一数据来自并通过此接口
- 插头是包含给定属性数据的构造。属性的所有数据访问都是通过插头来完成的。属性本身仅定义属性的数据类型和名称。插头也是节点之间进行连接的端口。
- 属性名称在整个节点层次结构中必须是唯一的，即在所有派生类和父类中，但可以在不相关的节点之间重用。
- 属性可能具有分配给它们的默认值。如果其值尚未设置，这将是该属性上的插头将具有的值。任何类型化属性的默认值都等于它们接受的数据类型的默认值——数字数据默认为零，矩阵数据默认为单位矩阵等


## 1. 脏传播

&emsp; 当修改了节点的属性时，若影响到节点的输出，那么属性则标记为**脏**，而连接的下游节点都会被标记为**脏**，直到节点进行了重新计算后，属性才会被标记**干净**

&emsp; 在以下情况下，节点会执行计算进行更新：

&emsp; &emsp; **ViewPort**：视图刷新

&emsp; &emsp; **Attibute Editor**：开启属性编辑器

&emsp; &emsp; **Channel box**：开启通道盒

&emsp; &emsp; **Play animation**：播放动画

&emsp; &emsp; **getAttr**：执行获取属性值的命令

&emsp; 当脏节点需要更新时，会沿着上游一直追寻脏节点，直到无法找到脏节点为止，则把每个脏节点进行计算，输出到下游，直至最终要更新的脏节点

## 2. 推拉机制

&emsp; 推拉机制是节点属性脏传播和评估的方式；当属性被修改，那么标记为脏时，该机制则会把节点连接到的下游节点进行脏传播，
该过程就是推送；当属性需要评估时，节点连接的上游则会从最上游节点开始把每个节点进行计算，该过程就是拉取，完成后标记干净

## 3. 惰性评估

&emsp; 在属性评估过程中，只有要求更新的脏属性才会进行计算，并之后标记为干净，如果节点某些脏属性没有接收到更新的请求则不会进行评估，
该过程被称为惰性评估

## 4. 基础DG节点类
```python
"""
MPxNode：所有DG节点的基类

MPxLocatorNode：父类是一个DAG节点，此类用于定义在空间中具有位置
                但没明确形状的实体并且不会被渲染

MPxGeometryFilter：获取输入几何形状并对其进行变形

MPxDeformerNode：处理每个顶点的权重列表

MPxBlendShape：处理输入目标和权重

MPxSkinCluster：处理每个顶点的蒙皮权重

MPxIkSolverNode：IK解算器的类，用于根据一组目标和设置在连接上的约束来为刚体连接设置动画

MPxFieldNode：可自定义动态场以影响场景中的其他几何体

MPxEmitterNode：将粒子发射到Maya场景中的类

MPxFluidEmitterNode：允许创建和操作表示流体发射器的DG节点。是Emitter节点功能集层次结构的顶层。
允许对所有类型的流体Emitter共有的属性进行操作

MPxSpringNode：在两个具有质量的端点之间相互作用的力。Maya定义了一个默认的弹簧力，它遵循传统的弹簧数学模型。
可以通过从MPxSpringNode. 预定义属性提供所有标准弹簧常数以及端点的位置和质量。节点的工作是通过 applySpringLaw()方法而不是compute()方法。

MPxObjectSet：用于在Maya中实现新类型的集合，这些集合可以具有可选择或可操作的组件，并且行为方式与Maya中包含的objectSet节点类似。

MPxHwShaderNode：允许创建用户定义的hwShaders。hwShader是一个节点，它采用任意数量的输入几何图形，
对它们进行变形并将输出放置到输出几何图形属性中。另请参阅编写硬件着色节点。

MPxTransform：允许创建用户定义的变换节点。用户定义的变换节点可以引入新的变换类型或改变变换顺序。
它们被设计为标准 Maya 变换节点的扩展，并包含所有正常变换属性


MPxImagePlane：允许创建新类型的图像平面节点。可以使用此类修改图像平面中的非标准图像数据或对此节点的行为更改。

MPxParticleAttributeMapperNode：是所有用户定义的每粒子属性映射节点的父类。此类允许插件定义新的“arrayMapper”节点的行为，
粒子通常使用该节点为纹理节点中的粒子着色。

MPxConstraint：是所有用户定义的约束节点的父类。这个类与MPxConstraintCommand提供默认Maya约束功能

MPxManipulatorNode：是操纵器的基类一种节点，它在 Maya 视口中绘制自身的可视化表示，并通过视口内的直接交互接受来自用户的输入。
Maya 提供了几种内置类型的操纵器，但如果要创建新类型的操纵器，可以创建一个新类，
该类派生自MPxManipulatorNode并编写确定操纵器如何绘制以及它如何对鼠标事件作出反应的虚拟方法。

MPxManipContainer：类为操纵器提供一个基本接口，该接口解释由一种或多种基本类型的操纵器节点记录的输入，
并通过更改其他连接节点上的属性来应用该输入。

MPxSurfaceShape 和 MPxComponentShape
创建一种新的形状图元，则需要创建一个派生自的新类MPxSurfaceShape或者MPxComponentShape，
它将用于表示场景层次结构和依赖关系图中的几何体实例

MPxAssembly：是场景组装节点的基类。

MPxCameraSet：可自定义节点视为内置cameraSet节点

MPxMotionPathNode：可自定义节点视为内置的motionPath节点

MPxPolyTrg：自定义面三角剖分算法的节点的基类


MPxThreadedDeviceNode：是在创建节点时启动辅助线程并使用辅助线程从设备获取数据的节点的基类

MPxClientDeviceNode：专精MPxThreadedDeviceNode用于与充当客户端的网络设备交互，例如来自TCP socket的数据

"""
```

## 参考

- [maya_API入门详解（中英文字幕）](https://www.bilibili.com/video/BV11K4y1e7zB/?p=1&vd_source=4655f6297e93c05206bd3f385bdc5342)

- [maya DG插件](https://help.autodesk.com/view/MAYAUL/2018/CHS/?guid=__files_Dependency_graph_plugins_htm)