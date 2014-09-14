---
layout: post
title: 高效地使用Django ORM
date: 2014-08-24 11:38:56
category: blog
tags: Django ORM
---

遵循以下两条原则，能够帮助我们高效地使用Django ORM写出效率更高的Web应用。

- Reduce the number of SQL queries.
- Minimize the amount of data retrieved from database.
- Django model object is NOT a regular Python object, just an utility used for
  database query.

在实践中，有以下方法。

EDO-1: Combine `QuerySet.select_related` and `QuerySet.only`

EDO-2: QuerySet.values and QuerySet.values_list

EDO-3: DO USE GROUP BY when data statistics

EDO-4: QuerySet.iterator()

EDO-5: Execute SQL directly, that will benefit us on

- saving the time Django generates SQL by parsing method calls
- reducing the complexity of multiple method calls
- making code more clear and understandable

EDO-6: Do not get or build completed Model object, when primary key is required
only

EDO-7: Above is also applied to this item, when use `Manager.[filter|exclude]`
to build query criteria. number is enough for foreign key field

EDO-8: Do not simply give `Model.objects.all()` to `ModelChoiceField.queryset`

EDO-9: Using `QuerySet.bulk_create` when creating lots of models

EDO-10: Prudently reconsider the field validation within Form

EDO-11: Consider to check necessity of existence of primary key. If no strict
business requirement, don't do that.

