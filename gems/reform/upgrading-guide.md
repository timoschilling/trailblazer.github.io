---
layout: default
---

# Upgrading Guide

We try to make upgrading as smooth as possible. Here's the generic documentation, but don't hesitate to ask for help on IRC.

## 2.0 to 2.1

* In a Rails environment with ActiveModel/ActiveRecord, you have to include the [reform-rails](https://github.com/trailblazer/reform-rails) gem.

{% highlight ruby %}
gem "reform"
gem "reform-rails"
{% endhighlight %}


## 1.2 to 2.0

### Validations

Validations like `validates_acceptance_of` are not available anymore, you have to use the new syntax.

{% highlight ruby %}
validates acceptance: true
{% endhighlight %}

### Form#valid?

Using `form.valid?` is a private concept and was never publicly documented. It is still available (private) but you are strongly recommended to use `#validate` instead.

### Form#update!

Apparently, some people used `form.update!({..})` to pre-fillout forms. `#update!` has never been publicly documented and got removed in Reform 2. However, you can achieve the same behavior using the following hack.

{% highlight ruby %}
Reform::Form.class_eval do
  alias_method :update!, :deserialize
  public :update!
{% endhighlight %}

### Validation Backend

This only is necessary when _not_ using `reform/rails`, which is automatically loaded in a Rails environment.

In an initializer, e.g. `config/initializers/reform.rb`.

{% highlight ruby %}
require "reform/form/active_model/validations"
Reform::Form.class_eval do
  include Reform::Form::ActiveModel::Validations
end
{% endhighlight %}
