# ble-relay-template
## 关于本仓库

本仓库基于 [vickyldr/svakom-ble-ai](https://github.com/vickyldr/svakom-ble-ai) 项目，已获得原作者授权。

原仓库侧重于 SVAKOM SL278H 的完整逆向流程和抓包教程，如果你需要了解如何抓包、逆向蓝牙协议，建议参考原仓库。

本仓库在原仓库架构基础上做了以下延伸：
1. **通用适配**：不再局限于特定设备，任何 BLE 设备都能用
2. **iOS 纯手机实现**：无需电脑，全部在手机上完成
3. **AI 生成代码**：用户只需提供设备信息，AI 自动生成中继页面

感谢原作者的逆向成果！

## 与原仓库的关系

本仓库的 **MCP 服务器（`bridge/index.js`）和部署方式（Railway）** 沿用了原仓库 [vickyldr/svakom-ble-ai](https://github.com/vickyldr/svakom-ble-ai) 的架构，已获得原作者授权。

本仓库做了以下改动：
1. **删除了写死的 `index.html`**：改为由用户用 AI 生成，适配自己的设备
2. **关闭了 `/toy` 接口的密码验证**：糯叽叽连接时不需要密码

简单说：**部署方式没变，设备适配方式变了。**

# BLE MCP 中继服务器

这是一个通用的 MCP 中继服务器模板，用于接收 AI 指令，供中继页面轮询。



## 这个仓库有什么

| 文件 | 作用 |
|------|------|
| `bridge/index.js` | MCP 服务器代码（通用，不需要改） |
| `package.json` | Railway 部署配置（不需要改） |


## 第一步：部署到 Railway

1. **Fork 本仓库**
   - 点击右上角「Fork」，复制到你的 GitHub 账号

2. **登录 Railway**
   - 打开 railway.app，用 GitHub 账号登录

3. **部署项目**
   - 点击「New Project」→「Deploy from GitHub repo」
   - 选择你 Fork 的仓库
   - 点击「Deploy」，等待完成（约 1-2 分钟）

4. **设置密码**
   - 点击「Variables」标签
   - 添加变量：
     - Key: `BRIDGE_SECRET`
     - Value: 你自己设的密码（比如 `abc123`）
   - 点击「Add」

5. **获取公网地址**
   - 点击「Settings」→「Networking」
   - 复制公网地址，格式如：`https://xxx.up.railway.app`


## 第二步：生成中继页面

1. **准备设备信息**
   - 用 nRF Connect 抓包获取：
     - 服务 UUID
     - 特征 UUID
     - 设备名
     - 指令格式
     - 续命间隔

2. **发给 AI 生成 HTML**
   - 把下面的话术发给AI(别给豆包)：
  
     
我的蓝牙设备信息：

· 服务 UUID: [你抓到的]
· 特征 UUID: [你抓到的]
· 设备名: [你的设备名]
· 指令格式: [你抓到的]
· 续命间隔: [你抓到的]

无声音频保活 URL：https://img.heliar.top/file/1772516513350_30min-osbvow_2.mp4

请帮我生成一个蓝牙连接 MCP 服务器的中继 HTML，包含：连接 MCP 服务器、连接蓝牙、强度控制、测试、停止、续命、轮询、日志、无声音频保活。

3. **保存文件**
- AI 生成代码后，复制保存


### 第三步：部署中继页面

1. **上传到 GitHub**
   - 回到你 Fork 的仓库
   - 点击「Add file」→「Create new file」
   - 文件名输入：`index.html`
   - 把 AI 生成的代码**完整粘贴**进去
   - 点击「Commit changes」

2. **开启 GitHub Pages**
   - 进入仓库「Settings」→「Pages」
   - 「Branch」选「main」，点击「Save」
   - 等待 1-2 分钟，获得链接


## 第四步：在 iOS 上使用

1. **安装 Bluefy**
- App Store 搜索「Bluefy」，安装

2. **打开中继页面**
- 在 Bluefy 地址栏输入你的 GitHub Pages 链接

3. **连接 MCP 服务器**
- 填入 Railway 公网地址
- 填入你设置的 `BRIDGE_SECRET` 密码
- 点击「连接」

4. **连接蓝牙**
- 确保设备已开机
- 点击「连接蓝牙」
- 在列表中选择你的设备

5. **启动保活**
- 点击「播放保活媒体」，让无声音频在后台播放

6. **测试控制**
- 点击「测试」，设备应该有反应


## 第五步：接入 AI 控制

1. 在糯叽叽的 MCP 配置页面
2. 服务 URL 填：`https://你的Railway地址`
3. 认证方式选「无」
4. 测试是否正常
-如果报错，把报错内容丢给AI让它改



## 常见问题

**Q：部署 Railway 要钱吗？**
A：Railway 有免费额度，足够个人使用。

**Q：连接不上蓝牙？**
A：关闭官方 App，重启手机蓝牙，确保设备已开机。

**Q：设备动一下就停？**
A：检查续命间隔是否设置正确。

**Q：切后台蓝牙断开？**
A：点击「播放保活媒体」保持后台活跃。

**Q：AI 发指令没反应？**
A：检查 MCP 服务器地址和密码是否正确。


## 完整教程

