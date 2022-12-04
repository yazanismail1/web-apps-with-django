1. **Inside the <app_name>/urls.py**
    ```
    from django.urls import path
    from .views import <view_class_name>

    urlpattenrs = [
      path('<path_name>/<int:pk>', <view_class_name>.as_view(), name='<any-name>')
    ]
    ```
 2. **Inside the <app_name>/views.py**
    ```
    from django.shortcuts import render
    from django.views.generic import UdateView
    from .models import <model_name>
    from django.urls import reverse_lazy

    class <view_class_name>(UdateView):
      template_name= 'thing_update.html'
      model= Thing
      fields= ['name','rank','reviewer']
      success_url=reverse_lazy('thing_list')
    ```
3. **Inside the templates directory >> create a template**    
    ```
    {% extends 'base.html' %}
    {% block content%}

    <h1>Update  {{thing.name}} </h1>
    <form   method='post'>
        {% csrf_token %}
        {{form.as_p}}
        <input type='submit' />
    </form>    

    {% endblock content%}
    ``` 
