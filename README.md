# Console Ninja

Console Ninja is a VS Code extension that displays `console.log` output and **runtime errors** directly in your editor from your running browser or node application. It's like your browser dev tools console tab or terminal output from your node app, but instead of having to context switch, values are connected to your code and displayed ergonomically in your editor.

![preview](https://console-ninja.com/images/docs-main.gif)

## Current status

Console Ninja is available for everyone to use for free. It's still very early days for the product, so we would appreciate if you [let us know](https://github.com/wallabyjs/console-ninja/issues) about any issues/bugs that you may find as well as any feature requests that you may have.

While we will probably commercialize Console Ninja at some point, we don't have any immediate plans to do so. Regardless, we plan to always have a free version available and any commercial offering would probably provide more advanced features.

## Supported technologies

Console Ninja currently supports the following tools:
- [Vite](https://vitejs.dev/), including 
  - web applications that were generated by Vite CLI such the ones using React, Vue, Preact, Lit, Svelte, Vanilla, TypeScript or JavaScript;
  - web applications configured to use Vite; 
  - web applications powered by app frameworks that are [using Vite](https://patak.dev/vite/ecosystem.html), such as [Nuxt](https://nuxtjs.org/) or [SvelteKit](https://kit.svelte.dev/).
- [Webpack](https://webpack.js.org/), including
  - [Create React App](https://create-react-app.dev/) generated applications;
  - [Angular CLI](https://angular.io/cli) and [Angular Nx](https://nx.dev/packages/angular) generated applications;
  - any web applications configured to use Webpack node module. 
- [Next.js](https://nextjs.org/), including first class support for client and server side logs.
- [Jest](https://jestjs.io/) test runner (only `console.log` output, no errors).
- [Cypress.io](https://www.cypress.io/) end-to-end test runner.
- [http-server](https://www.npmjs.com/package/http-server) and [serve](https://www.npmjs.com/package/serve) static HTTP servers.
- [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) VS Code extension (`off` by default, can be turned on in the Console Ninja extension VS Code settings).
- [Live Preview](https://marketplace.visualstudio.com/items?itemName=ms-vscode.live-server) VS Code extension (`off` by default, can be turned on in the Console Ninja extension VS Code settings).
- [Spotify Hydrogen Framework](https://apps.shopify.com/hydrogen).

We have designed Console Ninja in such a way that adding support for new tools is fast and easy, so please [let us know](https://github.com/wallabyjs/console-ninja/issues) if there's another technology you want to use Console Ninja with.

## Get started

Console Ninja is designed to fit seamlessly into most typical dev workflows. After you have installed the extension in VS Code and opened your project, you may perform the same steps you usually take:
- run `npm/pnpm/yarn` script or CLI command that starts/opens your app in dev/watch/hot-reload mode;
- start making code changes, such as adding `console.log` to your code **\***;
- if required **\*\***, perform the actions within your app that trigger the modified code, ie. click a button if you have added `console.log` to the button click handler;

**\*** *Most modern tools run in watch mode with hot reload enabled. If your tool doesn't have such mode, then, the same way as without Console Ninja, you need to refresh your app page to make it run your modified code.*

**\*\*** *Sometimes your modified code may be triggered immediately upon the (hot) reload of the app. For example, when code is located in the root of a component, it will be executed when the app (re)starts. If the modified code is not triggered by the app (re)start, then, the same way as without Console Ninja, you will need to perform your app specific actions to trigger the code execution.*

![overview](https://console-ninja.com/images/docs-overview.png)

You should now be able to see logs (and errors, if any) in your editor, next to the relevant line of code. Hovering over the `console.log` or **error** value will display more details. **If you don't see the expected output, please [check Console Ninja's current status](#troubleshooting)**.

Using the `Show Output` command displays the log viewer, allowing you to explore more details and navigate between logs and code.

### Log viewer

Using the `Show Output` command displays Console Ninja's log viewer. The log viewer shows all recorded logs and errors from your running application in chronological order, with newer entries at the bottom. When the command is triggered on a line with a `console.log` result, the last recorded log entry is focussed in the log viewer.

Each log entry provides summary details and is collapsed by default, providing a short preview that can be expanded (via mouse or `Arrow Right` keyboard key after it is focussed) to inspect its details. Once the details are expanded, the `Enter` keyboard key can be used to enter the details keyboard selection/navigation mode; the `Esc` key can be used to exit the mode.

Each entry preview and expanded error entry details contains clickable links to the target source code. Links that point to `http` locations are opened in the editor by downloading the file first (a setting allows `http` links to be opened in the browser instead).

The following keyboard shortcuts are supported for faster navigation:
- `Arrow Up` and `Arrow Down` to navigate to the next/previous displayed entry.
- `Home` and `End` to navigate to the first/last displayed entry.
- `Arrow Left` to collapse focussed entry details and `Ctrl/Cmd + Arrow Left` to collapse all expanded entries details.

Additionally, the following commands (also displayed as icon buttons at the top of the log viewer) are available:

- `Clear all output` from Console Ninja, including inline output (double `Esc`);
- `Toggle auto-clearing of the output on any file change`. If auto-clearing is set to `off`, log entries recorded prior to the latest file change are dimmed, and the file links hover icon is changed to indicate that the displayed position may have changed since the entry was recorded.
- `Toggle auto-scrolling to the last log entry` when new log entries are added. If the output is manually scrolled up so that the last entry row is not visible, then auto-scrolling is paused until the output is manually scrolled to make the last entry row visible.

The log viewer supports search using the `Ctrl/Cmd + F` keyboard shortcut.

The log viewer can be displayed in two modes, either `Beside File` and `In View`, there is a setting to configure the mode. 

![status](https://console-ninja.com/images/docs-view.gif)

In the `Beside File` mode (default, and recommended), the viewer is displayed in a new separate editor group to the left from your current editor. In this case the view behaves like other opened editor tabs, i.e. it is visible in the `Opened Editors` in VS Code file explorer view.

In the `In View` mode, the viewer is displayed as VS Code side view. The view can also be moved to be another VS Code panel. The recommended position for the viewer is to the right of the VS Code `Output` view in the bottom pane of your editor.

## Troubleshooting

If Console Ninja is not working for you as expected, the first thing to check is the extension status. You may see the status info by hovering over the extension icon in VS Code status bar.

![status](https://console-ninja.com/images/docs-status.gif?v=1)

The status popup information dialog explains the current state of the tool and provides some tips on what to do next.

## How does it work

Console Ninja integrates with locally installed tools that are building/preparing your code, and then inspects and adjusts your code (in a way that doesn't change how it executes) before it gets to the runtime (browser or node process that runs the code).

To integrate with supported tools seamlessly, Console Ninja patches your locally installed node modules. When you stop Console Ninja in the editor with the `Pause` command, all patches are removed.

Console Ninja detects if you are running your tool in production mode (by checking CLI flags and process environment variables). When production mode is detected, the tool will not modify your application code even if Console Ninja is running. In the (unlikely) case that you are running production builds on your local dev computer and are deploying or sharing local builds outside of your machine, we recommend running the Console Ninja `Pause` command in your editor prior to running your build to guarantee that no instrumented code ends up in your production code. 

Console Ninja instrumentation is limited to sending runtime values for `console.log` and errors to your locally running editor only (`localhost` hosted websocket server). The runtime data from your app is **never** sent outside of your local machine. If the code of your application is somehow instrumented by Console Ninja and then used outside of your local machine for some reason: 
- in browser, it will simply do nothing if the app host is not `localhost`.
- in node, it will fail to connect to `localhost` and will not stop your app from working.

## Differences between Console Ninja and other tools

### Quokka.js

[Quokka.js](https://quokkajs.com/) is another awesome tool from [our team](https://wallabyjs.com/). Like Console Ninja, Quokka.js allows you to see various execution results without leaving the comfort of your editor.

The fundamental difference between Console Ninja and Quokka.js is that Quokka runs new scratch files or existing code files as a self-contained program. Quokka allows you to quickly execute and iterate code in an **isolated playground** without unnecessary application execution. Console Ninja on the other hand runs within your application (started by your dev server, or test runner), and allows you to debug any **end to end scenarios within your running app**.

### Wallaby.js

The [Wallaby.js](https://wallabyjs.com/) tool from our team runs your JavaScript and TypeScript tests immediately as you type, highlighting results in your IDE right next to your code. 

While you may use Console Ninja to display logs from supported test runner CLIs, test errors are not handled by Console Ninja; this is because test errors are caught/handled by test runners themselves, and special processing is required to format and display test results.

Wallaby not only displays logs and test errors inline (and in a separate ergonomically designed view), but is also [packed with features](https://wallabyjs.com/#features), such as inline code coverage and a time travel debugger, that provide superpowers to your current testing framework/stack, such as [Jest, Vitest, Mocha. etc.](https://wallabyjs.com/#tools)

### Error Lens extension
[Error Lens](https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens) is a VS Code extension that provides inline output for VS Code `Problems` output window, typically **static code analysis errors** from your code linter or language service, such as a linter rule violation or TypeScript types-related error.

In contrast, Console Ninja runs within your application and displays **runtime logs and errors**.
