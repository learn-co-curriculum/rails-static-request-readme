# Rails Static Request

## Routing

How does your application know what view to render to users? This is where routing comes in, as a framework Rails has a comprehensive routing system for both dynamic and static pages. Below are the differences between a static and dynamic route:

* **Static route** - A static route will render a view that does not change, you typically will not send parameters to it, examples would be: a site's about or contact pages.
* **Dynamic route** - Dynamic routes are pages that accept parameters and render different content based on those parameters, an example would be: a blog's post page that contains a specific article.

In this lesson we are going to specifically cover static pages to ensure that you can get a firm understanding of how routing works in a Rails application.

Before we dive into the code and routing configurations, it helps to know how HTTP works at a high level. Below is the flow that takes place when a user attempts to go to a page on a Rails application:

1. A URL is entered into the browser, this is the HTTP request
2. That request is sent to the server where the application's router interprets the request and sends a message to the controller mapped to that route
3. The controller communicates with the view file mapped to the controller method
4. The server returns that HTTP response, which contains the view and page that can be viewed in the browser

## Implementing a Static Route

Let's try this out in our application, I'm going to continue using the ```blog-flash``` application that I walked through in the Rails Basic lesson.

To begin, startup the rails server and go to ```localhost:3000/about``` as you will see this throws a routing error: ```No route matches [GET] "/about"```, to fix this, stop the rails server by pressing ```control + c``` (anytime you make a routing change you need to restart the rails server)

Now draw the route by opening the ```config/routes.rb``` file and add the following route inside of the ```draw``` block:

```ruby
get 'about', to: 'static#about'
````

Now start the rails server back up and go back to ```localhost:3000/about``` and click refresh, you should now see that the error message has changed, it's no longer complaining about not having a route, the error should now say: ```uninitialized constant StaticController```

Let's fix this by creating a new controller for our static pages, you can do this manually by running the following command in the terminal:

```touch app/controllers/static_controller.rb```

This will create a blank controller file that we can use to map to the routing file. Since there are a number of methods built into the Rails controller system, you will also want the controller to inherit from the application controller. The new file should have code that looks like this:

```ruby
class StaticController < ApplicationController
end
```

The standard naming convention for controllers is the name of the controller followed by the word ```controller```.

If you refresh the browser now you will see a new error: ```The action 'about' could not be found for StaticController```

We're making good progress (even though we're using EDD - error driven development), and it's good to see each of the errors so that when you encounter these in your real world projects you will know how to fix them. This current error is fixed by adding the following method in the static controller:

```ruby
def about
end
```

Hitting refresh in the browser will give you a 'Template is missing' error, specifically it says: ```Missing template static/about...```

We're very close to getting our view to show up, but before we make our final addition it's important to understand the difference between explicit and implicit rendering for the views. Rails gives us two options for how views are mapped between the controller and view files:

* **Explicit rendering** - for explicit rendering, Rails lets you dictate what view file that you want to have the controller action mapped to.
* **Implicit rendering** - for implicit rendering, Rails follows a standard convention that automatically looks for the view file with the same name as the controller action.

First let's try out explicit rendering, create a new directory in the view's directory called ```static``` and create a new file called ```some_page.html.erb```. In that file add some basic HTML code, such as:

```html
<h1>Hello from some page</h1>
```

Inside the ```about``` method in the controller add the following code:

```render "static/some_page"``` you can also use: ```render "some_page"``` (Rails will automatically look inside the view directory with the same name as the controller, but you can also give the full view path).

If you refresh the ```/about``` page in the browser you will see our heading of **Hello from some page**

To compare that with how Rails utilizes implicit view rendering, create a new file in the static view directory called: ```about.html.erb``` and add some HTML code such as:

```html
<h1>Hello from the about page</h1>
```

Now in the controller, remove the ```render``` call completely. If you go and refresh the browser you will now see **Hello from the about page**.

So is explicit or implicit better? Typically you will discover that you will want to utilize the implicit workflow in your day to day coding practice, the rationale is quite practical. Imagine that you are taking over a legacy Rails project, as you are getting acclimated to the code, would you prefer that the previous dev followed a standard naming process or would you rather be forced to looking through each controller code to see how the controller actions were mapped to the views? Rails has always had the goal of making the development process as efficient as possible, which is why it is typically best to follow these types of implicit procedures. With that being said, it is important to understand how the views are mapped to the controller, which is why we walked through the explicit process.

## Summary

In summary, you should now have a firm understanding of how to implement basic routing in your application for static pages, as a review, the process is below:

1. The server receives an HTML request from the client
2. The application processes the request through the route file
3. The route file maps the request through whatever controller method is called
4. The controller then responds with the view that belongs to that specific method and delivers it to the client
