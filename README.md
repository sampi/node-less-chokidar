# node-less-chokidar

## HOWTO

I made this with [Create React App](https://github.com/facebookincubator/create-react-app) in mind, put something similar in your `package.json`

```json
"scripts": {
    "start": "npm run build-css && run-p -ncr watch-css start-js",
    "start-js": "react-scripts start",
    
    "build": "run-s -n build-css build-js",
    "build-js": "react-scripts build",
    
    "test": "run-s -n build-css test-js",
    "test-js": "react-scripts test --env=jsdom",
    
    "build-css": "node-less-chokidar src",
    "watch-css": "node-less-chokidar src --watch"
},
"devDependencies": {
    "node-less-chokidar": "^0.1.0",
    "npm-run-all": "^4.0.2"
}
```

`yarn start` should build all CSS from the LESS files and then keep watching,
`yarn test` will build all files and then run the tests,
`yarn build-css` will build all the CSS,
`yarn watch-css` will watch for changes in the LESS files and build them when they change.

## Known bug

There's a bug though in the following case:

Let's say you have these files:

* `variables.less` (variable definitions, no imports)
* `mixins.less` (mixins, importing `variables.less`)
* `main.less` (imports `variables.less`, `mixins.less` and uses the variables and mixins)

If you change `variables.less` or `mixins.less` it won't rebuild `main.css`, because it doesn't know about the dependency graph of the files.
The fix is that if you know you're editing one of these files that are imported by other files, just run `yarn build-css` and everything will be fine.
