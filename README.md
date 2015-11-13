# Rails Static Request

Objectives

1. Define the purposes of routing, the mapping between an HTTP request and a specific controller action in your application.
2. Identify route errors from Rails (going to a url with no route) and how to solve it (by drawing a route)
3. Draw a route with get, mapping a url to a controller action
4. Create a new controller by hand and inherit from application controller
5. Define a controller action by defining a method in a controller
6. Identify missing template errors from Rails (going to an action with no template) and how to solve it (by creating a template)
7. Use render to explicity render a template
8. Define the Reails implicit rendering convention
9. Define the steps required to create a static request


## Notes

Should explain that every URL that we want our application to handle must map to a specific controller and action.

When a request comes into our application, our router must pick a controller and a method in that controller that is responsible for generating a response. We've taught a lot of this in Sinatra so it can be a bit light here.

Use the example of how we want to have an 'about' page on our site. So let's try to go there, let's just type localhost:3000/about.

We get a routing error. Let's draw the route.

Let's draw a simple route, like /about. Let's have that map to a StaticController and an about action (though don't create that controller). Reload the page. No more routing error, now it can't find the controller.

Let's make a new controller, no generators, with touch app/controllers/static_controller.rb.

Controllers need to inherit from ApplicationController and explain briefly the controller naming convention.

Define our first controller action as a method, def about.

Let's hit refresh. Now it complains about a view.

Controller actions are responsible for determining what the response is - responses are built by views.

For some reason we'll discuss briefly, rails is looking for a view in a very specific place. lets just make it happy and build our view in views/static/about.html.erb.

Let's just put some basic HTML there.

Okay, now everything works.

Let's make this rendering process of choosing a view a little more explicit using the rails render method (equiv to sinatra's erb method if you're familair with it).

Render can accept a string that points to a view template relative to 'views', so we can use render 'static/about'. If you think about it, that's a great place to put a view that relates to the about action in the static controller. In fact, it's such a reasonable ad convineinent place to put it that rails actually assumes, given no other render instructions, that's where you want to put it. This is a big principal in rails, convention over configuration. That's why our action worked without a render call. As a beginner we believe being explicit is easier at first then being implicit, learn the conventions and then use them.

And like that we've created our first rails request - it's a static request, no data is being used, no matter what, everytime we load our about page it will be exactly the same. there is nothing dynamic about it. But that teaches us the general methodology of responding to a request with rails.

draw a route.
map it to a controller action.
program the action
create a template to render
render that template.
