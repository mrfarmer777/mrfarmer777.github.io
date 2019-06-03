---
layout: post
title: "The Problem with 'patch'"
sub-title: "Why PATCH requests only work when you shout them."
header-img: "img/patch.jpg"
tags: [fetch api, HTTP PATCH, CORS PATCH, technical]
---


Patch requests not working but everything else is fine? Maybe you have the same problem I did.
Let's say you'd like to implement an update route on a resource in a React-Rails app. You've got actions all set to go in the controller; just waiting patiently to update a ```section``` resource:


```ruby
#sections_controller.rb
def update
    @section=Section.find(params[:id])
    if @section.user_id == current_user.id #look, even double checks for authentication/authorization.
        @sections.update(section_params)
    end
    render json: @section
end

```

and your update route is defined and open for business:

```ruby
  resources :sections, only: [:show, :destroy, :update]
```

Just to make you feel even better, your ```:show``` and ```:destroy``` routes are already working like a charm. So you head over to your ```userActions.js``` on the client side. There you've carefully wired up your actions with your reducer (if you're using Redux, like me). You might have something that looks like this:

```javascript
//userActions.js

//This action accepts a request to join a section (a group of users) in the app.
//Comments included to help clarify what's going on.
export function acceptInvite(sid){
    return (dispatch) => {
        dispatch({type: "BEGIN_USER_REQUEST", payload: {request_type: "accept invite", id: sid}});     //tell state to wait up while we perform the acceptance of the invitation.

        const authHeader = JSON.stringify("Bearer "+localStorage.getItem("jwtToken"));    //Build the auth header and stringify for use in the fetch (make it happen)
    
        const body=JSON.stringify({us: {approved: "true"}});     //build the request body for use in the fetch, true because we're accepting the invitation
        
        return fetch(`BASEURL/sections/${sid}`,{ //Send the request to the sections controller
            method:'patch', //wonderful! We'll just patch the section record to say it's been approved and then we'll have pie. I like pie.
            body: body,
            headers: {
                "Accept":"application/json",
                "Content-type": "application/json",
                "Authorization": authHeader
            }
        }).then((res)=>res.json())  //jsonify the response
        .then((inv)=>{
            dispatch({type: "REMOVE_SECTION_INVITE", payload: inv}); //remove the section invite from the state
        });
    };
}
```

Awesome (I'm modest). Everything looks good. You're ready to send your patch request to update the ```section``` object to show that the user has _approved_ this section invite. You've even got your auth header so that only the applicable user can approve it. (See the controller code above). You're feeling great, you sip your coffee and click the "accept" button to send the request and 

_...crickets._

And by crickets, I mean that your console lights up with the following gem: 

>Fetch API cannot load . Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin 'http://127.0.0.1:3000' is therefore not allowed access. The response had HTTP status code 501. If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.


!["COOOOOOOORS!"]({{site.baseurl}}/img/StarTrekIIKhaaaaaan.png)
_"COOOOOORS!"_

Easy, Captain. You've dealt with this before. After all, your other methods are working. Why aren't they hitting CORS errors? With the help of some [other](https://medium.com/@damwhitaker/navigating-cors-using-react-and-rails-a58b4aee4733) [great](https://cecilycodes.com/rails-react-cors-issues/) [posts](https://stackoverflow.com/questions/17858178/allow-anything-through-cors-policy/17858276#17858276) on the topic, you remember you've already implemented the ```rack-cors``` gem that allows you to open the doors for your CORS needs. 

Checking the ```config/application.rb``` file you see that this is indeed already set up.


```ruby
# config/application.rb
config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'
    resource '*', headers: :any, methods: [:get, :post, :delete, :patch, :options]  #<---I see you, patch. Why can't you just be cool?
  end
end
```

Yup! We've even got ```:patch``` in there with the other methods. So if the other methods are working, what's the problem with 'patch'?

### Shouty-patch to the rescue...

The solution? I wish it was cooler, but the problem was way back in the react ```sectionActions.js``` file. Let's go back, zoom and enhance...

```javascript
//sectionActions.js fetch
        
        return fetch(`BASEURL/sections/${sid}`,{
            method:'patch', //<--the problem. This little fella needs to be PATCH!
            body: body,
            headers: {
                "Accept":"application/json",
                "Content-type": "application/json",
                "Authorization": authHeader
            }
        })
 
```

We used a lowercase `patch` method for the request.  This isn't a problem on our other methods like `get`, `post`, and even `delete`. But [for reasons of backward compatability](https://fetch.spec.whatwg.org/#methods) and consistency, methods are actually case-sensitive. The [WHATWG](https://fetch.spec.whatwg.org/#example-normalization) actually goes so far as to say:
>Using `patch` is highly likely to result in a `405 Method Not Allowed`. `PATCH` is much more likely to succeed.

So ```rack-cors``` never even gets a chance to work its magic. Before sending the ```patch``` request, the browser does a "pre-flight check" and finds that the ```patch``` request matches one it expects to receive/send (`PATCH`), but it isn't properly normalized. This prevents the fetch from ever being performed in the first place. It's a bit of a pain, sure, but this is the cost of doing business across the web uniformly. With all of the RESTful APIs floating around we need to keep things consistent. Separately, the documentation at [WHATWG](https://whatwg.org/) is really cool and helpful for similar issues. I'm glad this problem brought me there. 

### What did we learn? 
If you're like me, this lesson took you about 40 minutes of poking around to solve. I like to try and remember the fewest rules that allow for success in the most cases. With that in mind, I would leave you with the following:

>> When ever you are writing a request method, write it in uppercase.

or alternately

>> If your going to make a request, do it LOUDLY!


Shouty-PATCH for the win. 

Thanks for reading!

-Farmer


