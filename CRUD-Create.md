1. **Inside the <app_name>/urls.py**
  ```
  from django.urls import path
  from .views import <view_class_name>
  
  urlpattenrs = [
    path('<path_name>', <view_class_name>.as_view(), name='<any-name>')
  ]
  ```
 2. **Inside the <app_name>/views.py**
  ```
  from django.shortcuts import render
  from django.views.generic import CreateView
  from .models import <model_name>
  
  class <view_class_name>(CreateView):
    template_name = '<HTML_name>.html'
    model = <model_name>
    fields = ['<field_name_from_the_model>', '<field_name_from_the_model>',...]
  ```
3. **Inside the <app_name>/models.py**
    ```
    from django.db import models
    from django.contrib.auth import get_user_model
    from django.urls import reverse 

    class Thing(models.Model):
        name= models.CharField(max_length=255)
        rank = models.IntegerField()
        reviewer=models.ForeignKey(get_user_model(), on_delete=models.CASCADE)

        def __str__(self):
            return self.name

        class Meta:
            verbose_name_plural = "my things"   
            ordering=['-pk']

        def get_absolute_url(self):
            return reverse('thing_detail',args=[self.id])
    ```
4. **Inside the templates directory >> create a template**    
  ```
  {% extends 'base.html' %}
  {% block content%}

  <h1>Create a new  thing </h1>
  <form   method='post'>
      {% csrf_token %}
      {{form.as_p}}
      {% comment %} {{form.as_ul}} {% endcomment %}

      <input type='submit' />
  </form>    

  {% endblock content%}
  ``` 
