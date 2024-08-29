## 1. Project Description

- A " **Data Dashboard Demo** " based on the React, Dva, DataV, and ECharts frameworks. It supports dynamic data refresh and rendering, screen adaptation, data request simulation, partial styling, and flexible chart replacement/reuse.

- The project requires full-screen display (press F11).
- Project environment: react^16.2, webpack-4.0, npm-6.13, node-v12.16.
- Please pull code from the master branch; other branches are for development.
- The project dependencies are outdated; please upgrade them if needed.
- For other map data, please check my other projects (there is a map collection).

Links:

1. [React Official Documentation](https://react.docschina.org/docs/introducing-jsx.html)
2. [Dva Official Documentation](https://dvajs.com/guide/)
3. [DataV Official Documentation](http://datav-react.jiaminghi.com/guide/)
4. [ECharts Examples](https://echarts.apache.org/examples/zh/index.html), [ECharts API Documentation](https://echarts.apache.org/zh/api.html#echarts)
5. [styled-components Official Documentation](https://styled-components.com/), [SegmentFault Related Articles](https://segmentfault.com/a/1190000014682665)
6. [Project Gitee Address (faster access in China)](https://gitee.com/MTrun/react-big-screen)

Demo Screenshots:
![Interface Display](https://github.com/walker396/react-big-screen/blob/main/demo.gif "Interface Display.gif")

## 2. File Directory Description

```shell
Project
├── mock Mock Data
├── src
│  │  ├── assets Static Resources
│  │  ├── components Module Components
│  │  ├── models Dva Model Management
│  │  ├── routes Main Route Definitions
│  │  ├── services Asynchronous Functions
│  │  ├── style Global Styles
│  │  └── utils Utility Functions
│  │
│  ├── index.js Main Function File
│  └── router.jsx Route Definition File
│
└── .roadhogrc.mock.js Exports Mock Data
```

## 3. Detailed Introduction

### Starting the Project

You need to have `nodejs` and `npm` installed beforehand. After downloading the project, run `npm install` in the project root directory to pull dependencies. Use the command `npm run start` to start the project. After starting the project, you will need to manually enter full-screen mode (press F11) to view it.

### Data Request Simulation

The project uses Dva’s built-in mock data method. The data is placed in the `mock` folder and needs to be exported in `.roadhogrc.mock.js`, then registered in the main file `index.js`.

API request functions are written in `services/index.js` and are initiated by asynchronous functions in the `effects` object within `models/*` files. Since `subscriptions` listens to route changes triggering asynchronous functions, these functions will be automatically triggered when the interface is opened. For specific implementation, refer to the Dva documentation.

Request functions use `Dva`’s built-in `utils/request.js` for GET requests. POST requests are not supported by default and need to be modified manually.

Mock data retrieval for the interface is similar to `react-redux`. Use the `connect` higher-order function in `components/*` to receive and pass data to each `Chart` component.

### Chart Components

The chart components primarily use the ECharts and DataV visualization frameworks. Chart files are located in `components/*/charts`, and configuration files are in `charts/options.js`. Dynamic data is received and imported by various `page/index.js` files. The ECharts rendering function is uniformly encapsulated in `utils/chart.js`.

### Styling

Styling uses the `styled-components` plugin to achieve component-based styling similar to Vue’s scoped functionality. Styles in the interface will not affect each other. A simple example is as follows:

Style File `style`:

```js
import styled from "styled-components";

// Generates a div tag
export const Index = styled.div`
  display: flex;
  flex-direction: column;
  align-items: center;
`;
```

Usage:

```js
import { Index } from './style';

//......

 render() {
   return (
    // The content will be wrapped in a div tag after compilation
     <Index> Content </Index>
   )
 }
```

`styled-components` also supports parameter passing, inheritance, and property definition. For more information, please refer to the official website.

Global styles are registered in `router.jsx` using `styled-components -> createGlobalStyle` and applied globally, similar to how `icon` is imported.

### Icon Files

Icon files use `iconfont` icons and are registered with `styled-components` with an additional processing step.

- Download the `unicode` file to the project, such as `assets/icon`, and delete extra `demo*` files and `iconfont.js`.
- Rename the `iconfont.css` file to `iconfont.js`.
- Open the `iconfont.css` file and modify it to the following format:

```js
import { createGlobalStyle } from 'styled-components';

// Use styled-components global registration function to wrap and export the content
export const Iconstyle = createGlobalStyle`
  @font-face {font-family: "iconfont";
  ......
`
- Register it in `router.jsx` just like the global styles.
```

### Screen Adaptation

This project uses the `utils/flexible.js` plugin to adapt screen sizes by changing the value of rem. The original design is for 1920px, with an adaptation range of 1366px to 2560px. The project has made adjustments based on actual conditions. For small screens (e.g., width of 1366px), you may need to manually adjust certain dynamic components like the 'dynamic text change component,' which can affect layout.

```js
// flexible file location: `common/flexible.js`, changes are as follows
function refreshRem() {
  var width = docEl.getBoundingClientRect().width;
  // Minimum 1366px, maximum adaptation 2560px
  if (width / dpr < 1366) {
    width = 1366 * dpr;
  } else if (width / dpr > 2560) {
    width = 2560 * dpr;
  }
  // The original project is 1920px; I set it to 24 parts so that 1rem is 80px
  var rem = width / 24;
  docEl.style.fontSize = rem + "px";
  flexible.rem = win.rem = rem;
}
```

### Fixing Dva Version History Errors

Find the `dva` package in `node_modules` and modify `lib/index.js`.

Line 22:

```js
var _createHashHistory = _interopRequireDefault(
  require("history").createHashHistory
);
```

### Changing the Start Port

If there is a port conflict during project compilation, you may encounter a prompt to open a new port. If this causes issues, you need to change the start port. Modify the command in `package.json`, for example, to change the port to 9000:

```shell
 "start": "set PORT=9000 && roadhog server",
```

### Using Hooks

If you need to use Hooks instead of Class components, first uninstall the current React version and install a React version that supports Hooks (>=16.8). This project is scaffolded with Dva and does not currently support Hooks.

---

Feel free to let me know if you need any more adjustments!
