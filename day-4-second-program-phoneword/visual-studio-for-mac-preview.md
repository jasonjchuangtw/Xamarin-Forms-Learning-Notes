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



