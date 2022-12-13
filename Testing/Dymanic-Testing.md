1. **Inside <app_name>/tests.py**
  ```
  from django.test import TestCase
  from django.urls import reverse
  from django.contrib.auth import get_user_model
  from .models import <model_class_name>
  
  class ThingTest(TestCase):
    def test_list_view_status(self):
        url = reverse('<name_from_urls>')
        response = self.client.get(url)
        self.assertEqual(response.status_code, 200)

    def test_list_template(self):
        url = reverse('<name_from_urls>')
        response = self.client.get(url)
        self.assertTemplateUsed(response,'<HTML_template_name>.html')

    def setUp(self):
        self.user=get_user_model().objects.create_user(
            username='test',
            email='test@test.com',
            password='test'
         )

        self.thing=Thing.objects.create(
            name='test',
            rank=13,
            reviewer=self.user 
        )


    def test_str_method(self):
        self.assertEqual(str(self.thing),'test')    

    def test_detail_view(self):
        url = reverse('thing_detail',args=[self.thing.id])
        response = self.client.get(url)
        self.assertTemplateUsed(response,'thing_detail.html')


    def test_create_view(self):

        data={
            'name':'test',
            'rank':13,
            'reviewer':self.user.id

         }
        url = reverse('thing_create')
        response= self.client.post(path=url,data=data,follow=True)
        # self.assertEqual(len(Thing.objects.all()),2)
        # self.assertTemplateUsed(response,'thing_detail.html')
        self.assertRedirects(response,reverse('thing_detail',args=[2]))
  ```
