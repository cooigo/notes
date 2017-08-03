# Redux 参数传入组件使用方法

[issues](https://github.com/DefinitelyTyped/DefinitelyTyped/issues/6237)

I think the generic type parameter you've given is not correct. Depending on how you want to pass progress prop to the Component, connect type signature will differ.

## Case 1. You want to directly pass progress value to the component, not relying on react-redux

- 1.1: provide type parameter explicitly on connect

```javascript
type MyViewPropType = {
  progress: number
}
class MyView extends React.Component<MyViewPropType, any> { }
function mapStateToProps(state: any): {} {
  return {};
}
const ConnectedMyView = connect<{}, {}, MyViewPropType>(mapStateToProps)(MyView);
let myViewElement = <ConnectedMyView progress={42} />;
```

- 1.2: don't provide type parameter on connect

    In this case, you have to provide type parameter on mapStateToProps instead, otherwise connect doesn't know how to construct MyView's MyViewPropType from state props, dispatch props, and own props

```javascript
type MyViewPropType = {
  progress: number
}
class MyView extends React.Component<MyViewPropType, any> { }
function mapStateToProps(state: any, ownProp?: MyViewPropType): {} {
  return {};
}
const ConnectedMyView = connect(mapStateToProps)(MyView);
let myViewElement = <ConnectedMyView progress={42} />;
```

## Case 2. You want react-redux to provide progress value via mapStateToProps to the component

```javascript
type MyViewPropType = {
  progress: number
}
class MyView extends React.Component<MyViewPropType, any> { }
function mapStateToProps(state: any): MyViewPropType {
  return { progress: 42 };
}
// here connect's type parameters aren't necessarily required
const ConnectedMyView = connect<MyViewPropType, {}, {}>(mapStateToProps)(MyView);
let myViewElement = <ConnectedMyView />;
```

## Case 3. You want react-redux to provide progress value via mapDispatchToProps to the component

```javascript
type MyViewPropType = {
  progress: number
}
class MyView extends React.Component<MyViewPropType, any> { }
function mapStateToProps(state: any): {} {
  return {};
}
function mapDispatchToProps(dispatch: Redux.Dispatch): MyViewPropType {
  return { progress: 42 };
}
// here connect's type parameters aren't necessarily required
const ConnectedMyView = connect<{}, MyViewPropType, {}>(mapStateToProps, mapDispatchToProps)(MyView);
let myViewElement = <ConnectedMyView />;
```