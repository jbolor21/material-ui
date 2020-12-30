# Components 组件

<p class="description">The theme's `components` key allows you to customize a component without wrapping it in another component. You can change the styles, the default props, and more.</p>

## Global style overrides

You can use the theme's `styleOverrides` key to potentially change every single style injected by Material-UI into the DOM.

```js
const theme = createMuiTheme({
  components: {
    // Name of the component
    MuiButton: {
      styleOverrides: {
        // Name of the slot
        root: {
          // Some CSS
          fontSize: '1rem',
        },
      },
    },
  },
});
```

{{"demo": "pages/customization/theme-components/GlobalThemeOverride.js"}}

The list of each component's classes is documented under the **CSS** section of its API page.

To override a lab component's styles with TypeScript, check [this section of the documentation](/components/about-the-lab/#typescript).

## 默认属性

You can change the default of every prop of a Material-UI component. A `defaultProps` key is exposed in the theme's `components` key for this use case.

```js
const theme = createMuiTheme({
  components: {
    // Name of the component
    MuiButtonBase: {
      defaultProps: {
        // The props to change the default for.
        disableRipple: true, // No more ripple!
      },
    },
  },
});
```

{{"demo": "pages/customization/theme-components/DefaultProps.js"}}

To override lab component styles with TypeScript, check [this page](/components/about-the-lab/#typescript).

## 添加新的组件变量

You can use the `variants` key in the theme's `components` section to add new variants to Material-UI components. These new variants can specify what styles the component should have when specific props are applied.

这些定义在组件的名称下由一个数组指定。 For each of them a CSS class is added to the HTML `<head>`. The order is important, so make sure that the styles that should win are specified last.

```js
const theme = createMuiTheme({
  components: {
    MuiButton: {
      variants: [
        {
          props: { variant: 'dashed' },
          style: {
            textTransform: 'none',
            border: `2px dashed grey${blue[500]}`,
          },
        },
        {
          props: { variant: 'dashed', color: 'secondary' },
          style: {
            border: `4px dashed ${red[500]}`,
          },
        },
      ],
    },
  },
});
```

If you're using TypeScript, you'll need to specify your new variants/colors, using [module augmentation](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#module-augmentation).

```tsx
declare module '@material-ui/core/Button/Button' {
  interface ButtonPropsVariantOverrides {
    dashed: true;
  }
}
```

{{"demo": "pages/customization/theme-components/GlobalThemeVariants.js"}}

## 主题变量

Another way to override the look of all component instances is to adjust the [theme configuration variables](/customization/theming/#theme-configuration-variables).

```js
const theme = createMuiTheme({
  typography: {
    button: {
      fontSize: '1rem',
    },
  },
});
```

{{"demo": "pages/customization/theme-components/ThemeVariables.js"}}

## 全局 CSS 覆盖

Components expose [global class names](/styles/advanced/#with-material-ui-core) to enable customization with CSS.

```jsx
const GlobalCss = withStyles({
  // @global 由 jss-plugin-global 处理。
  '@global': {
    '.MuiButton-root': {
      fontSize: '1rem',
    },
  },
})(() => null);

// …

<GlobalCss />;
```

如果您使用 [CssBaseline](/components/css-baseline/) 组件来应用全局重置（global resets），那么也可以将它应用于全局样式。 就像这样：

```jsx
const theme = createMuiTheme({
  components: {
    MuiCssBaseline: {
      styleOverrides: {
        '@global': {
          html: {
            WebkitFontSmoothing: 'auto',
          },
        },
      },
    },
  },
});

// ...
return (
  <ThemeProvider theme={theme}>
    <CssBaseline />
    {children}
  </ThemeProvider>
);
```

{{"demo": "pages/customization/theme-components/GlobalCssOverride.js", "iframe": true, "height": 100}}