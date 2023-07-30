---
layout: post
title:  download Blob file
date:   2023-07-3 09:59:05 +0900
comments : true
categories: Note
tags: [axios]
---

excel 파일을 blob 형태로 다운로드 받아야 하는데, 할 줄 몰라서 헤맴...

blob 데이터 다운로드 시 header 에 response type 을 blob 으로 지정해 요청한다.

```typescript
export function post<T>(url: string, data: T, isResponseTypeBlob?: boolean) {
const config: AxiosRequestConfig = {
baseURL: API_ROOT,
};

if (isResponseTypeBlob) {
config.responseType = 'blob';
}

return axios.post(url, data, config);
}
```

blob 다운로드 시에만 해당 헤더를 보내도록 했다.

```typescript
export const downloadResponseTypeBlobFile = (data: any, filename: string, contentType?: string): void => {
  const url = window.URL.createObjectURL(new Blob([data], { type: contentType }));
  const link = document.createElement('a');
  link.href = url;
  link.setAttribute('download', filename);

  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};
```

