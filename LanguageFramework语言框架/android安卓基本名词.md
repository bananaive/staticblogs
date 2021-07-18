## Jitpack
>概念:一个类似于gitee的仓库,里面存有许多好用的**框架**(类似于python的第三方库)  
>可以通过某种引用方式来引用到项目中,实现某种功能(好用程度类似于pip)
## CustomActivityOnCrash库(在营回贯通中被弃用了)
>似乎是一个崩溃拦截的库,避免以下情况的发生
![这里是一个安卓程序崩溃的截图][pic_crash]
## multidex.MultiDex
>为了解决超过65536个方法的大包无法打包的问题，引入了google官方的解决方案，就是上者  
## LoadSir
>LoadSir是一个高效易用，低碳环保(~~绝了~~)，扩展性良好的加载反馈页管理框架，在加载网络或其他数据时候，根据需求切换状态页面，可添加自定义状态页面，如加载中，加载失败，无数据，网络超时，如占位图，登录失效等常用页面。可配合网络加载框架，结合返回状态码，错误码，数据进行状态页自动切换，封装使用效果更佳。。
## 以下是本项目对应的大部分技术
>### 'com.tencent:mmkv:1.0.22'  
>微信开源项目，替代SP  
>### 'me.hegj:JetpackMvvm:1.1.4'  
>项目核心框架  
>一个Jetpack结合MVVM的快速开发框架，基于MVVM模式集成谷歌官方推荐的JetPack组件库：LiveData、ViewModel、Lifecycle组件 使用Kotlin语言，添加大量拓展函数，简化代码 加入Retorfit网络请求,协程，帮你简化各种操作，让你快速开发项目.[这里是文档][web-address]  
>### 'com.github.CymChad:BaseRecyclerViewAdapterHelper:3.0.4'  
>BaseAdapter  
>### 'com.github.bumptech.glide:glide:4.11.0'  
>glide  
>### 'me.jessyan:autosize:1.2.1'  
>屏幕适配  
>### 'com.kingja.loadsir:loadsir:1.3.6'  
>管理界面状态库  
>### 'com.tapadoo.android:alerter:4.0.2'  
>alerter  
>### 'com.github.SherlockGougou:BigImageViewPager:androidx-6.0.1'  
>查看大图  
### 'com.yanzhenjie:permission:2.0.3'  
AndPermission  
### 'top.zibin:Luban:1.1.8'  
LuBan  
### 'com.flyco.tablayout:FlycoTabLayout_Lib:2.1.2@aar'  
FlycoTabLayout_Lib  
### 'cn.bingoogolapple:bga-qrcode-zbar:1.3.7'  
zbar BGA 扫码  
### 'com.qianwen:update-app:3.5.2'  
### 'com.qianwen:update-app-kotlin:1.2.3'  
### 'com.qianwen:okhttp-utils:3.8.0'  
版本更新  
### "com.contrarywind:Android-PickerView:4.1.9"  
时间日期选择器  
### "androidx.room:room-runtime:2.2.5"  
Room是一个对象关系映射(ORM)库。Room抽象了SQLite的使用，可以在充分利用SQLite的同时访问流畅的数据库。  
### "androidx.room:room-compiler:2.2.5"  
kapt 
### "androidx.room:room-guava:2.2.5"  
 optional - Guava support for Room, including Optional and ListenableFuture  
## 核心框架中Activity相关  
通用知识
>**activity的生命周期**  
>oncreate()->onstart()->onResume()->onRestart()->onPouse()->onStop()->onDestory() 
```
一个常见的activity
abstract class BaseActivity<VM : BaseViewModel, DB : ViewDataBinding> : BaseVmDbActivity<VM, DB> () {
    abstract override fun layoutId(): Int
    abstract override fun initView(savedInstanceState: Bundle?)
    override override fun createObserver()
    override fun showLoading(message: String) {
       ...
    }
    override fun dismissLoading() {
       ...
    }
}
```
#### 当前Activity绑定的视图布局Id abstract修饰供子类实现
``abstract override fun layoutId(): Int``
#### 当前Activityc创建后调用的方法 abstract修饰供子类实现
``abstract override fun initView(savedInstanceState: Bundle?)``
#### 创建liveData数据观察
``override override fun createObserver()``
#### 打开等待框 在这里实现你的等待框展示
```
override fun showLoading(message: String) {
       ...
    }
```
#### 关闭等待框 在这里实现你的等待框关闭
```
override fun dismissLoading() {
    ...
}
```
#### 用户点击Back键时调用的方法
```
override fun onBackPressed() {
    ...
}
```
#### 某种请求结束之后的回调函数?
```
override fun onFinish(finish: Boolean) {
    ....
}
```
## 一些杂乱的知识点
* MVVM模式  
  分为三个部分  
  1. View  
   用户界面  
   一般的路径是``app/src/main/res/layout``  
   res下的其他路径也是可以引用的界面或者图片、字体什么的资源
  2. Model  
   数据获取、逻辑处理等部分
  3. ViewModel  
   负责与其他二者相互沟通，通过双向绑定的概念，无论是View还是Model中数据的变化，都可以在对方响应  
* View的界面-XML语言  
  一个标签就是一个页面元素，其中的属性就是对标签的定位、id什么的
  * 数据绑定    
    ```
    <data>
    <variable name="user" type="com.example.User"/>
    </data>
    ```  
    上面这种写法是由view到model单向的么?~~非常有可能~~  
    **在Model中获取用户界面的数据**   
    ``findViewById``
  * 约束布局  
    通过其他元素去固定该元素的位置  
    写法就是在标签的属性中通过其他元素的id与其绑定,从而确定该元素与其的相对位置  
* 数据库链接部分
  文件名``AppDatabase_Impl.java``   
  ```
    // 杆塔表
    CREATE TABLE IF NOT EXISTS `upload_gt` (
        `taskId` TEXT NOT NULL,//主键
        `dsId` INTEGER NOT NULL,
        `dsName` TEXT NOT NULL,
        `gtName` TEXT NOT NULL,//杆塔名称 
        `gtNo` TEXT NOT NULL,//杆塔号
        `qId` INTEGER NOT NULL, 
        `tqName` TEXT NOT NULL,//台区名称
        `userId` TEXT NOT NULL,//用户id
        `jd` REAL NOT NULL,//经度
        `wd` REAL NOT NULL,//纬度
        `dwbm` TEXT NOT NULL, 
        `dd` TEXT NOT NULL, 
        `photos` TEXT NOT NULL,//照片名称
        PRIMARY KEY(`taskId`)
        //未确定的字段:管理单位,
        )
    // 计量箱表
    CREATE TABLE IF NOT EXISTS `upload_jlx` (
        `taskId` TEXT NOT NULL,//主键 
        `column` INTEGER NOT NULL,//列?
        `dsId` INTEGER NOT NULL, 
        `dsName` TEXT NOT NULL, 
        `jd` REAL NOT NULL,//经度 
        `wd` REAL NOT NULL,//纬度 
        `jlxName` TEXT NOT NULL,//计量箱名称 
        `row` INTEGER NOT NULL,//行 
        `tableNum` INTEGER NOT NULL,//表的数量 
        `tqId` INTEGER NOT NULL,//台区id 
        `tqName` TEXT NOT NULL,//台区名称 
        `userId` TEXT NOT NULL,//用户id 
        `dwbm` TEXT NOT NULL, 
        `jlxbh` TEXT NOT NULL,// 计量箱编号 
        `jlxtm` TEXT NOT NULL,// 计量箱条码 
        `dd` TEXT NOT NULL, 
        `photos` TEXT NOT NULL,//照片的名称
        //未找到的字段:管理单位,变压器类型,上级设备
        PRIMARY KEY(`taskId`)
        )
    CREATE TABLE IF NOT EXISTS room_master_table (
        id INTEGER PRIMARY KEY,//外键
        identity_hash TEXT//标识_哈希
        )
    INSERT OR REPLACE INTO room_master_table (id,identity_hash) 
    VALUES
    (42, 'dd2a02dce826f68a142b61739ba28042')
  ```
* TypeConverter自定义类型转换器   
* 数据库配置文件路径
  ``C:\Users\summe\Documents\work-ypgt\ypgt\src\main\resources``
* 安卓上传or下载文件   
  okhttp3
* 网络接口API相关文件  
  ApiServer.kt
  ```
  interface ApiService {

    companion object {
         const val SERVER_URL = "http://139.159.209.36:30001/ypgt/app/"
    }

    /**
     * 登录
     * /app/login?userName=admin&passWord=123456
     */
    @POST("login")
    suspend fun login(@Query("userName") username: String, @Query("passWord") passWord: String): ApiResponse<UserInfo>

    /**
     * 高德天气(即时)
     * https://restapi.amap.com/v3/weather/weatherInfo?key=d61f7d93e1608c46a7bf8851e321a720&city=140107&extensions=base
     */
    @GET("https://restapi.amap.com/v3/weather/weatherInfo")
    suspend fun baseWeather(@Query("key") key: String,
                            @Query("city") passWord: String,
                            @Query("extensions")extensions:String): WeatherResponse<List<LiveWeather>>


    /**
     * 高德天气(几天)
     * https://restapi.amap.com/v3/weather/weatherInfo?key=d61f7d93e1608c46a7bf8851e321a720&city=140107&extensions=all
     */
    @GET("https://restapi.amap.com/v3/weather/weatherInfo")
    suspend fun allWeather(@Query("key") key: String,
                            @Query("city") passWord: String,
                            @Query("extensions")extensions:String): WeatherResponse<List<AllWeather>>

    /**
     * 杆塔首页列表
     */
    //    @Headers("userId:1")
    @GET("gt/info")
    suspend fun getWatchBoxList(): ApiResponse<MutableList<TowerBean>>

    /**
     * 杆塔详情
     */
    //    @Headers("userId:1")
    @GET("gt/detail")
    suspend fun getWatchBoxDetails(@Query("id")id:String): ApiResponse<GanTaDetailBean>


    /**
     * 计量箱首页列表
     */
    //    @Headers("userId:1")
    @GET("jlx/info")
    suspend fun getMeteringBoxList(): ApiResponse<MutableList<TowerBean>>

    /**
     * 计量箱详情
     */
    //    @Headers("userId:1")
    @GET("jlx/detail")
    suspend fun getMeteringBoxDetails(@Query("id")id:String): ApiResponse<JiliangxDetailBean>


    /**
     * 删除杆塔或者计量箱
     */
    //    @Headers("userId:1")
    @DELETE("jlxAndGt/del")
    suspend fun delete(@Query("id")id:String,@Query("type")type:String): ApiResponse<String>

    /**
     *计量箱数据上传
     */
    //    @Headers("userId:1")
    @Multipart
    @POST("jlx/insertJlxInfo")
    suspend fun updateJlxPhotos(
            @Query("column")column:Int,                             //列  | query integer
            @Query("dsId")dsId:Int,                             //地市Id  | query integer
            @Query("dsName")dsName:String,              //地市Name  | query string
            @Query("jd")jd:Double,                              //经度  | query number
            @Query("wd")wd:Double,                              //维度  | query number
            @Query("jlxName")jlxName:String,                //计量箱名称  | query string
            @Query("row")row:Int,                                   //行  | query integer
            @Query("tableNum")tableNum:Int,                 //表位数  | query integer
            @Query("tqId")tqId:Int,                             //台区id  | query integer
            @Query("tqName")tqName:String,                //台区名称  | query string
            @Query("userId")userId:String,                  //用户id  | query string
             @Query("dwbm")dwbm:String,                  // dwbm 单位编码 query
            @Query("jlxbh")jlxbh:String,                      // jlxbh  计量箱编号
            @Query("jlxtm")jlxtm:String,                      //jlxtm  计量箱条码
            @Query("dd")dd:String,                      //地址
            @Part files: List<MultipartBody.Part>
    //    @Query("dnbFiles")dnbFiles,//电能表img  | formData ref
    //    @Query("jlxQmFilejlxQmFiles,//计量箱img  | formData ref
    ):ApiResponse<UpdateFileResponse>


    /**
     * 杆塔数据上传
     */
    //    @Headers("userId:1")
    @Multipart
    @POST("gt/insertGtInfo")
    suspend fun updateGtPhotos(
        @Query("dsId")dsId: Int,               //地市Id   |     query        |       false      | integer
        @Query("dsName")dsName: String,               //地市Name   |     query        |       false      | string
        @Query("gtName")gtName: String,               //杆塔名称   |     query        |       false      | string
        @Query("gtNo")gtNo: String,               //杆塔号   |     query        |       false      | string
        @Query("tqId")tqId: Int,               //台区编码   |     query        |       false      | integer
        @Query("tqName")tqName: String,               //台区名称   |     query        |       false      | string
        @Query("userId")userId: String,               //用户id   |     query        |       false      | string
        @Query("jd")jd: Double,               //经度   |     query        |       false      | number
        @Query("wd")wd: Double,               //维度   |     query        |       false      | number
        @Query("dwbm")dwbm: String,               //管理单位编码
        @Query("dd")dd: String,               //管理单位编码
        @Part files: List<MultipartBody.Part>
    //    gtQmFiles:String,               //杆塔全貌img   |     formData        |       false      | ref
    //    gtTtFiles:String,               //杆塔塔头img   |     formData        |       false      | ref
    //    gtMpFiles:String,               //杆塔铭牌img   |     formData        |       false      | ref
    ):ApiResponse<UpdateFileResponse>


    /**
     * 地图点
     */
    @GET("map/jlxAndGt/info")
    suspend fun selectMapMark():ApiResponse<List<MarkMapBean>>


    /**
     * 组织机构展示
     */
    @GET("depart/info")
    suspend fun selectDepart(@Query("dwbm")dwbm:String):ApiResponse<List<DepartBean>>
    }
  ```





















[pic_crash]: https:img-blog.csdn.net/20170629094312753?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGk1MzA4OTM4NTA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast
[web-address]: https://gitee.com/hegaojian/JetpackMvvm