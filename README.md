# Razor Pages vs ASP.NET Core MVC

By [Scott Sauber](https://twitter.com/scottsauber) and [Rick Anderson](https://twitter.com/RickAndMSFT)

**Warning: This is an opinionated document. This is not an official Microsoft document.**  

Razor Pages is a feature of the ASP.NET Core framework. Razor Pages follows the [MVVM](/xamarin/xamarin-forms/enterprise-application-patterns/mvvm) pattern. Developers and teams familiar with ASP.NET Core MVC development:

* Can continue app development featuring controllers and views.
* Can be assured the ASP.NET Core framework will provide improvements to controller/view development.

Razor Pages can be added to controller/view projects. The controller-view approach to MVC may be preferred by larger teams.

This article compares Razor Pages to controller/views for creating server-rendered HTML. ASP.NET Core MVC controllers are used to build web APIs. For example, a front-end like React, Angular, Vue, etc. would use ASP.NET Core MVC controllers.

If you haven't tried Razor Pages, consider using them. Many developers find Razor Pages development simpler and more productive than using controllers and views.

Razor Pages advantages over controller/view development:

* **Leads to more maintainable code and a folder structure by default that's easier to navigate.**  
  * Controllers often end up with a large number of `Action` methods, many of which are not related to each other.
  * Controllers contain all the shared code used by all the `Action` methods. Razor Pages include only the code they use. Shared code for Razor Pages is contained in a base class. For example, see [Create a base class to share common code](https://docs.microsoft.com/aspnet/core/data/ef-rp/update-related-data#create-a-base-class-for-course-create-and-edit).

   MVC uses separate folders for Views, Controllers, and ViewModels when all three are tightly coupled. You must bounce between all three folders when you need to add/change/debug a feature. With Razor Pages, the `PageModel` (Controller + ViewModel) is in the same folder as the View.  Hit F7 in Visual Studio to toggle between them.

* **Routing is easier.**  By default in Razor Pages, routing matches the folder structure. This makes nesting folders straightforward.  For example, consider an app where:
  * All of the HR administrator/privileged pages are under the */Administrator* folder.
  * All the Employee pages are under the */Employee* folder.  
  * The app can authorize an entire folder and require the user to be an Administrator to get to any subfolder of */Administrator*. The code to do this is more straightforward than using multiple Controllers for Administrator features.
* **More secure by default.**  Razor Pages provides:
  * [AntiForgeryToken validation](https://docs.microsoft.com/aspnet/core/razor-pages/index#xsrfcsrf-and-razor-pages) by default.
  * You opt-in to the properties that need to be model bound via `[BindProperty]`. `[BindProperty]` prevents [mass assignment/over-posting](https://www.owasp.org/index.php/Mass_Assignment_Cheat_Sheet) attacks.
  * The `PageModel` acts like a view model.

## Razor Pages compared to Web Forms

Razor Pages and Web Forms ASP.NET 4.x are page focused and each contains a *.cs* file associated with the markup page. Other than the page focus, Razor Pages are very different from Web Forms. It can be difficult to migrate Web Forms code to Razor Pages. Converting between Razor Pages and ASP.NET Core MVC is generally straightforward. Razor Pages and Web Forms have very little in common. Razor Pages and ASP.NET Core MVC share most of the ASP.NET Core framework. The following section shows many features shared between Razor Pages and ASP.NET Core MVC that are not found in Web Forms.

## Feature comparison of Razor Pages and MVC with controller and views

|Feature | Controller/Views | Razor Pages|
| ----| ----------------- | ------------ |
|[Testable](xref:test/index)| x | x |
|Separation of concerns| x | x |
|Areas| x | 2.1 |
| Routing | [MVC](https://docs.microsoft.com/aspnet/core/mvc/controllers/routing) | [RP](https://docs.microsoft.com/aspnet/core/razor-pages/razor-pages-conventions) |
| [Model binding/validation](https://docs.microsoft.com/aspnet/core/mvc/models/model-binding) | x | x |
| Filters | [MVC](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters) | [RP](https://docs.microsoft.com/aspnet/core/razor-pages/razor-pages/filter) |
| [Tag Helpers](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/intro) | x | x |
| [Layout files](https://docs.microsoft.com/aspnet/core/mvc/views/layout) | x | x |
|[Partial views](https://docs.microsoft.com/aspnet/core/mvc/views/partial)| x | x |
|View Components | x | x|
|[Built in DI](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) | x | x |
