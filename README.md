### Hi there 👋 

![Flynn Cao's GitHub stats](https://github-readme-stats-git-masterrstaa-rickstaa.vercel.app/api?username=FlynnCao&&show_icons=true&theme=dark)
<!-- Start

  <!--
  **FlynnCao/FlynnCao** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

  Here are some ideas to get you started:

  - 🔭 I’m currently working on ...
  - 🌱 I’m currently learning ...
  - 👯 I’m looking to collaborate on ...
  - 🤔 I’m looking for help with ...
  - 💬 Ask me about ...
  - 📫 How to reach me: ...
  - 😄 Pronouns: ...
  - ⚡ Fun fact: ...
  -->

### ✏️ Blog posts
<!-- BLOG-POST-LIST:START --><a href=`https://xlog.app/api/redirection?characterId=49213&noteId=10`>Thu May 04 2023 6:55 AM - 浏览器插件开发简易指南0</a><br><p style=`height:300px;overflow:hidden`>最近想参与一个开源项目，用于 moji 词典的点击查询日语单词，比官方的用的顺手一些。用着还不错但发现年久失修。

项目地址： https://github.com/Yukaii/mojidict-helper
Chrome 商店： https://chrome.google.com/webstore/detail/mojidict-helper/eknkjedaohafedihakaobhjfaabelkem

本篇只展示 Vite/Webpack+React 在 Chrome 浏览器中的使用：
你可以在 Chrome 浏览器地址栏键入chrome://extensions/来查看你现在安装的所有插件，
另外请保证右上角的开发者模式是打开的：

💻 开发环境
一般
IDE: Visual Studio Code
React: v18+
Vite: v3+
Vite: v4.2.0
Webpack: v5.76.2
我使用了pnpm来管理本机的所有依赖，如果没有或者使用其他包管理工具的可以自行替换pnpm为对应命令！
其他配合食用的

TailwindCSS
TypeScript

📦打包工具 （Module Bundler）
原生开发 （VanillaJS）
直接参考官方给的这些 demo 即可：
https://github.com/GoogleChrome/chrome-extensions-samples
Webpack
https://github.com/lxieyang/chrome-extension-boilerplate-react
Vite （推荐）
https://github.com/Jonghakseo/chrome-extension-boilerplate-react-vite
如何使用这个库？clone 后安装包依赖pnpm install，然后启动开发模式：
pnpm dev: 这是开发模式
pnpm build： 这是生产模式
以上命令生成的代码都会在 build 目录下，因此只需要到chrome://extension目录载入一下这个目录就能边开发边看结果了：

📑官方文档
https://developer.chrome.com/docs/extensions/
👟前置准备
生命周期

onInstalled: 安装、更新或重新加载插件触发
onSuspend: 插件即将被挂起触发
onSuspendCanceled: 插件被挂起取消时触发
onUpdateAvailable：插件可更新时（可以用于提醒用户）触发
onStartup: Chrome 启动并加载扩展时触发
onConnect 事件： 插件与 Chrome 的另一部分（例如内容脚本）建立连接时，将触发
onConnectExternal：当来自外部应用程序（例如本机应用程序）的连接建立时触发
onMessage： 当从 Chrome 的另一部分（例如内容脚本）接收到消息时，将触发 onMessage 事件

如何在代码中使用它？
chrome.runtime.XX XX 为上述对应的一个生命周期名称，如：
Copychrome.runtime.onInstalled.addListener&lpar;&lpar;details&rpar; =&gt; {
  console.log&lpar;&#39;Extension installed:&#39;, details.reason&rpar;;
}&rpar;;

插件配置 manifest.json
从这点就有很多和 manifest v2 版本很不同的地方，比较重要的是 manifest v2 的直接修改 don 机制被替换成 service worker，大意是不用他的时候该 service worker 会被 chrome 选择性屏蔽，但我们仍然可以通过其他方式操纵 dom。

详细解释： https://developer.chrome.com/docs/extensions/mv3/service_workers/

一个不太复杂的 manifest 配置文件长这样
Copy{
	&quot;manifest_version&quot;: 3,
	&quot;name&quot;: &quot;Life Helper&quot;,
	&quot;version&quot;: &quot;1.0.0&quot;,
	&quot;description&quot;: &quot;The things we must know in our life.&quot;,
	&quot;icons&quot;: {
		&quot;16&quot;: &quot;icon.png&quot;,
		&quot;48&quot;: &quot;icon.png&quot;,
		&quot;128&quot;: &quot;icon.png&quot;
	},
	&quot;action&quot;: {
		&quot;default_popup&quot;: &quot;popup.html&quot;,
		&quot;default_title&quot;: &quot;Popup&quot;
	},
	&quot;permissions&quot;: [
		&quot;scripting&quot;,
		&quot;bookmarks&quot;
	],
	&quot;background&quot;: {
		&quot;service_worker&quot;: &quot;background.js&quot;
	},
	&quot;options_page&quot;: &quot;options.html&quot;
}


manifest.json的格式请：https://developer.chrome.com/docs/extensions/mv3/manifest/

我们需要理解的只有


permissions 开启的权限就对应了你在 background.js 能够调用的 api，如bookmarks说明你可以用  chrome.runtime 下面的方法


actions 主要规定弹出层窗口的位置名称等

popup 点击插件图标弹出的页面




background 对应 service_worker 的承载对象，可以理解为插件后端 / 服务端


contentScript 前端脚本的位置，这里直接对应 react 的程序入口 &lpar;app.tsx-&gt;index.tsx&rpar;，之后所有逻辑 &amp;#x26; 样式 &lpar;popup, options, newTab&rpar; 都参照 react 的组织形式即可


开发流程
各模块的分工如下：

简单来说，所有之前交由后端的工作都由background来完成，所有之前交由前端的都由contentScript负责的options, newTab, popup来完成。
这也意味着，当你想做状态管理时，也能很好地遵循原子性原则组织各自的状态（M）和样式（V）、类型（M）等。
通信
通信在这里并没有明确的方向性，甚至也没有文件、模块等限制，需要就用。
但根据我们先前规定各模块大致的工作范畴，作为前端在通讯时最好也是来沟通各组件的样式、API 调用、用户事件，而后端在通讯时优先考虑更好地组织数据、构建 API 或执行外部请求。
基本格式如下：
Copy// 发送消息，并回调
chrome.runtime.sendMessage&lpar;{greeting: &quot;hello&quot;}&rpar;
  .then&lpar;response =&gt; console.log&lpar;response.farewell&rpar;&rpar;
  .catch&lpar;error =&gt; console.error&lpar;error&rpar;&rpar;;

// 接收消息，并回调
chrome.runtime.onMessage.addListener&lpar;function&lpar;request, sender&rpar; {
  console.log&lpar;sender.tab ?
              &quot;from a content script:&quot; + sender.tab.url :
              &quot;from the extension&quot;&rpar;;
  if &lpar;request.greeting == &quot;hello&quot;&rpar; {
    return Promise.resolve&lpar;{farewell: &quot;goodbye&quot;}&rpar;;
  }
}&rpar;;

消息发送和接受可以筛选 tab id，并指定类型，假设我们想从 popup 往 background 发送消息，并指定当前活动标签页：
全局状态管理和插件数据存储
快使用 Redux Toolkit！
时至今日，原生的 Redux 不再被推荐，新的 Redux Toolkit 也可以帮助我们更好结合现有模块作出分工，有类似pinia的用法：
我们首先初始化整体的，根据先前我们的分工，这里最好在后端（即 background 模块）来完成初始化和服务的构建。
Copy//store.ts
import { configureStore } from &#39;@reduxjs/toolkit&#39;;
import { slice1, slice2 } from &#39;./slices&#39;;

export default configureStore&lpar;{
  reducer: {
    slice1: slice1.reducer,
    slice2: slice2.reducer,
  },
}&rpar;;
export default store;

对于各自的 slice，定义如下：
Copyimport { createSlice } from &#39;@reduxjs/toolkit&#39;;

// 定义初始值
const initialState1 = {
  value: 0,
};

// 定义第一个slice
export const slice1 = createSlice&lpar;{
  name: &#39;slice1&#39;,
  initialState: initialState1,
  reducers: {
    increment1: state =&gt; {
      state.value += 1;
    },
    decrement1: state =&gt; {
      state.value -= 1;
    },
  },
}&rpar;;
export default slice1.reducer;

和 chrome.storage 结合保证状态持久
这里需要安装两个库redux-persist-storage-chrome和redux-persist，然后做如下改动：
在store.ts中改动如下：
Copyimport { configureStore } from &#39;@reduxjs/toolkit&#39;
import { chromeStorage } from &#39;redux-persist-storage-chrome&#39;;
import { persistReducer } from &#39;redux-persist&#39;;
import { slice1, slice2 } from &#39;./slices&#39;;

const persistConfig = {
	key: &#39;chrome-extension-extended&#39;,
	storage: chromeStorage,
}


export const store = configureStore&lpar;{
  reducer: {
    slice1: persistReducer&lpar;persistConfig, slice1.reducer&rpar;,
    slice2: persistReducer&lpar;persistConfig, slice2.reducer&rpar;,
  },
}&rpar;

export default store;

调试
如果你使用了 HMR 或者其他形式的热重载， 应该要关心的就只是 background 或 contentScript 本身发出的消息。

监视 background 脚本的方法：

在chrome://extensions/页的插件程序直接点击 service worker，会弹出单独的调试窗口


监视 options, popup 或者 new tab 等页面：

直接在对应的页面右键-&gt;inspect ，会弹出单独的调试窗口
总结
这篇给完全没接触过浏览器插件的小伙伴做一个快速指南，其实我也是刚入门，不过好在官方给了很多 sample，网上也有大量的现成教程和 boilderplates 来上手。
预告
下篇会尝试研究 chrome-extension-boilerplate-react-vite 项目的 rollup 和 ws 结合过程。（这个已经不属于插件开发的范畴了）
下篇会研究其他浏览器的插件与拓展其他 boilderplate 在 Vue3 上的使用。
（05/08 更新：）
antfu 的浏览器 extension 模板很好用。但在 firefox 上行不通，问题在于 web-ext 且Firefox 浏览器对 manifestV3 的支持仍不稳定。
🧩参考
https://dev.to/anobjectisa/how-to-build-a-chrome-extension-new-manifest-v3-5edk</p><a href=`https://xlog.app/api/redirection?characterId=49213&noteId=5`>Mon Mar 13 2023 2:57 AM - Word Power Made Easy &lpar;持续更新中）</a><br><p style=`height:300px;overflow:hidden`>一点小小的英语词汇提升路径：
前一段大概用网站测了下词汇量 &lpar;vocabulary.com8700, 扇贝 13000&rpar;，和成年 native 还相去甚远。课堂上的东西倒是能应付，但是对付我的论文可能有些捉襟见肘了。另外提升写作这个目前没想到什么好方法（我是 grammarly 重度用户），想到了再分享。
词根知识 &lpar;待补充&rpar;
词根（root，root word）又称语根、根词 [1]，是基本构词的基本词素，与词缀相对并携带主要词汇信息。[2][3][4][5][6]
词根有两种，能够独立构词的为自由词根（free root），必须与其他词素组合构词的是黏着词根（bound root）。[7][8][9]
什么时候需要这本书？
构词的前提是你对足够数量的单词足够熟悉，不然会出现用 @#@#% 找 A&amp;#x26;**@!# 的尴尬境地。我认为至少过了国内英语 CET6 再考虑这本，当然已经考过雅思托福 PTE 之类的出国考试更好。
本书的前两个章节是考试章节，如果结果是 below average 作者给出的建议是好好掂量掂量，如果是 average 及以上就推荐可以推进了。
如何使用？
以下是我的方法：

每天至少推进一个 session
每一个字都要读下来
做练习，自己画词源树（后面有我画的示例）
复习或者回顾的时候边读边拼写（因为词根和实际的单词一定有出入）

| 目前没有想着用它造句或者文章之类的，这又回到了我一开始说的问题，输出有限。

part1 结束按照上述方法得到了 94/120 分，失分点主要都在拼写上了.. 血的教训，因此还是推荐给大家加一个拼写 + 发音

如何计划？
我使用trello来管理我的计划，之前有使用过一段时间的linear.app也是免费的，但完全没有移动端，体验不佳。trello 比 notion 做项目规划的好处是自带功能足够丰富。
trello 的教程很多，这一个用来速成是 ok 的：
https://www.youtube.com/watchv=6drUzoeHZkg&amp;#x26;ab_channel=Kim%26Co.byKimberlyAnnJimenez
这是我的卡片：

看看你的？
练习和纠错
你想问我怎么处理练习的？很遗憾这不是应试，所以我并不会纠错，对于错误的回答，我就会重新回去再重读一遍重新理解。
词源树
你想问我怎么画词源树的？我把 Greek 雅典词源作为红色，把拉丁词源作为蓝色，把法国来源作为紫色
（思路是这样的：雅典是西方文明的母亲，拉丁和美洲靠近，法国有浪漫的标签，嗯记住这些你也记住我的颜色标记法了，灰色的话是词义和词根组合随着时代发展有出入）
言归正传，以 session10 精神科从业者这一篇为例，词源树可以这么画：

注意：不要标注中文意思。
如果你想看我画的每个章节完整的词源树，请看这里：&lpar;使用 drawIO 绘制&rpar;
https://drive.google.com/file/d/169vodHFGfrvv-jKUL57xWsnWZmDV9_ks/view?usp=sharing
口语规律（慎入）
Q:  这不是词汇吗。怎么扯到口语了？
A：你要明确一点，表音文字的文字是为口语服务的。而且很多时候词汇会为口语让步或者退化（a/an 在元音前的表现就是很好的例子），例如英语世界经常会用的asap=as soon as possible要读 eisep 而不是 esesp 原因是？当然是为了让人听得懂了
Q：那我能从这本书学到什么发音规律吗？
A： 这是我目前总结的。相信你读完也能感受到规律（我用感受是因为感受了你就真的会用了，自然而然地）。词根不是重音时或多或少会出现被部分牵连重音的情况。

体现专业性或学科性的ology logy  及其接续在单词中会以最重的音来读。
如：

Copypsychologist -&gt;  /saɪˈkɒl.ə.dʒɪst/

注意，ch 不是元音，因此会被元音 o 重化（即使 ch 是 psyche 的一部分）

体现职业的，以职业作为最重音：
如：

Copyorthodontist -&gt; /ˌɔː.θəˈdɒn.tɪst/   [重点：dontist]
optician -&gt;/ɒpˈtɪʃ.ən/  [重点：医师]
psychiatrist -&gt; /saɪˈkaɪə.trɪst/  [重点：iatreia的人]

注意：optician 如果你试图不重读 ti 发现后面也很难发力，因此最好的选择是把 ti 也重读（即使 opti - 的 ti 是 optiko 的一部分）
这本书有什么用？
大概是会激活一些你平时压根不会注意到的细节，提升你对英语的喜爱。比如说前几章大量涵盖了医学领域的词汇，后几章我还没看完。
总结
英语并不是一门 “精确” 的语言，但她确实是一门强逻辑语言，英语的精确建立在其无限的修饰之上，构词是修饰，词块是修饰，句子也是修饰，从句和主句也是修饰，有分析的成分吗？有，但你需要等她修饰完。
词源构词法相关视频
https://www.youtube.com/watch?v=QZ4yuGaFQMc</p><a href=`https://xlog.app/api/redirection?characterId=49213&noteId=3`>Sat Mar 04 2023 8:16 AM - S2 学期总结</a><br><p style=`height:300px;overflow:hidden`>由于当前还不是年终，此文章写于出成绩的第二天，本学期目标本来是全 pass，完成目标。目前绩点不算完美但可以接受。&lpar;全 A 选手？当我不是和本科阶段一样是班上最活跃的人我就知道自己和 A + 无缘了&rpar;
这学期感觉自己的 Software Engineering 理解又深刻了一些，虽然精力管理还是一样烂（悲）
代码倒是真没怎么写，大概提升了一下理论 &amp;#x26; 研究。

以下按照每个科目大概总结一些要素，括号后为人生时间占比：
Software Validation &amp;#x26; Verification &lpar;12%&rpar;
这门课的实际出勤比较少，恰逢大选圣诞节等等无限放假，但可以说涵盖了测试的大部分流程，recap 的要点如下：

Unit Test / System Test / User Acceptance Test
Blackbox / Whitebox Test
Test Plan / Log / Procedure / Specifications / Result / Summary

总之是 test 层面的东西是全覆盖到了。大部分时间确实在撸文档上。结论是实际测试的过程和构思要比开发细致很多，完全可以作为一个部门来分工协作（实际上我们 Group Project 也是在做这件事）。
Advanced Internet of Things &lpar;5.39%&rpar;
这门课作为选修占用的时间并不少，由于我自己也是个硬件小白，不过好在 arduino 并不算难，组装 -&gt; 代码糅合 -&gt; 就完成了，电路图我是真不懂。只是后期改进的阶段费了我不少周章。不过这个 Smart Home 是比我本科做的 project 实在多了：
输出一些环境监测参数后如何接受？ 请看这个库：
https://github.com/FlynnCao/WOC7020-Smart-Home
改进包括发邮件，如何实现？ 请看这个库（这里 Gmail 曲线救国，动手能力强的同学放自己服务器上也不是不可以）：
https://github.com/FlynnCao/WOC7020-Smart-Home-Backend
最终结论是：这种项目，树莓派最方便最好用，证明前期硬件 Plan 是项目正确推进的基石。（贴一张实验室照骗）

Research Methodology （4.74%）
对于需要修 Dissertation 的学生，这门课是很重要的。如你所见我并没有占用太多时间，不过我可以自信的低说我已经掌握了所需的研究方法了，接下来就是想一个比劲爆的课题。
重点其实还是在 Literature Review 上以及如何筛选。当然经过了这门课和（周四）我会在任何自己写的文章里加上引用.. 事实上只要不是你自己的想法，无论是图片（商用除外）、文段还是表格等其他形式的信息都要加上引用。
EndNote 很好用，但学校的图书馆并不好用。
Recap 的话：就是 LR 的各种类型以及围绕写 LR 的方法论。但目前看来是人类是绕不过 AI 这个课题了。
Architecturing Software Systems &lpar;7.23%&rpar;
这门课倒是这个学期最喜欢的，因为以前自己 big mud 风格很少会对框架层面进行思考，vue 的 p-b 架构也是无感。这门课 Recap 的话能想到的比较多，有：

Quality Attributes
Architecture Patterns
Views &amp;#x26; Models

QA 的话可以和 SVV 那门课联动一下，当然这门课可以和任何 CS 的程序联动一下，因为架构本质上就是思维模式，和设计模式或者算法等等没差。
Advcanced Machine Learning &lpar;5.23%&rpar;
这门课谈不上喜欢也谈不上讨厌，之前我是觉得 AI 很无聊。现在我仍然觉得他有非常对局限性（可能是我自己对 Chatgpt 调教不行？我觉得他只是比搜索引擎好用一点点的工具罢了）
就我自己的论断，AI 在艺术创作也可能威胁到人类，只是大家并不会因为这个吵吵着谁失业，人文不被归属在生产力范畴，也饿不死。
我同意的观点大概是如那位新加坡医学教授的话：会淘汰不用 AI 的人。
这门课 Recap 的话大概是 ML 的种种特征：
一言以蔽之，就是输入、输出，训练 + 测试，调参。数据集和应用的模型都很重要。
这个视频很好，可以给完全无概念的人一个 ML 的入门：
https://www.youtube.com/watch?v=5q87K1WaoFI&amp;#x26;ab_channel=WIRED
（Computer Scientist Explains Machine Learning in 5 Levels of Difficulty | WIRED）
结语
OK。目前对于我个人的任务就是找有趣的课题 + 继续我的 Coding 之路了。语言层面还是继续精进英文（口语 + 写作）和日语考级。
下学期有可能的话希望减肥 + Fiteness 一下！？。</p><a href=`https://xlog.app/api/redirection?characterId=49213&noteId=2`>Fri Feb 24 2023 4:19 AM - Obisidian 生活记录系统实操</a><br><p style=`height:300px;overflow:hidden`>最近重新翻 diygod 的博客，然后发现了半年前的这个系统。正好最近自己也在用 toggl track 还有其他一系列奇奇怪怪的玩意。
以下是一个 Obsidian 小白的视角：
demo 和前期准备
插件
这个教程需要的插件如下：

toggl track integration

怎么安装呢，安装好 obsidian 确定好 vault 之后，点击左下角的小齿轮然后前往community plugins -&gt; browse，

在这里搜索我前面提到的 plugin 名字然后安装就可以了！
Toggl Track - 时间
这个很好解决，Toggl Track 的 api key 到 Toggl Trackprofile setting最下面下拉找到api key就能找到，复制后重新打开 obsidian 的 setting
输入 API token, 选好 Toggl Workspace, 点击 connect 测试一下：
我是输入这些 markdown 语法成功显示第一个统计的：
CopySUMMARY FROM 2023-01-01 TO 2023-01-31
SORT DESC


可以看到目前 project 很乱，起因是我是按照学期内的课程来统计的，简单粗暴又不包含任何的生活因素。为了帮助我分类和 tag，因此我去 youtube 又做了一些功课：
最后分为以下类型：

Life &lpar;Sort, Landury, Cooking, Eating, WC&rpar;
Sleep
Entertainment &lpar;Games, Music, Drama, Anime&rpar;
Social &lpar;Media, Chatting, Video Channel&rpar;
Code &lpar;Open-Source, Self-learning&rpar;
Try new things &lpar;Painting&rpar;
Language &lpar;Japanese, English and more&rpar;
Work &lpar;Cetus, Other billable projects &rpar;
Blog
Exercise &lpar;Sports, Jogging&rpar;
Reading &lpar;Technical books, Novels&rpar;
University &lpar;Courses, Group Work&rpar;
Waste
</p><a href=`https://xlog.app/api/redirection?characterId=49213&noteId=1`>Thu Feb 23 2023 12:22 PM - 来个新家</a><br><p style=`height:300px;overflow:hidden`>一开始还是蛮纠结自己究竟用什么语言，最终还是：中文最方便最熟练。
观察了一些其他人的首页，这里是可以当作 “中文” 的琐碎事情的汇总（当然，最少量的技术博客），英文可以在另外一个站点用算了！好了就这样了。
大概会有以下内容：

重量级作品的发布（如果有）
年终总结
自主性过强的书评，影评
手办或者自己买的一些小玩意的 showcase
旅游

总之既然有了博客还是希望自己能不要那么懒惰了（233），有时候是讲不好写什么。</p><!-- BLOG-POST-LIST:END -->

### :zap: Recent Activity

<!--START_SECTION:activity-->
1. 🗣 Commented on [#129](https://github.com/antfu/vitesse-webext/issues/129) in [antfu/vitesse-webext](https://github.com/antfu/vitesse-webext)
2. ❗️ Opened issue [#129](https://github.com/antfu/vitesse-webext/issues/129) in [antfu/vitesse-webext](https://github.com/antfu/vitesse-webext)
3. 💪 Opened PR [#30](https://github.com/Yukaii/mojidict-helper/pull/30) in [Yukaii/mojidict-helper](https://github.com/Yukaii/mojidict-helper)
4. ❗️ Opened issue [#1](https://github.com/FlynnCao/chrome-extension-manifest-v3/issues/1) in [FlynnCao/chrome-extension-manifest-v3](https://github.com/FlynnCao/chrome-extension-manifest-v3)
<!--END_SECTION:activity-->

 
 
 <p style="display:flex">
 <a href="https://t.me/flynncao/"><img src="https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white"/>
</a>
  
<a href="https://discord.gg/v2bzdj7j">
  <img src="https://img.shields.io/badge/Discord-7289DA?style=for-the-badge&logo=discord&logoColor=white"/>
 </a>
  
<a href="https://dev.to/flynncao">
  <img src="https://img.shields.io/badge/dev.to-0A0A0A?style=for-the-badge&logo=devdotto&logoColor=white"/>
  </a>
 
 <a href="https://www.goodreads.com/user/show/165341751-cornfieldchase">
   <img src="https://img.shields.io/badge/Goodreads-372213?style=for-the-badge&logo=goodreads&logoColor=white"/>
  </a>
  
 
  
</p>
 







