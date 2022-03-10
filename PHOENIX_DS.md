# Phoenix Design System

> The Phoenix Design System repo is all about `style`! This is where all of the SCSS is written to support our themes, typesetting, components, mixins, utilities, and tokens.

## Overview

### How styles are packaged and exported/imported

This project uses `Webpack` to generate minified styles that are output to the `/dist` directory.

![dir structure](https://cdn.jsdelivr.net/gh/mikelbrierly/tmp_phx/ea1297900c5ead630afe04e0c30284173c5b67a098dbfb5d00e4cadf02942770.png)

Everything in that `/dist` directory is released as a `clientlib` that can be installed as a package into an AEM instance. They can also be imported using the `@import` statement. Take a look at this example of how the design system is used for the Button component's styles in phoenix-experience:

![picture 3](https://cdn.jsdelivr.net/gh/mikelbrierly/tmp_phx/02c98b910433ad20cad83a7daea05a200fc6313af2562f293db42bdc207be6c5.png)

That will import all the minified css from this file in `phoenix-design-systems`

![picture 4](https://cdn.jsdelivr.net/gh/mikelbrierly/tmp_phx/bcbfc260d10bb045acfba4b62019ce7ffd0ee0bd654062c012ae421e186ad15c.png)

### Technologies

You'll notice that this repository uses **React**, **TypeScript**, **MDX**, and **TSX** (Like JSX but with typescript instead). The purpose of this repository is two-fold, it allows us to export all of our styles that can be consumed as a package in other applications, but it also functions as a visual showcase and sandbox for the different components in our design system.

![picture 5](https://cdn.jsdelivr.net/gh/mikelbrierly/tmp_phx/462e5719d29d9d4c2d33bd503bc6dd770f77a051467afb446744291d6667bc94.png)

This visual showcase (or styleguide) is built using a tool called [Storybook](https://storybook.js.org/). Storybook is built on React and uses MDX to display the component pages. This is the reason for the inclusion of those technologies. Our components themselves remain AEM components, but we use React and Storybook to document, theme, and style them.

**Storybook** is used as a CLI tool in this repo, [this guide](https://storybook.js.org/docs/react/get-started/install) will help you get setup.

**MDX** is basically markdown and JSX blended together into a single syntax that is used to create Storybook "stories"

## Getting started

Run a local instance of the Phoenix Design System

```sh
npm run storybook
```

You can take a look at any of the components, and view them in the "Canvas" tab to test interactions.

![picture 6](https://cdn.jsdelivr.net/gh/mikelbrierly/tmp_phx/a52cb32c7d4a27b5824aeb7bc1970e72cf4d93fd43c1b4a13068a2082e387756.png)

Any styling changes in `button.scss` will be reflected in your storybook instance.

![picture 7](https://cdn.jsdelivr.net/gh/mikelbrierly/tmp_phx/1f2859797bbd295fe442f0935b5d729ab568f5faea10c1f89101f2da31ad6df9.png)

Now that you have the Design System running locally, and a general understanding of it's structure and purpose, you're ready to dive into a guide on using and creating Storybook Stories for components. [This guide](https://storybook.js.org/docs/react/writing-stories/introduction) is a perfect place to start!
