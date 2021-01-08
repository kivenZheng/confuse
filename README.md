# confuse(iOS马甲包混淆，上架神器)

<a name="X50Qx"></a>
#                             ![1593768128247-016fe60b-8853-48fb-8b76-f9f702b83db5.png](https://cdn.nlark.com/yuque/0/2020/png/213807/1606304234500-46a10b02-f83d-4996-99fc-ce092241ea7c.png#align=left&display=inline&height=100&margin=%5Bobject%20Object%5D&name=1593768128247-016fe60b-8853-48fb-8b76-f9f702b83db5.png&originHeight=200&originWidth=200&size=28522&status=done&style=shadow&width=100)
<a name="GZrkm"></a>
# 前言
机缘巧合偶遇iOS[马甲包业务](https://www.yuque.com/docs/share/7e70244c-5dea-4035-b634-65cc082097da?#《马甲包简介》)，前期也使用过目前市面上其他得工具，实际效果不太理想。经过大量实践，开发出一款功能齐全的[混淆工具](https://github.com/520coding/confuse)。工具的主要功能OC、C++已封装成Mac应用，其他功能还在封装中，敬请期待。目前公测阶段: _**免费**_
<a name="qPY4i"></a>
# 提示
为了提高通用性，近期不断重构(>=v1.2.0)之前老版本的功能，为此新建测试工程[**confuse_test**](https://github.com/520coding/confuse/tree/master/confuse_test)，大家在实际使用过程中如果遇到问题，欢迎扩展测试工程，请在工程中请注明bug细节。
> 1.2.0之前的老版本说明：  
> 简介：不涉及语法及编译要求，但是混淆后可能出现局部漏改或者改错，请自行添加至黑名单过滤。  
> 适用项目：C++、Swift、RN等还未适配的混合项目。  
> 使用条件：目前能用v1.1.3，其他过期了  

目前Swift和OC混合项目的Swift默认不修改，但是Swift引用到OC的需要自己添加值黑名单，后续将优化。
<a name="VPIgN"></a>
# 自述
其实识别一个工具的优劣，只需看看它能否修改方法名的所有参数名（极少）、带block的参数的方法（极少），修改布局（较少）、插入的是代码还是‘垃圾’（Apple今年不止三岁了）。更别说“还有谁...”能识别宏、区分继承链等上下文关联内容，智能识别不可修改部分“+ (void)init;- (void)reloadData;基本能改，做到的有几个呢？”，正常混淆完基本不报错（除了一些个别[语法不严谨](https://www.yuque.com/docs/share/4a87ec96-80fe-4d25-873d-93cb428b3e15?#5sCql)造成混淆后报错）。也欢迎大家使用不同工具混淆测试工程**[confuse_test](https://github.com/520coding/confuse/tree/master/confuse_test)**或者第三方开源库项目，对比效果。<br />马甲包的本质：

1. 阶段一减低重复率 ，本人开发初期的版本和目前市面上的其它工具基本相似，主要是‘名称’全局替换这一个基本的功能
1. 阶段二减少相似度（相同元素的正态分布），目前该工具经过优化已经有了很大的改善，已经在慢慢往这方面靠近，详情见以下功能介绍。事物都有两面性，功能越强大混淆耗时越长，如果你的项目很大，混淆几个小时也是有可能的，请不要见怪，后续持续优化中。
<a name="ICzWQ"></a>
# 功能
confuse是一款[混淆工具](https://github.com/520coding/confuse)，尽可能模拟人工开发，仿照Xcode部分功能，避免机核4.3、2.1、2.3.1、账号调查等。<br />目标：**模拟人工修改一切能改的地方**，这也是为什么本工具只有黑名单没有白名单的原因<br />详细功能如下（基本功能不做描述，详见其他工具）：
<a name="fad5222c"></a>
## 已完成
<a name="Pfu38"></a>
### 通用部分

1. [项目配置]，只要选择项目路径，自动完成其他默认配置
   1. 全局设置‘忽略路径’，支持正则
   1. 可以根据‘target’混淆
   1. 自定义添加‘敏感词’
2. [资源替换]，混淆前指定需要替换的资源文件夹，自动进行同名文件替换，方便快捷
2. [修改文件属性]，如创建时间、访问时间、修改时间
2. [修改项目]，无需删除Cocoapods
   1. 可设置‘修改uuid’，彻底翻新
   1. ‘修改target’名称，相关联信息同步更新
5. 自动备份源码
<a name="uYJj8"></a>
### Objective-C

1. [删除注释]，可‘保留空格’‘保留pragma’设置，利于测试阶段查看
1. [修改图片]，质量修改、大小偏移、局部像素微调
1. [重命名图片]，智能名词替换，自动纠正图片名和xcassets文件夹名不对应的情况
   1. 可设置‘运行拼接名称’，用于运行时通过字符串拼接生成的图片名
   1. 可设置‘重命名关联字符串，用于修改字符串与图片名相等的情况
   1. 可设置‘忽略危险名称’开关
4. [插入图片]，自动插入图片，同时根据上下文及类型模拟人工调用，可指定插入个数
4. [重命名属性]，支持@property的所有类型，基本功能，改名字、前缀类似其他工具，不做过多描述，优势：
   1. 识别语法，识别类型、继承关系，**属性名混淆和类名（包含继承链）关联**，同名属性不同类混淆后将不一致，自动识别系统属性
   1. 可设置文件名Model后缀过滤
   1. 【智能名词替换】，混淆后不同类存在相同属性名，模拟正常开发
   1. 【智能识别不可修改部分】，实现最大化混淆
6. [插入属性]，关联已有类型，智能名词替换
   1. ‘百分比控制’
   1. ‘Model后缀’开关，目的：避免Model归档或者数据转模型失败
   1. 可多次执行，指数x2递增
7. [重命名方法]，近似Xcode的Rename功能，基本功能：改名字、前缀类似其他工具，不做过多描述，优势：
   1. 语法相关，识别类型、继承关系，支持**多参修改，方法名混淆和类名（包含继承链）及类型关联**，同名方法不同类、同类同名方法不同类型（类方法、对象方法）混淆后将不一致
   1. 【智能名词替换】，混淆后不同类存在相同方法名，以及相同类相同方法不同类型，模拟正常开发
   1. 【智能识别不可修改部分】，不放过任何一个可以修改的内容
8. [插入方法]，插入并调用上下文关联方法，告别“垃圾代码”，优势：
   1. 根据方法的返回值类型，在分类中创建相应的方法。同时封装原方法的返回值并调用。
   1. 可多次执行，指数x2递增
9. [修改方法]，模拟人工封装调用，优势：
   1. 对原方法进行**拆分调用并根据参数类型（支持继承）局部调整**，详情见[支持参数类型汇总表](https://www.yuque.com/docs/share/315b72d9-28f9-4fa6-bf20-c40d94f2253a?#《修改方法-支持参数类型汇总表》)
   1. 可多次执行，指数x2递增
10. [重命名全局变量]，智能名词替换
10. [修改全局变量]，替换全局变量名、**全局变量转化为全局函数**、混淆字符串变量值
10. [修改局部变量]，模拟人工封装调用，变量名关联类型，优势：
   1. 局部变量值运行时保持不变，详情见[支持类型汇总表](https://www.yuque.com/docs/share/90444065-4f4e-49c8-9e1a-5bd3d3b4f84d?#《修改局部变量-支持类型汇总表》)
   1. 可多次执行，指数x2递增
13. [重命名多语言]，对直接或间接使用系统方法**NSLocalizedString**、**NSLocalizedStringFromTable**的多语言进行修改
13. [修改字符串]，支持任意字符串，加密处理（硬编码->内存），原始字符串保留在注释中方便检查
   1. 设置‘最少长度’过滤
   1. 也可设置‘有效个数’搭配使用
15. [修改xib、storyboard]，自动插入视图，并修改内部结构属性
15. [修改字体]，对项目中使用的字体随机微调，识别宏
15. [修改颜色]，对项目中UI控件颜色随机偏移，识别宏
15. [UI布局偏移]，支持Frame、Mansonry、SDAutoLayout常见布局微调
15. [重命名类]，类名不限制（例如：my、My），可指定添加前缀，优势：
   1. 智能名词替换
   1. 可设置‘重命名同名文件’
   1. 可设置‘重命名相似字符串’，(忽略|相等|包含)三种设置
   1. 新增‘纠正非标准点语法’，针对非标准的点语法调用（方法当做属性调用）
20. [插入文件]，生成其它文件（创建自定义控件，模拟正常开发），项目中自动调用；**注意：目前需要手动导入文件**(在项目根路径下，会生成"**other_xxx_file**"的文件夹，将它拖入工程即可）
20. [插入文本]，生成json、txt、doc、plist等文本文件，项目中自动调用；**注意：目前需要手动导入文件**(在项目根路径下，会生成"**other_xxx_text**"的文件夹，将它拖入工程即可）
<a name="GTE99"></a>
### C++

1. [重命名类]，支持模板等类型
   1. 可切换旧模式
   1. 前缀设置
   1. 可设置‘重命名同名文件’
2. [重命名方法]，近似Xcode的Rename功能，【智能识别不可修改部分】
   1. 语法相关，识别类型、模板、重载、重写、继承等关系
   1. 【智能名词替换】，混淆后不同类存在相同方法名，模拟正常开发
3. [重命名属性]，支持所有类型属性，【智能识别不可修改部分】
   1. 识别语法，识别类型、继承，同名属性不同类混淆后将不一致
   1. 【智能名词替换】，混淆后不同类存在相同属性名，模拟正常开发
4. [修改字符串]，支持任意字符串，加密处理（硬编码->内存），原始字符串保留在注释中方便检查
   1. 设置‘最少长度’过滤
   1. 也可设置‘有效个数’搭配使用
<a name="NVIkj"></a>
### Cocos2d-x
该部分功能整合至C++中，支持cocos2dx自动过滤<br />_以上所有功能均支持黑名单过滤，独立分级过滤指定的内容，灵活自定义忽略混淆。_
<a name="cf7Dx"></a>
### 名词解释

- 智能名词替换：重命名时使用关联类型已有信息+相近语义+类型+部分旧词汇等组合，并且过滤敏感词汇，同时用户也可以自定义敏感词，~~弃用‘随机单词无脑组合’~~
- 智能识别不可修改部分：通过类型及继承链方式识别系统、第三方、Pod方法，并不是‘简单’的相等判断，例如：
   - 类方法：+ (void)init;原则上任何地方都能改
   - 对象方法：- (void)reloadData;不是UITableView的子类是可以改的
   - 属性：@property (readonly) NSUInteger length;如果不是NSString的子类也是可以改的
<a name="ifIVC"></a>
## 规划中
更新迭代将按照以下顺序依次进行

1. Objective-C（90%），重构的目的是为了提高工具的通用性和稳定性，及强化功能
   1. 音频、视频文件使用少，后续添加
2. C++（40%），开发中...
   1. 方法：插入、修改
   1. 属性：插入
3. Lua（0%）的针对性太强了，暂时不开放，暂时不打算重构有需要在说吧
3. Swift（5%），本人实际项目使用不多，故排在最后，看用户需求再决定
3. 其他功能：
   1. 迭代混淆（50%），保留上次混淆记录，用户只要维护源码就行
   1. 快速混淆模式
<a name="vlfzY"></a>
# 图文介绍
运行APP效果图，使用前请详细阅读[工具使用教程](https://www.yuque.com/docs/share/edd2603f-d09d-4795-ae71-b42419b99446?#《confuse使用说明》)<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/213807/1607931800015-f60e682f-6ef3-4c5a-bfc5-4c88222bb1a7.png#align=left&display=inline&height=540&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1080&originWidth=1920&size=489209&status=done&style=none&width=960)
<a name="WtuYs"></a>
# 更新日志
<a name="4HjGS"></a>
### v3.5.0（2021.01.08）

1. 新增OC[重命名多语言]，对直接或间接使用系统方法**NSLocalizedString**、**NSLocalizedStringFromTable**的多语言进行修改
1. 优化OC[重命名图片]、[插入文件]、[插入文本]，减少混淆时间20%~30%，文件不包含字符串时不处理
1. 修复OC[插入文件]bug，插入文件概率性出错（出现同名局部变量名view，导致编译报错）

[查看更多历史更新记录](https://www.yuque.com/docs/share/39f2f60e-b6a8-443b-b005-b9364fb79b95?#《confuse更新说明》)
<a name="63ca6131"></a>
# 感谢反馈
[shizu2014](https://github.com/shizu2014)、[myhonior](https://github.com/myhonior)、[imbahong](https://github.com/imbahong)
<a name="BUG"></a>
# 链接导航

1. [工具使用教程](https://www.yuque.com/docs/share/edd2603f-d09d-4795-ae71-b42419b99446?#《confuse使用说明》)
1. [软件使用问答（Q&A）](https://www.yuque.com/docs/share/4a87ec96-80fe-4d25-873d-93cb428b3e15?#《软件使用问答（Q&A）》)
1. [[修改方法]参数类型汇总表](https://www.yuque.com/docs/share/315b72d9-28f9-4fa6-bf20-c40d94f2253a?#《修改方法-支持参数类型汇总表》)
1. [[修改局部变量]修改局部变量-支持类型汇总表](https://www.yuque.com/docs/share/90444065-4f4e-49c8-9e1a-5bd3d3b4f84d?#《修改局部变量-支持类型汇总表》)
