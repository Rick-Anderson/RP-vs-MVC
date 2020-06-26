# Razor Pages vs ASP.NET Core MVC

By [Scott Sauber](https://twitter.com/scottsauber) and [Rick Anderson](https://twitter.com/RickAndMSFT)

https://docs.microsoft.com/en-us/aspnet/core/security/authorization/simple?view=aspnetcore-3.1#authorize-attribute-and-razor-pages

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

Razor Pages and Web Forms ASP.NET 4.x are page focused and each contains a *.cs* file associated with the markup page. Other than the page focus, Razor Pages are different from Web Forms. It can be difficult to migrate Web Forms code to Razor Pages. Converting between Razor Pages and ASP.NET Core MVC is generally straightforward. Razor Pages and Web Forms have very little in common. Razor Pages and ASP.NET Core MVC share most of the ASP.NET Core framework. The following section shows many features shared between Razor Pages and ASP.NET Core MVC that are not found in Web Forms.

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

## Common myths on RP

* Myth: Razor Pages are like Web Forms
  Reality: The only thing RP share with Web Forms is the page focused approach. RP don't include any of the many [anti-patterns inherent in Web Forms](https://www.codemag.com/Article/070183/What%E2%80%99s-Ailing-ASP.NET-Web-Forms).
* Myth: Use MVC or RP, don't mix them.
  Reality: In some cases, Microsoft recommends mixing them. For example:
    * In the official doc's, [Authorize attribute and Razor Pages](https://docs.microsoft.com/en-us/aspnet/core/security/authorization/simple?view=aspnetcore-3.1#authorize-attribute-and-razor-pages) Consider using an ASP.NET Core MVC controller for pages with different authorization requirements for different handlers.
    * [Identity](https://docs.microsoft.com/en-us/aspnet/core/security/authentication/scaffold-identity?view=aspnetcore-3.1&tabs=visual-studio) is provided in a Razor Class Library. You can scaffold the source code into your MVC project, but you get Identity as Razor Pages.

## Comments from developers who prefer RP over MVC

* [Scott Sauber](https://twitter.com/scottsauber) comments [here](https://github.com/dotnet/AspNetCore.Docs/issues/6146#issuecomment-398599679)
 We're using Razor Pages for a multi-tenant Health Insurance portal where HR Admins can CRUD on employee data, run reports, upload docs, download docs, billing statements, etc. and the employees who are insured can see details about their Health Insurance, change it, download docs, etc.

We have 60+ pages and I can say that **for Server-rendered HTML,** I will never go back to MVC.  It's just a better fit for server-rendered HTML apps.  It's also not just for simple things.  The Health Insurance domain is inherently complex and combine this with the fact that it's a multi-tenant app (we sell the product to other insurance companies), which adds more complexity as the app is highly configurable as different insurance companies do things a bit differently.

 Why use it?
- **Razor Pages is more secure by default.**  Razor Pages gives you AntiForgeryToken validation by default.  Plus you opt-in to what properties you want to be model bound via `[BindProperty]` which limits your exposure to over-posting attacks.
- **Razor Pages has a better folder structure by default that scales better.**  In MVC, the default folder structure simply does not scale.  Having separate folders for Views, Controllers, and often ViewModels when all three are ultimately tightly coupled to one another is a huge PITA to work with.  You end up bouncing to all 3 folders and navigating a bunch anytime you need to add or change a feature.  It's horrible.  This is why I advocated for [Feature Folders](https://scottsauber.com/2016/04/25/feature-folder-structure-in-asp-net-core/).  With Razor Pages, your PageModel (Controller + ViewModel) are in the same folder as your View.  You can just hit F7 to toggle between them which is also super convenient.
- **Leads to more maintainable code that scales better.**  With MVC it was super easy to bloat a Controller with 10+ Actions.  Often, these Actions weren't even related to one another in any way (except maybe a Redirect between the two).  This made navigating the Controller to find code very difficult.  It got worse if there were private methods in the Controller too, further adding to the method bloat.  With Razor Pages, it's nearly impossible to bloat up your Page Model with unrelated methods to your page.  Everything you put in your PageModel is related to your Page.
- **Unit Testing is easier.**  With a Controller, you might have 8 Actions and some of your dependencies you inject in were only related to one or two Actions.  So when unit testing a single Action either you need to mock those out unnecessarily or pass a null, both of which feels gross (this can be solved a bit with the Builder pattern).  With Razor Pages, the dependencies you inject in are 100% related to GET and POST actions you're working with.  It just feels natural.
- **Routing is easier.**  By default in Razor Pages, routing just matches your folder structure.  This makes nesting folders way easier to accomplish.  For instance, all of our HR Admin pages are under the /Administrator folder and all the Employee pages are under the /Employee folder.  We can authorize an entire folder and say the person must be an Administrator to get to any subfolder of /Administrator, which was way easier to do that than with multiple Controllers that make up the Administrator features.

I think that's the big stuff.

Some stuff we do that has helped us:
- Make constants files for your Page routes vs magic strings.  [An example might look like this](https://gist.github.com/scottsauber/fe12d9341463e16ee6fe9fefb32c88b1).
- Put the InputModels/ViewModels in a separate file (living in the same folder, right next to the RazorPage it belonged to) vs. an inline class in the PageModel.  We found this was easier to reason about and de-cluttered the PageModel because we had some pages with lots of input fields.  Although I know some people feel the opposite about this.

Hope that helps clear up why we chose Razor Pages and why we won't willingly go back to MVC for server-rendered HTML apps.  If you're not rendering HTML server-side and you're just writing API's for a React/Angular/Vue/whatever front-end, then don't use Razor Pages, use MVC.
* [Anthony Stivers]() comments [here](https://github.com/dotnet/AspNetCore.Docs/issues/6146#issuecomment-398245724)
  The primary reason *I* chose to start using Razor Pages over MVC is because it gets rid of the massive controller problem that exists within MVC. Each `PageModel` is only responsible for itself. Whereas with MVC, a `Controller` is responsible for multiple Views and ViewModels. MVC is still the logical solution for the API portions of ASP.NET projects, though. And because it wouldn't make sense to implement API in Razor Pages, MVC is almost certainly going to co-exist with Razor Pages, rather than Razor Pages replacing MVC.

## Additional RP vs. MVC blogs, posts, etc

* [ASP.NET Razor Pages vs MVC: How Do Razor Pages Fit in Your Toolbox?](https://stackify.com/asp-net-razor-pages-vs-mvc/) Excellent article by [MATT WATSON](https://stackify.com/asp-net-razor-pages-vs-mvc/#wpautbox_about), Founder & CEO of [Stackify](https://stackify.com/)
* [How Does Razor Pages Differ From MVC In ASP.NET Core?](https://exceptionnotfound.net/razor-pages-how-does-it-differ-from-mvc-in-asp-net-core/) Excellent article by [Matthew Jones](https://exceptionnotfound.net/author/matthew-jones/)
* [MSDN Magazine: Simpler ASP.NET MVC Apps with Razor Pages](https://docs.microsoft.com/en-us/archive/msdn-magazine/2017/september/asp-net-core-simpler-asp-net-mvc-apps-with-razor-pages) by [Steve Smith](https://twitter.com/ardalis)
* Stack Overflow [ASP.NET Core Razor pages vs Full MVC Core](https://stackoverflow.com/questions/48121928/asp-net-core-razor-pages-vs-full-mvc-core)
* Stack Overflow [Why is Razor Pages the recommended approach to create a Web UI in Asp.net Core?](https://stackoverflow.com/questions/46777404/why-is-razor-pages-the-recommended-approach-to-create-a-web-ui-in-asp-net-core)
* Microsoft official GitHub closed discussion issue on [Why choose MVC tutorials over Razor Pages](https://github.com/dotnet/AspNetCore.Docs/issues/6146). You'll need to select **Load more...** several times to see all the responses.
