---
title: pureadmin-table
tag:
  - vue
  - pureadmin
  - admin
categories:
  - 前端开发
article_type: 1
no_word_count: false
no_toc: false
no_date: false
no_declare: false
no_reward: false
no_comments: false
no_share: false
no_footer: false
mathjax: false
abbrlink: 
top:
---

# PureAdmin Table 类型定义文档

## 概述

本文档描述了 `@pureadmin/table` 组件库的完整类型定义，包括表格组件、分页组件、加载配置等相关类型。

## 目录

- [基础类型](#基础类型)
- [表格属性 (TableProps)](#表格属性-tableprops)
- [Pure表格属性 (PureTableProps)](#pure表格属性-puretableprops)
- [表格列配置 (TableColumns)](#表格列配置-tablecolumns)
- [分页配置 (PaginationProps)](#分页配置-paginationprops)
- [加载配置 (LoadingConfig)](#加载配置-loadingconfig)
- [国际化配置](#国际化配置)
- [安装选项](#安装选项)

## 基础类型

### Size
```typescript
type Size = "large" | "default" | "small";
```
组件尺寸类型。

### Align
```typescript
type Align = "left" | "center" | "right";
```
对齐方式类型。

### Effect
```typescript
type Effect = "dark" | "light";
```
tooltip 效果类型。

### Layout
```typescript
type Layout = "fixed" | "auto";
```
表格布局类型。

## 表格属性 (TableProps)

基础的 Element Plus 表格属性配置。

### 基本属性

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `data` | `Array<any>` | - | 显示的数据（必填） |
| `height` | `string \| number` | - | Table 的高度 |
| `maxHeight` | `string \| number` | - | Table 的最大高度 |
| `stripe` | `boolean` | `false` | 是否为斑马纹 table |
| `border` | `boolean` | `false` | 是否带有纵向边框 |
| `size` | `Size` | - | Table 的尺寸 |
| `fit` | `boolean` | `true` | 列的宽度是否自撑开 |
| `showHeader` | `boolean` | `true` | 是否显示表头 |
| `highlightCurrentRow` | `boolean` | `false` | 是否要高亮当前行 |

### 行和单元格样式

| 属性名 | 类型 | 说明 |
|--------|------|------|
| `currentRowKey` | `string \| number` | 当前行的 key（只写属性） |
| `rowClassName` | `ColumnCls<any>` | 行的 className 回调方法 |
| `rowStyle` | `ColumnStyle<any>` | 行的 style 回调方法 |
| `cellClassName` | `CellCls<any>` | 单元格的 className 回调方法 |
| `cellStyle` | `CellStyle<any>` | 单元格的 style 回调方法 |
| `headerRowClassName` | `ColumnCls<any>` | 表头行的 className 回调方法 |
| `headerRowStyle` | `ColumnStyle<any>` | 表头行的 style 回调方法 |
| `headerCellClassName` | `CellCls<any>` | 表头单元格的 className 回调方法 |
| `headerCellStyle` | `CellStyle<any>` | 表头单元格的 style 回调方法 |

### 数据处理

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `rowKey` | `string \| ((row: any) => string)` | - | 行数据的 Key |
| `emptyText` | `string` | `"No Data"` | 空数据时显示的文本内容 |
| `defaultExpandAll` | `boolean` | `false` | 是否默认展开所有行 |
| `expandRowKeys` | `any[]` | - | 展开行的 keys 数组 |
| `defaultSort` | `Sort` | - | 默认的排序列的 prop 和顺序 |

### Tooltip 配置

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `tooltipEffect` | `Effect` | `"dark"` | tooltip effect 属性 |
| `tooltipOptions` | `TableOverflowTooltipOptions` | - | 溢出 tooltip 的选项 |

### 合计行配置

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `showSummary` | `boolean` | `false` | 是否在表尾显示合计行 |
| `sumText` | `string` | `"合计"` | 合计行第一列的文本 |
| `summaryMethod` | `SummaryMethod<any>` | - | 自定义的合计计算方法 |
| `spanMethod` | `Function` | - | 合并行或列的计算方法 |

### 选择和展开

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `selectOnIndeterminate` | `boolean` | `true` | 多选表格中点击表头多选框的行为 |
| `indent` | `number` | `16` | 展示树形数据时，树节点的缩进 |
| `lazy` | `boolean` | - | 是否懒加载子节点数据 |
| `load` | `Function` | - | 加载子节点数据的函数 |

### 树形数据配置

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `treeProps` | `{ hasChildren?: string; children?: string; }` | `{ hasChildren: 'hasChildren', children: 'children' }` | 渲染嵌套数据的配置选项 |

### 其他配置

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `tableLayout` | `Layout` | `"fixed"` | 设置表格单元、行和列的布局方式 |
| `scrollbarAlwaysOn` | `boolean` | `false` | 总是显示滚动条 |
| `flexible` | `boolean` | `false` | 确保主轴的最小尺寸 |

## Pure表格属性 (PureTableProps)

扩展的表格属性，继承自 `TableProps`。

### 扩展属性

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `tableKey` | `string \| number` | - | 唯一键，用于多表格实例区分 |
| `columns` | `Array<TableColumns>` | - | Table-column 配置（必填） |
| `loading` | `boolean` | `false` | 是否开启加载动画 |
| `loadingConfig` | `LoadingConfig` | - | 加载动画的相关配置 |
| `alignWhole` | `Align` | `"left"` | 对齐方式 |
| `headerAlign` | `Align` | - | 表头对齐方式 |
| `showOverflowTooltip` | `boolean` | `false` | 当内容过长被隐藏时显示 tooltip |
| `rowHoverBgColor` | `string` | - | 鼠标经过行时的背景色 |
| `pagination` | `PaginationProps` | - | 分页相关配置 |
| `adaptive` | `boolean` | `false` | 表格是否撑满内容区自适应高度 |
| `adaptiveConfig` | `AdaptiveConfig` | - | 撑满内容区自适应高度相关配置 |
| `locale` | `DefaultLanguage \| Language` | - | 国际化配置 |

### 自适应配置 (AdaptiveConfig)

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `offsetBottom` | `number` | `96` | 表格距离页面底部的偏移量 |
| `fixHeader` | `boolean` | `true` | 是否固定表头 |
| `timeout` | `number` | `60` | 页面 resize 时的防抖时间（ms） |
| `zIndex` | `number` | `3` | 表头的 z-index |

## 表格列配置 (TableColumns)

表格列的完整配置选项。

### 基本属性

| 属性名 | 类型 | 说明 |
|--------|------|------|
| `label` | `string` | 显示的标题 |
| `prop` | `string \| ((index: number) => string)` | 字段名称 |
| `type` | `TableColumnType` | 列的类型（selection/index/expand） |
| `index` | `number \| ((index: number) => number)` | 自定义索引 |
| `columnKey` | `string` | column 的 key |
| `width` | `string \| number` | 对应列的宽度 |
| `minWidth` | `string \| number` | 对应列的最小宽度 |
| `fixed` | `TableColumnFixed` | 列是否固定（true/left/right） |

### 渲染配置

| 属性名 | 类型 | 说明 |
|--------|------|------|
| `renderHeader` | `Function` | 列标题 Label 区域渲染函数 |
| `formatter` | `Function` | 用来格式化内容 |
| `showOverflowTooltip` | `boolean` | 当内容过长被隐藏时显示 tooltip |
| `align` | `Align` | 对齐方式 |
| `headerAlign` | `Align` | 表头对齐方式 |
| `className` | `string` | 列的 className |
| `labelClassName` | `string` | 当前列标题的自定义类名 |

### 排序配置

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `sortable` | `TableColumnSortable` | `false` | 对应列是否可以排序 |
| `sortMethod` | `Function` | - | 指定数据按照哪个属性进行排序 |
| `sortBy` | `string \| Function \| string[]` | - | 指定数据按照哪个属性进行排序 |
| `sortOrders` | `Array<TableColumnSortOrders>` | `['ascending', 'descending', null]` | 数据在排序时所使用排序策略的轮转顺序 |

### 其他配置

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `resizable` | `boolean` | `true` | 对应列是否可以通过拖动改变宽度 |
| `selectable` | `Function` | - | 仅对 type=selection 的列有效 |
| `reserveSelection` | `boolean` | `false` | 仅对 type=selection 的列有效 |

### 过滤配置

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `filters` | `Array<{text: string; value: string}>` | - | 数据过滤的选项 |
| `filterPlacement` | `TableColumnFilterPlacement` | - | 过滤弹出框的定位 |
| `filterClassName` | `string` | - | 过滤弹出框的 className |
| `filterMultiple` | `boolean` | `true` | 数据过滤的选项是否多选 |
| `filterMethod` | `Function` | - | 数据过滤使用的方法 |
| `filteredValue` | `Array<any>` | - | 选中的数据过滤项 |

### 扩展属性

| 属性名 | 类型 | 说明 |
|--------|------|------|
| `hide` | `boolean \| CallableFunction` | 是否隐藏 |
| `slot` | `string` | 自定义列的内容插槽 |
| `headerSlot` | `string` | 自定义表头的内容插槽 |
| `children` | `Array<TableColumns>` | 多级表头 |
| `cellRenderer` | `Function` | 自定义单元格渲染器（jsx语法） |
| `headerRenderer` | `Function` | 自定义头部渲染器（jsx语法） |

## 分页配置 (PaginationProps)

分页组件的配置选项。

### 必填属性

| 属性名 | 类型 | 说明 |
|--------|------|------|
| `pageSize` | `number` | 每页显示条目个数（必填） |
| `total` | `number` | 总条目数（必填） |
| `currentPage` | `number` | 当前页数（必填） |

### 可选属性

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `size` | `Size` | `"default"` | 分页大小 |
| `background` | `boolean` | `false` | 是否为分页按钮添加背景色 |
| `defaultPageSize` | `number` | - | 每页显示条目数的初始值 |
| `pageCount` | `number` | - | 总页数 |
| `pagerCount` | `number` | - | 设置最大页码按钮数 |
| `defaultCurrentPage` | `number` | - | 当前页数的初始值 |
| `layout` | `string` | `"total, sizes, prev, pager, next, jumper"` | 组件布局 |
| `pageSizes` | `number[]` | `[5, 10, 15, 20]` | 每页显示个数选择器的选项设置 |
| `popperClass` | `string` | - | 每页显示个数选择器的下拉框类名 |
| `prevText` | `string` | - | 替代图标显示的上一页文字 |
| `nextText` | `string` | - | 替代图标显示的下一页文字 |
| `disabled` | `boolean` | `false` | 是否禁用分页 |
| `hideOnSinglePage` | `boolean` | - | 只有一页时是否隐藏 |
| `align` | `Align` | `"right"` | 分页的对齐方式 |
| `style` | `CSSProperties` | - | 自定义分页样式 |
| `class` | `string` | - | 自定义类名 |

## 加载配置 (LoadingConfig)

加载动画的相关配置。

| 属性名 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `text` | `string` | - | 显示在加载图标下方的加载文案 |
| `spinner` | `string` | - | 自定义加载图标 |
| `svg` | `string` | - | 自定义 svg 加载图标 |
| `viewBox` | `string` | - | 自定义 svg 加载图标的大小 |
| `background` | `string` | - | 背景遮罩的颜色 |

## 国际化配置

### DefaultLanguage
```typescript
type DefaultLanguage = "en" | "zhCn" | "zhTw";
```

### Language
```typescript
type Language = {
    name: string;
    el: TranslatePair;
};
```

### TranslatePair
```typescript
type TranslatePair = {
    [key: string]: string | string[] | TranslatePair;
};
```

## 安装选项

### PureTableInstallOptions

| 属性名 | 类型 | 说明 |
|--------|------|------|
| `locale` | `DefaultLanguage \| Language` | 国际化配置 |
| `i18n` | `I18n` | 自适应国际化语言 |
| `ssr` | `boolean` | 是否是服务端渲染 |

## 类型枚举

### TableColumnType
```typescript
type TableColumnType = "selection" | "index" | "expand";
```

### TableColumnSortable
```typescript
type TableColumnSortable = false | true | "custom";
```

### TableColumnFixed
```typescript
type TableColumnFixed = true | "left" | "right";
```

### TableColumnSortOrders
```typescript
type TableColumnSortOrders = "ascending" | "descending" | null;
```

### TableColumnFilterPlacement
```typescript
type TableColumnFilterPlacement = "top-start" | "top-end" | "top" | "bottom-start" | "bottom-end" | "bottom" | "left-start" | "left-end" | "left" | "right-start" | "right-end" | "right";
```

## 使用示例

### 基本用法

```typescript
import type { PureTableProps, TableColumns } from '@pureadmin/table';

const columns: TableColumns[] = [
  {
    label: '姓名',
    prop: 'name',
    width: 120
  },
  {
    label: '年龄',
    prop: 'age',
    width: 80,
    sortable: true
  }
];

const tableProps: PureTableProps = {
  data: [],
  columns,
  loading: false,
  pagination: {
    pageSize: 10,
    total: 0,
    currentPage: 1
  }
};
```

### 自定义渲染

```typescript
const columns: TableColumns[] = [
  {
    label: '操作',
    prop: 'action',
    cellRenderer: ({ row, index }) => {
      return (
        <el-button
          type="primary"
          size="small"
          onClick={() => handleEdit(row, index)}
        >
          编辑
        </el-button>
      );
    }
  }
];
```

## 参考链接

- [Element Plus Table 组件](https://element-plus.org/zh-CN/component/table.html)
- [Element Plus Pagination 组件](https://element-plus.org/zh-CN/component/pagination.html)
- [Element Plus Loading 指令](https://element-plus.org/zh-CN/component/loading.html)