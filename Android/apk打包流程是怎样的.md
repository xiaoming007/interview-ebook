# apk打包流程
打包的过程就是gradle通过SDK提供的工具，将资源文件进行处理的过程
### 用到的工具
1.aapt 2.aidl 3.java Compiler 4.dex 5.apkbuilder 6.Jarsigner
### 资源
1.Application Resources 2.R.java 3.Application Source Code 4.Java Interface 5..class Files 6..dex Files 7.3rd Party Libraries and .class Files 8.Other Resources 9.Compiled Resources 10.Android Package(.apk) 11.Debug or Release Keystore 12.Signed apk 
### 具体流程
* Application Resource通过aapt工具编译成Compiled Resources
* .aidl Files 通过aidl工具生成Java Interfaces
* R.java,Application Source Code,Java Interfaces通过Java Compiler工具编译成.class Files
* .class Files,3rd Party Libraries and .class Files通过dex工具打成.dex Files
* .dex Files,Compiled Resources, Other Resources 通过apkbuilder工具打成Android Package(.apk)
* 然后在通过Jarsigner工具与签名打成Signed .apk
