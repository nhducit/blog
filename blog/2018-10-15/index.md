---
date: '2018-10-15'
title: 'How to consume typescript unknown type?'
category: 'note'
---

```ts
import ky from 'ky'
const prefixUrl = 'https://api.nhducit.com/api'
export const api = ky.extend({ prefixUrl })

export interface LoginResponse {
  data: { token: string }
}

export const apis = {
  login: async ({
    username,
    password
  }: {
    username: string
    password: string
  }) => {
    try {
      // response is unknown type
      const response = await api
        .post('/login', { json: { optus_string: optusToken } })
        .json()
      console.log('login response', response.data)
      // [ts] Object is of type 'unknown'.
      return response
    } catch (error) {
      return error
    }
  }
}
```

from Typescript Document

> TypeScript 3.0 introduces a new top type unknown. unknown is the type-safe counterpart of any. Anything is assignable to unknown, but unknown isn’t assignable to anything but itself and any without a type assertion or a control flow based narrowing. Likewise, no operations are permitted on an unknown without first asserting or narrowing to a more specific type.

solution:
LoginResponse to assert type of API's response

```ts
const response = (await api
  .post('/login', { json: { optus_string: optusToken } })
  .json()) as LoginResponse
```
