*********
Q1.Jack is required to java_8
原因：AS新版本java8新特性需要jack工具链

新的 Java 8 语言功能，还需使用新的 Jack 工具链。新的 Android 工具链将 Java 源语言编译成 Android 可读取的 Dalvik 可执行文件字节码，
且有其自己的 .jack 库格式，在一个工具中提供了大多数工具链功能：重新打包、压缩、模糊化以及 Dalvik 可执行文件分包。

以下是构建 Android Dalvik 可执行文件可用的两种工具链的对比：

旧版 javac 工具链：
javac (.java –> .class) –> dx (.class –> .dex)
新版 Jack 工具链：
Jack (.java –> .jack –> .dex)

官方说明网址：https://developer.android.com/preview/j8-jack.html


*************
proguardFiles这部分有两段，前一部分代表系统默认的android程序的混淆文件，该文件已经包含了基本的混淆声明，免去了我们很多事，
这个文件的目录在 /tools/proguard/proguard-android.txt, 后一部分是我们项目里的自定义的混淆文件，目录就在 app/,默认生成的文件名是 proguard-rules.pro,
这个名字没关系，在这个文件里你可以声明一些第三方依赖的一些混淆规则，最终混淆的结果是这两部分文件共同作用的。