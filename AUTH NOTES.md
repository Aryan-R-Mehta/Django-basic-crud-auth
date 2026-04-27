DJANGO PROJECT IMPORTANT THINGS

DJANGO LOGIN FLOW :-

>> In Django if you want any view only get accessible if user is login, so it is very simple in this. You can use the inbuild decorator for this.
## from django.contrib.auth.decorators import login_required
## @login_required
>> Put this decorator in top of any view function it will be not accessbile by anyone if he is not login.

>> First on base app add this. (BASE APP = The name you have given with startproject).
    config >> settings.py
        LOGIN_URL = '/accounts/login'       # On login you want to redirect on which url.
        LOGIN_REDIRECT_URL = '/'      # This is the link where you want to redirect the user after login.

>> make sure your config > urls.py have url path to that apps url.
    config >> urls.py
        urlpatterns = [
            ...
                path('accounts/', include('django.contrib.auth.urls')),
            ...
        ]

>> Now for template there is a rule in django you will create the login, registration, logged out that all at the root level where manage.py is there.
>> So you can create that login form design at this location.
    PROJECT_NAME  >>  templates  >>  registration  >>  login.html
>> And if your username and password is correct, you will be logged in and redirect to the link mention above.



DJANGO REGISTRATION FLOW:-

>> In your app(tweet here) first you will add a url regrading the register. So on request inside that mentioned view function will be called.
    tweet  >>  urls.py
        path('register/', views.register, name="register"),


>> First in forms.py (needed to create manually) create a class and inside that use the inbuild auth form 'UserCreationForm'.
    tweet  >>  forms.py
        from django import forms
        from .models import User
        from django.contrib.auth.forms import UserCreationForm

        class UserRegistrationForm(UserCreationForm):
            class Meta:
                model = User
                fields = ('username', 'email', 'password1', 'password2')
>> In this the UserCreationForm is the default form which is there for registration.
>> User is the model which is default created in django which holds all user data.
>> UserRegistrationForm > It is the classname we will use to use this form.


>> Then in the views define the method for registration. As per the link given in urls.py view function name will be 'register'.
    tweet  >>  views.py
        from .forms import UserRegistrationForm
        from django.contrib.auth import login

        def register(request):
            if request.method == "POST":
                form = UserRegistrationForm(request.POST)
                if form.is_valid():
                    user = form.save(commit=False)
                    user.set_password(form.cleaned_data['password1'])
                    user.save()
                    login(request, user)
                    return redirect('tweet_list')
            else:
                form = UserRegistrationForm()
            return render(request, 'registration/register.html', {'form': form})
>>  register is the view function name which we have pass in the urls.py.
>> When form gets only open then it stays in else part. It display the UserRegistrationForm(). And it will render the ""registration/register.html"" template.
>> In django how it works when we click on submit the request.method is called, so when the method type is POST the if part will be exectuted.
>> We pass request.POST in class of form so he will be notified that register is needed to do.
>> is_valid() is the default validation porvided by django.
>> If validation is passed we first store form locally, because we need to add user so we will add "commit = false".
>> Now to add the password. We used user.set_password in build method. and to retrive data from form we use form.cleaned_data['field_name'].
>> And then we will save that user data.
>> Now we want to do login as well at signup so we will do login directly from login method.
>> So the login method from 'django.contrib.auth' we will pass the request and user in that login method so it will automatically get login.


>> And you can add your register form html here.
    PROJECT_NAME  >>  templates  >>  registration  >>  register.html