# Vue Introduce

这篇PPT是关于最近在做H5版杭州公交的经验分享，里面部分细节也包括工作一年的总结。

## 简介

![简介](http://xiangsongtao.com/uploads/1481464550000.jpeg)

Vue是我在8月份第一次接触，用周末两天时间熟悉了下API及Vue全家桶，框架学完练练手可以熟悉思路，在之后的项目中能马上使用到。所以我将我的博客从Angualr1.3改写到Vue1.0，花了不少下班时间。项目是由vue-cli基础上开发，现已在GitHub上开源，另外还包括对博客支持的后台代码，使用的是Node+Express+Mongodb。

之后Vue2.0发布后，趁着项目测试的空隙，又将博客升级到2.0，改动的部分及总结在博客中已写出，可以作为升级的参考，[链接](http://xiangsongtao.com/article/5846277f089b4c33d4ec79a9)。到目前为止，已经使用Vue直接开发了3个项目，加上今天的分享，应该是第四个了。因为Vue对浏览器的唯一的限制是IE9+，故移动端应该是没问题的。

## 目录

![目录](http://xiangsongtao.com/uploads/1481464684000.jpeg)

这个是ppt的顺序，有些思路可能某些同学没接触过，我也是用比较形象的方式给大家讲解。我希望以后大家在学到新的东西都能通过这种方式分享出来共同进步，回顾总结，加深自己的印象。


## 如何选择框架

![如何选择框架](http://xiangsongtao.com/uploads/1481464705000.jpeg)

这里主要是我对我已有的经验来分析如何根据项目来选择框架。可能由我的局限性，希望能和大家碰撞下思想。

### 1. 对于后台管理系统

我曾经使用过两次jQuery+ifream框架（二次维护，历史遗留问题），通过接口获取数据后拼接HTML字符串然后append到DOM中，因为需求没有设计成UI，当时拿到的是功能清单，UI只能靠我们前端自己设计。因此在写render函数之前，我将画好的UI给leader看，然后讨论交互，连问多遍有没问题后才敢开始写render函数，因为需求改动后，render函数就会废弃再写！不过最后结果还好，返工的不多。另一个是给后台写jsp模板，分不清前后端界线，js交互也不敢多写（后端同学理解JS也是头疼）。

以上，也许也能看出问题点了，代码无法复用，需求改动后代码改动多；前后端界线不清，工作内容会有重叠，口头确认不是很好的规范。所以我倾向于基于MVVM模式的SPA系统。前后端由API文档划分界线，因为不涉及到首次加载速度、浏览器适配范围和SEO等问题，我认为SPA的方式是可行的，也是大多数公司在用的方式，例如阿里云控制台、收钱吧控制台、盈联智能商户管理系统等。

### 2. 对于面对消费者的PC页面

这种场景，使用后端渲染的方式无可厚非，加载快，利于SEO，页面可缓存等优点。

### 3. 移动端展示类型的页面

这种情况大多数不会超过5个页面（类似于抽奖、活动展示、答题、留言板等），且页面间衔接的紧密度不高，使用zepto框架即可。开发快速，a标签跳转，如果不喜欢300ms点击延迟，那就上fastclick就好，一般开发人员就能胜任。

### 4. 移动微应用

最后一个场景就是较为复杂的移动微应用，**古老的方法**是使用后端渲染的方式进行，例如jsp、hbs等，如果不同页面间数据需要传递也只能放到url中或者sessionStorage或者localStorage中，开发逻辑不好维护，需要加大量的注释。页面跳转还需要再次加载公共的脚本，如果服务器开了缓存还好，否则用户体验不好。我这里倾向于使用SPA模式开发，由路由控制增量加载的内容（懒加载），webpack进行代码分割，确保首屏之外的资源按需加载。前端控制范围增多，API划分界限。

## MVC和MVVM对比

![MVC和MVVM对比](http://xiangsongtao.com/uploads/1481464740000.jpeg)

Vue是基于MVVM开发模式的框架。在MVVM之前还有很多开发模式，不过都被踩在时代的脚下，比如MVC。就我经验所及的范围，我认为MVVM在处理UI交互方面应该是抽象的最全面的模式。

上张图是MVVM的结构，在SilverLight或者WPF开发中常用的模式，同理，前端也是和UI交互的部分，模式借鉴很有意义。View层只依赖数据，降低View-Model与View之间的耦合；只要数据相同，View-Model可复用，UI测试转化为对Data的测试。

jQuery是MVC开发模式的一个代表。在jQuery代码中，controller中存在大量的和view耦合的逻辑，例如jq中的html片段拼接，无法复用和维护困难。另外，controller基本没有测试的可能。


## MVC和MVVM业务实现区别

### 1. MVC方式

![MVC方式](http://xiangsongtao.com/uploads/1481464793000.jpeg)


以我现在在做的页面对MVC和MVVM做个对比。

页面1是第一次的需求，我按照页面将HTML片段和数据拼接写好render函数，之后append到页面中，数据通过ajax获取。当UED改图时（页面2），可以看出页面的结构发生了变化：由之前的上下结构变成了左右结构，对用的HTML片段也要发生变化，故render函数需要修改以适应新需求，我醉了。。。这种需求以前遇到很多回了！view和controlelr高度耦合，并且HTML片段不易再次维护，不可测试也无法复用。

### 2. MVVM方式

![MVVM方式](http://xiangsongtao.com/uploads/1481464827000.jpeg)

如果页面是使用MVVM模式做的话，从上面可看出，页面的数据是没变动的，只是HTML变动，故使用Vue双向绑定的基础功能，我这里只需要修改HTML即可，View-Model中的代码无需修改。另外，只要数据格式没问题，页面的渲染效果也固定，可测试。之后讲解下代码复用部分。

### 3. View-Model代码复用

![View-Model代码复用](http://xiangsongtao.com/uploads/1481464853000.jpeg)

换乘摘要[1]、换乘详情[2]、换乘地图[3]三部分的页面有公共个数据部分：URL参数（起始终止位置信息+换乘策略信息）、当前策略下换乘信息（allData -> 包含起止经纬度、摘要和详情），换乘地图独用transferMap。因此，此部分的View-Model可复用，只需要判断path后开启必要的部分即可。

**以上就是MVVM的三个优点：**低耦合、可复用、可测试、易维护。


## Vue性能

![Vue性能](http://xiangsongtao.com/uploads/1481464887000.jpeg)

我一般在使用Vue都是在做移动端开发，既然选用是看中他的两个特性：**运行快+加载快**；上图是Vue2.0的综合性能对比图，右边是Benchmark跑分（不服跑跑分），绿色越深代表越好。红框圈出的部分从左到右依次是react、Vue2.0、原生JS。想必大家能看出来差异了。值得一说的是，Vue2.0是基于更快的Virtual-DOM。

## Vue构建大型项目及生态

![Vue构建大型项目及生态](http://xiangsongtao.com/uploads/1481464916000.jpeg)

Vue在构建大型项目也不逊色，因为Vue.js自身只是处理View层的框架，做大型项目需要协同插件的协助，这个与react很像。可以说Vue构建大型项目的插件都已完备。参见上图。

## 我为什么使用Vue

![我为什么使用Vue](http://xiangsongtao.com/uploads/1481464940000.jpeg)

最后说下我为什么倾向于使用Vue。我认为，工具的价值在于为人服务。Vue及其附属的Vue-cli工具很好的在开发者体验和最终生成代码之间做好了桥梁，即：说明文档详细，代码编写简单，API抽象到位方便别人阅读，最终代码运行高效。

另外，框架的选择取决于团队的水平，Vue还是很简单的，而且还是中文文档哦。


## 最后

> **用了Vue，我可以正常下班了**       


(完)