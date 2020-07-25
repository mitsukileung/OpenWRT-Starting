# OpenWrt Starting 

OpenWRT自动化固件编译程序。

基于 [Actions-OpenWrt](https://github.com/P3TERX/Actions-OpenWrt) 项目魔改版。

[Actions-OpenWrt 官方教程](https://p3terx.com/archives/build-openwrt-with-github-actions.html)

## 相关说明：

- 根目录可上传 `build.config` 与 `feeds.conf` 配置文件。
- `build.config` 为安装配置文件，使用  *make menuconfig*  命令生成.config 文件内容（因 . 开头文件易被系统隐藏不易管理，此处更改名称为build.config）默认为 [Lean' Lede](https://github.com/coolsnowwolf/lede) 开源项目。
- `Feeds.conf` 为插件包Git项目地址。放在仓库根目录，自动覆盖内部文件
- OtherFile目录为待替换脚本文件，用于修改源代码中部分文件的替换，相关命令定义在`custom1.sh`中。
- 当处于DEBUG模式时不会编译产生文件。

## 配置：

编译配置文件目录为 `.github/workflows/build-openwrt.yml` 内部相关环境变量可根据自己需求进行调整。

|         环境变量         | 功能                                            |
| :----------------------: | :---------------------------------------------- |
|        `REPO_URL`        | 源码仓库地址                                    |
|      `REPO_BRANCH`       | 源码分支                                        |
|       `FEEDS_CONF`       | 自定义`feeds.conf`文件名                        |
|      `CONFIG_FILE`       | 自定义`build.config`文件名                      |
|       `DIY_P1_SH`        | 自定义`custom1.sh`文件名                        |
|       `DIY_P2_SH`        | 自定义 `custom2.sh`文件名                       |
|      `SSH_ACTIONS`       | SSH 连接 Actions 功能。默认`false`              |
|     `UPLOAD_BIN_DIR`     | 上传 bin 目录。即包 ipk 文件和固件。默认`false` |
| `UPLOAD_BIN_COWTRANSFER` | 上传固件到奶牛快传。默认`false`                 |
|    `UPLOAD_FIRMWARE`     | 上传固件目录。默认`true`                        |
|   `UPLOAD_COWTRANSFER`   | 上传固件到奶牛快传。默认`false`                 |
|     `UPLOAD_RELEASE`     | 上传固件并发布Releases。默认`false`             |
|      `DEBUG_MODEL`       | 调试模式。仅拉取安装插件等不编译任何文件。      |
|           `TZ`           | 时区设置                                        |



##  编译触发

#### 定时编译

编辑 ``.github/workflows/build-openwrt.yml` 文件，取消以下注释开启定时编译。使用Cron格式定时。

```
#  schedule: 
#    - cron: 0 8 * * 5
```

####  一键编译

取消以下注释，点击 Star 即可开始编译

```
#  watch:
#    types: started
```



## Custom 脚本使用说明

#### custom1.sh

当远程仓库克隆项目代码（openwrt）克隆完成后执行此脚本，执行完毕后进行 Feed 的升级和安装操作。可进行相关脚本替换，添加软件包等编译前操作。

```
# Add a feed source
sed -i '$a src-git lienol https://github.com/Lienol/openwrt-package' feeds.conf.default
```

#### custom2.sh

当 Feeds 升级并安装完成后执行此脚本。如替换 Luci 目录内 argon主题为最新版本。

```
rm -rf ./package/lean/luci-theme-argon
git clone https://github.com/jerrykuku/luci-theme-argon.git ./package/lean/luci-theme-argon/
```

####  注意：

Custom自定义脚本位置：

Custom脚本自身位置：/home/runner/work/仓库名称/仓库名称/custom1/2.sh 。

脚本执行目录为：/home/runner/work/仓库名称/仓库名称/openwrt 目录内。





© Meloncn  2020



