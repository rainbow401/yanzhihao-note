# Lunz+ angular框架

常用组件：

## ngx-query

在组件中声明变量如下变量

```` tsx
queryTemplates: QueryTemplate[] = [
  {
    name: 'Default',
    template: {
      op: 'and',
      rules: [
        { field: 'organCode', op: 'eq' },
        { field: 'organName', op: 'cn' },
        { field: 'retailFormat', op: 'eq', data: '' },
        { field: 'organLevel', op: 'eq', data: '' },
      ],
      groups: [],
    },
  },
];

queryMode: QueryMode = QueryMode.plainCollapse; //有展开和收缩按钮的模式
collapseType: CollapseToolbarLayout = CollapseToolbarLayout.Inline; //查询按钮所在位置，inline为与查询条件同行
````

在组件的模板html文件中使用，并且绑定ts中声明的属性

```` html
<ngx-query
  #ngxQuery
  [mode]="queryMode"
  [collapseToolbarLayout]="collapseType"
  class="full-screen no-header"
  [columnNumber]="3"
  [queryTemplates]="queryTemplates"
  [showModeButtons]="true"
  >
  <ngx-query-field name="organCode" label="组织编码" [type]="'string'" [isCollapse]="true">
    <ng-template ngx-query-value-input-template let-rule="rule" let-dataIndex="dataIndex" let-options="custom">
      <input nz-input placeholder="请输入" [(ngModel)]="rule.datas[dataIndex]" />
    </ng-template>
  </ngx-query-field>
  <ngx-query-field name="organName" label="组织名称" [type]="'string'" [isCollapse]="true" [canDisable]="true">
    <ng-template ngx-query-value-input-template let-rule="rule" let-dataIndex="dataIndex" let-options="custom">
      <input nz-input placeholder="请输入" [(ngModel)]="rule.datas[dataIndex]" />
    </ng-template>
  </ngx-query-field>
  <ngx-query-field name="retailFormat" label="所属业态" [type]="'string'" [canDisable]="true">
    <ng-template ngx-query-value-input-template let-rule="rule" let-dataIndex="dataIndex" let-options="custom">
      <nz-select nzPlaceHolder="请选择" [(ngModel)]="rule.datas[dataIndex]">
        <nz-option nzLabel="请选择" nzValue=""></nz-option>
        <nz-option *ngFor="let item of retailList" [nzLabel]="item.name" [nzValue]="item.id"></nz-option>
      </nz-select>
    </ng-template>
  </ngx-query-field>
  <ngx-query-field name="organLevel" label="组织层级" [type]="'string'" [isCollapse]="true" [canDisable]="true">
    <ng-template ngx-query-value-input-template let-rule="rule" let-dataIndex="dataIndex" let-options="custom">
      <nz-select nzPlaceHolder="请选择" [(ngModel)]="rule.datas[dataIndex]">
        <nz-option nzLabel="请选择" nzValue=""></nz-option>
        <nz-option *ngFor="let item of organLevelList" [nzLabel]="item.name" [nzValue]="item.id"></nz-option>
      </nz-select>
    </ng-template>
  </ngx-query-field>
</ngx-query>
````

## ngx-datatable

### 使用示例：

```` html
<ngx-datatable
  #dt
  [footerHeight]="10"
  class="material"
  [rowHeight]="50"	
  [headerHeight]="56"
  [scrollbarH]="true"
  [scrollbarV]="true"
  [saveState]="false"
  columnMode="force"
  [rows]="dataList"
  (activate)="showReports($event)"
  [loadingIndicator]="isLoading"
  #appNgxDataTable="NgxDataTableDirective"
  appNgxDataTable
  [ngxQuery]="ngxQuery"
  (data)="loadList($event)"
>
  <ngx-datatable-column name="公司名称" headerClass="text-left" cellClass="text-left">
    <ng-template let-row="row" ngx-datatable-cell-template>
      <span class="m-tooltip" [pTooltip]="row.organName" tooltipPosition="bottom">
        {{ row.organName | isNull }}
      </span>
    </ng-template>
  </ngx-datatable-column>
  <ngx-datatable-column
    name="操作"
    [width]="230"
    [resizeable]="false"
    [sortable]="false"
    [frozenRight]="true"
    [frozenLeft]="false"
     headerClass="text-center"
   >
    <ng-template let-row="row" ngx-datatable-cell-template>
      <app-list-btn
       [appApplyPermission]="'delete'"
       *ngIf="row.reportStatus === 'DT0000000087'"
       [btnTitle]="'删除'"
       (action)="deleteReport(row)"
       [isTrue]="true"
       appNgxDatatableBtn
      ></app-list-btn>
      <app-list-btn
       [appApplyPermission]="'detail'"
       [btnTitle]="'操作记录'"
       (action)="actionRecord(row)"
       [isTrue]="true"
       appNgxDatatableBtn
      ></app-list-btn>
    </ng-template>
  </ngx-datatable-column>
</ngx-datatable>
````

### 使用经验：

1.列居中（与列宽有关）

![image-20220112151409522](/Users/zhihaoyan/Library/Application Support/typora-user-images/image-20220112151409522.png)



## ant-design

按照官方文档介绍使用即可

## 注意事项

### 代码规范：

1.不能在html标签中写style，可以使用[ngStyle]

```` html
错误示例：
<div style='width: 100px'></div>
正确示例：
<div [ngStyle]="{'width': '100px'}"></div>
````

2.格式化代码

​	要使用Prettier格式化代码

以上两点提交代码时验证不通过，不符合规范无法提交代码

