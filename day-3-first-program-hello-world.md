# 第一支程式

啟動`Visual Studio for Mac`，圖示如下：

![](https://msdnshared.blob.core.windows.net/media/2017/03/vs-for-mac-logo-caption2.png)

啟動畫面如下：![](/images/Day3/01.jpg)

在menu bar點選`Visual Studio`的`Check for Update`，啟動畫面如下：

![](/images/Day3/02.jpg)

選項中有[Portable Class Libraries](https://msdn.microsoft.com/en-us/library/gg597391%28v=vs.100%29.aspx)跟Shared Library，這個技術上差異另外討論。

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

`MainActivity`類別在`OnCreate`事件中呼叫`LoadApplication`方法啟動`Ｘamarin.Forms`。

觀察`HelloWorld`專案中的`App.xaml.cs`，如下：

```csharp
using Xamarin.Forms;

namespace HelloWorld
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();

            MainPage = new HelloWorldPage();
        }
        ...
    }
}
```

在設定`MainPage`屬性為`HelloWorldPage`物件，就這點跟WPF有差異，WPF是透過`App.xaml`中的`Application`頁籤中設定。

由上可以初步得知，雖然都是基於XAML技術，但WPF跟Xamarin.Forms還是有差異。

觀察專案``HelloWorld``的``HelloWorldPage.xaml``內容：

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:HelloWorld" x:Class="HelloWorld.HelloWorldPage">
	<Label Text="Welcome to Xamarin Forms!" VerticalOptions="Center" HorizontalOptions="Center"/>
</ContentPage>
```

修改``<Label Text=""/>``內容為``"Hello, World."``。

分別對``HelloWorld.iOS``及``HelloWorld.Dorid``除錯：

![](/images/Day3/11.jpg) ![](/images/Day3/12.jpg)

Simulator畫面如下：

![](/images/Day3/10.jpg) ![](/images/Day3/13.jpg)



