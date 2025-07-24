---
title: LiteGraph.js 类型定义文档
tag:
  - LiteGraph
  - 蓝图编辑
categories:
  - 前端开发
---

# LiteGraph.js 类型定义文档

## 概述

LiteGraph.js 是一个用于在浏览器中创建节点图的JavaScript库。本文档基于其TypeScript类型定义文件，提供了完整的API参考和中文说明。

## 基础类型

### Vector2
```typescript
type Vector2 = [number, number];
```
**说明**: 二维向量类型，表示 [x, y] 坐标。

### Vector4
```typescript
type Vector4 = [number, number, number, number];
```
**说明**: 四维向量类型，通常用于表示边界框 [x, y, width, height]。

### widgetTypes
```typescript
type widgetTypes = "button" | "toggle" | "slider" | "number" | "combo" | "text";
```
**说明**: 小部件类型枚举，定义了可用的UI控件类型。

### SlotShape
```typescript
type SlotShape = 0 | 1 | 2 | 3 | 4;
```
**说明**: 插槽形状类型，定义了节点输入/输出插槽的视觉形状。

## 插槽接口

### INodeSlot
```typescript
interface INodeSlot {
    name: string;        // 插槽名称
    type: string;        // 插槽数据类型
    link?: number;       // 连接的链接ID（可选）
    label?: string;      // 显示标签（可选）
    dir?: "UP" | "RIGHT" | "DOWN" | "LEFT";  // 插槽方向（可选）
    color_on?: string;   // 激活状态颜色（可选）
    color_off?: string;  // 非激活状态颜色（可选）
    shape?: SlotShape;   // 插槽形状（可选）
}
```
**说明**: 节点插槽的基础接口，定义了插槽的基本属性。

### INodeInputSlot
```typescript
interface INodeInputSlot extends INodeSlot {
    link: number | null;  // 输入插槽的链接ID
}
```
**说明**: 输入插槽接口，继承自基础插槽接口。

### INodeOutputSlot
```typescript
interface INodeOutputSlot extends INodeSlot {
    links: number[] | null;  // 输出插槽连接的所有链接ID数组
}
```
**说明**: 输出插槽接口，可以连接到多个输入插槽。

## 小部件接口

### WidgetCallback
```typescript
type WidgetCallback<T extends IWidget> = (
    this: T,
    value: T["value"],
    canvas: LGraphCanvas,
    node: LGraphNode,
    pos: Vector2,
    event?: Event
) => void;
```
**说明**: 小部件回调函数类型，当小部件值发生变化时调用。

### IWidget
```typescript
interface IWidget {
    name: string;           // 小部件名称
    value: any;            // 小部件当前值
    type: widgetTypes;     // 小部件类型
    options?: any;         // 小部件选项配置
    callback?: WidgetCallback<this>;  // 值变化回调
    marker?: boolean;      // 是否显示标记
    disabled?: boolean;    // 是否禁用
    readonly?: boolean;    // 是否只读
    property?: string;     // 关联的节点属性名
}
```
**说明**: 小部件基础接口，定义了所有小部件的通用属性。

### 具体小部件类型

#### IButtonWidget
```typescript
interface IButtonWidget extends IWidget {
    type: "button";
    value: undefined;
}
```
**说明**: 按钮小部件，用于触发操作。

#### IToggleWidget
```typescript
interface IToggleWidget extends IWidget {
    type: "toggle";
    value: boolean;
}
```
**说明**: 切换开关小部件，用于布尔值输入。

#### ISliderWidget
```typescript
interface ISliderWidget extends IWidget {
    type: "slider";
    value: number;
    options: {
        min?: number;      // 最小值
        max?: number;      // 最大值
        step?: number;     // 步长
        precision?: number; // 精度
    };
}
```
**说明**: 滑块小部件，用于数值范围选择。

#### INumberWidget
```typescript
interface INumberWidget extends IWidget {
    type: "number";
    value: number;
    options: {
        min?: number;      // 最小值
        max?: number;      // 最大值
        step?: number;     // 步长
        precision?: number; // 精度
    };
}
```
**说明**: 数字输入小部件，用于精确数值输入。

#### IComboWidget
```typescript
interface IComboWidget extends IWidget {
    type: "combo";
    value: string;
    options: {
        values: string[] | ((widget: IComboWidget, node: LGraphNode) => string[]);
    };
}
```
**说明**: 下拉选择小部件，用于从预定义选项中选择。

#### ITextWidget
```typescript
interface ITextWidget extends IWidget {
    type: "text";
    value: string;
    options?: {
        multiline?: boolean;  // 是否多行文本
        max_length?: number;  // 最大长度
        password?: boolean;   // 是否密码输入
    };
}
```
**说明**: 文本输入小部件，用于字符串输入。

## 上下文菜单

### IContextMenuItem
```typescript
interface IContextMenuItem {
    content: string;                    // 菜单项内容
    value?: any;                       // 菜单项值
    callback?: ContextMenuEventListener; // 点击回调
    submenu?: {
        options: ContextMenuItem[];
        callback?: ContextMenuEventListener;
    };                                 // 子菜单
    disabled?: boolean;                // 是否禁用
    title?: string;                    // 标题
    has_submenu?: boolean;             // 是否有子菜单
    className?: string;                // CSS类名
}
```
**说明**: 上下文菜单项接口，定义菜单项的属性和行为。

### IContextMenuOptions
```typescript
interface IContextMenuOptions {
    callback?: ContextMenuEventListener;  // 菜单回调
    event?: MouseEvent | CustomEvent;    // 触发事件
    parentMenu?: any;                    // 父菜单
    ignore_item_callbacks?: boolean;     // 是否忽略项回调
    title?: string;                      // 菜单标题
    extra?: any;                         // 额外数据
    autoopen?: boolean;                  // 是否自动打开
}
```
**说明**: 上下文菜单配置选项。

## LiteGraph 全局对象

### LiteGraph 常量
```typescript
declare const LiteGraph: {
    // 方向常量
    LEFT: 1;
    RIGHT: 2;
    UP: 3;
    DOWN: 4;
    
    // 链接类型
    STRAIGHT_LINK: 0;
    LINEAR_LINK: 1;
    SPLINE_LINK: 2;
    
    // 输入输出类型
    INPUT: 1;
    OUTPUT: 2;
    
    // 事件类型
    EVENT: -1;
    ACTION: -1;
    
    // 节点模式
    ALWAYS: 0;
    ON_EVENT: 1;
    NEVER: 2;
    ON_TRIGGER: 3;
    
    // 节点形状
    BOX_SHAPE: 1;
    ROUND_SHAPE: 2;
    CIRCLE_SHAPE: 3;
    CARD_SHAPE: 4;
    ARROW_SHAPE: 5;
    GRID_SHAPE: 6;
    
    // 插槽形状
    DEFAULT_SHAPE: 0;
    BOX_SHAPE: 1;
    ARROW_SHAPE: 2;
    GRID_SHAPE: 3;
    CIRCLE_SHAPE: 4;
    
    // 全局配置
    node_images_path: string;           // 节点图片路径
    debug: boolean;                     // 调试模式
    catch_exceptions: boolean;          // 是否捕获异常
    throw_errors: boolean;              // 是否抛出错误
    allow_scripts: boolean;             // 是否允许脚本
    registered_node_types: Record<string, LGraphNodeConstructor>; // 已注册的节点类型
    Nodes: Record<string, LGraphNodeConstructor>; // 节点类型映射
    searchbox_extras: Record<string, any>; // 搜索框扩展
    auto_sort_node_types: boolean;      // 是否自动排序节点类型
    node_types_by_file_extension: Record<string, string>; // 按文件扩展名分类的节点类型
    
    // 方法
    registerNodeType(type: string, baseClass: LGraphNodeConstructor): void;
    createNode(type: string): LGraphNode | null;
    getNodeType(type: string): LGraphNodeConstructor | null;
    getNodeTypesInCategory(category?: string): string[];
    getNodeCategories(): string[];
    clearRegisteredTypes(): void;
    
    // 工具函数
    compareObjects(a: any, b: any): boolean;
    distance(a: Vector2, b: Vector2): number;
    colorToString(c: [number, number, number]): string;
    isInsideRectangle(x: number, y: number, left: number, top: number, width: number, height: number): boolean;
    growBounding(bounding: Vector4, x: number, y: number): Vector4;
    isInsideBounding(p: Vector2, bb: Vector4): boolean;
    hex2num(hex: string): [number, number, number];
    num2hex(triplet: [number, number, number]): string;
    extendClass(target: any, origin: any): any;
    getParameterNames(func: Function): string[];
    cloneObject<T>(obj: T, target?: T): T;
    overlapBounding(a: Vector4, b: Vector4): boolean;
    pointerListenerAdd(oDOM: any, sEvType: string, fCall: any, capture?: boolean): void;
    pointerListenerRemove(oDOM: any, sEvType: string, fCall: any, capture?: boolean): void;
    pointerEventType(sType: string): string;
    getTime(): number;
};
```
**说明**: LiteGraph全局对象，包含所有常量、配置和工具函数。

## LGraph 类

### 静态属性
```typescript
static supported_types: string[];      // 支持的数据类型
static STATUS_STOPPED: number;         // 停止状态
static STATUS_RUNNING: number;         // 运行状态
```

### 构造函数
```typescript
constructor(o?: any);
```
**说明**: 创建新的图实例。

### 实例属性
```typescript
list_of_graphcanvas: LGraphCanvas[];    // 关联的画布列表
config: any;                           // 图配置
status: number;                        // 图状态
elapsed_time: number;                  // 已运行时间
last_update_time: number;              // 最后更新时间
iteration: number;                     // 迭代次数
_version: number;                      // 版本号
_last_trigger_time: number;            // 最后触发时间
_nodes: LGraphNode[];                  // 节点数组
_nodes_by_id: Record<number, LGraphNode>; // 按ID索引的节点
_nodes_in_order: LGraphNode[];         // 按执行顺序排列的节点
_groups: LGraphGroup[];                // 组数组
_links: Record<number, LLink>;         // 链接映射
_links_by_id: Record<number, LLink>;   // 按ID索引的链接
inputs: Record<string, any>;           // 图输入
outputs: Record<string, any>;          // 图输出
```

### 主要方法

#### 图管理
```typescript
clear(): void;                         // 清空图
attachCanvas(graphCanvas: LGraphCanvas): void; // 附加画布
detachCanvas(graphCanvas: LGraphCanvas): void; // 分离画布
start(interval?: number): void;        // 开始执行
stop(): void;                         // 停止执行
runStep(num?: number, do_not_catch_errors?: boolean): void; // 执行步骤
```

#### 节点操作
```typescript
add(node: LGraphNode, skipComputeOrder?: boolean): void; // 添加节点
remove(node: LGraphNode): void;        // 移除节点
getNodeById(id: number): LGraphNode | null; // 根据ID获取节点
findNodesByTitle(title: string): LGraphNode[]; // 根据标题查找节点
findNodesByType(type: string): LGraphNode[]; // 根据类型查找节点
findNodesByClass(classObject: any, result?: LGraphNode[]): LGraphNode[]; // 根据类查找节点
findNodeByTitle(title: string): LGraphNode | null; // 根据标题查找单个节点
```

#### 连接管理
```typescript
connect(nodeA_id: number, slotA: number, nodeB_id: number, slotB: number): LLink | null; // 连接节点
disconnect(nodeA_id: number, slotA: number, nodeB_id: number, slotB: number): boolean; // 断开连接
```

#### 序列化
```typescript
serialize(): any;                      // 序列化图
configure(data: any, keep_old?: boolean): void; // 配置图
toJSON(): string;                      // 转换为JSON
```

#### 执行控制
```typescript
updateExecutionOrder(): void;          // 更新执行顺序
fixLinks(): void;                      // 修复链接
checkNodeTypes(): void;                // 检查节点类型
```

## LLink 类

### 构造函数
```typescript
constructor(
    id: number,
    type: string,
    origin_id: number,
    origin_slot: number,
    target_id: number,
    target_slot: number
);
```
**说明**: 创建节点间的连接链接。

### 属性
```typescript
id: number;                           // 链接ID
type: string;                         // 数据类型
origin_id: number;                    // 源节点ID
origin_slot: number;                  // 源插槽索引
target_id: number;                    // 目标节点ID
target_slot: number;                  // 目标插槽索引
data?: any;                          // 传输的数据
```

### 方法
```typescript
serialize(): any;                     // 序列化链接
configure(o: any): void;              // 配置链接
```

## LGraphNode 类

### 静态属性
```typescript
static title: string;                 // 节点标题
static desc: string;                  // 节点描述
static size: Vector2;                 // 默认大小
static color: string;                 // 默认颜色
static bgcolor: string;               // 默认背景色
static boxcolor: string;              // 默认边框色
static shape: number;                 // 默认形状
static slot_start_y: number;          // 插槽起始Y位置
static node_width: number;            // 节点宽度
static title_height: number;          // 标题高度
static title_text_font: string;       // 标题字体
static title_color: string;           // 标题颜色
static TITLE_HEIGHT: number;          // 标题高度常量
static SLOT_HEIGHT: number;           // 插槽高度
static WIDGET_HEIGHT: number;         // 小部件高度
static MAX_NUMBER_OF_SLOTS: number;   // 最大插槽数
static DEFAULT_BGCOLOR: string;       // 默认背景色
static DEFAULT_BOXCOLOR: string;      // 默认边框色
static DEFAULT_SHAPE: number;         // 默认形状
static BOX_SHAPE: number;             // 盒子形状
static ROUND_SHAPE: number;           // 圆角形状
static CIRCLE_SHAPE: number;          // 圆形形状
static CARD_SHAPE: number;            // 卡片形状
static ARROW_SHAPE: number;           // 箭头形状
static GRID_SHAPE: number;            // 网格形状
static title_mode: number;            // 标题模式
static NORMAL_TITLE: number;          // 普通标题
static NO_TITLE: number;              // 无标题
static TRANSPARENT_TITLE: number;     // 透明标题
static AUTOHIDE_TITLE: number;        // 自动隐藏标题
static ignore_remove: boolean;        // 是否忽略移除
```

### 构造函数
```typescript
constructor(title?: string);
```
**说明**: 创建新的图节点。

### 实例属性
```typescript
id: number;                           // 节点ID
title: string;                        // 节点标题
type: string;                         // 节点类型
pos: Vector2;                         // 节点位置
size: Vector2;                        // 节点大小
graph: LGraph | null;                 // 所属图
flags: any;                          // 节点标志
mode: number;                        // 执行模式
inputs: INodeInputSlot[] | null;      // 输入插槽
outputs: INodeOutputSlot[] | null;    // 输出插槽
properties: Record<string, any>;      // 节点属性
properties_info: Record<string, any>; // 属性信息
widgets: IWidget[] | null;            // 小部件数组
widgets_up: boolean;                 // 小部件是否在上方
widgets_start_y: number;             // 小部件起始Y位置
widgets_values: Record<string, any>;  // 小部件值
color: string;                       // 节点颜色
bgcolor: string;                     // 背景颜色
boxcolor: string;                    // 边框颜色
shape: number;                       // 节点形状
order: number;                       // 执行顺序
horizontal: boolean;                 // 是否水平布局
skip_subgraph_button: boolean;       // 是否跳过子图按钮
subgraph: LGraph;                    // 子图
clipper: any;                        // 裁剪器
isSelected: boolean;                 // 是否选中
mouseOver: boolean;                  // 鼠标是否悬停
```

### 配置和序列化
```typescript
configure(info: any): void;           // 配置节点
serialize(): any;                     // 序列化节点
clone(): LGraphNode;                  // 克隆节点
```

### 属性管理
```typescript
setProperty(name: string, value: any): void; // 设置属性
getProperty(name: string): any;       // 获取属性
addProperty(name: string, default_value: any, type?: string, extra_info?: any): void; // 添加属性
removeProperty(name: string): void;   // 移除属性
```

### 数据流
```typescript
getInputData(slot: number, force_update?: boolean): any; // 获取输入数据
setOutputData(slot: number, data: any): void; // 设置输出数据
trigger(action: string, param?: any): void; // 触发动作
triggerSlot(slot: number, param?: any, link_id?: number): void; // 触发插槽
```

### 插槽管理
```typescript
addInput(name: string, type: string, extra_info?: any): INodeInputSlot; // 添加输入
addOutput(name: string, type: string, extra_info?: any): INodeOutputSlot; // 添加输出
removeInput(slot: number): void;      // 移除输入
removeOutput(slot: number): void;     // 移除输出
findInputSlot(name: string): number;  // 查找输入插槽
findOutputSlot(name: string): number; // 查找输出插槽
```

### 连接操作
```typescript
connect(slot: number | string, targetNode: LGraphNode, targetSlot: number | string): any | null; // 连接到其他节点
disconnectOutput(slot: number | string, targetNode?: LGraphNode): boolean; // 断开输出连接
disconnectInput(slot: number | string): boolean; // 断开输入连接
getConnectionPos(is_input: boolean, slot: number | string, out?: Vector2): Vector2; // 获取连接位置
```

### 小部件管理
```typescript
addWidget<T extends IWidget>(
    type: T["type"],
    name: string,
    value: T["value"],
    callback?: WidgetCallback<T> | string,
    options?: T["options"]
): T; // 添加小部件
addCustomWidget<T extends IWidget>(customWidget: T): T; // 添加自定义小部件
```

### 几何和渲染
```typescript
computeSize(minHeight?: Vector2): Vector2; // 计算大小
getBounding(out?: Vector4, compute_outer?: boolean): Vector4; // 获取边界
isPointInside(x: number, y: number, margin?: number, skipTitle?: boolean): boolean; // 检查点是否在内部
getSlotInPosition(x: number, y: number): any; // 获取位置上的插槽
```

### 实用方法
```typescript
alignToGrid(): void;                  // 对齐到网格
trace(msg: string): void;             // 控制台输出
setDirtyCanvas(fg: boolean, bg: boolean): void; // 设置画布为脏
loadImage(url: string): void;         // 加载图片
captureInput(v: any): void;           // 捕获输入
collapse(force: boolean): void;       // 折叠节点
pin(v?: boolean): void;               // 固定节点
localToScreen(x: number, y: number, graphCanvas: LGraphCanvas): Vector2; // 本地坐标转屏幕坐标
```

### 事件回调
```typescript
// 绘制事件
onDrawBackground?(ctx: CanvasRenderingContext2D, canvas: HTMLCanvasElement): void; // 绘制背景
onDrawForeground?(ctx: CanvasRenderingContext2D, canvas: HTMLCanvasElement): void; // 绘制前景

// 鼠标事件
onMouseDown?(event: MouseEvent, pos: Vector2, graphCanvas: LGraphCanvas): void; // 鼠标按下
onMouseMove?(event: MouseEvent, pos: Vector2, graphCanvas: LGraphCanvas): void; // 鼠标移动
onMouseUp?(event: MouseEvent, pos: Vector2, graphCanvas: LGraphCanvas): void; // 鼠标释放
onMouseEnter?(event: MouseEvent, pos: Vector2, graphCanvas: LGraphCanvas): void; // 鼠标进入
onMouseLeave?(event: MouseEvent, pos: Vector2, graphCanvas: LGraphCanvas): void; // 鼠标离开

// 键盘事件
onKey?(event: KeyboardEvent, pos: Vector2, graphCanvas: LGraphCanvas): void; // 键盘事件

// 生命周期事件
onSelected?(): void;                  // 节点被选中
onDeselected?(): void;                // 节点取消选中
onExecute?(): void;                   // 节点执行
onSerialize?(o: any): void;           // 序列化时
onConfigure?(o: any): void;           // 配置时
onAdded?(graph: LGraph): void;        // 添加到图时
onRemoved?(): void;                   // 从图移除时

// 连接事件
onConnectInput?(inputIndex: number, outputType: string, outputSlot: INodeOutputSlot, outputNode: LGraphNode, outputIndex: number): boolean; // 输入连接时
onConnectOutput?(outputIndex: number, inputType: string, inputSlot: INodeInputSlot, inputNode: LGraphNode, inputIndex: number): boolean; // 输出连接时
onBeforeConnectInput?(inputIndex: number): number; // 连接输入前
onConnectionsChange(type: number, slotIndex: number, isConnected: boolean, link: LLink, ioSlot: INodeOutputSlot | INodeInputSlot): void; // 连接变化

// 属性事件
onPropertyChanged?(property: string, value: any, prevValue: any): void | boolean; // 属性变化

// 菜单事件
getMenuOptions?(graphCanvas: LGraphCanvas): ContextMenuItem[]; // 获取菜单选项
getSlotMenuOptions?(slot: INodeSlot): ContextMenuItem[]; // 获取插槽菜单选项
```

## LGraphGroup 类

### 属性
```typescript
title: string;                        // 组标题
color: string;                        // 组颜色
font: string;                         // 字体
```

### 方法
```typescript
configure(o: any): void;              // 配置组
serialize(): any;                     // 序列化组
move(deltaX: number, deltaY: number, ignoreNodes?: boolean): void; // 移动组
recomputeInsideNodes(): void;         // 重新计算内部节点
isPointInside: LGraphNode["isPointInside"]; // 检查点是否在内部
setDirtyCanvas: LGraphNode["setDirtyCanvas"]; // 设置画布为脏
```

## DragAndScale 类

### 构造函数
```typescript
constructor(element?: HTMLElement, skipEvents?: boolean);
```
**说明**: 处理画布的拖拽和缩放功能。

### 属性
```typescript
offset: [number, number];             // 偏移量
scale: number;                        // 缩放比例
max_scale: number;                    // 最大缩放
min_scale: number;                    // 最小缩放
onredraw: Function | null;           // 重绘回调
enabled: boolean;                     // 是否启用
last_mouse: Vector2;                  // 最后鼠标位置
element: HTMLElement | null;         // 关联元素
visible_area: Vector4;                // 可见区域
```

### 方法
```typescript
bindEvents(element: HTMLElement): void; // 绑定事件
computeVisibleArea(): void;           // 计算可见区域
onMouse(e: MouseEvent): void;         // 鼠标事件处理
toCanvasContext(ctx: CanvasRenderingContext2D): void; // 应用到画布上下文
convertOffsetToCanvas(pos: Vector2): Vector2; // 偏移坐标转画布坐标
convertCanvasToOffset(pos: Vector2): Vector2; // 画布坐标转偏移坐标
mouseDrag(x: number, y: number): void; // 鼠标拖拽
changeScale(value: number, zooming_center?: Vector2): void; // 改变缩放
changeDeltaScale(value: number, zooming_center?: Vector2): void; // 改变缩放增量
reset(): void;                        // 重置
```

## LGraphCanvas 类

### 静态属性
```typescript
static node_colors: Record<string, { color: string; bgcolor: string; groupcolor: string; }>; // 节点颜色配置
static link_type_colors: Record<string, string>; // 链接类型颜色
static gradients: object;             // 渐变配置
static search_limit: number;          // 搜索限制
static active_canvas: HTMLCanvasElement; // 当前活动画布
```

### 静态方法
```typescript
static getFileExtension(url: string): string; // 获取文件扩展名
static decodeHTML(str: string): string; // 解码HTML
static onMenuCollapseAll(): void;     // 折叠所有菜单
static onMenuNodeEdit(): void;        // 编辑节点菜单
static onShowPropertyEditor(item: any, options: any, e: any, menu: any, node: any): void; // 显示属性编辑器
static onGroupAdd: ContextMenuEventListener; // 添加组菜单
static onMenuAdd: ContextMenuEventListener; // 添加菜单
// ... 更多静态菜单方法
```

### 构造函数
```typescript
constructor(
    canvas: HTMLCanvasElement | string,
    graph?: LGraph,
    options?: {
        skip_render?: boolean;
        autoresize?: boolean;
    }
);
```
**说明**: 创建图形画布，负责渲染和交互。

### 配置属性
```typescript
allow_dragcanvas: boolean;            // 允许拖拽画布
allow_dragnodes: boolean;             // 允许拖拽节点
allow_interaction: boolean;           // 允许交互
allow_reconnect_links: boolean;       // 允许重连链接
multi_select: boolean;                // 多选模式
allow_searchbox: boolean;             // 允许搜索框
always_render_background: boolean;    // 总是渲染背景
autoresize?: boolean;                 // 自动调整大小
```

### 画布属性
```typescript
canvas: HTMLCanvasElement;            // 主画布
bgcanvas: HTMLCanvasElement;          // 背景画布
ctx: CanvasRenderingContext2D;        // 主画布上下文
bgctx: CanvasRenderingContext2D;      // 背景画布上下文
ds: DragAndScale;                     // 拖拽缩放控制器
graph: LGraph;                        // 关联的图
```

### 渲染属性
```typescript
render_shadows: boolean;              // 渲染阴影
render_canvas_border: boolean;        // 渲染画布边框
render_connections_border: boolean;   // 渲染连接边框
render_connections_shadows: boolean;  // 渲染连接阴影
render_curved_connections: boolean;   // 渲染曲线连接
render_connection_arrows: boolean;    // 渲染连接箭头
render_collapsed_slots: boolean;      // 渲染折叠插槽
render_execution_order: boolean;      // 渲染执行顺序
render_title_colored: boolean;        // 渲染彩色标题
render_only_selected: boolean;        // 仅渲染选中的
use_gradients: boolean;               // 使用渐变
highquality_render: boolean;          // 高质量渲染
```

### 交互状态
```typescript
selected_nodes: Record<number, LGraphNode>; // 选中的节点
selected_group: LGraphGroup | null;   // 选中的组
current_node: LGraphNode | null;      // 当前节点
node_over: LGraphNode | null;         // 鼠标悬停的节点
node_capturing_input: LGraphNode | null; // 捕获输入的节点
node_dragged: LGraphNode | null;      // 被拖拽的节点
connecting_node: LGraphNode | null;   // 正在连接的节点
dragging_canvas: boolean;             // 是否在拖拽画布
dragging_rectangle: Vector4 | null;   // 拖拽矩形
```

### 事件回调
```typescript
onDrawBackground: ((ctx: CanvasRenderingContext2D, visibleArea: Vector4) => void) | null; // 绘制背景回调
onDrawForeground: ((ctx: CanvasRenderingContext2D, visibleArea: Vector4) => void) | null; // 绘制前景回调
onDrawOverlay: ((ctx: CanvasRenderingContext2D) => void) | null; // 绘制覆盖层回调
onMouse: ((event: MouseEvent) => boolean) | null; // 鼠标事件回调
onNodeSelected: ((node: LGraphNode) => void) | null; // 节点选中回调
onNodeDeselected: ((node: LGraphNode) => void) | null; // 节点取消选中回调
onNodeMoved: ((node: LGraphNode) => void) | null; // 节点移动回调
onShowNodePanel: ((node: LGraphNode) => void) | null; // 显示节点面板回调
onNodeDblClicked: ((node: LGraphNode) => void) | null; // 节点双击回调
onSelectionChange: ((nodes: Record<number, LGraphNode>) => void) | null; // 选择变化回调
```

### 主要方法

#### 画布管理
```typescript
clear(): void;                        // 清空画布
setGraph(graph: LGraph, skipClear?: boolean): void; // 设置图
setCanvas(canvas: HTMLCanvasElement, skipEvents?: boolean): void; // 设置画布
bindEvents(): void;                   // 绑定事件
unbindEvents(): void;                 // 解绑事件
```

#### 渲染控制
```typescript
startRendering(): void;               // 开始渲染
stopRendering(): void;                // 停止渲染
setDirty(fg: boolean, bg: boolean): void; // 设置脏标记
draw(forceFG?: boolean, forceBG?: boolean): void; // 绘制
drawFrontCanvas(): void;              // 绘制前景画布
drawBackCanvas(): void;               // 绘制背景画布
```

#### 节点操作
```typescript
selectNode(node: LGraphNode, add?: boolean): void; // 选择节点
selectNodes(nodes?: LGraphNode[], add?: boolean): void; // 选择多个节点
deselectNode(node: LGraphNode): void; // 取消选择节点
deselectAllNodes(): void;             // 取消选择所有节点
deleteSelectedNodes(): void;          // 删除选中节点
centerOnNode(node: LGraphNode): void; // 居中到节点
bringToFront(node: LGraphNode): void; // 置于前景
sendToBack(node: LGraphNode): void;   // 置于背景
```

#### 视图控制
```typescript
setZoom(value: number, center: Vector2): void; // 设置缩放
computeVisibleNodes(nodes: LGraphNode[]): LGraphNode[]; // 计算可见节点
convertEventToCanvasOffset(e: MouseEvent): Vector2; // 事件坐标转画布坐标
convertOffsetToCanvas: DragAndScale["convertOffsetToCanvas"]; // 偏移转画布坐标
convertCanvasToOffset: DragAndScale["convertCanvasToOffset"]; // 画布转偏移坐标
```

#### 交互处理
```typescript
processMouseDown(e: MouseEvent): boolean | undefined; // 处理鼠标按下
processMouseMove(e: MouseEvent): boolean | undefined; // 处理鼠标移动
processMouseUp(e: MouseEvent): boolean | undefined; // 处理鼠标释放
processMouseWheel(e: MouseEvent): boolean | undefined; // 处理鼠标滚轮
processKey(e: KeyboardEvent): boolean | undefined; // 处理键盘事件
processDrop(e: DragEvent): void;      // 处理拖放
```

#### 绘制方法
```typescript
drawNode(node: LGraphNode, ctx: CanvasRenderingContext2D): void; // 绘制节点
drawNodeShape(node: LGraphNode, ctx: CanvasRenderingContext2D, size: [number, number], fgColor: string, bgColor: string, selected: boolean, mouseOver: boolean): void; // 绘制节点形状
drawConnections(ctx: CanvasRenderingContext2D): void; // 绘制连接
renderLink(a: Vector2, b: Vector2, link: object, skipBorder: boolean, flow: boolean, color?: string, startDir?: number, endDir?: number, numSublines?: number): void; // 渲染链接
drawNodeWidgets(node: LGraphNode, posY: number, ctx: CanvasRenderingContext2D, activeWidget: object): void; // 绘制节点小部件
drawGroups(canvas: any, ctx: CanvasRenderingContext2D): void; // 绘制组
```

#### 实用方法
```typescript
resize(width?: number, height?: number): void; // 调整大小
switchLiveMode(transition?: boolean): void; // 切换实时模式
showSearchBox(event?: MouseEvent): void; // 显示搜索框
showEditPropertyValue(node: LGraphNode, property: any, options: any): void; // 显示编辑属性值
createDialog(html: string, options?: { position?: Vector2; event?: MouseEvent }): void; // 创建对话框
getCanvasMenuOptions(): ContextMenuItem[]; // 获取画布菜单选项
getNodeMenuOptions(node: LGraphNode): ContextMenuItem[]; // 获取节点菜单选项
processContextMenu(node: LGraphNode, event: Event): void; // 处理上下文菜单
```

## ContextMenu 类

### 静态方法
```typescript
static trigger(element: HTMLElement, event_name: string, params: any, origin: any): void; // 触发事件
static isCursorOverElement(event: MouseEvent, element: HTMLElement): void; // 检查光标是否在元素上
static closeAllContextMenus(window: Window): void; // 关闭所有上下文菜单
```

### 构造函数
```typescript
constructor(values: ContextMenuItem[], options?: IContextMenuOptions, window?: Window);
```
**说明**: 创建上下文菜单。

### 属性
```typescript
options: IContextMenuOptions;         // 菜单选项
parentMenu?: ContextMenu;             // 父菜单
lock: boolean;                        // 是否锁定
current_submenu?: ContextMenu;        // 当前子菜单
```

### 方法
```typescript
addItem(name: string, value: ContextMenuItem, options?: IContextMenuOptions): void; // 添加菜单项
close(e?: MouseEvent, ignore_parent_menu?: boolean): void; // 关闭菜单
getTopMenu(): void;                   // 获取顶级菜单
getFirstEvent(): void;                // 获取首个事件
```

## 工具函数

### clamp
```typescript
function clamp(v: number, min: number, max: number): number;
```
**说明**: 将数值限制在指定范围内。

## 使用示例

### 创建基本图形
```typescript
// 创建图
const graph = new LGraph();

// 创建画布
const canvas = document.getElementById('mycanvas') as HTMLCanvasElement;
const graphCanvas = new LGraphCanvas(canvas, graph);

// 创建节点
const nodeA = LiteGraph.createNode('basic/const');
const nodeB = LiteGraph.createNode('basic/watch');

// 添加到图中
graph.add(nodeA);
graph.add(nodeB);

// 连接节点
nodeA.connect(0, nodeB, 0);

// 开始执行
graph.start();
```

### 自定义节点
```typescript
class MyCustomNode extends LGraphNode {
    static title = "我的自定义节点";
    static desc = "这是一个自定义节点示例";
    
    constructor() {
        super();
        this.addInput("输入", "number");
        this.addOutput("输出", "number");
        this.addProperty("乘数", 1.0);
    }
    
    onExecute() {
        const input = this.getInputData(0);
        if (input != null) {
            const multiplier = this.getProperty("乘数");
            this.setOutputData(0, input * multiplier);
        }
    }
}

// 注册节点类型
LiteGraph.registerNodeType("custom/multiply", MyCustomNode);
```

### 添加小部件
```typescript
class NodeWithWidgets extends LGraphNode {
    constructor() {
        super();
        
        // 添加数字滑块
        this.addWidget("slider", "值", 0.5, (value) => {
            console.log("滑块值变化:", value);
        }, { min: 0, max: 1, step: 0.01 });
        
        // 添加下拉选择
        this.addWidget("combo", "选项", "选项1", null, {
            values: ["选项1", "选项2", "选项3"]
        });
        
        // 添加按钮
        this.addWidget("button", "点击我", undefined, () => {
            console.log("按钮被点击!");
        });
    }
}
```

## 总结

LiteGraph.js 是一个功能强大的节点图编辑器库，提供了完整的图形编辑、节点管理、数据流处理和用户交互功能。通过其丰富的API，开发者可以创建复杂的可视化编程环境、数据处理流水线或交互式图形应用。

主要特性包括：
- 完整的节点图编辑功能
- 灵活的数据类型系统
- 丰富的UI小部件
- 可扩展的节点类型
- 强大的事件系统
- 高性能的渲染引擎
- 完善的序列化支持

本文档涵盖了所有主要的类、接口和方法，为开发者提供了全面的参考资料。