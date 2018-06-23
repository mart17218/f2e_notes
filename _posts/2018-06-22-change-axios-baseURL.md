---
layout: post
title: Change axios baseURL
---

## Change axios baseURL dev/prod modes

### Background

  The versions of vue and webpack I use in project:  
  - "vue": "^2.5.2"
  - "webpack": "^3.6.0"

### Issue

  For development and production codes run in different environment,
  we need to set respective base urls for them.
  For example, in development mode, we need to use an absolute path 
  
  ```markdown
  axios.get('http://xxx.xxx.xxx.xxx/my-request/:id')
  ```
  
  , while in production mode, it just set to a relative path like
  
  ```markdown
  axios.get('/relative-path/my-request/:id')
  ```
  
### Solution

  Just add some codes in webpack config, to set different path for dev/prod modes.

#### Step 1. Modify _webpack.dev.conf.js_

  ```markdown
    plugins: [
      // ...
      __API__: 'http://xxx.xxx.xxx.xxx/'
    ]
  ```

#### Step 2. Modify _webpack.prod.conf.js_

  ```markdown
    plugins: [
      // ...
      __API__: '/relative-path/'
    ]
  ```

#### Step 3.Modify the entry .js file (which is defined in the _webpack.config.js_ file, e.g. _main.js_)

  ```markdown
    // Some global settings of vue project.
    axios.defaults.baseURL = __API__;
  ```


