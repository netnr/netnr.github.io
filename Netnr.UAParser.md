# Netnr.UAParser
提取浏览器名称、浏览器版本号、系统名称、系统版本号、是否为爬虫

### 使用 (Usage)
```csharp
var uap = new UAParsers(userAgent);

var clientModel = uap.GetClient();
var deviceModel = uap.GetDevice();
var osModel = uap.GetOS();
var botModel = uap.GetBot();
```

预编译性能翻一倍，但：多占约 80MB 内存，首次解析耗时约 10s
// UAParsers.PreCompile();
// var uap = new UAParsers(userAgent); // 首次解析耗时较长

### 附
正则：<https://github.com/matomo-org/device-detector>  
去除详细型号检测，包精简，轻依赖，可预编译正则，速度快。