# Json Login

Coming soon...

So how do we want our users to log into our API or log into our application? There's about a million possible answers for this and really it depends, not really as much on the API but what you're actual application is that you're building. And most of the time still it's probably going to be that you are logging in via username and password and it doesn't matter if that's, if you're logging in on a mobile app or if you're logging in on a, on a single page application for login via a some sort of email password combination.

Um,

that's a pretty common situation. And so that's actually what we are going to show. The real question is not how you're logging in, but what happens, how does your API respond when you log in? Does it respond with an API token or does it use session based authentication? And you might think that hey, we're building an API. So an API means that it's stateless, it has no sessions and that means it always returns API tokens. And that is super not true. Session based authentication is actually just a special type of token authentication. Session based authentication works by returning a cookie. That cookie is then sent automatically and ever request. Basically it's a token. The nice thing about session based authentication is it's been around forever and it's super secure. If you are, if you are building an API for your own javascripts own javascript to consume, you should use session based authentication.

If you're building it for a mobile app, mobile apps also could support session based authentication. So yes, later we are gonna talk about uh, creating an API authentication mechanism using some sort of API token. But there's a good chance you don't need it and you should use session based authentication. So we're going to build a super nice, uh, API friendly session based authentication mechanism where were able to post the email and the email and password of a specific user as a Jason Body. And then that endpoint, instead of returning like an API token, it's just going to start our session and the response will send back a cookie.

Okay.

So if you have a fairly traditional email or username, password system, um, there's a nice built in way to handle authentication inside of symphony security under the main firewall. I'm going to add Jason Underscore login and below that I'm going to say check path AP app underscore login. This is the name of a route that we're going to create in a second and it's the URL is going to be slashed. So by setting check Pat's a login basically means that if we send a post request two slash login and we include then this, uh, then this piece of code will know that we're trying to authenticate. What we'll do is then set username underscore path to email and password underscore path to password. It will look for a Jason Body or Jason Decode that body and it will expect an email and password fields to live inside of there.

Now the way that this knows how to query for our user by email is thanks to our user provider up here, which was something that we set up in the last thing. But basically since we're using doctrine, this is the entities or provider, this property email here says that a t tells it how to query for the email. So if you have something that's a little more complicated than you might need to create your own custom user provider. And if you have a login mechanism that looks nothing like an email and password thing, then you're probably gonna need to create a guard authenticator. The point is, so that part might look a little bit different based on the user. But the really important thing is what we're going to do on success and what we're going to do on failure.

So to get this to work, we need a great that app on a score logging route. So I'm actually going to go into source controller, create a new um, peach class. I'll security controller, make it, extend the normal abstract controller and that'll say public function login. And above this I will put our at route annotation and I'll hit tab to article, plead that so it as the use statement, make it slash login with name equals app underscore login. And we'll also add methods equals a post. So initially you just need to have this route here because just the way symphony security works, you can't post to slash login and have the Jason Login do its magic unless you at least have a route. If you don't have a route, it's going to four oh four before the Jason logging to do with magic. But also by default, if we log in successfully with Jason Login on success, it does nothing. It will actually allow the requests to continue and hit our controller. So the easiest way to handle like what happens on what is this endpoint return on success is just to return something in here. So I don't really know what we're going to need to return yet. We're actually gonna think about this a bit more later, but for now I'm just gonna return this Arrow, Jason, they user key, um, set too. Um, if the user's logged in

and we'll set it to the users ID, otherwise we'll set it to null. And is that, we'll think a little bit more later about whether or not that makes sense. And what we actually should return on success. All right. As I mentioned, if you go to just a local host, 8,000 we are a front end here that's built in Vue, js and don't worry, you don't need to know a lot about bjs. I just wanted to use it so we have something a little bit more realistic and here I have a um, a little log in form. If you open up assets, js components log and formed that view. This is the view component that represents that and the important thing to understand is that when we submit then when we, when we press submit on this form, it's going to submit via Ajax. I already have it hooked up so that when we submit the form it calls, handles submit and I also have some example code down here to make that Ajax request.

I'm using a library called Axios, which is just really good at making Ajax requests and very simply what you can see here is I'm doing a post request to slash login and I'm sending email and password up to there. This, that email and this stop password will be the actual values that I filled in to those two boxes. Now, one thing to know about axios is that by default it will send this information here as a Jason, as Jason, up to the end point. A lot of Ajax, uh, uh, libraries won't do that. And we all need a Jason string of Phi it manually. But I'm actually gonna talk a little bit more about that in a second. Then a little then on, uh, success. Right now I'm just logging to see what our server returns on success and down here. Dot. Catch is what will happen if we have some sort of authentication air. And I'm also dumping right here. This is actually getting the data from the response. So we have not even tried to add any really users to our database yet. So this is all going to fail. So let's, let's see what authentication that failure looks like. So well per case of lover, at example.com. Any password, it's summit and nothing happens.

Okay?

Now nothing happens like you saw with me. Uh, make sure that you actually are running webpack encore in the background cause that's actually what's updating our data. And if that still doesn't work, I'm just gonna do a little forest refresh here. I think it might be a little stale, but my, a big email address back in there and yes, we got it. Look at login four oh one status code here. Perfect. You can see it actually returns with invalid credentials. So if it looked back in the network here, it turns out the way that Jason Login, um, uh, works is that it actually has built in, um, error message that returns Jason Response with air here. So it works out of the box. So next, let's actually use that on the front end and learn a little bit more about some how that Jason login authentication works.