## 第一章

#### 创建第一个Android项目

Name:表示项目名称；

Package name ：表示项目的包名，

Save location :表示项目代码存放的位置

#### 分析第一个项目

##### project下的文件目录

一开始现实的项目结构是Android，但真正的项目结构是project

1、.gradle和.idea

这两个目录下放的是Android Studio自动生成的文件.

2、app

项目中的代码、资源等内容都是放在这个目录下的，包括开发内容。

3、build

包含了一些在编译时自动生成的文件。

4、gradle

这个项目包含了gradle wrapper的配置文件。Android Studio默认就是启用gradle wrapper方式的，如果需要更改成 离线模式，可以点击Android Studio 导航栏，—File—Setting—Build，Execution，Deployment—Gradle。

5、.gitignore 

这个文件用于指定目录或文件排除在版本控制之外的。

6、build.gradle

这是项目全局的gradle构建脚本，通常这个文件中的内容是不需要修改的。

7、gradle.properties

这个文件是全局的gradle配置文件在这里配置的属性将会影响到项目中所有的gradle编译脚本.

8、gradlew和gradlew.bat

这两个文件是在命令行界面中指向gradle命令的。前者是在Linux或Mac系统中用的，后者用于Windows

9、HelloWorld.iml

iml文件是所有IntelliJ IDEA项目都会自动生成的一个文件

10、local.properies

这个文件用于 指定本机中的Android SDK路径，通常是自动生成的。

11、settings.gradle

这个文件用于指定项目中所有引入的模块。

##### app目录下的内容

1、build

这个和外层的build类似，包含了编译时自动生成的文件，不用管 

2、libs

如果用到了jar包，就把jar包，放在libs目录下，他会自动添加到项目的构建路径中。

3、AndroidTest

用来编写Android Test测试用的，可以对项目进行自动化测试。

4、java

这就是放Java代码的地方，（Kotlin代码也放在这）

5、res

这个目录下东西可多了，图片，布局，字符串等资源都放在这个目录下，它里面还有细分。

6、AndroidManifest.xml

这是整个Android项目的配置文件，你在程序中定义的四大组件都在这个文件里注册，还可以在这个文件中给应用程序添加 权限声明。以后会用到。

7、test

用来编写Unit Test测试用的。

8、.gitignore

这个文件用于将app模块内指定目录或文件排除在版本控制之外，和外层的.gitignore作用一样.

9、app.iml

IntelliJ IDEA项目自动生成文件。

10、这是app模块的gradle构建脚本。会指定很多项目构建相关的配置。

11、proguard-rules.pro

用于指定项目代码的混淆规则，当代码开发完成，安装打包时，不希望被别人破解，就会将代码混淆。

##### 如何运转起来

```xml
<intent-filter>
    <action android:name="android.intent.action.MAIN" />

                    <categoryandroid:name="android.intent.category.LAUNCHER" /> </intent-filter>
```

以上代码是对MainActivity进行注册。没用在AndroidManifest.xml中注册是不能使用的。

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
}
```

**MainActivity**是继承自App CompatActivity的。Activity类是Android系统提供的一个基类。我们项目中所有自动逸的Activity都必须继承或者它的子类才能拥有Activity的特性。

在**onCreate**的第二行调用了**setContentView（）**方法。，这个方法给当前的activity引入了一个布局。HelloWorld的字样是在布局中找到的。

##### 详解res目录

res目录中有许多文件，其中以：drawable开头的目录都是用来放图片的 。所有以mipmap开头的目录都是用来放图标的，所有以values开头的目录都是用来放字符串、样式、颜色等配置的。所有以layout开头的目录都是用来放布局文件的。

如何使用这些资源

```xml
<resources>
    <string name="app_name">HelloWorld</string>
</resources>
```

看到了这里定义了一个应用程序名的字符串。

在代码中通过R.string.app_name 可以获得该字符串的引用

在xml中通过@string/app_name可以获得该字符串的引用

基本语法如上，其中string是可以替换的，比如说要引用图片资源，可以换成drawable,要引用啥，就换啥。

##### 详解build.grade文件

```
buildscript {
    ext.kotlin_version = "1.3.72"
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:4.0.1"
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        google()
        jcenter()
    }
}
```

其中两处**repositories**的闭包都声明了Google（）和jcenter（）这两行配置，它们分别对应了一个代码仓库，google仓库中包含的主要是Google自家的扩展依赖库。而jcenter仓库中包含的大多数是一些第三方的开源库。

**dependencies闭包**中使用两个**classpath**声明了两个插件，Gradle插件并不是专门为构建Android项目而 开发的，所以我们要声明是用来写Android的。而kotlin插件表示当前项目是使用kotlin进行开发的。

注：闭包就是指一个读取其他函数中内部变量的函数。

```
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'

android {
    compileSdkVersion 30
    buildToolsVersion "30.0.2"

    defaultConfig {
        applicationId "com.example.helloworld"
        minSdkVersion 21
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(dir: "libs", include: ["*.jar"])
    implementation "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
    implementation 'androidx.core:core-ktx:1.1.0'
    implementation 'androidx.appcompat:appcompat:1.1.0'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'androidx.test.ext:junit:1.1.1'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0'

}
```

第一行应用了一个插件，一般有两种插件可选:com.android.application表示这是一个应用程序模块，com.android.library表示这是一个库模块。前者可以直接运行，而后者只能作为代码库依附在别的应用程序模块来运行。

```
kotlin-android'
kotlin-android-extensions
```

这两个插件，前者用kotlin开发Android，就必须要用，而 第二个插件帮助我们实现了一些非常好用的kotlin扩展功能。

**defaultConfig闭包**，接下来我们看一下，buildTypes闭包。其中，applicationId是每个应用的唯一标识符，minSdkVersion用于指定项目最低兼容的Android系统版本，targetSdkVersion指定的值表示你在该项目版本上已经做过了充分的测试。

**buildTypes闭包**：这个用于指定生成安装文件的相关配置，通常有两个子闭包，一个debug，一个release。debug闭包用于指定生成测试版安装文件的配置，release闭包用于指定生成正式版安装文件的配置。另外，debug可以忽略不屑。release闭包内，minfyEnabled用于指定是否对代码混淆，true表示混淆，false表示不混淆。proguardFiles 用于指定混淆时使用的规则文件，这里指定了两个文件：第一个proguard-android-optimize.txt用于所有项目通用的混淆规则，第二个proguard-rules.pro是当前目录下的，里面可以编写当前项目特有的混淆规则。

dependencies闭包，功能非常强大，它可以指定当前项目的所有的依赖关系。第一行的implementation fileTree是一个本地依赖声明。它表示 将libs目录下所有的.jar后缀的文件全部添加到项目的构建路径中。而implementation则是远程依赖声明。androidx.appcompat:appcompat:1.1.0就是一个标准的远程依赖库格式，其中androidx.appcompat就是域名部分，