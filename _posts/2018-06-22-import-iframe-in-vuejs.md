---
layout: post
title: Import iframe in Vuejs
---

## How to import iframe in Vuejs when its src is a .html file

### Background

  The versions of vue and webpack I use in project:  
  - "vue": "^2.5.2"
  - "webpack": "^3.6.0"

### Issue

  I tried to use `<iframe>` tag in my Vue project as follows,

  ```markdown
  <iframe src="myPage.html"></iframe>
  ```

  but _myPage.html_ is not loaded and the iframe src redirects to _index.html_.

### Solution

  Thus I find this [article](http://blog.pixelastic.com/2017/09/12/importing-iframe-with-webpack-and-vue-js/).
  It almost solves my problem.

#### Step 1. Modify _build/vue-loader.conf.js_

```markdown
transformToRequire: {
    iframe: 'src' // add this line
}
```

#### Step 2. Modify _build/webpack.base.conf.js_

```markdown
resolve: {
  alias: {
    'vue$': 'vue/dist/vue.esm.js',
    '@': resolve('src'),
    'html': resolve(__dirname, 'src'),  // If your .html file is not under the src folder, just change the file path here 
  }
},
module: {
  rules: [
    // ...
    {
      test: /\.html$/,                // Add file-loader 
      loader: 'file-loader',
      exclude: /index.html/,
      options: {
        name: '[hash].[ext]'
      }
    },
    // ...
  ]
}
```
...But webpack shows an error message like

```markdown
*Failed to compile.*
```

#### Step 3.

Then I find this [issue](https://github.com/vuejs/vue-loader/issues/193) and change my iframe src to this,
  ```markdown
  <iframe src="~@/myPage.html"></iframe>
  ```
Finally everything works fine.

### Reference
  - [Importing iframe with Webpack and Vue.js](http://blog.pixelastic.com/2017/09/12/importing-iframe-with-webpack-and-vue-js/)  
  - [vue-loader issue - html image src require not respecting webpack aliases? #193](https://github.com/vuejs/vue-loader/issues/193)
