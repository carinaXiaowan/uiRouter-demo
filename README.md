# uiRouter-demo
http://www.cnblogs.com/haogj/p/4885928.html
ui-router 是 AngularUI 库提供的特别有用的一个部分，是一个通过提供状态机机制，而不是简单的 URL 来组织我们的界面的路由框架。
ui-router 从状态着手来管理路由，它将应用视为多个状态的组合，通过状态的切换进行路由。
一个状态可以对应一个页面地址，通过特定的地址到达应用的特定状态。
通过状态的 controller、template 和 views 来管理特定状态的 UI 和行为
通过嵌套视图来解决页面中重复出现的内容。
当在 ui-router 中考虑路由和状态的关系时，我们主要关注应用的什么状态对应应用的什么路由。
url 用来设置应用对应的一个特定状态. 也就是说，我们可以通过 url 来到达某个特定的状态，所以这里的 url 不是简单的 url 地址，而是某个可到达状态的标志。


方便获得当前状态的方法，绑到根作用域

app.run(['$rootScope', '$state', '$stateParams', function($rootScope, $state, $stateParams) { $rootScope.$state = $state; $rootScope.$stateParams = $stateParams; } ]);
绝对路由

使用绝对 url 方式，需要在 url 字符串的开发加上特殊字符 ^

'contacts'状态将匹配"/contacts"
'contacts.list'状态将匹配"/list"。子状态的url没有附在父状态的url之后的，因为使用了^。
嵌套路由

创建嵌套的视图（案列1）
我们可以使用 url 参数添加到路由中来实现嵌套路由。这样可以提供多个 ui-views 在我们的页面中，例如，我们可以在 /inbox 之上，提供嵌套的独立路由。这里使用了子状态。


第一个路由与前面一样，现在还有第二个路由，一个匹配 inbox 之下的子路由，语法 (.) 表示这是一个子路由。
/inbox/1 匹配第一个状态，/inbox/1/priority 则匹配第二个状态。使用这种语法，我们可以在父路由中支持嵌套的 url。在父视图中的 ui-view 指令将会处理 priority。

Views（单页面定义多个视图）

单个页面使用多个视图（案列2）
我们可以在 state 中提供命名的视图


注意：千万不要忘了写url!!!!!!!!!!!!!!!!!!!!!!!
abstract 抽象模板

抽象模板不能被激活，但是它的子模板可以被激活。
抽象模板可以提供一个包括了多个有名的视图的模板
它可以传递作用域变量$scope给子模板。
使用它可以在同一个url下传递自定义数据或者预载入的依赖。
除了需要添加abstract属性外，其他设置和设定一个常规状态是相同的：
事件	
描述
$scope.$on('$stateChangeStart', 
function(evt, toState, toParams, fromState, fromParams){
  // 如果需要阻止事件的完成
  evt.preventDefault();
});
状态改变事件

stateChangeStart

当状态改变开始的时候被触发
$stateChangeSuccess

当状态改变成功后被触发
$stateChangeError

当状态改变遇到错误时被触发，错误通常是目标无法载入，需要预载入的数据无法被载入等。
$viewContentLoading

当视图正在被载入且在DOM被渲染之前触发
$viewContentLoaded

当视图被载入且DOM已经渲染完成后被触发
$stateParams 状态参数

在上面提及使用$stateparams来提取在url中的不同参数。该服务的作用是处理url的不同部分。例如，当上述的inbox状态是这样时：
url: '/inbox/:inboxId/messages/{sorted}?from&to'

//当用户访问者链接时：
'/inbox/123/messages/ascending?from=10&to=20'
$stateParams对象的值为：
{inboxId: '123', sorted: 'ascending', from: 10, to: 20}

$urlRouterProvider

方法
参数
$urlRouterProvider.when('', '/inbox')

1.当前的路径
2.需要重定向到的路径（或者是需要在路径被访问是运行的函数）
$urlRouterProvider.otherwise('/');
otherwise()只接受一个参数，要么函数要么字符串，字符串必须为合法的url路由地址
