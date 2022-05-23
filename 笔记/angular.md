# Angular

## 什么是Angular？

Angular 是一个基于 [TypeScript](https://www.typescriptlang.org/) 构建的开发平台。它包括：

- 一个基于组件的框架，用于构建可伸缩的 Web 应用

- 一组完美集成的库，涵盖各种功能，包括路由、表单管理、客户端-服务器通信等

- 一套开发工具，可帮助你开发、构建、测试和更新代码


## 创建一个组件先决条件

要创建一个组件，请先验证你是否满足以下先决条件：

1. [安装 Angular CLI](https://angular.cn/guide/setup-local#install-the-angular-cli)。

2. [创建一个带有初始项目的 Angular 工作区](https://angular.cn/guide/setup-local#create-a-workspace-and-initial-application)。如果还没有项目，你可以用 `ng new <project-name>` 创建一个，其中 `<project-name>` 是你的 Angular 应用的名字。

## 使用 Angular CLI 创建一个组件

1. 在终端窗口中，导航到要放置你应用的目录。
2. 运行 `ng generate component <component-name>` 命令，其中 `<component-name>` 是新组件的名字。

 默认情况下，该命令会创建以下内容：

- 一个以该组件命名的文件夹
- 一个组件文件 `<component-name>.component.ts`
- 一个模板文件 `<component-name>.component.html`
- 一个 CSS 文件， `<component-name>.component.css`
- 测试文件 `<component-name>.component.spec.ts`

 其中 `<component-name>` 是组件的名称。

## 组件

组件是构成应用的砖块。组件包括三个部分：带有 `@Component()` 装饰器的 TypeScript 类、HTML 模板和样式文件。`@Component()` 装饰器会指定如下 Angular 专属信息：

- 一个 CSS 选择器，用于定义如何在模板中使用组件。模板中与此选择器匹配的 HTML 元素将成为该组件的实例。
- 一个 HTML 模板，用于指示 Angular 如何渲染此组件。
- 一组可选的 CSS 样式，用于定义模板中 HTML 元素的外观。

下面是一个最小化的 Angular 组件：

```` typescript

@Component({
  selector: 'hello-world',
  template: `
    <h2>Hello World</h2>
    <p>This is my first component!</p>
  `
})
export class HelloWorldComponent {
  // The code in this class drives the component's behavior.
}
````

要在某个html页面中使用此组件，请在模板中编写以下内容：

```tsx
<hello-world></hello-world>
```

## 模板

每个组件都有一个 HTML 模板，用于声明该组件的渲染方式。

Angular 使用**额外**的语法扩展了 HTML，使你可以从组件中插入动态值。当组件的状态更改时，Angular 会自动更新已渲染的 DOM。此功能的应用之一是插入动态文本，如下例子所示。

```` tsx 
<p>{{ message }}</p>
````

这里 message 的值来自组件类：

````  tsx
import { Component } from '@angular/core';

@Component ({
  selector: 'hello-world-interpolation',
  templateUrl: './hello-world-interpolation.component.html'
})
export class HelloWorldInterpolationComponent {
    message = 'Hello, World!';
}
````

Angular 还支持**属性绑定**，以帮助你设置 HTML 元素的 Property 和 Attribute 的值，并将这些值传给应用的展示逻辑。

````  html
<p
  [id]="sayHelloId"
  [style.color]="fontColor">
  You can set my color in the component!
</p>
````

注意这里所用的方括号 —— 该语法表明你正在将 Property 或 Attribute 绑定到组件类中的值。

可以声明**事件监听器**来监听并响应用户的操作，例如按键、鼠标移动、单击和触摸等。你可以通过在圆括号中指定事件名称来声明一个事件监听器：

```` html
<button
  [disabled]="canClick"
  (click)="sayMessage()">
  Trigger alert message
</button>
````

```` typescript
sayMessage() {
  alert(this.message);
}
````

## 声明组件的样式

有两种方式可以为组件的模板声明样式：引用一个外部文件，或直接写在组件内部。

要在单独的文件中声明组件的样式，就要把 `styleUrls` 属性添加到 `@Component` 装饰器中。

```` typescript
@Component({
  selector: 'app-component-overview',
  templateUrl: './component-overview.component.html',
  styleUrls: ['./component-overview.component.css']
})
````

要想在组件内部声明样式，就要把 `styles` 属性添加到 `@Component`，该属性的内容是你要用的样式。

```` typescript
@Component({
  selector: 'app-component-overview',
  template: '<h1>Hello World!</h1>',
  styles: ['h1 { font-weight: normal; }']
})
````

`styles` 属性接受一个包含 CSS 规则的字符串数组。

## 组件生命周期

![Peek-a-boo](https://angular.cn/generated/images/guide/lifecycle-hooks/peek-a-boo.png)



## 目录结构分析

用angular cli新建项目之后，创建的这个项目下面的每个文件是干什么的呢？



![img](https://cdn.nlark.com/yuque/0/2018/png/218140/1544606097718-2906dc49-4642-46b2-9572-a0faa18b556d.png)



对每个文件的用途有了一些了解之后，我们再来看看几个比较重要的知识点



app.module.ts、组件分析

定义 AppModule，这个根模块会告诉 Angular 如何组装该应用。 目前，它只声明了 AppComponent。 稍后它还会声明更多组件。



app.module.ts

```typescript
//每个 Angular 应用都至少有一个 NgModule 类，也就是根模块，它习惯上命名为 AppModule，并位于一个名叫 app.module.ts 的文件中。

import { BrowserModule } from '@angular/platform-browser'; //浏览器解析的模块
import { NgModule } from '@angular/core'; //angular核心模块

import { AppComponent } from './app.component'; //根组件

//@NgModule()装饰器是一个函数，它接受一个元数据对象，该对象的属性用来描述这个模块。
@NgModule({

    //（可声明对象表） 那些属于本 NgModule 的组件、指令、管道都要在这里引入。
    declarations: [

        //引入了AppComponent组件
        AppComponent
    ],

    //（导入表）引入当前模块需要依赖的其他模块 。
    imports: [

        //引入了浏览器解析模块
        BrowserModule
    ],

    //在本模块中声明的一些组件、指令和管道在本模块任何地方都可以使用， 导出的这些可声明对象就是该模块的公共 API。
    exports:[],

    //定义的服务 ：本模块被所有的服务 都需要在providers中指定， 这些服务能被本应用中的任何部分使用。（你也可以在组件级别指定服务提供商，这通常是首选方式。）
    providers: [],

    //定义此 NgModule 中要编译的组件集，这样它们才可以动态加载到视图中。
    entryComponents:[],

    //应用的主视图，称为根组件。它是应用中所有其它视图的宿主。只有根模块才应该设置这个 bootstrap 属性。
    bootstrap: [

        //这里定义AppComponent为主视图
        AppComponent
    ]
})

//根模块不需要导出任何东西
export class AppModule { }
```



app.component.ts

```typescript
import { Component, OnInit } from '@angular/core';//引入angular核心

//@Component 装饰器会指出紧随其后的那个类是个组件类，并为其指定元数据。这里可以看出AppComponent只是一个普通类，只有给它加上了 @Component 装饰器，它才变成了组件。
@Component({

    //在其他地方使用这个组件时的名称：<app-root></app-root>
    selector: 'app-root',

    //该组件的 HTML 模板文件相对于这个组件文件的地址(相对路径)
    templateUrl: './app.component.html',

    //该组件的 css样式文件相对于这个组件文件的地址(相对路径)
    styleUrls: ['./app.component.css'],

    //在组件中指定服务
    providers: []
})
export class AppComponent implements OnInit {

    //类的成员(title属性) 类型为string 
    public title: string = 'first-app';

    //构造函数
    constructor() {

    }

    //初始化生命周期函数
    ngOnInit() {

    }
}
```

## 内置属性型指令

- [`NgClass`](https://angular.cn/guide/built-in-directives#ngClass) —— 添加和删除一组 CSS 类。
- [`NgStyle`](https://angular.cn/guide/built-in-directives#ngstyle) —— 添加和删除一组 HTML 样式。
- [`NgModel`](https://angular.cn/guide/built-in-directives#ngModel) —— 将数据双向绑定添加到 HTML 表单元素。

`Ngstyle`

可以用 `NgStyle` 根据组件的状态同时设置多个内联样式。

要使用 `NgStyle`，请向组件类添加一个方法。

```` typescript
currentStyles: Record<string, string> = {};
/* . . . */
setCurrentStyles() {
  // CSS styles: set per current state of component properties
  this.currentStyles = {
    'font-style':  this.canSave      ? 'italic' : 'normal',
    'font-weight': !this.isUnchanged ? 'bold'   : 'normal',
    'font-size':   this.isSpecial    ? '24px'   : '12px'
  };
}
````

要设置元素的样式，请将 `ngStyle` 属性绑定到 `currentStyles`。

```` html
<div [ngStyle]="currentStyles">
  This div is initially italic, normal weight, and extra large (24px).
</div>
````

在这个例子中，Angular 会在初始化以及发生更改的情况下应用这些类。完整的示例会在 `ngOnInit()` 中进行初始化以及通过单击按钮更改相关属性时调用 `setCurrentClasses()`。

```` html
<div [ngStyle]="{'width': '100px'}">
</div>
````



`NgClass`

`NgModel`

## 内置结构型指令

- [`NgIf`](https://angular.cn/guide/built-in-directives#ngIf) —— 从模板中创建或销毁子视图。
- [`NgFor`](https://angular.cn/guide/built-in-directives#ngFor) —— 为列表中的每个条目重复渲染一个节点。
- [`NgSwitch`](https://angular.cn/guide/built-in-directives#ngSwitch) —— 一组在备用视图之间切换的指令。

1.用 `NgIf` 添加或删除元素

可以将 `NgIf` 指令应用于宿主元素来添加或删除元素。

如果 `NgIf` 为 `false`，则 Angular 将从 DOM 中移除一个元素及其后代。然后，Angular 会销毁其组件，从而释放内存和资源。

要添加或删除元素，请在以下示例 `*ngIf` 绑定到条件表达式，例如 `isActive`

```` html
<app-item-detail *ngIf="isActive" [item]="item"></app-item-detail>
````

当 `isActive` 表达式返回真值时，`NgIf` 会把 `ItemDetailComponent` 添加到 DOM 中。当表达式为假值时，`NgIf` 会从 DOM 中删除 `ItemDetailComponent` 		并销毁该组件及其所有子组件。



2.`NgFor` 条目列表

可以用 `NgFor` 来指令显示条目列表。

1. 定义一个 HTML 块，该块会决定 Angular 如何渲染单个条目。
2. 要列出你的条目，请把一个简写形式 `let item of items` 赋值给 `*ngFor`。

```` html
<div *ngFor="let item of items">{{item.name}}</div>
````

字符串 `"let item of items"` 会指示 Angular 执行以下操作：

- 将 `items` 中的每个条目存储在局部循环变量 `item` 中
- 让每个条目都可用于每次迭代时的模板 HTML 中
- 将 `"let item of items"` 转换为环绕宿主元素的 `<ng-template>`
- 对列表中的每个 `item` 复写这个 `<ng-template>`



可以获取 `*ngFor` 的 `index`，并在模板中使用它。

在 `*ngFor` 中，添加一个分号和 `let i=index` 简写形式。下面的例子中把 `index` 取到一个名为 `i` 的变量中，并将其与条目名称一起显示。

```` html
<div *ngFor="let item of items; let i=index">{{i + 1}} - {{item.name}}</div>
````

`NgFor` 指令上下文的 `index` 属性在每次迭代中都会返回该条目的从零开始的索引号。



## 组件与模板语法

先推荐一个vscode的angular插件,可以在vscode右键文件目录时快速生成Component、Directive、Module、Pipe、Service、Class、Enum、Interface、Route：





![img](https://cdn.nlark.com/yuque/0/2019/png/218140/1552788941694-c095c26f-bafb-4a2f-aec6-dae3cabdf1c2.png)





![img](https://cdn.nlark.com/yuque/0/2019/png/218140/1552789182918-bc5624a6-62e8-4d10-b2d2-1fa42d6804e0.png)





- 使用插值表达式显示组件属性



先看新生成项目的代码：



![img](https://cdn.nlark.com/yuque/0/2019/png/218140/1552790195851-1abff462-e18a-49de-8705-dc466998f1d9.png)



模板中将组件属性名放进{{}}中，即可实现在模板中插入组件内的属性值，当这些属性值发生变化时，模板也会发生改变。




![img](https://cdn.nlark.com/yuque/0/2019/png/218140/1552790642784-dc33eb3b-0ba7-46af-bf24-db2db6408d86.png)







我们试着改变组件内title的值再保存：



![img](https://cdn.nlark.com/yuque/0/2019/png/218140/1552790700454-f46d690f-05c4-48ae-a779-b711dab83631.png)





没错，浏览器自动刷新并且更新了值。





- ngFor的使用



现在组件中新增一个数组类型的属性,并且使用ng的*[ngFor](https://angular.cn/api/common/NgForOf)指令进行遍历，如图：




![img](https://cdn.nlark.com/yuque/0/2019/png/218140/1552791333285-4b84faae-5126-489d-98e5-e46466fda4a8.png)









- ngIf的使用



在ng中 使用*ngIf指令进行条件显示，如图：




![img](https://cdn.nlark.com/yuque/0/2019/png/218140/1552791955508-60ab9dba-4737-4a5c-8a72-50e3fd2949f5.png)

ngIf会根据后面布尔值来显示当前模板或者移除当前模板，注意，时移除，不是隐藏。ngIf后面可以用简单的表达式语句。
