# Airbnb React/JSX 代码规范

*一套对于 React 和 JSX 基本合理的规范*

本指南是主要基于目前流行的 JavaScript 标准，因此一些惯例（如：async/await 或静态类字段）可能仍然包含其中或者被禁止。 目前，处在第 3 阶段的任何内容都不包含在本指南中，也不建议使用。

## <a id="table-of-contents">目录</a>

  1. [基本规则](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Mixins](#mixins)
  1. [命名](#naming)
  1. [声明模块](#declaration)
  1. [代码对齐](#alignment)
  1. [单引号和双引号](#quotes)
  1. [空格](#spacing)
  1. [属性](#props)
  1. [Refs](#refs)
  1. [括号](#parentheses)
  1. [标签](#tags)
  1. [方法](#methods)
  1. [函数生命周期](#ordering)
  1. [`isMounted`](#ismounted)

## <a id="basic-rules">基本规则</a>

  - 每个文件只存在一个 React 组件。
    - 但是，每个文件允许存在多个 [无状态或纯组件](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) 。 eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - 始终使用 JSX 语法。
  - 除非你从一个非 JSX 文件中初始化你的应用，否则不要使用 `React.createElement` 。

  **[⬆ 返回目录](#table-of-contents)**

## <a id="class-vs-reactcreateclass-vs-stateless">Class vs `React.createClass` vs stateless</a>

  - 如果你的组件有内部状态或者是 `refs` ，推荐使用 `class extends React.Component` 而不是 `React.createClass`。 eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```jsx
    // bad
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // good
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    如果你的组件没有状态或者 `refs` 推荐使用普通函数（不是箭头函数）而不是类定义：

    ```jsx
    // bad
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // bad (relying on function name inference is discouraged)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // good
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```
  **[⬆ 返回目录](#table-of-contents)**

## <a id="mixins">Mixins</a>

  - [不要使用 mixins](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).

  > 为什么？ Mixins 会引入隐式的依赖关系，会导致名称冲突，并致使复杂性指数级增加。 在多数情况下 mixins 能够被更好的方法替代，如：组件化，高阶组件，工具模块等。

  **[⬆ 返回目录](#table-of-contents)**

## <a id="naming">命名</a>

  - **扩展名**: 使用 `.jsx` 作为 React 组件的扩展名。
  - **文件名**: 使用帕斯卡命名法给文件命名。 例如： `ReservationCard.jsx`.
  - **引用命名**: 使用帕斯卡命名法给 React 组件命名并用驼峰命名法给组件实例命名。 eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```jsx
    // bad
    import reservationCard from './ReservationCard';

    // good
    import ReservationCard from './ReservationCard';

    // bad
    const ReservationItem = <ReservationCard />;

    // good
    const reservationItem = <ReservationCard />;
    ```

  - **组件命名**: 使用文件名作为组件的名字。 例如： `ReservationCard.jsx` 应该包含一个名字叫 `ReservationCard` 的组件。 但是，如果整个文件夹是一个模块，使用 `index.jsx` 作为入口文件，并使用目录名称作为组件名称：

    ```jsx
    // bad
    import Footer from './Footer/Footer';

    // bad
    import Footer from './Footer/index';

    // good
    import Footer from './Footer';
    ```
  - **高阶组件命名**: 在生成的新组件上，使用高阶组件的名称和传入组件的名称作为新组件的 `displayName` 。 例如：高价组件 `withFoo()`，当被传入到一个叫做 `Bar` 的组件中的时候，应该生成一个拥有 `displayName` 为 `withFoo(Bar)` 的组件。

    > 为什么？ 一个组件的 `displayName` 可能在开发者工具或者错误信息中用到， 因此拥有一个能清楚表达层次结构的值能够帮助我们更好的理解触发的事件。

    ```jsx
    // bad
    export default function withFoo(WrappedComponent) {
      return function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }
    }

    // good
    export default function withFoo(WrappedComponent) {
      function WithFoo(props) {
        return <WrappedComponent {...props} foo />;
      }

      const wrappedComponentName = WrappedComponent.displayName
        || WrappedComponent.name
        || 'Component';

      WithFoo.displayName = `withFoo(${wrappedComponentName})`;
      return WithFoo;
    }
    ```

  - **属性命名**: 避免使用 DOM 组件属性来用作其他用途。

    > 为什么？ 对于像 `style` 和 `className` 这样的属性我们默认它们代表一些特殊的含义。 改变这些 API 会降低你代码的可读性和可维护性，并且可能导致问题。

    ```jsx
    // bad
    <MyComponent style="fancy" />

    // bad
    <MyComponent className="fancy" />

    // good
    <MyComponent variant="fancy" />
    ```

    **[⬆ 返回目录](#table-of-contents)**

## <a id="declaration">声明模块</a>

  - 不要使用 `displayName` 来命名组件。 应该使用引用来命名组件。

    ```jsx
    // bad
    export default React.createClass({
      displayName: 'ReservationCard',
      // stuff goes here
    });

    // good
    export default class ReservationCard extends React.Component {
    }
    ```

    **[⬆ 返回目录](#table-of-contents)**

## <a id="alignment">代码对齐</a>

  - 遵循以下的 JSX 代码对齐方式。 eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md) [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

    ```jsx
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // 如果能写在一行，直接写成一行
    <Foo bar="bar" />

    // 子元素按照常规方式缩进
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>
    ```

    **[⬆ 返回目录](#table-of-contents)**

## <a id="quotes">单引号和双引号</a>

  - 对于 JSX 属性总是使用双引号 （`"`）， 对于其他的 JS 来说使用单引号 （`'`）。 eslint: [`jsx-quotes`](https://eslint.org/docs/rules/jsx-quotes)

    > 为什么？ HTML 属性的规则就是使用双引号而不是单引号，因此 JSX 的属性也遵循这个约定。

    ```jsx
    // bad
    <Foo bar='bar' />

    // good
    <Foo bar="bar" />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

    **[⬆ 返回目录](#table-of-contents)**

## <a id="spacing">空格</a>

  - 总是在自闭和标签的标签前加一个空格。 eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

    ```jsx
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

  - 不要在 JSX 中的括号两边添加空格。 eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```jsx
    // bad
    <Foo bar={ baz } />

    // good
    <Foo bar={baz} />
    ```

    **[⬆ 返回目录](#table-of-contents)**

## <a id="props">属性</a>

  - 总是使用驼峰命名法命名属性名。

    ```jsx
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - 当一个属性的值为显式的 `true` 时，应该省略。 eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```jsx
    // bad
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />

    // good
    <Foo hidden />
    ```

  - `<img>` 标签应该始终添加  `alt` 属性。 如果图片是直观的， `alt` 可以为空或者 `<img>` 必须有 `role="presentation"` 属性。 eslint: [`jsx-a11y/alt-text`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)

    ```jsx
    // bad
    <img src="hello.jpg" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />

    // good
    <img src="hello.jpg" alt="" />

    // good
    <img src="hello.jpg" role="presentation" />
    ```

  - 不要在 `<img>` 的 `alt` 属性中使用 "image"， "photo"， 或 "picture" 这些词汇。 eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

    > 为什么？ 屏幕阅读器已经把 `img` 元素作为图片了，因此 alt 中不需要再进行说明。

    ```jsx
    // bad
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - 只是用有效的，非抽象的 [ARIA roles](https://www.w3.org/TR/wai-aria/roles#role_definitions)。 eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```jsx
    // bad - 不是一个 ARIA role
    <div role="datepicker" />

    // bad - 抽象的 ARIA role
    <div role="range" />

    // good
    <div role="button" />
    ```

  - 不要在元素上使用 `accessKey` 。 eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  > 为什么？ 设置的键盘快捷键和使用显示器和键盘的人的键盘命令不一致会导致访问的复杂性。

  ```jsx
  // bad
  <div accessKey="h" />

  // good
  <div />
  ```

  - 避免使用数组的索引作为 `key` 属性的值，应该是唯一的 ID。 ([why?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

  ```jsx
  // bad
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

  // good
  {todos.map(todo => (
    <Todo
      {...todo}
      key={todo.id}
    />
  ))}
  ```

  - 总是为所有非必要的属性定义明确的 defaultProps 。

  > 为什么？ propTypes 可以作为组件的文档说明，并且声明 defaultProps 意味着，阅读代码的人不需要假设一下默认的值。更重要的是，显式的声明默认属性可以让你的组件跳过属性的类型检查。

  ```jsx
  // bad
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };

  // good
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  }
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };
  SFC.defaultProps = {
    bar: '',
    children: null,
  };
  ```

  - 尽可能少使用扩展运算符。
  > 为什么？ 这样你有更大的可能将不必要的属性传递个组件。对于 React v15.6.1 和更老的版本，你可以查看 [将无效的属性传递给 DOM ](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

  例外：

  - 代理高阶组件和 propTypes 变量提升。

  ```jsx
  function HOC(WrappedComponent) {
    return class Proxy extends React.Component {
      Proxy.propTypes = {
        text: PropTypes.string,
        isLoading: PropTypes.bool
      };

      render() {
        return <WrappedComponent {...this.props} />
      }
    }
  }
  ```

  - 对于已知的，明确的对象使用扩展运算符。这非常有用尤其是在使用 Mocha 测试组件的时候。

  ```jsx
  export default function Foo {
    const props = {
      text: '',
      isPublished: false
    }

    return (<div {...props} />);
  }
  ```

  使用注意事项：尽可能的过滤掉不必要的属性。 此外，可以使用 [prop-types-exact](https://www.npmjs.com/package/prop-types-exact) 来帮助避免错误。

  ```jsx
  // good
  render() {
    const { irrelevantProp, ...relevantProps  } = this.props;
    return <WrappedComponent {...relevantProps} />
  }

  // bad
  render() {
    const { irrelevantProp, ...relevantProps  } = this.props;
    return <WrappedComponent {...this.props} />
  }
  ```

  **[⬆ 返回目录](#table-of-contents)**

## <a id="refs">Refs</a>

  - 总是在 ref 中使用回调函数。 eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

    ```jsx
    // bad
    <Foo
      ref="myRef"
    />

    // good
    <Foo
      ref={(ref) => { this.myRef = ref; }}
    />
    ```

    **[⬆ 返回目录](#table-of-contents)**

## <a id="parentheses">括号</a>

  - 当存在多行时，使用括号包裹 JSX 标签。 eslint: [`react/jsx-wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

    ```jsx
    // bad
    render() {
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // good, 当存在一行时
    render() {
      const body = <div>hello</div>;
      return <MyComponent>{body}</MyComponent>;
    }
    ```

    **[⬆ 返回目录](#table-of-contents)**

## <a id="tags">标签</a>

  - 没有子节点的标签总是自闭和。 eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```jsx
    // bad
    <Foo variant="stuff"></Foo>

    // good
    <Foo variant="stuff" />
    ```

  - 如果你的组件具有多行属性，请在新行上关闭标签。 eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```jsx
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

    **[⬆ 返回目录](#table-of-contents)**

## <a id="methods">方法</a>

  - 使用箭头函数关闭局部变量。

    ```jsx
    function ItemList(props) {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={() => doSomethingWith(item.name, index)}
            />
          ))}
        </ul>
      );
    }
    ```

  - 在构造器中为 render 函数绑定处理方法。 eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

    > 为什么？ 渲染路径中的绑定函数会在每个渲染器上创建一个全新的函数。

    ```jsx
    // bad
    class extends React.Component {
      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />;
      }
    }

    // good
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // do stuff
      }

      render() {
        return <div onClick={this.onClickDiv} />;
      }
    }
    ```

  - 不要在 React 组件的内部方法中使用下划线前缀。
    > 为什么？ 下划线前缀有时被用作其他语言的约定来表示隐私。 但是，与其他语言不同， JavaScript 没有隐私的原生支持，所有东西都是公开的。 无论你是什么意图，在属性前添加下划线前缀并不会使它们成为私有属性，并且任何属性（有或者没有前缀）都应该被视为公共的。 可以查看问题 [#1024](https://github.com/airbnb/javascript/issues/1024)，和 [#490](https://github.com/airbnb/javascript/issues/490) 进行更深入的了解、讨论。

    ```jsx
    // bad
    React.createClass({
      _onClickSubmit() {
        // do stuff
      },

      // other stuff
    });

    // good
    class extends React.Component {
      onClickSubmit() {
        // do stuff
      }

      // other stuff
    }
    ```

  - 确保你的 `render` 方法存在返回值。 eslint: [`react/require-render-return`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)

    ```jsx
    // bad
    render() {
      (<div />);
    }

    // good
    render() {
      return (<div />);
    }
    ```

    **[⬆ 返回目录](#table-of-contents)**

## <a id="ordering">函数生命周期</a>

  - `class extends React.Component` 的函数生命周期:

  1. 可选的 `static` 方法
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *单击处理时间或者事件处理器* 像 `onClickSubmit()` 或者 `onChangeDescription()`
  1. *对于 `render` 的 get 方法* 像 `getSelectReason()` 或者 `getFooterContent()`
  1. *可选的 render 方法* 像 `renderNavigation()` 或者 `renderProfilePicture()`
  1. `render`

  - 如何定义 `propTypes`, `defaultProps`, `contextTypes`, 等...

    ```jsx
    import React from 'react';
    import PropTypes from 'prop-types';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>;
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - `React.createClass` 的函数生命周期: eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *单击处理时间或者事件处理器* 像 `onClickSubmit()` 或者 `onChangeDescription()`
  1. *对于 `render` 的 get 方法* 像 `getSelectReason()` 或者 `getFooterContent()`
  1. *可选的 render 方法* 像 `renderNavigation()` 或者 `renderProfilePicture()`
  1. `render`

  **[⬆ 返回目录](#table-of-contents)**

## `isMounted`

  - 不要使用 `isMounted`。 eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > 为什么？ [`isMounted` 是一种反模式][anti-pattern]，在使用 ES6 类定义的时候不可用，并且正在被正式弃用。

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

    **[⬆ 返回目录](#table-of-contents)**
