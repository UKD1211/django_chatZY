# django_chatZy


It's a very basic Chatbot for Python Django using the Django-REST-framework. This Chatbot is currently working without
Machine learning algorithms. Decisions are made by simple statistic evaluation.

The Algorithm is based on labeled data on your Django Database and the tool
is supporting continuous labeling.

## Requirements
- Python (3.7, 3.8, 3.10.5)
- Django (4.1.3)

## Dependencies
- [Django REST-Framework - Awesome web-browsable Web APIs.](https://www.django-rest-framework.org)


## Installation

Install using `pip` ...

```
pip install django-chatzy
```

add `django_chatzy` to your `INSTALLED_APPS` setting.

```
INSTALLED_APPS = [
    ...,
    'django_chatzy'
]
```

**Note:** Make sure to run `manage.py migrate` after changing your settings.
The django_chatzy app provides Django database migrations.

## SNotes...

* `django-admin startproject [project-name]`
it will create the folder in the directry.open it using vs-code.

* `python manage.py startapp home`
write down it in vs-code terminal.it starts the django app.
home is the name of my django app and chatbot or whatever is the name of the project.

* `python manage.py runserver`
it will start the django server.

* `path('', include('home.urls'))`
when somebody comes to url of chatbot it simply handsover the control to home urls.py

* whenever you type something on the url bar the controls of the urls handled by the projects[chatbot here]`urls.py`.
so chatbots `urls.py` take control and it will see weather the `admin` is matching or not[`path('admin/', admin.site.urls)`]
no admin is not matching as its wriiten(`/chat`)in url
now since [`path('', include('home.urls'))`] this is empty it will ok now hand over the controls to home.urls
then it will go to home app and it will looking for the url there it will see : 
* {`urlpatterns = [path('chat'[this is the name of the endpoint], home[this is the name of the function], name='home'[this is a name which i can give to my urls])
]`}
 this path written here (chat) its matching .and it will go to home
home == home is imported from `home.views`(homes `views.py` there we will find the home function)


* `def chat(request):return HttpResponse("This Works")`
this will show us the page
  
* `def chatAPI(request):return HttpResponse("This Works")`
this func. is an api which will return us the json. response


* now we made a folder name templates there to put our html file(`index.html`)
and in `views.py` to made it shown we return the page like 
`def chat(request):return render(request, "index.html")`
but unluckyly it will not work 
so now we have to go to settings.py in chatbot and there we have to register our app and then it will be treat it as an app 
then we also have to add "templates" in `settings.py` as dir.
this is basically for to understand where is the pathis.


* what to do if we add another pages?
* we have to go in our apps[home] `urls.py` and we have to create there another path.
`path('about', chat, name='about')`
* import it in same way like chat before
* in `views.py` we have to create a function called about
and call it in the same way
`def about(request):return render(request, "about.html")`
* then we have to create a fine names `about.html` in templates folder.

* now we have to go to the open AI playground there we copy the code and paste it in `views.py`


## Quickstart

Create a `response.py` file inside of an already installed app.
```
from django_chatzy.responses import GenericRandomResponse


class GreetingResponse(GenericRandomResponse):
    choices = ("Hey, how can I help you?",
               "Hey friend. How are you? How can I help you?")


class GoodbyeResponse(GenericRandomResponse):
    choices = ("See you later.",
               "Thanks for visiting.",
               "See ya! Have a nice day.")
```

Add this Response to your `django_chatzy` setting
```
django_chatzy = {
    ...
    'responses': (
        ("YOUR_APP.responses.GreetingResponse", "Greeting"),
        ("YOUR_APP.responses.GoodbyeResponse", "Goodbye"),
    ),
}
```

Go to your Django admin and create `greeting` and `goodbye` tags.
Your response options will be selectable via choices.

Go to your Django admin, write some patterns and label them. You can just use
the following labels:
```
[Greeting]
"Hi, how are you?", "Is anyone there?", "Hello", "What's up?!", "hey there!"

["Goodbye"]
"Bye", "See you later", "Goodbye", "I need to go now."
```
**Note** If you do not want to write that patterns by yourself, use a command
`manage.py django_chatzy_initial`. You need to label them after initializing.

The package will automatically tokenize the input and map tokens to labels.

Add django_chatzy url to your routings:
```
from django_chatzy.views import DjangoChatbot

urlpatterns = [
    ...
    path("django_chatzy/", DjangoChatbot.as_view())
]
```

Make a Post request to your new endpoint:
```
curl \
    -H "Content-Type: application/json" \
    --data '{"message":"how r u?"}' \
    http://localhost:8000/django_chatzy/
```

The response should look like
```
{
    "tag": "Greeting",
    "message": "Hey, how can I help you?"
}
```

## Raw Documentation

### Database Models
- `Pattern` - message which might be send by a user. Add a tag to the pattern
    for being able to identify and response to that message
- `Tag` - includes information about Response class for a specific method
- `Token` - tokenized words which are referencing to different patterns. The
    user-input will be identified by different tokens.
- `UserMessageInput` - new inputs from production. It contains information
    about chosen pattern. You can label that messages later and include them
    into the system.

### settings options
You can add following options to your `django_chatzy` setting:
- STEMMER_MODULE: nltk package for stumming your strings - 
    default: `nltk.stem.lancaster.LancasterStemmer`.
- responses: choices for your tags. It should reference to a response class.
    **Warning** You won't be able to create tags without response classes.

### Response Classes
The `django_chatzy.responses` package provides currently following response classes:
- BaseResponse
- GenericRandomResponse

#### BaseResponse
It's just an abstract class for require a specific shape of your response
classes. If you are creating a new response, you should inheritance from that
class.

#### GenericRandomResponse
It will choose a generic answer from class property `choices`.

### Views
This `django_chatzy.responses` includes a single view `DjangoChatbot`.
This view is reusable. The most important changeble option:
- `save_pattern`: if True each message will be saved and you can post label
    the incoming messages. default `True`.

#### django_Chatbot API Documentation
- Required Request type: `POST`
- payload: `{message: "YOUR MESSAGE"}`
- response: `{tag: 'TAG', message: 'RESPONSE'}`



### Contributing
Fork the repo and get stuck in!
