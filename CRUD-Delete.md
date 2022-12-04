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
    from django.views.generic import DeleteView
    from .models import <model_name>
    from django.urls import reverse_lazy

    class <view_class_name>(DeleteView):
      template_name = '<HTML_name>.html'
      model = <model_name>
      success_url=reverse_lazy('thing_list')
    ```
3. **Inside the templates directory >> create a template**    
    ```
    {% extends 'base.html' %}
    {% block content%}

    <form   method='post'>
        {% csrf_token %}
        <h1>are you sure you want to delete {{thing.name}} ?  </h1>

        <input type='submit'  value='yes'/>
    </form>    

    {% endblock content%}
    ``` 
