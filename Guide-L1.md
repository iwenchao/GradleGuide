【Groovy简介】

由于Gradle使用Groovy作为默认的编译语言，所以在学习Gradle之前，可以简单的了解一下Groovy的一些简单语法会使得看gradle文件不那么难以理解。

Groovy特点

1.Groovy和Java一样，源码都会被编译成class字节码文件然后可以在JVM中运行；
2.Groovy兼容所有的Java语法，也就是说，你可以在.groovy中直接写Java代码，编译运行；
3.Groovy中自定义变量和方法都使用关键字 【def】 来定义；
4.闭包（注意，这将是在Gradle配置中经常使用的）



【Gradle】

1.在Gradle中，
每个Project都对应一个build.gradle文件；
而buidl.gralde中定义了每个Project所需要执行的Task；
而这些Task是由具体的插件(Plugin)决定的。

1.1修改源集目录
sourceSets {
        main {
              manifest.srcFile 'AndroidManifest.xml'
              java.srcDirs = ['src']
              resources.srcDirs = ['src']
              aidl.srcDirs = ['src']
              renderscript.srcDirs = ['src']
              res.srcDirs = ['res']
              assets.srcDirs = ['assets']
        }
        // Move the tests to tests/java, tests/res, etc...    instrumentTest.setRoot('tests')
        // Move the build types to build-types/<type>
        // For instance, build-types/debug/java, build-types/debug/AndroidManifest.xml, ...
        // This moves them out of them default location under src/<type>/... which would
        // conflict with src/ being used by the main source set.
        // Adding new build types or product flavors should be accompanied
        // by a similar customization.
        debug.setRoot('build-types/debug')
        release.setRoot('build-types/release')
        androidTest.setRoot('test')
  }


2.任务Task

2.1 gradle的android插件使用了java基础插件，而java基础插件又使用了基础插件。
基础插件给出了Task的标准生命周期和一些共同约定的属性，比如assemnble,clean任务
java基础插件给出了check和build任务。基础插件中，这些任务既不被实现，也不执行任何操作。

assemble:项目的输出
clean:清理项目的输出
check:运行所有检查，通常是用作单元测试和集成测试
build:同时运行assemble和check

2.2 android任务
android插件实现了并扩展了基础插件；
assemble:为每个构建版本创建一个apk；assemble任务依赖与assembleDebug，assembleRelease等构建类型，运行assemble意味着要触发每一个构建类型任务
clean:删除所有构建内容
check:运行Lint检查
build:同时运行assemble和check


3.依赖仓库
源
repositories {
     jcenter() 优先
     mavenCentral()
     mavenLocal()
     //企业
     maven{
           url "http://repo.company.com/maven"
           credentials{//增加权限用户
                user 'xxx'
                password 'YYY'
           }
     }
}
依赖库
格式：  compile 'group.name:version'
或者   compile group：'com.google.gson', name :'gson',version:'xxx'

其中 version的命名规范一般是 major.minor.patch,
当做不兼容API变化时，major增加
当以向后兼容的方式添加功能时，minor增加
当修复一些bug时，patch增加

dependencies {

   //库依赖
    compile 'com.android.support:appcompat-v7:22.2.0'
   //文件依赖
    compile file('libs/xxx.jar')
   //某个目录下，某类文件的依赖
    compile fileTree(dir: 'libs', include: ['*.jar'])
  //工程依赖
    compile project(':projectName')
 }


4.构建类型
gradle的android插件中，构建类型buildTypes通常被用来定义如何构建一个应用或依赖库。每个构建类型都可个性化定制

5.productFlavor
gradle的android插件中，productFlavor通常被用来创建不同版本，比如免费，收费；还可以针对不同版本构建代码源集


6.构建variant
构建variant是构建类型和productFlavor结合的结果