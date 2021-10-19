##### vue项目动态监听localStorage或sessionStorage中数据的变化

1、首先在main.js中给Vue.prototype注册一个全局方法，然后创建一个StorageEvent方法，当我们执行sessionStorage.setItem(k,val)的时候，初始化事件并派发事件。

```
/**
 * @description
 * @param { number } type 1 localStorage 2 sessionStorage
 * @param { string } key 键
 * @param { string } data 要存储的数据
 * @returns 
 */
 Vue.prototype.$addStorageEvent = function (type, key, data) {
    // localStorage
  if (type === 1) {
    // 创建一个StorageEvent事件
    var newStorageEvent = document.createEvent('StorageEvent');
    const storage = {
      setItem: function (k, val) {
        localStorage.setItem(k, val);
        // 初始化创建的事件
        newStorageEvent.initStorageEvent('setItem', false, false, k, null, val, null, null);
        // 派发对象
        window.dispatchEvent(newStorageEvent);
      }
    }
    return storage.setItem(key, data);
  } else {
     // sessionStorage
    // 创建一个StorageEvent事件
    var newStorageEvent = document.createEvent('StorageEvent');
    const storage = {
      setItem: function (k, val) {
        sessionStorage.setItem(k, val);
        // 初始化创建的事件
        newStorageEvent.initStorageEvent('setItem', false, false, k, null, val, null, null);
        // 派发对象
        window.dispatchEvent(newStorageEvent);
      }
    }
    return storage.setItem(key, data);
  }
}

```

2、在组件A中调用，写入缓存

```
this.$addStorageEvent(1,'userMess',data);
```

3、在组件B中监听(mounted周期内)

```
window.addEventListener('setItem',() => {
	this.user = sessionStorage.getItem('userMess')
})
```

 