# Don’t moving Forward Too Fast With A New Release
  Although new release maybe has some great features and you would like to apply it into your project but maybe bugs also contains in it so be careful
# Remove unused View Engine
  As you know, MVC have 2 view engines by default Webform & Razor so to increase performance in searching view you should remove unused View Engine, something like that:


ViewEngines.Engines.Clear();
ViewEngines.Engines.Add(new RazorViewEngine());
1
2
ViewEngines.Engines.Clear();
ViewEngines.Engines.Add(new RazorViewEngine());
Routing consideration – Turn off static files
Should prevent handle with statis files, there are some ways to do that (change web.config, httpmodule, …). Use MVC route


routes.IgnoreRoute("{*staticfile}", new { staticfile = @".*\.(css|js|gif|jpg|png)(/.*)?" });
1
routes.IgnoreRoute("{*staticfile}", new { staticfile = @".*\.(css|js|gif|jpg|png)(/.*)?" });
Write HTML when you can
Should use HTML instead html helper to render html when you can
Views should not contain presentation logic
The logic should place at controller level
Don’t placing Everything In An HtmlHelper
The purpose of HtmlHelper rendering html so don’t place everything on it (bussiness logic, html template, …)
Avoid using ViewBag or ViewData for passing data
Strong-typed, easy read, maintain
Do Post Redirect Get pattern
When you summit a form, you end up on a subtly dangerous page. The post is a “unsafe” method so if you press “refresh” after POST-ing a web form, you’ll get a warning from the browser asking you if you are sure you want to re-play the POST.
form-resubmission-3
How to fix:
PG
Use UpdateModel/Default Model Binder carefully
Imagine you have a view model


public class UserViewModel
    {
        public int Id { get; set; }

        public string UserName { get; set; }

        public string FirstName { get; set; }

        public string LastName { get; set; }
    }
1
2
3
4
5
6
7
8
9
10
public class UserViewModel
    {
        public int Id { get; set; }
 
        public string UserName { get; set; }
 
        public string FirstName { get; set; }
 
        public string LastName { get; set; }
    }
Bussiness: create a page for update user info (UserName can’t update). If you reuse that model, it’s very risky if someone post UserName into the update request.
How to fix:
Create new model with FirstName and LastName only
Use Exclude parameter on post method

[HttpPost] public ViewResult Edit([Bind(<strong>Exclude = "UserName")(UserViewModel user) 
{ 
         // ... 
}
1
2
3
4
[HttpPost] public ViewResult Edit([Bind(<strong>Exclude = "UserName")(UserViewModel user) 
{ 
         // ... 
}
Do consider using asynchronous controllers for long running requests
