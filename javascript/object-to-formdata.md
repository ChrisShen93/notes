## convert javascript object to form-data

```javascript
const isEmpty = function (obj) {
  for (const k in obj) {
    return false
  }
  return true
}

const isObject = function (value) {
  return value === Object(value)
}

const isArray = function (value) {
  return Array.isArray(value)
}

const isFile = function (value) {
  return value instanceof File
}

const makeArrayKey = function (key) {
  if (key.length > 2 && key.lastIndexOf('[]') === key.length - 2) {
    return key
  } else {
    return key + '[]'
  }
}

const objectToFormData = function (obj, form, pre) {
  if (!obj || isEmpty(obj)) {
    return {}
  }
  let fd = form || new FormData()

  Object.keys(obj).forEach((prop) => {
    const key = pre ? `${pre}[${prop}]` : prop
    if (isObject(obj[prop]) && !isArray(obj[prop]) && !isFile(obj[prop])) {
      objectToFormData(obj[prop], fd, key)
    } else if (isArray(obj[prop])) {
      obj[prop].forEach((value) => {
        const arrayKey = makeArrayKey(key)
        if (isObject(value) && !isFile(value)) {
          objectToFormData(value, fd, arrayKey)
        } else {
          fd.append(arrayKey, value)
        }
      })
    } else {
      fd.append(key, obj[prop])
    }
  })

  return fd
}

export {
  objectToFormData
}
```
