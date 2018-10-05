---
date: '2018-10-05'
title: 'Type safe environment variable'
category: 'Coding'
---

Thay vì dùng `process.env.REACT_APP_API_ENV` trực tiếp thì nên tạo 1 file `env.ts`/`env.js` như bên dưới.

```ts
const {
  REACT_APP_API_ENV,
  REACT_APP_SENTRY,
  REACT_APP_PRIVACY_URL,
  REACT_APP_TERM_OF_USE_URL,
  REACT_APP_FAQ_URL,
  REACT_APP_LOGO_URL
} = process.env

export {
  REACT_APP_API_ENV,
  REACT_APP_SENTRY,
  REACT_APP_PRIVACY_URL,
  REACT_APP_TERM_OF_USE_URL,
  REACT_APP_FAQ_URL,
  REACT_APP_LOGO_URL
}
```

Tại sao phải mới import rồi lại export? Làm vậy để giải quyết vấn đề gì?

##### 1. Document

Một đồng nghiệp/contributor nhìn vào file env.ts này có thể biết được có bao nhiêu environment variable mà không phải đi search `process.env` trong toàn bộ file của project.

##### 2. Type safe (flowtype/typescript)

`process.env` trong typescript có type như sau:

```ts
// NodeJS.Process.env: NodeJS.ProcessEnv
interface ProcessEnv {
  [key: string]: string | undefined
}
```

Nghĩa là khi chúng ta access 1 environment variable không tồn tại mà typescript hay flowtype chả phàn nàn gì . Ví dụ:

```
process.env.NONE_EXIST_VARIABLE
```

**_Q:_** Èo. Có mấy khi mà đi access một env variable mà mình chưa gán đâu, lúc code nhìn là biết liền?

**_A:_** Giả sử bạn có set variable này là `process.env` file:

```
THIS_IS_A_LONG_ENVIRONMENT_VARIABLE_NAME_IN_THE_WORLD=‘hello world’
```

và access variable này như sau:

```ts
const { THIS_IS_A_LONG_ENVIROMENT_VARIABLE_NAME_IN_THE_WORLD } = process.env
console.log(THIS_IS_A_LONG_ENVIROMENT_VARIABLE_NAME_IN_THE_WORLD)
```

kết quả in ra trong console là:

```js
undefined
```

Trong vòng bao lâu thì bạn tìm ra nguyên nhân là `ENVIROMENT` thiếu `N` (`ENVIRONMENT`)? 5s, 10s hay lâu hơn?

#### 3. Autocomplete

Nếu xài typescipt/flowtype thì việc tạo file `env.ts/env.js` còn giúp chúng ta autocomplete khi code nữa.

[TBD] add gif/video demo

### Kết

Xài `env.ts/env.js` file để quản lý environment variable để document tốt hơn, tránh lỗi typo, **_type safe_** và **_autocomplete_**.
