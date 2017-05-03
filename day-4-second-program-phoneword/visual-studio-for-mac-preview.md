新增一個專案名為`Phoneword`，並寫新增一個檔案`MainPage`，選擇`Forms ContentPage Xaml`。

![](/images/Day4/01.jpg)

修改檔案 `MainPage.xaml` 內容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Phoneword.MainPage">
     <ContentPage.Padding>
        <OnPlatform x:TypeArguments="Thickness"
                    iOS="20, 40, 20, 20"
                    Android="20, 20, 20, 20"
                    WinPhone="20, 20, 20, 20" />
     </ContentPage.Padding>
    <ContentPage.Content>
        <StackLayout VerticalOptions="FillAndExpand"
                     HorizontalOptions="FillAndExpand"
                     Orientation="Vertical"
                     Spacing="15">
            <Label Text="Enter a Phoneword:" />
            <Entry x:Name="phoneNumberText" Text="1-855-XAMARIN" />
            <Button x:Name="translateButon" Text="Translate" Clicked="OnTranslate" />
            <Button x:Name="callButton" Text="Call" IsEnabled="false" Clicked="OnCall" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

修改檔案 `MainPage.xaml.cs` 內容如下：

```csharp
namespace Phoneword
{
    public partial class MainPage : ContentPage
    {
        string translatedNumber;

        public MainPage ()
        {
            InitializeComponent ();
        }

        void OnTranslate (object sender, EventArgs e)
        {
            translatedNumber = Core.PhonewordTranslator.ToNumber (phoneNumberText.Text);
            if (!string.IsNullOrWhiteSpace (translatedNumber)) {
                callButton.IsEnabled = true;
                callButton.Text = "Call " + translatedNumber;
            } else {
                callButton.IsEnabled = false;
                callButton.Text = "Call";
            }
        }

        async void OnCall (object sender, EventArgs e)
        {
            if (await this.DisplayAlert (
                    "Dial a Number",
                    "Would you like to call " + translatedNumber + "?",
                    "Yes",
                    "No")) {
                var dialer = DependencyService.Get<IDialer> ();
                if (dialer != null)
                    dialer.Dial (translatedNumber);
            }
        }
    }
}
```

修改 `App.xaml.cs` ，使得 `MainPage` 指向 `MainPage` ，如下：

```cs
MainPage = new MainPage();
```

新增 `PhoneTranslator.cs` ，並修改內容如下：

```cpp
using System.Text;

namespace Core
{
    public static class PhonewordTranslator
    {
        public static string ToNumber(string raw)
        {
            if (string.IsNullOrWhiteSpace(raw))
                return null;

            raw = raw.ToUpperInvariant();

            var newNumber = new StringBuilder();
            foreach (var c in raw)
            {
                if (" -0123456789".Contains(c))
                    newNumber.Append(c);
                else
                {
                    var result = TranslateToNumber(c);
                    if (result != null)
                        newNumber.Append(result);
                    // Bad character?
                    else
                        return null;
                }
            }
            return newNumber.ToString();
        }

        static bool Contains(this string keyString, char c)
        {
            return keyString.IndexOf(c) >= 0;
        }

        static readonly string[] digits = {
            "ABC", "DEF", "GHI", "JKL", "MNO", "PQRS", "TUV", "WXYZ"
        };

        static int? TranslateToNumber(char c)
        {
            for (int i = 0; i < digits.Length; i++)
            {
                if (digits[i].Contains(c))
                    return 2 + i;
            }
            return null;
        }
    }
}
```

新增`IDialer.cs`，並修改內容如下：

```csharp
namespace Phoneword
{
    public interface IDialer
    {
        bool Dial(string number);
    }
}
```

新增`PhoneDialer.cs`，並修改內容如下：

```csharp
using Foundation;
using Phoneword.iOS;
using UIKit;
using Xamarin.Forms;

[assembly: Dependency(typeof(PhoneDialer))]
namespace Phoneword.iOS
{
    public class PhoneDialer : IDialer
    {
        public bool Dial(string number)
        {
            return UIApplication.SharedApplication.OpenUrl (
                new NSUrl ("tel:" + number));
        }
    }
}
```

新增`PhoneDialer.cs`，並修改內容如下：

```csharp
using Android.Content;
using Android.Telephony;
using Phoneword.Droid;
using System.Linq;
using Xamarin.Forms;
using Uri = Android.Net.Uri;

[assembly: Dependency(typeof(PhoneDialer))]
namespace Phoneword.Droid
{
    public class PhoneDialer : IDialer
    {
        public bool Dial(string number)
        {
            var context = Forms.Context;
            if (context == null)
                return false;

            var intent = new Intent (Intent.ActionCall);
            intent.SetData (Uri.Parse ("tel:" + number));

            if (IsIntentAvailable (context, intent)) {
                context.StartActivity (intent);
                return true;
            }

            return false;
        }

        public static bool IsIntentAvailable(Context context, Intent intent)
        {
            var packageManager = context.PackageManager;

            var list = packageManager.QueryIntentServices (intent, 0)
                .Union (packageManager.QueryIntentActivities (intent, 0));

            if (list.Any ())
                return true;

            var manager = TelephonyManager.FromContext (context);
            return manager.PhoneType != PhoneType.None;
        }
    }
}
```

因為需要撥打電話，在Andorid需要使用者enable該功能所以在Android專案開啟選項如下：

![](/images/Day4/02.jpg)

編譯程式確認是否有問題。

