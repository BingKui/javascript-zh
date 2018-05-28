# Airbnb CSS-in-JavaScript 代码规范

*一种 CSS-in-JavaScript 最合理的规范。*

## <a id="table-of-contents">目录</a>

1. [命名](#naming)
1. [订阅](#ordering)
1. [嵌套](#nesting)
1. [内联](#inline)
1. [主题](#themes)

## <a id="naming">命名</a>

  - 使用驼峰命名法给对象的 key 命名。 (i.e. "selectors")

    > 为什么？ 我们在组件的 `styles` 对象上访问这些键作为属性，使用驼峰命名法最为方便。

    ```js
    // bad
    {
      'bermuda-triangle': {
        display: 'none',
      },
    }

    // good
    {
      bermudaTriangle: {
        display: 'none',
      },
    }
    ```

  - 使用下划线标识修改为其他样式。

    > 为什么？ 与 BEM 类似，这种约定清楚明确的表明，修改样式的元素是下划线之前的元素。下划线不需要引导，因此它比其他字符（如破折号）更受欢迎。

    ```js
    // bad
    {
      bruceBanner: {
        color: 'pink',
        transition: 'color 10s',
      },

      bruceBannerTheHulk: {
        color: 'green',
      },
    }

    // good
    {
      bruceBanner: {
        color: 'pink',
        transition: 'color 10s',
      },

      bruceBanner_theHulk: {
        color: 'green',
      },
    }
    ```

  - 使用 `selectorName_fallback` 设置回退样式集。

    > 为什么？ 与修饰符类似，保持命名一致有助于揭示这些样式与在浏览器中覆盖它们的样式之间的关系。

    ```js
    // bad
    {
      muscles: {
        display: 'flex',
      },

      muscles_sadBears: {
        width: '100%',
      },
    }

    // good
    {
      muscles: {
        display: 'flex',
      },

      muscles_fallback: {
        width: '100%',
      },
    }
    ```

  - 使用单独的选择器设置备用样式集。

    > 为什么？ 保留一个单独的对象中包含的回退样式可以明确它们的目的，从而提高可读性。

    ```js
    // bad
    {
      muscles: {
        display: 'flex',
      },

      left: {
        flexGrow: 1,
        display: 'inline-block',
      },

      right: {
        display: 'inline-block',
      },
    }

    // good
    {
      muscles: {
        display: 'flex',
      },

      left: {
        flexGrow: 1,
      },

      left_fallback: {
        display: 'inline-block',
      },

      right_fallback: {
        display: 'inline-block',
      },
    }
    ```

  - 使用与设备无关的名字 (如： "small", "medium", and "large") 来命名媒体查询的断点。

    > 为什么？ 常用的名字如： ”phone“, “tablet”，和 “desktop” 与现实世界的设备的特点不匹配。使用这些名字会带来错误的期望。

    ```js
    // bad
    const breakpoints = {
      mobile: '@media (max-width: 639px)',
      tablet: '@media (max-width: 1047px)',
      desktop: '@media (min-width: 1048px)',
    };

    // good
    const breakpoints = {
      small: '@media (max-width: 639px)',
      medium: '@media (max-width: 1047px)',
      large: '@media (min-width: 1048px)',
    };
    ```

## <a id="ordering">订阅</a>

  - 在组件之后定义样式。

    > 为什么？ 使用一个高阶组件来定义我们的主题，这是在组件定义之后自然使用的。 将样式对象直接传递给这个函数可以减少间接性。

    ```jsx
    // bad
    const styles = {
      container: {
        display: 'inline-block',
      },
    };

    function MyComponent({ styles }) {
      return (
        <div {...css(styles.container)}>
          Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.
        </div>
      );
    }

    export default withStyles(() => styles)(MyComponent);

    // good
    function MyComponent({ styles }) {
      return (
        <div {...css(styles.container)}>
          Never doubt that a small group of thoughtful, committed citizens can
          change the world. Indeed, it’s the only thing that ever has.
        </div>
      );
    }

    export default withStyles(() => ({
      container: {
        display: 'inline-block',
      },
    }))(MyComponent);
    ```

## <a id="nesting">嵌套</a>

  - 在相同缩进级别的相邻块之间留下空白。

    > 为什么？ 空间可以提高可读性，减少合并冲突的可能性。

    ```js
    // bad
    {
      bigBang: {
        display: 'inline-block',
        '::before': {
          content: "''",
        },
      },
      universe: {
        border: 'none',
      },
    }

    // good
    {
      bigBang: {
        display: 'inline-block',

        '::before': {
          content: "''",
        },
      },

      universe: {
        border: 'none',
      },
    }
    ```

## <a id="inline">内联</a>

  - 对于具有高基数的样式使用内联样式（例如：使用 prop 的值），而不是具有低基数的样式。
    > 为什么？ 主题样式是非常昂贵的，因此对于不同类型的样式内联样式是最好的选择。

    ```jsx
    // bad
    export default function MyComponent({ spacing }) {
      return (
        <div style={{ display: 'table', margin: spacing }} />
      );
    }

    // good
    function MyComponent({ styles, spacing }) {
      return (
        <div {...css(styles.periodic, { margin: spacing })} />
      );
    }
    export default withStyles(() => ({
      periodic: {
        display: 'table',
      },
    }))(MyComponent);
    ```

## <a id="themes">主题</a>

  - 可以使用一个如 [react-with-styles](https://github.com/airbnb/react-with-styles) 的抽象层启用主题。*react-with-styles 给了我们像 `withStyles()`， `ThemedStyleSheet`， 和 `css()` 的一些示例在它的文档中。*

  > 为什么？ 有一组共享的变量来设计组件式有用的。 使用抽象层使其更方便。 此外，这有助于防止组件与任何特定的底层实现逻辑实现紧密耦合，从而使你获得更多的自由。

  - 只在主题中定义颜色。

    ```js
    // bad
    export default withStyles(() => ({
      chuckNorris: {
        color: '#bada55',
      },
    }))(MyComponent);

    // good
    export default withStyles(({ color }) => ({
      chuckNorris: {
        color: color.badass,
      },
    }))(MyComponent);
    ```

  - 只在主题中定义字体。

    ```js
    // bad
    export default withStyles(() => ({
      towerOfPisa: {
        fontStyle: 'italic',
      },
    }))(MyComponent);

    // good
    export default withStyles(({ font }) => ({
      towerOfPisa: {
        fontStyle: font.italic,
      },
    }))(MyComponent);
    ```

  - 将字体定义为相关样式集。

    ```js
    // bad
    export default withStyles(() => ({
      towerOfPisa: {
        fontFamily: 'Italiana, "Times New Roman", serif',
        fontSize: '2em',
        fontStyle: 'italic',
        lineHeight: 1.5,
      },
    }))(MyComponent);

    // good
    export default withStyles(({ font }) => ({
      towerOfPisa: {
        ...font.italian,
      },
    }))(MyComponent);
    ```

  - 在主题中定义基本网格单元（无论是作为一个值还是一个使用乘数的函数）。

    ```js
    // bad
    export default withStyles(() => ({
      rip: {
        bottom: '-6912px', // 6 feet
      },
    }))(MyComponent);

    // good
    export default withStyles(({ units }) => ({
      rip: {
        bottom: units(864), // 6 feet, assuming our unit is 8px
      },
    }))(MyComponent);

    // good
    export default withStyles(({ unit }) => ({
      rip: {
        bottom: 864 * unit, // 6 feet, assuming our unit is 8px
      },
    }))(MyComponent);
    ```

  - 只在主题中定义媒体查询。

    ```js
    // bad
    export default withStyles(() => ({
      container: {
        width: '100%',

        '@media (max-width: 1047px)': {
          width: '50%',
        },
      },
    }))(MyComponent);

    // good
    export default withStyles(({ breakpoint }) => ({
      container: {
        width: '100%',

        [breakpoint.medium]: {
          width: '50%',
        },
      },
    }))(MyComponent);
    ```

  - 在主题中定义棘手的回退属性。

    > 为什么？ 许多 CSS-in-JavaScript 的实现中将样式对象合并在一起，这使得指定样式的属性（如： `display` ）回退有点棘手。 为了保持统一的方法，把这些回退属性放在主题中。

    ```js
    // bad
    export default withStyles(() => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        'display ': 'table',
      },
    }))(MyComponent);

    // good
    export default withStyles(({ fallbacks }) => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        [fallbacks.display]: 'table',
      },
    }))(MyComponent);

    // good
    export default withStyles(({ fallback }) => ({
      .muscles {
        display: 'flex',
      },

      .muscles_fallback {
        [fallback('display')]: 'table',
      },
    }))(MyComponent);
    ```

  - 尽可能少的自定义主题。 许多应用只有一套主题。

  - 自定义主题的命名空间是设置在一个具有独特和描述性的 key 之下的一个嵌套对象。

    ```js
    // bad
    ThemedStyleSheet.registerTheme('mySection', {
      mySectionPrimaryColor: 'green',
    });

    // good
    ThemedStyleSheet.registerTheme('mySection', {
      mySection: {
        primaryColor: 'green',
      },
    });
    ```

---

CSS 规则改编自 [Saijo George](https://saijogeorge.com/css-puns/).
