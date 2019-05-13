---
Layout: post
toc: true
toc_label: "On this page"
section-type: post
title: "Paginating results of a django form post request"
category: python
description: "This post discusses some ways by which we can paginate results when after a post request in django"
tags: ['python','django','cookie','session']
---
Several ideas has been shared in [this](https://stackoverflow.com/questions/2266554/paginating-the-results-of-a-django-forms-post-request) stackoverflow answer as how to paginate results
after a post request is made in django. I will be discussing on how to implement some of these ways in this post.

## Challenge

In case you wanted to show the results on single page, it would not have been a problem. The challenge lies in the fact that when you are
showing your results using several pages, you need to store the post data when returning the second page. Let's see this with an example.

```python
def index(request):
    if(request.method == 'POST'):
        form = EmployeeForm(request.POST)
        sex = form.cleaned_data['sex']
        age = form.cleaned_data['age']
        if form.is_valid():
            result = Employee.objects.all(sex=sex,age_gte=age) # filter all employees based on sex and age
            paginator = Paginator(result, 20) # Show 20 results per page
            page = request.GET.get('page')
            r = paginator.get_page(page)
            render(request, 'app/result.html',{'result':result})
    else:
        form = EmployeeForm()
    
    return render(request,'app/home.html',.{'form':form})
```

* When the index() is called for the first time, the request will contain the post data and it will return the first 20 results.
* Now let's say you wanted to access the 2nd page(/?page=2) of the result. This time the post data won't be sent to along with the request, how will
  you apply filter on query this time? The challenge here is to store the initial post request somewhere.

## A solution using cookies

A cookie is basically a small piece of data which is sent by the server and is stores on the client side(web browsers). As we know, http is a stateless protocol.
So, cookies were made to maintain a state between client and server. Do keep in mind that cookie is an insecure way of storing information in the sense that
anyone having access to your computer can see the contents of the cookie.

In our case, what we can do is to store the post request fields in a cookie. When the post form is submitted, you can send the 20 results and also set the cookie on the client side
which contains the post request fields. Let's see how you can do this with code.

```python
def index(request):
    is_cookie_set = 0
    # Check if the cookie is already set. If set, get their values and store it.
    if 'age' in request.COOKIES and 'sex' in request.COOKIES: 
        age = request.COOKIES['age']
        sex = request.COOKIES['sex']
        is_cookie_set = 1
    if(request.method == 'POST'):
        if(is_cookie_set == 0): # form submission by the user
            form = EmployeeForm(request.POST)
            sex = form.cleaned_data['sex']
            age = form.cleaned_data['age']
            if form.is_valid():
                result = Employee.objects.all(sex=sex,age_gte=age) # filter all employees based on sex and age
        else: # When the cookie has already been set
            result = Employee.objects.all(sex=sex,age_gte=age)
        paginator = Paginator(result, 20) # Show 20 results per page
        page = request.GET.get('page')
        r = paginator.get_page(page)
        response = render(request, 'app/result.html',{'result':result})
        # When the form is submitted, set the cookie values on client side which can be used to paginate.
        if(is_cookie_set == 0): 
            response.set_cookie('age',age)
            response.set_cookie('sex',sex)
        return response
    else:
        form = EmployeeForm()
    return render(request,'app/home.html',.{'form':form})
```

I have included comments to explain the code. This code has not been tested. I am just giving a logic as how to implement it.

## A solution using sessions

A major drawback of a cookie is that it is insecure form of storing information as anyone having access to your computer can see it.
A session token is a unique identifier that is generated and sent from a server to a client to identify the current interaction session. The client usually stores and sends the token as an HTTP cookie and/or sends it as a parameter in GET or POST queries.
Remember only the token is stored on the client side but the actual data(age,sex) is stored on the server side.

The logic and code will be similar to one we implemented for cookie. Remember when you have some sensitive data in your form like phone number, address etc., you should not
use cookie to store them. Session should only be used in those cases.


```python
def index(request):
    is_cookie_set = 0
    # Check if the session has already been created. If created, get their values and store it.
    if 'age' in request.session and 'sex' in request.session: 
        age = request.session['age']
        sex = request.session['sex']
        is_cookie_set = 1
    else:
        # Store the data in the session object which can be used later
        request.session['age'] = age
        request.session['sex'] = sex
    if(request.method == 'POST'):
        if(is_cookie_set == 0): # form submission by the user
            form = EmployeeForm(request.POST)
            sex = form.cleaned_data['sex']
            age = form.cleaned_data['age']
            if form.is_valid():
                result = Employee.objects.all(sex=sex,age_gte=age) # filter all employees based on sex and age
        else: # When the session has been created
            result = Employee.objects.all(sex=sex,age_gte=age)
        paginator = Paginator(result, 20) # Show 20 results per page
        page = request.GET.get('page')
        r = paginator.get_page(page)
        response = render(request, 'app/result.html',{'result':result})    
        return response
    else:
        form = EmployeeForm()
    return render(request,'app/home.html',{'form':form})
```

In the later posts, I will discuss some other ways that were discussed on the stackoverflow answer. Remember that the code is not tested and in case you find some mistakes, do comment and suggest
your solutions.

## References

* [https://docs.djangoproject.com/en/2.2/topics/pagination/](https://docs.djangoproject.com/en/2.2/topics/pagination/)
* [https://docs.djangoproject.com/en/2.2/topics/forms/](https://docs.djangoproject.com/en/2.2/topics/forms/)
* [https://www.abidibo.net/blog/2014/09/19/paginating-results-django-form-post-request/](https://www.abidibo.net/blog/2014/09/19/paginating-results-django-form-post-request/)
* [https://stackoverflow.com/questions/2266554/paginating-the-results-of-a-django-forms-post-request](https://stackoverflow.com/questions/2266554/paginating-the-results-of-a-django-forms-post-request)