<p align="center">
  <a href="https://github.com/GoAdminGroup/go-admin">
    <img width="50%" alt="go-admin" src="http://file.go-admin.cn/introduction/logo.png">
  </a>
</p>
<p align="center">
    遗失的Golang语言编写的数据可视化与管理平台构建框架
</p>
<p align="center">
<a href="https://travis-ci.com/GoAdminGroup/go-admin"><img alt="Go Report Card" src="https://api.travis-ci.com/GoAdminGroup/go-admin.svg?branch=master"></a>
  <a href="https://goreportcard.com/report/github.com/GoAdminGroup/go-admin"><img alt="Go Report Card" src="https://camo.githubusercontent.com/59eed852617e19c272a4a4764fd09c669957fe75/68747470733a2f2f676f7265706f7274636172642e636f6d2f62616467652f6769746875622e636f6d2f6368656e6867352f676f2d61646d696e"></a>
  <a href="https://goreportcard.com/report/github.com/GoAdminGroup/go-admin"><img alt="golang" src="https://img.shields.io/badge/awesome-golang-blue.svg"></a>
  <a href="https://t.me/joinchat/NlyH6Bch2QARZkArithKvg" rel="nofollow"><img alt="telegram" src="https://img.shields.io/badge/chat%20on-telegram-blue" style="max-width:100%;"></a>
  <a href="https://jq.qq.com/?_wv=1027&k=5L3e3kS"><img alt="qq群" src="https://img.shields.io/badge/QQ-756664859-yellow.svg"></a>
  <a href="https://godoc.org/github.com/GoAdminGroup/go-admin" rel="nofollow"><img src="https://camo.githubusercontent.com/a9a286d43bdfff9fb41b88b25b35ea8edd2634fc/68747470733a2f2f676f646f632e6f72672f6769746875622e636f6d2f646572656b7061726b65722f64656c76653f7374617475732e737667" alt="GoDoc" data-canonical-src="https://godoc.org/github.com/derekparker/delve?status.svg" style="max-width:100%;"></a>
  <a href="https://raw.githubusercontent.com/GoAdminGroup/go-admin/master/LICENSE" rel="nofollow"><img src="https://img.shields.io/badge/license-Apache2.0-blue.svg" alt="license" data-canonical-src="https://img.shields.io/badge/license-Apache2.0-blue.svg" style="max-width:100%;"></a>
</p>
<p align="center">
    由<a href="https://github.com/z-song/laravel-admin" target="_blank">laravel-admin</a>启发
</p>

## 前言

GoAdmin 可以帮助你的golang应用快速实现数据可视化，搭建一个数据管理平台。

demo: [http://demo.go-admin.cn/admin](http://demo.go-admin.cn/admin)
账号：admin  密码：admin

demo代码： https://github.com/GoAdminGroup/demo

![](http://file.go-admin.cn/introduction/interface_2.png)

## 特征

- 🚀 **高生产效率**: 10分钟内做一个好看的管理后台
- 🎨 **主题**: 默认为adminlte，更多好看的主题正在制作中，欢迎给我们留言
- 🔢 **插件化**: 提供插件使用，真正实现一个插件解决不了问题，那就两个
- ✅ **认证**: 开箱即用的rbac认证系统
- ⚙️ **框架支持**: 支持大部分框架接入，让你更容易去上手和扩展

## 翻译
我们需要您的帮忙： [https://github.com/GoAdminGroup/docs/issues/1](https://github.com/GoAdminGroup/docs/issues/1)

## 谁在使用GoAdmin

[评论这个issue告诉我们](https://github.com/GoAdminGroup/go-admin/issues/71).

## 使用

通过以下三步运行：

### 第一步：导入 sql

[mysql](https://raw.githubusercontent.com/GoAdminGroup/go-admin/master/data/admin.sql)
[postgresql](https://raw.githubusercontent.com/GoAdminGroup/go-admin/master/data/admin.pgsql)
[sqlite](https://raw.githubusercontent.com/GoAdminGroup/go-admin/master/data/admin.db)

### 第二步：创建 main.go

<details><summary>main.go</summary>
<p>

```go
package main

import (
	"github.com/gin-gonic/gin"
	_ "github.com/GoAdminGroup/go-admin/adapter/gin"
	"github.com/GoAdminGroup/go-admin/engine"
	"github.com/GoAdminGroup/go-admin/plugins/admin"
	"github.com/GoAdminGroup/themes/adminlte"
	"github.com/GoAdminGroup/go-admin/modules/config"
	"github.com/GoAdminGroup/go-admin/examples/datamodel"
	"github.com/GoAdminGroup/go-admin/modules/language"
)

func main() {
	r := gin.Default()

	eng := engine.Default()

	// global config
	cfg := config.Config{
		Databases: config.DatabaseList{
		    "default": {
			Host:         "127.0.0.1",
			Port:         "3306",
			User:         "root",
			Pwd:          "root",
			Name:         "godmin",
			MaxIdleCon: 50,
			MaxOpenCon: 150,
			Driver:       "mysql",
		    },
        	},
		UrlPrefix: "admin",
		// STORE 必须设置且保证有写权限，否则增加不了新的管理员用户
		Store: config.Store{
		    Path:   "./uploads",
		    Prefix: "uploads",
		},
		Language: language.CN, 
		// 开发模式
                Debug: true,
                // 日志文件位置，需为绝对路径
                InfoLogPath: "/var/logs/info.log",
                AccessLogPath: "/var/logs/access.log",
                ErrorLogPath: "/var/logs/error.log",
                ColorScheme: adminlte.ColorschemeSkinBlack,
	}

    	// Generators： 详见 https://github.com/GoAdminGroup/go-admin/blob/master/examples/datamodel/tables.go
	adminPlugin := admin.NewAdmin(datamodel.Generators)
	
	// 增加 generator, 第一个参数是对应的访问路由前缀
	// 例子:
	//
	// "user" => http://localhost:9033/admin/info/user
	//
	// adminPlugin.AddGenerator("user", datamodel.GetUserTable)

	_ = eng.AddConfig(cfg).AddPlugins(adminPlugin).Use(r)

	_ = r.Run(":9033")
}
```

</p>
</details>

其他框架的例子: [https://github.com/GoAdminGroup/go-admin/tree/master/examples](https://github.com/GoAdminGroup/go-admin/tree/master/examples)

### 第三步：运行

```shell
GO111MODULE=on go run main.go
```

访问：[http://localhost:9033/admin](http://localhost:9033/admin)

更多细节详见 [文档说明](http://www.go-admin.cn/docs/#/README)

[这里一个超级简单上手的例子](https://github.com/GoAdminGroup/example)

## 贡献

[这里有一份贡献指南](CONTRIBUTING_CN.md)

非常欢迎提pr，<strong>这里可以加入开发小组</strong>

<strong>QQ群</strong>：756664859，记得备注加群来意

这里是[开发计划](https://github.com/GoAdminGroup/go-admin/projects)

<strong>[点击这里加微信群](http://quick.go-admin.cn/resource/wechat_qrcode.jpg)</strong>

## 十分感谢

inspired by [laravel-admin](https://github.com/z-song/laravel-admin)

## 打赏

留下您的github/gitee用户名，我们将会展示在[捐赠名单](DONATION.md)中。

<img src="http://quick.go-admin.cn/official/assets/imgs/shoukuan.jpg" width="650" />