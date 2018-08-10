---
layout: post
title: "puppeteer无头浏览器安装"
author: Kehao Wu
tags:
 - Ubuntu
 - locale
 - utf-8
---

### 完整解决方案

```
sudo apt-get install libx11-xcb1
sudo apt-get install libxcomposite-dev
sudo apt-get install libxcursor-dev
sudo apt-get install libxdamage-dev
sudo apt-get install libxi-dev
sudo apt-get install libxtst-dev
sudo apt-get install libxss-dev
sudo apt-get install libxrandr-dev
sudo apt-get install libgconf2-4
sudo apt-get install libasound-dev
sudo apt-get install libpangocairo-1.0-0
sudo apt-get install libgtk2.0-0:i386
sudo apt-get install libatk-bridge2.0-0
sudo apt-get install libgtk-3-0
```

---

```
node_modules/puppeteer/.local-chromium/linux-515411/chrome-linux/chrome: error while loading shared libraries: libX11-xcb.so.1: cannot open shared object file: No such file or directory
```

Solution

```
sudo apt-get install libx11-xcb1
```

---

```
chrome: error while loading shared libraries: libXcomposite.so.1: cannot open shared object file: No such file or director
y

```
Solution

```
sudo apt-get install libxcomposite-dev
```

---

```
chrome: error while loading shared libraries: libXcursor.so.1: cannot open shared object file: No such file or directory
```

Solution

```
sudo apt-get install libxcursor-dev
```

---

```
chrome: error while loading shared libraries: libXdamage.so.1: cannot open shared object file: No such file or directory
```

Solution

```
sudo apt-get install libxdamage-dev
```

---

```
chrome: error while loading shared libraries: libXi.so.6: cannot open shared object file: No such file or directory
```

Solution

```
sudo apt-get install libxi-dev
```

---

```
chrome: error while loading shared libraries: libXtst.so.6: cannot open shared object file: No such file or directory
```

Solution

```
sudo apt-get install libxtst-dev
```

---

```
chrome: error while loading shared libraries: libXss.so.1: cannot open shared object file: No such file or directory
```

Solution

```
sudo apt-get install libxss-dev
```

---

```
chrome: error while loading shared libraries: libXrandr.so.2: cannot open shared object file: No such file or directory
```

Solution

```
sudo apt-get install libxrandr-dev
```

---

```
chrome: error while loading shared libraries: libgconf-2.so.4: cannot open shared object file: No such file or directory
```

Solution

```
sudo apt-get install libgconf2-4
```

---

```
chrome: error while loading shared libraries: libasound.so.2: cannot open shared object file: No such file or directory
```

Solution

```
sudo apt-get install libasound-dev
```

---

```
chrome: error while loading shared libraries: libpangocairo-1.0.so.0: cannot open shared object file: No such file or directory
```

Solution

```
sudo apt-get install libpangocairo-1.0-0
```

---

```
chrome: error while loading shared libraries: libatk-1.0.so.0: cannot open shared object file: No such file or directory
```

Solution 

```
sudo apt-get install libgtk2.0-0:i386
```

---

```
chrome: error while loading shared libraries: libatk-bridge-2.0.so.0: cannot open shared object file: No such file or directory
```

Solution

```
sudo apt-get install libatk-bridge2.0-0
```

---

```
chrome: error while loading shared libraries: libgtk-3.so.0: cannot open shared object file: No such file or directory
```

Solution

```
sudo apt-get install libgtk-3-0
```

