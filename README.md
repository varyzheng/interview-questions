# 经典面试题

## 一. 浏览器输入一个URL，到页面完全展示出来，都经历了哪些过程？
0. **前提假设：** 先做一个前提假设，就是这个问题假设我们访问的是一个站点的主入口，去访问它的index.html
1. **DNS解析：** 浏览器首先会对输入的URL进行DNS解析，将域名转换为对应的IP地址。这个过程可能会涉及到本地DNS缓存、操作系统DNS缓存、ISP的DNS服务器以及根域名服务器等。
2. **建立TCP连接：** 浏览器与服务器建立TCP连接，通常使用三次握手协议。这个过程确保了客户端和服务器之间的通信是可靠的。
3. **发送HTTP请求：** 浏览器构建一个HTTP请求，然后通过已建立的TCP连接发送给服务器。
4. **服务器处理请求：** 服务器接收到HTTP请求后，进行处理，服务器会生成一个HTTP响应。一般这个响应是一个html文档, 如果是服务端渲染的架构，这可能包括查询数据库、执行服务器端代码等操作。如果是前后端分离的项目，那么可能是由nginx代理服务器返回一个静态的html文档。
5. **接收HTTP响应：** 浏览器接收到服务器返回的HTTP响应，包括响应状态码（如200、404等）、响应头（如Content-Type、Set-Cookie等）和响应体（本假设下是HTML文档）。
6. **解析HTML：** 浏览器开始解析HTML文档，构建DOM树。在这个过程中，浏览器可能会遇到CSS、JavaScript等资源的引用。
7. **请求CSS、JavaScript等资源：** 浏览器会发起额外的HTTP请求，获取CSS、JavaScript等资源。这些资源可能来自同一服务器，也可能来自第三方服务器（如CDN）。
8. **构建CSSOM树：** 浏览器解析CSS资源，构建CSSOM树。
9. **执行JavaScript：** 浏览器解析并执行JavaScript代码。这可能会修改DOM树和CSSOM树。
10. **生成渲染树：** 浏览器将DOM树和CSSOM树合并为渲染树，渲染树包含了页面上所有可见元素的布局信息。
11. **布局：** 浏览器根据渲染树计算每个元素的准确位置和大小，这个过程称为布局或重排。
12. **绘制：** 浏览器将渲染树中的每个元素绘制到屏幕上，这个过程称为绘制或重绘。
13. **页面展示：** 经过上述步骤，页面内容被完全展示在浏览器中。

## 二. 请描述一下三次握手和四次挥手，为什么握手是三次，而挥手是四次？
* 三次握手和四次挥手是TCP协议中用于建立和终止连接的过程。
* SYN是Synchronize的缩写，ACK是Acknowledgement的缩写，FIN是Finish的缩写。
* 三次握手的过程如下：
  1. 客户端向服务器发送一个SYN包，表示请求建立连接。
  2. 服务器收到SYN包后，回复一个SYN+ACK包，表示确认收到请求，并请求建立连接。
  3. 客户端收到SYN+ACK包后，回复一个ACK包，表示确认建立连接。
* 四次挥手的过程如下：
  1. 客户端向服务器发送一个FIN包，表示请求关闭连接。
  2. 服务器收到FIN包后，回复一个ACK包，表示确认收到请求。
  3. 服务器向客户端发送一个FIN包，表示请求关闭连接。
  4. 客户端收到FIN包后，回复一个ACK包，表示确认关闭连接。
* 需要四次挥手，是因为TCP协议需要确保双方都能够安全地关闭连接，以避免数据丢失或重复传输。关闭TCP链接的充分必要条件时，客户端和服务端都收到了对方的FIN和ACK。这里第2次和第3次不能合并发送，是因为服务端在收到客户端的FIN包后，可能还有数据需要传输，因此只能先回复ACK。在确保所有数据都传输完成后，再发送FIN包通知客户端，请求关闭连接。

## 三. HTTP 响应常见的状态码有哪些？其中301，302，304有什么区别？
### HTTP响应常见的状态码有以下几种：
* 1xx：信息性状态码，表示请求已被接收，继续处理。
* 2xx：成功状态码，表示请求已成功被服务器接收、理解、并接受。
* 3xx：重定向状态码，表示需要客户端进一步操作才能完成请求。
* 4xx：客户端错误状态码，表示客户端发送的请求有误。
* 5xx：服务器错误状态码，表示服务器无法完成请求。

### 301、302、304状态码的区别如下：
* 301 Moved Permanently：永久重定向。表示请求的资源已经被永久移动到了新的URL地址，客户端应该使用新的URL地址重新发送请求。搜索引擎会把新的URL地址作为该资源的唯一标识，原来的URL地址会被废弃。
* 302 Found：临时重定向。表示请求的资源已经被临时移动到了新的URL地址，客户端应该使用新的URL地址重新发送请求。搜索引擎会把原来的URL地址作为该资源的唯一标识，新的URL地址只是临时的。
* 304 Not Modified：未修改。表示客户端发送的请求的资源在服务器上没有被修改，可以直接使用客户端缓存的版本。服务器会返回一个空的响应体，客户端可以使用本地缓存的资源。

## 四. script 标签引入的 JavaScript 脚本是否会阻止 DOM树 和 CSSOM树 的构建？
### script标签引入的JavaScript脚本会阻止DOM树和CSSOM树的构建。
当浏览器解析HTML文档时遇到script标签时，会停止解析HTML文档，先加载和执行JavaScript脚本，然后再继续解析HTML文档。因此，如果JavaScript脚本文件很大或者加载速度很慢，会导致页面加载速度变慢，用户体验变差。
### 为了避免JavaScript脚本阻塞DOM树和CSSOM树的构建，可以使用以下方法：
1. 把script标签放在HTML文档底部，这样浏览器在解析HTML文档时会先构建DOM树和CSSOM树，然后再加载和执行JavaScript脚本，不会阻塞DOM树和CSSOM树的构建。
2. 使用defer属性，这样浏览器会异步加载JavaScript脚本，但会在HTML文档解析完成后按照顺序执行，不会阻塞DOM树和CSSOM树的构建。
3. 使用async属性，这样浏览器会异步加载JavaScript脚本，并且在加载完成后立即执行，不会阻塞DOM树和CSSOM树的构建。但是，使用async属性加载的JavaScript脚本不能保证按照顺序执行，因此适用于不依赖其他JavaScript脚本的情况。

## 五. DOM，BOM 和 CSSOM 分别是什么含义？
### DOM、BOM 和 CSSOM 是Web前端开发中常用的三个概念，它们分别代表了文档对象模型、浏览器对象模型和CSS对象模型，开发者可以通过JavaScript来操作这些对象，实现对文档、浏览器和样式的增删改查等操作。

* DOM（Document Object Model）文档对象模型：是指HTML或XML文档的编程接口，它将整个文档抽象成一个树形结构，每个节点都是一个对象，开发者可以通过JavaScript来操作这些对象，实现对文档的增删改查等操作。DOM分为标准DOM和扩展DOM，标准DOM是W3C制定的标准，而扩展DOM是浏览器厂商自己扩展的一些接口。

* BOM（Browser Object Model）浏览器对象模型：是指浏览器提供的一组JavaScript接口，用于操作浏览器窗口和框架，包括浏览器窗口大小、位置、历史记录、浏览器版本等信息。BOM是浏览器厂商自己定义的一些接口，不属于W3C标准。

* CSSOM（CSS Object Model）CSS对象模型：是指浏览器将CSS样式表抽象成一组对象，开发者可以通过JavaScript来操作这些对象，实现对样式的增删改查等操作。CSSOM是W3C制定的标准，它与DOM类似，但是它是针对CSS样式表的，而不是HTML文档。

## 六. 重排和重绘有什么区别？

* 重排（Reflow）：是指浏览器对页面进行重新布局的过程，也称为回流。当页面中的元素发生变化时，浏览器需要重新计算元素的位置和大小，然后重新布局整个页面。重排的代价比较高，会导致页面的性能下降，因此应该尽量避免。常见的触发重排的属性有：width和height、margin和padding、border、display、position、float、clear、top、right、bottom、left、font-size、line-height、`background-attachment: fixed`
* 重绘（Repaint）：是指浏览器对页面进行重新绘制的过程。当页面中的元素的样式发生变化时，浏览器会重新绘制这些元素，但不会重新布局整个页面。重绘的代价比重排低，但也会影响页面的性能。 常见的触发重绘的属性有： color、background-color、border-color、outline-color、box-shadow、text-shadow、opacity、transform、visibility、background-image

重排和重绘的区别在于，重排会重新计算元素的位置和大小，然后重新布局整个页面，而重绘只会重新绘制元素的样式，不会重新布局整个页面。因此，重排的代价比重绘高，应该尽量避免。开发者可以通过以下方法来减少重排和重绘的次数：

1. 使用CSS3动画代替JavaScript动画，因为CSS3动画可以使用GPU加速，性能更好。

2. 避免频繁操作DOM元素，可以使用文档片段（DocumentFragment）来减少DOM操作次数。

3. 使用CSS Sprites来减少HTTP请求次数，提高页面加载速度。

4. 使用CSS3的transform属性来代替position属性，因为transform属性不会触发重排。

5. 使用requestAnimationFrame方法来代替setTimeout方法，因为requestAnimationFrame方法可以更好地控制动画的帧率，避免过度绘制。

## 七. 前端性能优化方案有哪些？
1. 资源优化：
* 压缩和合并：对CSS、JavaScript文件进行压缩和合并，减少文件大小和请求数量。
* 图片优化：使用合适的图片格式（如WebP）、压缩图片、使用CSS Sprites或SVG来减少图片大小和请求数量。
* 字体优化：选择合适的字体格式，减少字体文件大小。

2. 缓存策略：
* 使用浏览器缓存：通过设置HTTP响应头的Cache-Control和Expires字段，让浏览器缓存静态资源。通常是仅为index.html 设置不缓存，其余所有静态资源使用缓存，因为代码修改后会生成新的hash值，index.html 所引用的资源会相应改变。
* 使用Service Worker：通过Service Worker实现离线缓存和资源预加载，提高页面加载速度。

3. 网络优化：
* 使用CDN：将静态资源部署到CDN，减少网络延迟。
* 使用HTTP/2：相较于HTTP/1.1，HTTP/2提供了多路复用、头部压缩等特性，可以提高网络传输效率。
* 预加载和预渲染：使用 `<link rel="preload">`、`<link rel="prefetch">`等技术预加载关键资源，提高页面加载速度。

4. 代码优化：
* 代码分割：将大型JavaScript文件拆分为多个小文件，实现按需加载。
* 懒加载：对于图片、组件等资源，实现懒加载，只在需要时加载。
* 避免阻塞渲染：将关键CSS内联到HTML中，将非关键CSS和JavaScript设置为异步加载，以避免阻塞页面渲染。

5. 优化渲染性能：
* 减少重排和重绘：避免频繁修改DOM和CSS，使用requestAnimationFrame进行动画处理。
* 使用CSS3动画：相较于JavaScript动画，CSS3动画性能更好，尽量使用CSS3动画实现页面效果。
* GPU加速：通过使用transform、opacity等属性，触发GPU加速，提高渲染性能。

6. 框架和库的选择：
* 选择性能优秀的框架和库：在项目中使用性能优秀的框架和库，如React、Vue等。
* 按需引入：只引入所需的库和框架功能，避免加载不必要的代码。

7. Web性能监控：
* 使用前端监控工具：通过Google Lighthouse、WebPageTest等工具，定期检查网站性能，找出性能瓶颈。
* 实时监控：使用Real User Monitoring（RUM）工具，收集用户真实访问数据，分析性能问题。

8. 服务端渲染（SSR）和静态站点生成（SSG）：
* 服务端渲染：通过在服务器端渲染页面，减少浏览器端的渲染时间，提高首屏加载速度。
* 静态站点生成：对于内容较为固定的网站，使用静态站点生成器（如Gatsby、Next.js等）生成静态页面，提高页面加载速度。

## 八. Vue2 和 Vue3 分别是如何实现双向绑定的？
1. Vue2 双向绑定实现：
* Vue2 使用数据劫持和发布订阅模式来实现双向绑定。具体来说，Vue2 使用 Object.defineProperty 对数据对象的属性进行劫持，分别设置 getter 和 setter 方法。当数据发生变化时，setter 方法会被触发，通知依赖该数据的订阅者更新视图。

Vue2 的双向绑定主要包括以下几个步骤：
* 数据观察：使用 Object.defineProperty 对数据对象的每个属性进行递归劫持。
* 依赖收集：当访问数据时，getter 方法会被触发，收集当前数据的依赖（即订阅者，通常是 Vue 组件实例）。
* 订阅者更新：当数据发生变化时，setter 方法会被触发，通知所有依赖该数据的订阅者更新视图。

2. Vue3 双向绑定实现：
* Vue3 使用Proxy对象来实现双向绑定。相较于 Vue2 的 Object.defineProperty，Proxy 提供了更强大的数据劫持能力，可以直接劫持整个对象，而不仅仅是对象的属性。这使得 Vue3 的双向绑定实现更加简洁高效。

Vue3 的双向绑定主要包括以下几个步骤：
* 数据观察：使用 Proxy 对数据对象进行劫持，拦截对象的各种操作，如获取、设置、删除属性等。
* 依赖收集：当访问数据时，Proxy 的 get 方法会被触发，收集当前数据的依赖（即订阅者，通常是 Vue 组件实例）。
* 订阅者更新：当数据发生变化时，Proxy 的 set 方法会被触发，通知所有依赖该数据的订阅者更新视图。

总之，Vue2 和 Vue3 在实现双向绑定方面的主要区别在于底层数据劫持的技术：Vue2 使用 Object.defineProperty，而 Vue3 使用 Proxy。这使得 Vue3 的双向绑定实现更加简洁高效。

## 九. Vue2 在组建件更新时的diff算法时怎样的？
Vue2 在组件更新时使用了一种高效的 diff 算法，称为 Virtual DOM（虚拟 DOM）。这种算法基于树的同层比较策略，只会比较同一层级的节点，而不会跨层级进行比较。这大大提高了 diff 算法的性能。

Vue2 的 diff 算法主要包括以下几个步骤：

1. 生成 Virtual DOM：当组件的状态发生变化时，Vue2 会根据最新的状态生成一颗新的 Virtual DOM 树。

2. 比较新旧 Virtual DOM：Vue2 使用 diff 算法比较新旧 Virtual DOM 树，找出需要更新的节点。diff 算法主要包括以下几个方面的比较：

* 节点类型：如果新旧节点的类型不同（如元素节点和文本节点），则直接替换旧节点。
* 节点标签：如果新旧节点的标签不同（如 div 和 span），则直接替换旧节点。
* 节点属性：如果新旧节点的属性不同（如 class 和 style），则更新旧节点的属性。
* 子节点：如果新旧节点的子节点不同，Vue2 会使用一种称为 "双端比较" 的策略来比较子节点。具体来说，Vue2 会同时从子节点的头部和尾部进行比较，根据不同情况进行节点的移动、插入和删除操作。
* 更新真实 DOM：根据 diff 算法得到的需要更新的节点，Vue2 会对真实 DOM 进行相应的更新操作。

其中，子节点的“双端比较”策略是指：
1. 定义新旧子节点的头部和尾部指针，分别为 oldStartIdx、oldEndIdx、newStartIdx 和 newEndIdx。

2. 进行循环比较，直到新旧子节点的任一头部指针大于尾部指针为止。在循环中，根据以下四种情况进行处理：

* 头部节点相同：如果新旧子节点的头部节点相同（即 oldStartVNode 和 newStartVNode 相同），则对头部节点进行更新，并将新旧子节点的头部指针分别向后移动一位（即 oldStartIdx++ 和 newStartIdx++）。

* 尾部节点相同：如果新旧子节点的尾部节点相同（即 oldEndVNode 和 newEndVNode 相同），则对尾部节点进行更新，并将新旧子节点的尾部指针分别向前移动一位（即 oldEndIdx-- 和 newEndIdx--）。

* 旧头部节点与新尾部节点相同：如果旧子节点的头部节点与新子节点的尾部节点相同（即 oldStartVNode 和 newEndVNode 相同），则对旧头部节点进行更新，并将其移动到旧子节点的尾部。然后将旧子节点的头部指针向后移动一位（即 oldStartIdx++），新子节点的尾部指针向前移动一位（即 newEndIdx--）。

* 旧尾部节点与新头部节点相同：如果旧子节点的尾部节点与新子节点的头部节点相同（即 oldEndVNode 和 newStartVNode 相同），则对旧尾部节点进行更新，并将其移动到旧子节点的头部。然后将旧子节点的尾部指针向前移动一位（即 oldEndIdx--），新子节点的头部指针向后移动一位（即 newStartIdx++）。

3. 当循环结束后，可能会出现以下两种情况：

* 新子节点未处理完：如果新子节点的头部指针仍小于等于尾部指针（即 newStartIdx <= newEndIdx），说明还有新子节点未处理。此时，将这些新子节点插入到旧子节点的合适位置。具体来说，可以将这些新子节点插入到旧子节点的头部指针所指向节点的前面（如果头部指针已越过尾部指针，则插入到旧子节点的尾部节点之后）。

* 旧子节点未处理完：如果旧子节点的头部指针仍小于等于尾部指针（即 oldStartIdx <= oldEndIdx），说明还有旧子节点未处理。此时，将这些旧子节点从 DOM 中移除，因为它们已经不存在于新子节点中了。

通过这种双端比较策略，Vue2 能够在比较新旧子节点时更加高效地进行节点的移动、插入和删除操作，从而提高页面渲染性能。

## 十. React的setState是如何更新页面状态的？
React 的 setState 方法用于更新组件的状态（state）。当你调用 setState 时，React 会执行以下步骤来更新页面状态：

1. 合并状态：setState 可以接受一个新的状态对象或一个返回新状态对象的函数作为参数。React 会将新的状态对象与当前状态对象进行浅合并（shallow merge），即只合并最外层的属性。嵌套的对象和数组不会被合并，需要开发者自行处理。

2. 调度更新：setState 是异步的，这意味着 React 不会立即更新状态和重新渲染组件。相反，React 会将更新操作放入一个队列中。这样做的目的是为了优化性能，因为多个 setState 调用可能会在一个事件循环（event loop）中被批量处理，从而减少不必要的渲染。

3. 更新生命周期：当更新操作被调度后，React 会触发组件的更新生命周期。首先，React 会调用 shouldComponentUpdate 方法（如果组件定义了该方法）来决定是否需要更新组件。如果 shouldComponentUpdate 返回 false，则更新过程会被跳过，否则会继续执行以下步骤。

4. 渲染组件：React 会调用组件的 render 方法来生成新的虚拟 DOM（Virtual DOM）树。

5. 比较虚拟 DOM：React 会使用 diff 算法比较新旧虚拟 DOM 树，找出需要更新的节点。

6. 更新真实 DOM：根据 diff 算法得到的需要更新的节点，React 会对真实 DOM 进行相应的更新操作。

7. 完成更新：在真实 DOM 更新完成后，React 会调用 componentDidUpdate 方法（如果组件定义了该方法）来通知组件更新已完成。