# Xamarin.Forms.MVVMBase

Simple MVVM framework for Xamarin.Forms projects

###### This is the component, works on iOS, Android and UWP.

**NuGet**

|Name|Info|
| ------------------- | :------------------: |
|Xamarin.Forms.MVVMBase|[![NuGet](https://buildstats.info/nuget/Xamarin.Forms.MVVMBase)](https://www.nuget.org/packages/Xamarin.Forms.MVVMBase/)|

**Platform Support**

Xamarin.Forms.MVVMBase is a .NET Standard 2.0 library.Its dependencies are Xamarin.Forms and DryIoc

## Initialize

App.cs :

```csharp

 public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            BuildDependencies();
            InitNavigation();
        }

        public void BuildDependencies()
        {
            ViewModelLocator.Current.RegisterForNavigation<MainPage, MainViewModel>();
        }

        async void InitNavigation()
        {
            var navigationService = ViewModelLocator.Current.Resolve<INavigationService>();
            await navigationService.InitializeAsync<MainViewModel>(null, true);
        }
    }
```

Determines which Initial ViewModel of your app :

```csharp
InitializeAsync<MainViewModel>(null, true)
```

To register the View Matching ViewModel, use:

```csharp
ViewModelLocator.Current.RegisterForNavigation <View, ViewModel> ();
```

## Base ViewModel

BaseViewModel is based on all ViewModels. BaseViewModel Implements LoadAsync in addition to Navigation and Dialog Service


```csharp
 public class MainViewModel : BaseViewModel
    {
       //Set Title
        public MainViewModel() : base("Main View")
        {
        }

        //Override Load
        public override async Task LoadAsync(object navigationData)
        {
           
        }
    }
```

BaseViewModel already behind the implementations of Title and isBusy by default to use in all views

## Navigation

Use BaseViewModel NavigationService to navigate between them. You can also send parameters that will be received in LoadAsync.

```csharp
await NavigationService.NavigateToAsync<DetalhesViewModel>();
await NavigationService.NavigateToAsync<DetalhesViewModel>(parameter);
```

## Dialog

Use BaseViewModel DialogService to display custom alerts or an actionsheet.


```csharp
 //Alert
 await DialogService.AlertAsync("Title", "Message", "Cancel Button Label");
 //Dialog
 await DialogService.AlertAsync("Title", "Message", "Accept Button Label", "Cancel Button Label");
 //ActionSheet
 await DialogService.ActionSheetAsync("Title", "Message", "Destruction Button Label", buttons);
```

## Dependency Injection

Xamarin.Forms.MVVMBase uses DryIoc. You can use your container in the app.cs with :

```csharp
 ViewModelLocator.Current.ContainerBuilder.Register <Interface, Implementation> ();
```

