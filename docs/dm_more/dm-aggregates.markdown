---
layout:     default
title:      DM-Aggregates
created_at: 2008-09-06 14:11:54.160780 -05:00
---

{{ page.title }}
================

DM-Aggregates provides a reporting API which offers aggregating functions like
`count`, `min`, `max`, `avg`, `sum`, and `aggregate`.

### Setup

Simply `require 'dm-aggregates'` somewhere before you connect to your data-store
and you're ready to go.

### Count

You can issue a totaling query with DM-Aggregates in a couple of ways. First,
you can build a totaling query off of your Resource:

{% highlight ruby %}
  Dragon.count(:homes_destroyed.lte => 12)
{% endhighlight %}

Secondly, you can call `count` off of a pre-build query object.

{% highlight ruby %}
  Dragon.all(:age.lte => 3).count
{% endhighlight %}

Your query will become a totaling query without having to retrieve the objects
and total on the retrieved array. These two approaches can be combined as well.

{% highlight ruby %}
  Dragon.all(:homes_destroyed.lte => 12).count(:rating => 'amateur')
{% endhighlight %}

### Min, Max

Minimum and Maximum values from fields in your data-store can be retrieved by
specifying which property you want to retrieve the value from.

{% highlight ruby %}
  Dragon.min(:homes_destroyed)
  Dragon.max(:homes_destroyed)
{% endhighlight %}

Further conditions to your min and max queries are simply supplied after the
property.

{% highlight ruby %}
  Dragon.min(:homes_destroyed, :battles_won.gte => 20)
  Dragon.max(:homes_destroyed, :title.not => nil, :nickname.like => "%puff%")
{% endhighlight %}

### Sum, Average

Both `sum`, and `avg` work very similarly to `min` and `max`.

{% highlight ruby %}
  Dragon.sum(:toes_on_claw)
  Dragon.sum(:toes_on_claw, :is_fire_breathing => true)

  Dragon.avg(:toes_on_claw)
  Dragon.avg(:toes_on_claw, :is_fire_breathing => true)
{% endhighlight %}

### Aggregate

The `aggregate` method will let you combine all of the other aggregating methods
together to retrieve in one call to the data-store. DM-Aggregates adds in a few
extra symbol operators in order to enable this.

{% highlight ruby %}
  Dragon.aggregate(:all.count, :name.count, :toes_on_claw.min,
    :toes_on_claw.max, :toes_on_claw.avg, :toes_on_claw.sum,
    :is_fire_breathing)
  # [
  #   [ 1, 1, 3, 3, 3.0, 3, false ],
  #   [ 2, 1, 4, 5, 4.5, 9, true ]
  # ]
{% endhighlight %}

This will compile all of the aggregating queries together and return them in an
array of arrays.
