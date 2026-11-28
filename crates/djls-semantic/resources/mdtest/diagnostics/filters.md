# Filter diagnostics

## filter requires an argument

```htmldjango
{{ value|default }}
```

```snapshot
error[S115]: Filter 'default' requires an argument
 --> test.html:1:10
  |
1 | {{ value|default }}
  |          ^^^^^^^
```

## filter does not accept an argument

```htmldjango
{{ value|upper:"arg" }}
```

```snapshot
error[S116]: Filter 'upper' does not accept an argument
 --> test.html:1:10
  |
1 | {{ value|upper:"arg" }}
  |          ^^^^^^^^^^^
```

## filter whose first parameter is named self

Django counts `self` like any other parameter, so this filter requires an argument.

`settings.py`:

```py
INSTALLED_APPS = []
TEMPLATES = [{'BACKEND': 'django.template.backends.django.DjangoTemplates', 'DIRS': ['/templates'], 'APP_DIRS': False, 'OPTIONS': {'builtins': ['self_filters'], 'libraries': {}}}]
```

`self_filters.py`:

```py
from django import template
register = template.Library()
@register.filter
def with_self(self, value): return value
```

```htmldjango
{{ value|with_self }}
{{ value|with_self:"arg" }}
```

```snapshot
error[S115]: Filter 'with_self' requires an argument
 --> test.html:1:10
  |
1 | {{ value|with_self }}
  |          ^^^^^^^^^
```
