---
layout: post
title: "How To Document Ruby"
tags:
  - help
---

## Getting Set Up

Fork MU's fork of Ruby [on github](https://github.com/rmu/ruby).

Clone your new repo:
{% highlight bash %}
git clone git@github.com:YOUR_NAME/ruby.git
{% endhighlight %}

Add the upstream remote:
{% highlight bash %}
git remote add upstream https://github.com/rmu/ruby.git
git fetch upstream
git merge upstream/trunk
{% endhighlight %}

Choose some code to document. You can use our [suggestions for code to document](http://projects.mendicantuniversity.org/mri-list.html) or follow these instructions to generate your own list of undocumented code. Check MU's [fork of Ruby issues](https://github.com/rmu/ruby/issues) for a list of documentation other people are working on.

Get a list of undocumented code (you must have rdoc >= 3.3 for this to work):
{% highlight bash %}
 rdoc -C1 > documentation_coverage.txt
{% endhighlight %}

Search for "is not documented" in "documentation_coverage.txt". Find something that interests you, and document it. You can see how much of Ruby is documented by looking at the stats at the bottom of the file.

## Documenting

To help others know what you're documenting, [open an issue](https://github.com/rmu/ruby/issues/new) on MU's fork of Ruby.

Write your new documentation.

- For writing top-level docs in C files, look for `Document-class` or `rb_define_class` (may be towards the bottom of the file.) For writing method docs, look for `rb_define_method`, and then look for the function it uses.
- To emit `<code>foo.bar</code>`, you can use `+foo.bar+`. It doesn't always work on operators, so use `<code>==</code>`, not `+==+`.
- Use `#foo` when writing about the foo method - it'll make a link to foo's documentation. `Foo.new(42)` will create a link to the constructor's docs.

Generate your new HTML documentation:
{% highlight bash %}
 rdoc -o tmpdoc PATH/TO/YOUR/FILES
{% endhighlight %}

This for example will document all `drb` files:
{% highlight bash %}
 rdoc -o tmpdoc lib/drb*
{% endhighlight %}

Preview your new documentation in `tmpdoc/index.html`.

> Tip: You can use the `adsf` gem to serve your files locally, similar to `gem server`.  Install the gem and `cd tmpdoc && adsf`. It defaults to port&nbsp;3000.

Once it looks good delete your extra files:
{% highlight bash %}
 rm -rf tmpdoc documentation_coverage.txt
{% endhighlight %} 

Add your documentation change:
{% highlight bash %}
 git add .
{% endhighlight %}

Commit your documentation change:
{% highlight bash %}
 git commit -m "adding documentation for WHAT_YOU_CHANGED"
{% endhighlight %}

Submit a pull request to MU's fork if you want a quality review, or need help creating a patch to submit to Ruby Core. Or if you have something that's ready to go, you can submit the patch yourself (see instructions below). You should **not** submit a pull request to ruby/ruby, they don't use github's mechanism for this.

## Instructions for creating your own patch

Create a patch to submit to ruby-core:
{% highlight bash %}
 git format-patch HEAD~1
{% endhighlight %}  

[Open a new ticket](http://redmine.ruby-lang.org/projects/ruby-19/issues/new) on RedMine and submit your patch (will be called something like "0001-\*.patch" in the root directory of the project). You'll need to [sign up](http://redmine.ruby-lang.org/account/register) if you haven't before. Assigning Eric Hodel to the ticket should help make sure your patch is pulled in a timely manner.

## Additional Information

[Here is](https://github.com/ruby/ruby/commit/071a678a156dde974d8e470b659c89cb02b07b3b) an example of a documentation patch that Eric Hodel added. You can reference this for style.

Thanks to [Steve Klabnik](http://steveklabnik.com/) for his [excellent post](http://blog.steveklabnik.com/2011/05/10/contributing-to-ruby-s-documentation.html) on which most of this information is based.