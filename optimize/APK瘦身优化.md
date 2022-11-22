# APK瘦身优化
## APK的结构
### 包含以下目录
#### (1).assets
包含了应用的资源，这些资源能够通过AssetManager对象获得
#### (2).lib
包含了针对处理器层面的被编译的代码。这个目录针对每个平台类型都一个子目录，比如armeabi，armeabi-v7a，arm64-v8a，x86，x86_64,和mips
#### (3).res
包含了没被编译到resources.arsc的资源
#### (4).META-INF
包含CERT.SF和CE.RSA签名文件，也包含了MANIFEST.MF文件
### 包含以下文件
#### (1).class.dex
包含了能被Dalvik或Art虚拟机理解的dex文件格式的类
#### (2).resources.arsc
包含了被编译的资源。该文件包含了res/values目录的所有配置的xml内容，打包工具将xml内容编译成二进制形式并压缩。这些内容包含了语言字符串和styles，还包含了那些内容虽然不直接存储在resources.arsc文件中，但是给定了该内容的路径，比如布局文件和图片。所以又叫资源映射表
#### (3).AndroidMnaifest.xml
包含了主要的Android配置文件。这个文件列出了应用名称，版本，访问权限，引用的库文件，该文件使用二进制XML格式存储。
## 优化方法
### 图片资源过多问题
转成svg矢量图
### 动态lib so库
只需要支持armeabi-v7a，就行，微信就是这样，已经适配市面上绝大多数机型
### 移除无用资源
AS提供了一键移除所有无用的资源，Refact -> remove unused ,有点问题，当通过隐式引用的时候加载的资源，会导致被删掉
### 混淆代码
minifyEnabled true
### 压缩
build.gradle 中使用shrinkResources 配置，这种要配合混淆一起使用
### 动态加载SO库
服务器下载so，拷贝到data目录下
### 插件化压缩包
插庄式加载
### 一套资源
### 去除无用资源
去除无用语言资源

