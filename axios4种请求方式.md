### axios GET请求

```
export function fetchTemplateList (params) {
  return axios.get('url', {
    emulateJSON: true,
    params: params
  }).then(res => {
    if (checkData(res.data) && res.data.code === 200) {
      return Promise.resolve(res.data)
    } else {
      throw res
    }
  }).catch(err => {
    return Promise.reject(err)
  })
}
```
### axios POST请求

```
// post请求后第二个参数为要传递的数据
// 这个headers是为了将传输方式边为form-data
export function upLoadPic (data) {
  return axios.post(`${env.FE_API_URL}/api/v2/admin/images.upload`, data, {
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded'
    },
    emulateJSON: true}).then(res => {
    if (checkData(res.data) && res.data.code === 200) {
      return Promise.resolve(res.data)
    } else {
      throw res
    }
  }).catch(err => {
    return Promise.reject(err)
  })
}
```
### axios PUT请求

```
export function updateSchedule (data) {
  return axios.put(`url`, data, {emulateJSON: true}).then(res => {
    if (checkData(res.data) && res.data.code === 200) {
      return Promise.resolve(res.data)
    } else {
      throw res
    }
  }).catch(err => {
    return Promise.reject(err)
  })
}
```
### axios DELETE请求

```
export function deletePic (data) {
  return axios.delete(`${env.FE_API_URL}/api/v2/admin/images.remove`, {...data, emulateJSON: true}).then(res => {
    if (checkData(res.data) && res.data.code === 200) {
      return Promise.resolve(res.data)
    } else {
      throw res
    }
  }).catch(err => {
    return Promise.reject(err)
  })
}
```
