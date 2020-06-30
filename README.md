# Rails Static Request

## Routing

How does your application know what view to render to users? This is where routing comes in. As a framework, Rails has a comprehensive routing system for both dynamic and static pages. Below are the differences between a static and dynamic route:

* **Static route** - A static route will render a view that does not change. Typically, you will not send parameters to it. Examples would be a site's about or contact pages.
* **Dynamic route** - Dynamic routes are pages that accept parameters and render different content based on those parameters. An example would be a blog's post page that contains a specific article.

In this lesson we're going to specifically cover static pages to ensure that you can get a firm understanding of how routing works in a Rails application.

Before we dive into the code and routing configurations, it helps to know how HTTP works at a high level. Below is the flow that takes place when a user attempts to go to a page on a Rails application:

1. A URL is entered into the browser; this is the HTTP request
2. That request is sent to the server where the application's router interprets the request and sends a message to the controller mapped to that route
3. The controller communicates with the view file mapped to the controller method
4. The server returns that HTTP response, which contains the view page that can be viewed in the browser

## Implementing a Static Route

Let's try this out in our application. I'm going to use a blogging application as a case study in this lesson.

To begin, start up the Rails server and go to `localhost:3000/about`. As you will see, this throws a routing error: `No route matches [GET] "/about"`. To fix this, stop the Rails server by pressing `control + c`. (Anytime you make a routing change you need to restart the Rails server.)

Now draw the route by opening the `config/routes.rb` file and adding the following route inside of the `draw` block:

```ruby
get 'about', to: 'static#about'
````

Let's look at the components that make up this route code:

* The HTTP verb - in this case we're using the `get` HTTP verb.

* The path - `'about'` represents the path in the URL bar that the route will be mapped to.

* The controller action - `'static#about'` tells the Rails routing system that this route should be passed through the `static` controller's `about` action. If the term `action` sounds foreign, actions are just Ruby speak for a method in a controller. So in the `StaticController` will be a method called `about` that gets called when a user goes to `/about`.

Now start the Rails server back up, go back to `localhost:3000/about`, and click refresh. You should now see that the error message has changed. It's no longer complaining about not having a route; it should now say: `uninitialized constant StaticController`.

Let's fix this by creating a new controller for our static pages, adding a new file to the application: `app/controllers/static_controller.rb`.

This will create a blank controller file that we can use to map to the routing file. Since there are a number of methods built into the Rails controller system, you will also want the controller to inherit from the application controller. The new file should have code that looks like this:

```ruby
class StaticController < ApplicationController
end
```

The standard naming convention for controllers is the name of the controller followed by the word `Controller`.

If you refresh the browser now, you will see a new error: `The action 'about' could not be found for StaticController`. This means that it found our controller (woot!) but couldn't find the action `about` in that controller. On a side note, since controllers are located within the `app` directory, you can make changes to controller files and see the result in the browser without having to restart the Rails server.

We're making good progress (even though we're using EDD - error driven development), and it's good to see each of the errors so that when you encounter these in your real world projects you will know how to fix them. This current error is fixed by adding the following method in the static controller:

```ruby
def about
end
```

Hitting refresh in the browser will give you a 'Template is missing' error; specifically, it says: `Missing template static/about...`. Also note that you do not have to restart the Rails server here. As long as your changes are within the `app` directory, you can keep the server going; only code changes outside of the `app` directory require stopping and starting the Rails server.

We're very close to getting our view to show up. Rails gives us two options for how views are mapped between the controller and view files. It's important to understand the difference between explicit and implicit rendering for the views:

* **Explicit rendering** - for explicit rendering, Rails lets you dictate which view file you want to have the controller action mapped to.
* **Implicit rendering** - for implicit rendering, Rails follows a standard convention that automatically looks for the view file with the same name as the controller action.

First, let's try out explicit rendering. Create a new directory within the views directory called `static`, and create a new file called `some_page.html.erb`. In that file add some basic HTML code, such as:

```html
<h1>Hello from some page</h1>
```

Inside the `about` method in the controller add the following code: `render "static/some_page"`. You can either provide the full view path (`static/some_page`) or you can omit the enclosing directory, e.g., `render "some_page"`. Rails will automatically look inside the view directory with the same name as the controller. It's typically considered best practice to use the `render "some_page"` syntax since it won't rely on the name of the directory (in case that gets changed later on). So the `about` method should look something like this:

```ruby
def about
  render "some_page"
end
```

If you refresh the `/about` page in the browser, you will see our heading of **Hello from some page**.

To compare that with how Rails utilizes implicit view rendering, create a new file in the `static` views directory called `about.html.erb`, and add some HTML code, such as:

```html
<h1>Hello from the about page</h1>
```

Now completely remove the `render` call from the controller. If you refresh the browser, you will now see **Hello from the about page**.

Whoa! How is an empty method generating the same behavior as when we were calling the view template directly? This follows along with the popular 'convention over configuration' pattern that Rails utilizes. This means that the Rails core team has built out a number of standardized processes, such as implicit view rendering to help make development life a little easier. It's not some kind of black code magic; behind the scenes, Rails has a large number of complex processes that make things like implicit view rendering work properly.

So is explicit or implicit better? Typically, you will find that you want to utilize the implicit workflow in your day to day coding practice. The rationale is quite practical. Imagine that you are taking over a legacy Rails project. As you are getting acclimated to the code, would you prefer that the previous dev followed a standard naming process, or would you rather be forced to look through each controller to see how the controller actions were mapped to the views? Rails has always had the goal of making the development process as efficient as possible, which is why it is typically best to follow these types of implicit procedures. With that being said, it is important to understand how the views are mapped to the controller, which is why we also walked through the explicit process.

## Summary

In summary, you should now have a firm understanding of how to implement basic routing in your application for static pages. As a review, the process is below:

1. The server receives an HTTP request from the client
2. The application processes the request through the `routes.rb` file
3. The route file maps the request through whichever controller method is called
4. The controller then responds with the view that belongs to that specific method and delivers it to the client

## PhantomJS Errors

If you receive this error:

`Could not find an executable ["phantomjs"] on your path.`

You'll need to install PhantomJS on your computer before the tests will work. 

For Mac users, run the following:

```sh
brew cask install phantomjs
```

For Windows users, run the following:

```sh
npm install -g phantomjs-prebuilt
```

You can confirm the installation has worked by running `phantomjs -v`.
