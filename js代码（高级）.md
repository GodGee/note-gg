###### js代码（高级）



```
 const define = function (target, key, computed, val) {
        let notify = [];
        Object.defineProperty(target, key, {
            get: () => {
                !!define.target &&
                    define.target.key !== key &&
                    notify.indexOf(define.target.notify) < 0 &&
                    notify.push(define.target.notify);
                return (val = val);
            },
            set: (value) => {
                val = value;
                notify.forEach(
                    (depend) =>
                        depend && typeof depend === "function" && depend()
                );
            },
        });
        if (!computed || typeof computed !== "function") return;
        const toNotify = () => (target[key] = computed(target));
        define.target = { notify: toNotify, key };
        define.target.notify();
        define.target = null;
    };
```

