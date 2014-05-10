Pagination
================================================================================

A simple paginator for Django that allows you to choose the number of items
per page and features a drop-down menu to jump to the next page.
Twitter Bootstrap friendly.


Requirements:
--------------------------------------------------------------------------------
- jQuery
- Twitter Bootstrap not absolutely required, but recommended.


Installation:
--------------------------------------------------------------------------------

Add `pagination` to your INSTALLED_APPS.

Add `(r'^pagination/', include('pagination.urls')),` to your base urls:

    urlpatterns = patterns('',
        url(r'^admin/doc/', include('django.contrib.admindocs.urls')),
        url(r'^admin/', include(admin.site.urls)),

        (r'^pagination/', include('pagination.urls')),
        ...
    )

Add:
- `<script type="text/javascript" src="{{ STATIC_URL }}pagination/js/pagination.js"></script>`
- `<link type="text/css" href="{{ STATIC_URL }}pagination/css/pagination.css" rel="stylesheet" />`
- `{% load pagination_tags %}`

to the template(s) where you are leveraging pagination. If you're using Django 1.5 or above, you would replace `{{ STATIC_URL }}` with `{% static '[path-to-file]' %}`. Don't forget to also include the statifiles template tag: `{% load static from staticfiles %}`

To show the "items per page" selector:
`{% items_per_page_form 'items-per-page-cache-key' [objects] %}`

`items-per-page-cache-key` could also be a context variable , as long as it's
suitable to be used as a cache key.


To show the "page" selector:
`{% paginator [objects] %}`


Usage:
--------------------------------------------------------------------------------
    # my_view.py

    from pagination.utils import paginate_items

    def some_view(request):
        some_objects = MyModel.objects.all()  # Can also be a list or tuple
        objects = paginate_items(some_objects, 'my_objects_items_per_page', request)

        return render(request, 'my-template.html', {'objects': objects})


Items Per Page
--------------------------------------------------------------------------------
Pagination stores the number of objects per page in session. The number of items
per page can be stored for different sets of paginated objects by handing in the
`cache_prefix` parameter. This parameter is not optional.

Setting the choices for the items per page can be done by providing a tuple of
choices in settings.py:

    PAGINATION_ITEMS_PER_PAGE_CHOICES = (
        ('1', 1),
        ('5', 5),
        # etc
    )

This value will default to:

    ITEMS_PER_PAGE_CHOICES = (
        ('10', 10),
        ('25', 25),
        ('50', 50),
        ('100', 100),
    )

The starting value will default to the first item in ITEMS_PER_PAGE_CHOICES.
This can be overridden by supplying the starting index in settings.py, e.g.:

    PAGINATION_DEFAULT_ITEMS_PER_PAGE_START_INDEX = 1
