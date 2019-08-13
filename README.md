### django-viewlet
---
https://github.com/5monkeys/django-viewlet

```py
INSTALLED_APPS = (
  'viewlet',
)

VIEWLET_TEMPLATE_ENGINE = 'jinja2'

JINJA2_EXTENSIONS = (
  'viewlet.loaders.jinja2_loader.ViewletExtension',
)
VIEWLET_JINJA2_ENVIRONMENT = 'coffin.common.env'

JINJA_CONFIG = {
  'extensions': {
    'viewlet.loaders.jinja2_loader.ViewletEtension',
  },
}
VIEWLET_JINJA2_ENVIRONMENT = 'jingo.get_env'

TEMPLATES = [
  {
    'BACKEND': 'django.template.backends.jinja2.Jinja2',
    'OPTIONS': {
      'extensions': [
        'viewlet.loader.jinja2_loader.ViewletExtension',
      ],
    }
  }
]
```

```sh
pip install django-viewlet
```

```py
from django.template.loader import render_to_string
from viewlet import viewlet
@viewlet
def hello_user(context, name):
  return render_to_string('hello_user.html', {'name': name})

VIEWLET_DEFAULT_CACHE_ALIAS = 'template_cache'
CACHES = {
  'template_cache': {
  },
}

@viewlet(using='super_cache')
def hello_user(context, name):
  return render_to_string('hello_user.html', {'name': name})
  
import viewlet
viewlet.refresh('hello_user', 'monkey')
hello_user.refresh('monkey')

@viewlet(name, template, key, timeout)

@viewlet(timeout=30*60)
def hello_user(context, name):
  return render_to_string('hello_user.html', {'name': name})
  
@viewlet(timeout=30*60, key='some_cache_key_%s')
def hello_user(context, name):
  return render_to_string('hello_user.html', {'name': name})
  
@viewlet(template='hello_user.html', timeout=30*60)
def hello_user(context, name):
  return {'name': name}

@viewlet(timeout=0)
def hello_user(context, name):
  return render_to_string('hello_user.html', {'name': name})

@viewlet(name='greeting')
def hello_user(context, name):
  return render_to_string('hello_user.html', {'name': name})

class Product(Model):
  name = CharField(max_length=225)
@viewlet(timeout=None)
def product_teaser(context, id):
  product = get_context_object(Prodct, id, context)
  return render_to_string('product_teaser.html', locals())

def refresh_product_teaser(instance, **kwargs):
  viewlet.refresh('product_teaser', instance.id)
post_save.connect(refresh_prodcut_teaser, Product)

urlpatterns = patterns('',
  (r'^viewlet/', include('viewlet.urls')),
)
```


