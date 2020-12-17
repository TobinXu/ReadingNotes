# Ts学习笔记

## jsx



```typescript
let num : number;
//类型断言
var foo = <foo>bar;
// 在tsx中不支持上述语法
// 可以写成
var foo = bar as foo;

// 如果接口JSX.IntrinsicElements存在，固有元素的名字需要在其中查找
declare namespace JSX {
  interface IntrinsicElements {
    foo: any;
    [elemName: string]: any;
  }
}
  <foo /> // 正确
  <bar /> // 错误
   

```



## 无状态函数组件

```typescript
// 它的第一个参数是props对象，ts强制它的返回值赋值给JSX.Element
interface FooProp {
  name: string;
  X: number;
  Y: number;
}
declare function AnotherComponent(prop: {name: string});
function ComponentFoo(prop: FooProp) {
  return <AnotherComponent name={prop.name} />;
}

const Button = (prop: {value: string}, context: {color: string}) => <button>
      
```

## 无状态函数组件的函数重载

```typescript
interface ClickableProps {
  children: JSX.Element[] | JSX.Element
}

interface HomeProps extends ClickableProps {
  home: JSX.Element;
}

interface SideProps extends ClickableProps {
  side: JSX.Element | string;
}

function MainButton(prop: HomeProps): JSX.Element;
function MainButton(prop: SidePros): JSX.Element {
  ....
};
```

## 类组件

```typescript
class MyComponent {
  render() {}
}

// 使用构造签名
var myComponent = new MyComponent();

// 元素类的类型 => MyComponent
// 元素实例的类型 => {render: () => void}

function MyFactoryFunction() {
  return {
    render: () => {
      
    }
  }
}

// 使用调用签名
var myComponent = MyFactoryFunction();

// 元素类的类型 => FactoryFunction
// 元素实例的类型 => {render: () => void}

declare namespace JSX {
  interface ElementClass {
    render: any;
  }
}
  
class MyComponent {
  render() {}
}
function MyFactoryFunction() {
  return {render: () => {}}
}
  
<MyComponent />; // 正确
<MyFactoryFunction />; // 正确

class NotAValidComponent {}
function NotAVaildFactoryFunction() {
  return {};
}
<NotAValidComponent /> // 错误
<NotAValidFactoryFunction /> // 错误
# 类组件直接render(), 函数组件需要return对象，在该对象里面render();
```

## 属性类型检查

```typescript
declare namespace JSX {
  interface IntrinsicElements {
    foo: {bar?: boolean}
  }
}
  // `foo`的元素属性类型为`{bar?: boolean}`
<foo bar />
  
对于值得元素
declare namespace JSX {
  interface ElementAttributesProperty {
    props; // 指定用来使用的属性名
  }
}

class MyComponent {
  // 在元素实例类型上指定属性
  props: {
    foo?: string;
  }
}

// `MyComponent`的元素属性类型为`{foo?: string}`
<MyComponent foo="bar" />
  
```

