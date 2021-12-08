drfexts
=======

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
![Python Version](https://img.shields.io/badge/python-3.8-blue)
[![pypi-version](https://img.shields.io/pypi/v/drfexts.svg)](https://pypi.python.org/pypi/drfexts)
[![Build Status](https://travis-ci.com/dwyl/esta.svg?branch=master)](https://travis-ci.org/)

**Extensions for Django REST Framework**

**Author:** aidenlu.

Installation
------------

``` {.bash}
$ pip install drfexts
```

Usage
-----

*views.py*

``` {.python}
from rest_framework.views import APIView
from rest_framework.settings import api_settings
from drfexts.viewsets import ExportMixin

class MyView (ExportMixin, APIView):
    ...
```

Ordered Fields
--------------

By default, a `CSVRenderer` will output fields in sorted order. To
specify an alternative field ordering you can override the `header`
attribute. There are two ways to do this:

1)  Create a new renderer class and override the `header` attribute
    directly:

    > ``` {.python}
    > class MyUserRenderer (CSVRenderer):
    >     header = ['first', 'last', 'email']
    >
    > @api_view(['GET'])
    > @renderer_classes((MyUserRenderer,))
    > def my_view(request):
    >     users = User.objects.filter(active=True)
    >     content = [{'first': user.first_name,
    >                 'last': user.last_name,
    >                 'email': user.email}
    >                for user in users]
    >     return Response(content)
    > ```

2)  Use the `renderer_context` to override the field ordering on the
    fly:

    > ``` {.python}
    > class MyView (APIView):
    >     renderer_classes = [CSVRenderer]
    >
    >     def get_renderer_context(self):
    >         context = super().get_renderer_context()
    >         context['header'] = (
    >             self.request.GET['fields'].split(',')
    >             if 'fields' in self.request.GET else None)
    >         return context
    >
    >     ...
    > ```

Labeled Fields
--------------

Custom labels can be applied to the `CSVRenderer` using the `labels`
dict attribute where each key corresponds to the header and the value
corresponds to the custom label for that header.

1\) Create a new renderer class and override the `header` and `labels`
attribute directly:

> ``` {.python}
> class MyBazRenderer (CSVRenderer):
>     header = ['foo.bar']
>     labels = {
>         'foo.bar': 'baz'
>     }
> ```

Pagination
----------

Using the renderer with paginated data is also possible with the new
[PaginatedCSVRenderer]{.title-ref} class and should be used with views
that paginate data

For more information about using renderers with Django REST Framework,
see the [API
Guide](http://django-rest-framework.org/api-guide/renderers/) or the
[Tutorial](http://django-rest-framework.org/tutorial/1-serialization/).

Running the tests
-----------------

To run the tests against the current environment:

``` {.bash}
$ ./manage.py test
```

### Changelog

1.0.0
-----

-   Initial release

## Thanks

[![PyCharm](docs/pycharm.svg)](https://www.jetbrains.com/?from=drfexts)