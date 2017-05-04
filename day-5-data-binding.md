### Data Binding

以下內容參考 [Part 4. Data Binding Basics](https://developer.xamarin.com/guides/xamarin-forms/xaml/xaml-basics/data_binding_basics/)。

Data Binding是XAML語法中的物件屬性直接關連技術。

相較於WPF/XAML，Windows Form物件屬性無法直接關聯，必須透過event得知property value changed後，將新的屬性值更新到被關聯的物件屬性，但這樣的方式其實，會讓coding變得複雜。WPF/XAML就發展出Data Binding技術，WPF/XAML將UI/UX與Application Logic分離，而XAML中可以透過Data Binding技術關連其他支援Binding的物件屬性。

> Data bindings allow properties of two objects to be linked so that a change in one causes a change in the other. This is a very valuable tool, and while data bindings can be defined entirely in code, XAML provides shortcuts and convenience. Consequently, one of the most important markup extensions in Xamarin.Forms is Binding.



物件主要分兩個：source跟target，



> Data bindings connect properties of two objects, called the_source\_and the\_target_. In code, two steps are required: The`BindingContext`property of the target object must be set to the source object, and the`SetBinding`method \(often used in conjunction with the`Binding`class\) must be called on the target object to bind a property of that object to a property of the source object.
>
> The target property must be a bindable property, which means that the target object must derive from`BindableObject`. The online Xamarin.Forms documentation indicates which properties are bindable properties.
>
> In markup, these same two steps are also required, except that the`Binding`markup extension takes the place of the`SetBinding`call and the`Binding`class.
>
> However, there is no single technique to set the`BindingContext`of the target object. Sometimes it’s set from the code-behind file, sometimes using a`StaticResource`or`x:Static`markup extension, and sometimes as the content of`BindingContext`property-element tags.
>
> Bindings are used most often to connect the visuals of a program with an underlying data model, usually in a realization of the MVVM \(Model-View-ViewModel\) application architecture, as discussed in[Part 5. From Data Bindings to MVVM.](https://developer.xamarin.com/guides/xamarin-forms/xaml/xaml-basics/data_bindings_to_mvvm/)But other scenarios are possible.



