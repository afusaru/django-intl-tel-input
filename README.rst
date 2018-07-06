Django intl-tel-input
=====================

[![image]]

[![image][1]]

A Django form widget for international telephone numbers based on the
jQuery plugin [intl-tel-input].

This is a new package, so it doesn\'t implement all the features of
intl-tel-input. However, it is well tested, and has been stable in
production.

Version support
---------------

Tested on the following versions of Python and Django.

Python: 2.7, 3.4, 3.5, 3.6
Django: 1.8, 1.9, 1.10, 1.11

Installation
------------

Install from PyPI.

``` {.sourceCode .shell}
pip install django-intl-tel-input
```

Add intl-tel-input to your INSTALLED\_APPS, so Django can find the init
script.

``` {.sourceCode .python}
...
INSTALLED_APPS += ('intl_tel_input',)
...
```

Usage
-----

Simply add `IntlTelInputWidget` to your form field.

``` {.sourceCode .python}
from intl_tel_input.widgets import IntlTelInputWidget

class MyForm(forms.ModelForm):
    class Meta:
        model = MyModel
        fields = ['foo', 'bar']
        widgets = {
            'bar': IntlTelInputWidget()
        }
...
```

With a standard form:

``` {.sourceCode .python}
class MyForm(forms.Form):
    tel_number = forms.CharField(widget=IntlTelInputWidget())

...
```

Form media
----------

Include `{{ form.media.css }}` in the `<head>` of your template. This
will ensure all styles are parsed before the widget is displayed.

If you have included jQuery at the end of your document, then don\'t
forget to update the template where this widget appears with a
`{{ form.media.js }}`. Put it in a block that allows it to come after
jQuery.

If you\'re using [crispy-forms], the static content will be inserted
automatically beside the input. To prevent this, be sure to set
`include_media = False` on your form helper.

``` {.sourceCode .python}
class MyForm(forms.Form):
...
    def __init__(self, *args, **kwargs):
        self.helper = FormHelper()
        self.helper.include_media = False
...
```

If you need to load all JS in the head, you can make the `init.js`
script wait for the document to be ready with the following snippet.

``` {.sourceCode .javascript}
jQuery(document).ready(
  {{ form.media.js }}
);
```

All this assumes your form context variable is called `form`.

  [image]: https://travis-ci.org/benmurden/django-intl-tel-input.svg?branch=master
  [![image]]: https://travis-ci.org/benmurden/django-intl-tel-input
  [1]: https://img.shields.io/codecov/c/github/benmurden/django-intl-tel-input.svg
  [![image][1]]: https://codecov.io/gh/benmurden/django-intl-tel-input
  [intl-tel-input]: https://github.com/jackocnr/intl-tel-input
  [crispy-forms]: https://github.com/django-crispy-forms/django-crispy-forms

Options
=======

The widget can be invoked with keyword arguments which translate to the
options available in intl-tel-input.

allow\_dropdown

:   Shows the country dropdown. Default: `True`

default\_code

:   Country code selected by default. Overridden when using
    `auto_geo_ip`. Default: `'us'`

preferred\_countries

:   Array of countries that will always appear at the top of the
    dropdown. Default: `['us', 'gb']`

auto\_geo\_ip

:   When True, [freegeoip] will be used to autodetect the user\'s
    country via Ajax. There is a limit of 15,000 queries per hour, so it
    should not be used on high-traffic sites. Alternatively use
    [pygeoip], detect server-side, then set the `default_code`. Default:
    `False`

  [freegeoip]: https://freegeoip.net
  [pygeoip]: https://pypi.python.org/pypi/pygeoip
