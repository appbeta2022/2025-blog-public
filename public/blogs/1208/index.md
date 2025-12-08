本文整理了 Safari、Chrome 浏览器以及 macOS/Windows 系统层面的 DNS 和网络缓存刷新方法，可用于域名解析变更后无法访问、Chrome 缓存过深等情况。

---

## 🍎 一、Chrome 浏览器刷新缓存

### 1. 清理 Chrome DNS 缓存
在地址栏输入：
```
chrome://net-internals/#dns
```
点击按钮：**Clear host cache**

---

### 2. 清理 Chrome Socket 连接池
在地址栏输入：
```
chrome://net-internals/#sockets
```
点击按钮：**Flush socket pools**

---

### 3. 清除 HSTS 缓存（安全策略缓存）
在地址栏输入：
```
chrome://net-internals/#hsts
```
找到：
**Delete domain security policies**

输入域名并点击 **Delete**

用于解决：Chrome 记住错误的 HTTPS/证书访问策略。

---

### 4. 关闭/检查 DNS over HTTPS（DoH）
访问：
```
chrome://settings/security
```
找到 **Use secure DNS（安全 DNS）**，关闭它，或选择自定义 DNS。

用于解决：Chrome 使用 DoH 导致解析与系统 DNS 不一致。

---

### 5. 隐身模式测试
快捷键：
- macOS：`Command + Shift + N`
- Windows：`Ctrl + Shift + N`

隐身模式不使用历史缓存，用于测试解析是否正确。

---

## 🧭 二、Safari 刷新缓存

### 1. 隐身模式（私密窗口）
快捷键：
- macOS：`Command + Shift + N`

Safari 不带多重缓存机制，刷新可直接生效。

---

### 2. 清除浏览器缓存
菜单：Safari → 清除历史记录 → 选择“所有历史记录”。

或者：Safari → 偏好设置 → 隐私 → 管理网站数据 → 删除所有数据。

---

## 🖥 三、macOS 系统刷新 DNS 缓存

打开“终端”输入：
```
sudo dscacheutil -flushcache
sudo killall -HUP mDNSResponder
```
输入密码后回车。

作用：清除系统级 DNS 缓存，Safari 与其他 App 使用系统 DNS，会立刻生效。

---

## 🪟 四、Windows 系统刷新 DNS 缓存

打开管理员 CMD：
```
ipconfig /flushdns
```

作用：清除 Windows 系统 DNS 缓存。

---

## 🔍 五、测试 DNS 是否生效

### 1. ping 测试
```
ping yourdomain.com
```

### 2. nslookup 查询
```
nslookup yourdomain.com
```

若 IP 返回为新值，则解析生效。

---

## 💡 建议使用顺序（推荐做法）

若 Chrome 无法访问域名：

```
chrome://net-internals/#dns → Clear host cache
chrome://net-internals/#sockets → Flush socket pools
chrome://net-internals/#hsts → Delete domain policy
```

然后执行 macOS 或 Windows DNS 刷新命令，重新打开浏览器即可。

---

## ❤️ 备注
DNS 缓存不仅在浏览器，还包括：

- 浏览器 DNS 缓存
- HSTS 缓存
- 证书缓存
- 系统 DNS 缓存
- ISP DNS 缓存
- CDN 全局缓存

因此有时需要等待几分钟到几小时传播时间。

