---
layout: post
title:  eslint 사용 환경에서 no-unused-vars 에러 -> 경고로 설정 변경
date:   2023-08-13 00:03:24 +0900
comments : true
categories: Note
tags: [IDE, typescript]
---

<br />

#### tsconfig.json

`noUnusedLocals`, `noUnusedParameters` 를 false 로 변경

```json
{
  "compilerOptions": {
    "noUnusedLocals": false,
    "noUnusedParameters": false
  }
}
```

<br />

#### .eslintrc.cjs

`rules` 하위의 `no-unused-vars` 는 `off`로, `@typescript-eslint/no-unused-vars` 를 `warn` 으로 변경

```javascript
module.exports = {
  rules: {
    'no-unused-vars': 'off',
    '@typescript-eslint/no-unused-vars': 'warn',
  },
}
```

<br />