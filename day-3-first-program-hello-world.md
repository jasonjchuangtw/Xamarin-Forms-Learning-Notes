# 第一支程式

啟動`Visual Studio for Mac`，圖示如下：

![](https://msdnshared.blob.core.windows.net/media/2017/03/vs-for-mac-logo-caption2.png)

啟動畫面如下：![](/images/Day3/01.jpg)

在menu bar點選`Visual Studio`的`Check for Update`，啟動畫面如下：

![](/images/Day3/02.jpg)

選擇`Restart and Install Updates`，畫面如下：

![](/images/Day3/03.jpg)

選擇`Multiplatform`的`Blank Forms App`，啟動`App Wizard`：

![](/images/Day3/04.jpg)

`App Name`跟`Organization identifier`要輸入，就Next：

![](/images/Day3/05.jpg)

出現`Visual Studio for Mac`主畫面及`Getting Started`頁面如下：

![](/images/Day3/06.jpg)

有幾個重點：

1. `Open Xamarin.Forms Previewer`：可以透過這個預覽XAML，但沒有`Drag and Draw`拖拉的效果，僅預覽。
2. `Add a new XAML ContentPage`：用以新增頁面。
3. 在`Solution Pad`中有一個方案下包含三個專案，分別是`HelloWorld`、`HelloWorld.Droid`及`HelloWorld.iOS`，值得一提的是`HelloWorld`是被另外兩個專案所參考。

觀察`HelloWorld.iOS`的`Main.cs`：

```csharp
namespace HelloWorld.iOS
{
    public class Application
    {
        // This is the main entry point of the application.
        static void Main(string[] args)
        {
            // if you want to use a different Application Delegate class from "AppDelegate"
            // you can specify it here.
            UIApplication.Main(args, null, "AppDelegate");
        }
    }
}
```

可以發現會去呼叫`AppDelegate`類別：

```csharp
namespace HelloWorld.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init();

            LoadApplication(new App());

            return base.FinishedLaunching(app, options);
        }
    }
}
```

`AppDelegate`類別在`FinishedLaunching`事件中，透過`FormsApplicationDelegate.LoadApplication`方法啟動Xamarin.Forms。

觀察`HelloWorld.Droid`專案中的`MainActivity.cs`，

```csharp
namespace HelloWorld.Droid
{
    [Activity(Label = "HelloWorld.Droid", Icon = "@drawable/icon", Theme = "@style/MyTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle bundle)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(bundle);

            global::Xamarin.Forms.Forms.Init(this, bundle);

            LoadApplication(new App());
        }
    }
}
```



