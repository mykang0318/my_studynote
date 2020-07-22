# Android学习

## 目录结构：

> **.gradle和.idea**

Android Studio自动生成的一些文件,不要去手动编辑

>**app**

项目中的代码、资源等内容几乎都是放置在这个目录下的

> **build**

包含了一些在编译时自动生成的文件。

>**gradle 类似php的composer**

这个目录下包含了gradle wrapper的配置文件，使用gradle wrapper的方式不需要提前将gradle下载好，而是会自动根据本地的缓存情况决定是否需要联网下载gradle。Android Studio默认没有启用gradle wrapper的方式，如果需要打开，可以`点击Android Studio导航栏→File→Settings→Build, Execution, Deployment→Gradle`，进行配置更改。

> **.gitignore**

这个文件是用来将指定的目录或文件排除在版本控制之外的

> **build.gradle**

这是项目全局的gradle构建脚本

两处repositories的闭包中都声明了jcenter()这行配置。

它是一个`代码托管仓库`，很多Android开源项目都会选择将代码托管到jcenter上，声明了这行配置之后，我们就可以在项目中轻松引用任何jcenter上的开源项目了。

>**gradle.properties**

全局的gradle配置文件，在这里配置的属性将会影响到项目中所有的gradle编译脚本。

> **gradlew和gradlew.bat**

是用来在命令行界面中执行gradle命令的，其中gradlew是在Linux或Mac系统中使用的，gradlew.bat是在Windows系统中使用的。

> **local.properties**

这个文件用于`指定本机中的Android SDK路径`，通常内容都是自动生成的，不需要修改。除非本机中的Android SDK位置发生了变化，那么就将这个文件中的路径改成新的位置即可。

> **settings.gradle**

这个文件用于指定项目中所有引入的模块。由于HelloWorld项目中就只有一个app模块，因此该文件中也就只引入了app这一个模块。通常情况下模块的引入都是自动完成的，需要我们手动去修改这个文件的场景可能比较少。

### APP目录

> **build**

这个目录和外层的build目录类似，主要也是包含了一些在编译时自动生成的文件，不过它里面的内容会更多更杂

> **libs**

jar包都放在libs目录下，放在这个目录下的jar包都会被自动添加到构建路径里去。

> **src/androidTest**

此处是用来编写Android Test测试用例的，可以对项目进行一些自动化测试。

> **src/main/java**

放置所有Java代码的地方

>**src/main/res**

项目中使用到的所有图片、布局、字符串等资源都要存放在这个目录下。当然这个目录下还有很多子目录，

图片放在drawable目录下，

布局放在layout目录下，

字符串放在values目录下

> **src/main/AndroidManifest.xml**

这是整个Android项目的配置文件，

在程序中定义的所有四大组件都需要在这个文件里注册，

另外还可以在这个文件中给应用程序添加权限声明。

```xml
<activity
    android:name=".MainActivity"
    android:label="@string/app_name"
    android:theme="@style/AppTheme.NoActionBar">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

这段代码表示对MainActivity这个活动进行注册

没有在AndroidManifest.xml里注册的活动是不能使用的。其中`intent-filter里的两行代码非常重要`

MainActivity是这个项目的主活动，在手机上点击应用图标，首先启动的就是这个活动。

> **src/test**

此处是用来编写Unit Test测试用例的，是对项目进行自动化测试的另一种方式。

> **.gitignore**

这个文件用于将app模块内的指定的目录或文件排除在版本控制之外，作用和外层的.gitignore文件类似。

> **build.gradle**

这是app模块的gradle构建脚本，这个文件中会指定很多项目构建相关的配置

```java
apply plugin: 'com.android.application'

android {
    compileSdkVersion 30
    buildToolsVersion "30.0.0"

    defaultConfig {
        applicationId "mykang.com"
        minSdkVersion 16
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
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
    //noinspection GradleCompatible
    implementation 'com.android.support:appcompat-v7:28.0.0'
    //noinspection GradleCompatible
    implementation 'com.android.support:design:28.0.0'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    implementation 'android.arch.navigation:navigation-fragment:1.0.0'
    implementation 'android.arch.navigation:navigation-ui:1.0.0'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'

}
```

**com.android.application**`表示这是一个应用程序模块`

**com.android.library**`表示这是一个库模块`

应用程序模块和库模块的最大区别在于，一个是可以直接运行的，一个只能作为代码库依附于别的应用程序模块来运行。

**compileSdkVersion**`用于指定项目的编译版本`

**buildToolsVersion**`用于指定项目构建工具的版本`

**defaultConfig**`闭包中可以对项目的更多细节进行配置`

​		**applicationId**`用于指定项目的包名`

​		**minSdkVersion**`用于指定项目最低兼容的Android系统版本`

​		**targetSdkVersion**指定的值表示你在该目标版本上已经做过了充分的测试，系统将会为你的应用程序启用一些最		新的功能和特性。

​		**比如说Android 6.0系统中引入了运行时权限这个功能，如果你将targetSdkVersion指定成23或者更高，那么系		统就会为你的程序启用运行时权限功能，而如果你将targetSdkVersion指定成22，那么就说明你的程序最高只		在Android 5.1系统上做过充分的测试，Android 6.0系统中引入的新功能自然就不会启用了。**

**versionCode**`用于指定项目的版本号`

**versionName**`用于指定项目的版本名`

**buildTypes**

闭包中用于指定生成安装文件的相关配置，通常只会有两个子闭包，一个是debug，一个是release。debug闭包用于指定生成测试版安装文件的配置，release闭包用于指定生成正式版安装文件的配置。另外，debug闭包是可以忽略不写的，因此我们看到上面的代码中就只有一个release闭包。

​	**minifyEnabled**`用于指定是否对项目的代码进行混淆`,true表示混淆，false表示不混淆。

​	**proguardFiles**用于指定混淆时使用的规则文件，这里指定了两个文件，第一个proguard-android.txt是在	Android 	SDK目录下的，里面是所有项目通用的混淆规则，第二个proguard-rules.pro是在当前项目的根目录下的，里面可	以编写当前项目特有的混淆规则。需要注意的是，通过Android Studio直接运行项目生成的都是测试版安装文件

**dependencies**闭包

可以指定当前项目所有的`依赖关系`。

通常Android Studio项目一共有3种依赖方式：本地依赖、库依赖和远程依赖。本地依赖可以对本地的Jar包或目录添加依赖关系，库依赖可以对项目中的库模块添加依赖关系，远程依赖则可以对jcenter库上的开源项目添加依赖关系。

> **proguard-rules.pro**

这个文件用于指定项目代码的混淆规则，当代码开发完成后打成安装包文件，如果不希望代码被别人破解，通常会将代码进行混淆，从而让破解者难以阅读。



## 布局

> **线性布局：LinearLayout**

**属性**

```java
   	    android:id="@+id/l1_1"
        android:layout_width="200dp"
        android:layout_height="200dp"
        android:background="#eeeeee"
        android:padding="20dp"
        android:layout_margin="10dp"
        android:gravity="bottom"
        android:orientation="vertical"
        android:layout_weight="1"//权重
```

> **相对布局：RelativeLayout**

**重要属性**

```java
android:layout_toRightOf="@id/view_1"//与xx相对xx布局
```

## 步骤

1. `创建xxxActivity.xml文件（在src/main/res/layout/xxx.xml）`

2. `在src/main/AndroidManifest.xml文件下声明`

   如：

   ```java
   <activity android:name=".TextViewActivity"/>
   ```

3. `在此文件里面写布局、元素（页面）`

4. `创建java文件（在src/main/java/xxx/xxxActivity.class）`

5. `java文件写回调函数`

## 内容

> Button

```java
<Button
android:id="@+id/btn_1"
android:layout_width="match_parent"
android:layout_height="wrap_content"
android:text="TextView" />
```

自定义背景形状

在res/drawable目录下创建文件：xxx.xml

![image-20200711203038381](C:%5CUsers%5Cdell%5CDesktop%5C%E5%AD%A6%E4%B9%A0%5Cupload%5Cimage-20200711203038381.png)

`圆角`

```java
android:background="@drawable/btn1"
```

```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <solid
        android:color="@color/colorOrange"/>
    <corners
        android:radius="15dp"/>
</shape>
```

`描边`

```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <stroke
        android:width="1dp"
        android:color="@color/colorOrange"/>
<!--    <solid-->
<!--        android:color="@color/colorOrange"/>-->
    <corners
        android:radius="15dp"/>
</shape>
```

`自定义按压效果`

```java
android:background="@drawable/btn2"
```

```xml
<selector xmlns:android="http://schemas.android.com/apk/res/android">
        <!--   按压-->
    <item android:state_pressed="true">
        <shape android:shape="rectangle">
            <solid android:color="@color/colorOrange2"/>
            <corners android:radius="15dp"/>
        </shape>
    </item>
    <item android:state_pressed="false">
        <shape android:shape="rectangle">
            <solid android:color="@color/colorOrange"/>
            <corners android:radius="15dp"/>
        </shape>
    </item>
</selector>
```

`点击弹窗`

![image-20200711212024340](upload%5Cimage-20200711212024340.png)

![image-20200711211941432](upload%5Cimage-20200711211941432.png)

![image-20200711222938140](upload%5Cimage-20200711222938140.png)

注意：**

`使用这个可以让Button不是默认的全大写`

![image-20200711213010777](upload%5Cimage-20200711213010777.png)



`点击回调函数`

```java
private Button mBtnTextView;
@Override
    protected void onCreate(Bundle savedInstanceState) {
    	super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mBtnTextView = findViewById(R.id.btn_1);
        mBtnTextView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //跳转
                Intent intent = new Intent(MainActivity.this, TextViewActivity.class);
                startActivity(intent);
            }
        });
}
```

>TextView

```java
<TextView
    android:id="@+id/view2"
    android:layout_width="50dp"
    android:layout_height= "wrap_content"
    android:maxLines="1"
    android:ellipsize="end" //超出部分用...表示
    android:text="@string/app_name2"
    android:textColor="#000000"
    android:textSize="20sp"/> //字体大小用sp
        
```

```java
private TextView mTv4;
private TextView mTv5;
private TextView mTv6;
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_text_view);
    mTv4 = findViewById(R.id.view4);
    mTv4.getPaint().setFlags(Paint.STRIKE_THRU_TEXT_FLAG);//中划线
    mTv4.getPaint().setAntiAlias(true);//去除锯齿

    mTv5 = findViewById(R.id.view5);
    mTv5.getPaint().setFlags(Paint.UNDERLINE_TEXT_FLAG);//下划线

    mTv6 = findViewById(R.id.view6);
    mTv6.setText(Html.fromHtml("<u>hello addroid</u>"));
}
```

**文字+图片：**

```java
android:drawableRight="@drawable/user"
```

**中划线：**

```java
mTv4 = findViewById(R.id.view4);
mTv4.getPaint().setFlags(Paint.STRIKE_THRU_TEXT_FLAG);//中划线
mTv4.getPaint().setAntiAlias(true);//去除锯齿
```

**下划线：**

​	**方法1**

```java
mTv5 = findViewById(R.id.view5);
mTv5.getPaint().setFlags(Paint.UNDERLINE_TEXT_FLAG);//下划线
```

​	**方法2**

```java
mTv6 = findViewById(R.id.view6);
mTv6.setText(Html.fromHtml("<u>hello addroid</u>"));
```

**跑马灯：**

`需要下面这些`

```java
android:ellipsize="marquee"
android:singleLine="true"
android:marqueeRepeatLimit="marquee_forever"
android:focusable="true"
android:focusableInTouchMode="true"
```

> RadioButton

**注意：**必须写ID，没有id，不生效！

![image-20200712100451722](upload%5Cimage-20200712100451722.png)

**按钮式单选框**

![image-20200712101638227](upload%5Cimage-20200712101638227.png)

**监听事件：**

![image-20200712101846077](upload%5Cimage-20200712101846077.png)

> **CheckBox**

![image-20200712103626888](upload%5Cimage-20200712103626888.png)

**监听事件：**

![image-20200712104019560](upload%5Cimage-20200712104019560.png)

> **其他button常用衍生空**

**ToggleButton、Switc**

> **ImageView**

![image-20200712105522187](upload%5Cimage-20200712105522187.png)

> **使用第三方库加载网络图片**

![image-20200712112252836](upload%5Cimage-20200712112252836.png)

![image-20200712112214880](upload%5Cimage-20200712112214880.png)

**错误提示：**

`This project uses AndroidX dependencies, but the 'android.useAndroidX' property is not enabled. Set this property to true in the gradle.properties file and retry.`

**解决办法：**

在gradle.properties里面添加`android.useAndroidX=true android.enableJetifier=true`

**使用：**

![image-20200712115511505](upload%5Cimage-20200712115511505.png)



>ListView（列表视图）



>GridView（网格视图）



> **ScrollView & HorizontalScrollView**

`子元素只有一个`

> **RecyclerView**