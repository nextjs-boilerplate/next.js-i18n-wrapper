# next-redux-helper

make redux easier to work in next.js both server side and client side, support ssr

让redux在next.js的服务端和浏览器端更容易使用，支持服务端渲染


## basic | 基本使用

```
import reduxHelper from 'next.js-redux-helper'
import wrapper from 'next.js-redux-helper/dest/wrapper'

const initStore = reduxHelper(reducers, initialState)
const reduxWrapper = wrapper(initStore)

export default reduxWrapper(connect(state=>state)(Page))

```

demo http://nextjs.i18ntech.com/components/reduxhelper

source: https://github.com/nextjs-boilerplate/components/blob/master/pages/components/reduxhelper.js

## async action | 异步事件

```
//in redux
export const login = (username, passwd) => dispatch => {
  var form = JSON.stringify({ username, passwd })

  return fetch('/api/auth', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: form
  })
    .then(r => r.json())
    .then((user) => {
      if (user.error) {
        return Promise.reject(user.error)
      }
      return user
    })
    .then((user) => {
      return dispatch({
        type: actionTypes.login,
        user
      })
    })
}

// in component
handleLogin() {
    const { dispatch } = this.props
    dispatch(login(this.refs.username.value, this.refs.passwd.value))
      .catch((e) => {
        alert('登录失败:' + e)
      })
  }
```

demo: http://nextjs.i18ntech.com/login

source: https://github.com/nextjs-boilerplate/components/blob/master/tools/store/user.js

## ssr | 服务端渲染

```
static async getInitialProps(ctx) {
  //user
  const { store, req } = ctx
  if (store && req) {
    const user = req.cookies.user
    await store.dispatch(set(user))
  }
}
```
demo: https://github.com/nextjs-boilerplate/next-express-redux-i18n

source: https://github.com/nextjs-boilerplate/next-express-redux-i18n/blob/master/components/layout/wrapper.js

## with i18next

```

import reduxHelper from 'next.js-redux-helper'
import wrapper from 'next.js-redux-helper/dest/wrapper'
import { wrapper as i18nWrapper } from './i18n'

const initStore = reduxHelper(reducers, initialState)
const reduxWrapper = wrapper(initStore)

const FinalPage = reduxWrapper(i18nWrapper(Page))
```

about i18n: https://github.com/nextjs-boilerplate/next-i18n-helper

demo: https://github.com/nextjs-boilerplate/next-express-redux-i18n

source: https://github.com/nextjs-boilerplate/next-express-redux-i18n/blob/master/components/layout/wrapper.js