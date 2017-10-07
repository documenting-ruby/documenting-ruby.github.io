---
layout: post
title: "How To Document Ruby"
tags:
  - help
---

<a name="SetUp"></a>
## Getting Set Up

Fork the [ruby repository on GitHub](https://github.com/ruby/ruby).

Clone your new repo:
{% highlight bash %}
git clone git@github.com:ruby/ruby.git
{% endhighlight %}

Add the upstream remote:
{% highlight bash %}
git remote add upstream https://github.com/ruby/ruby.git
git fetch upstream
git merge upstream/trunk
{% endhighlight %}

Choose some code to document. You can use our [suggestions for code to
document](undocumented-areas.html) or follow these instructions to generate
your own list of undocumented code. Check ruby's [list of Doc-labeled Pull
Requests](https://github.com/ruby/ruby/labels/Doc) for a list of
documentation other people are working on.

Get a list of undocumented code (you must have rdoc >= 3.3 for this to work):
{% highlight bash %}
rdoc -C1 > documentation_coverage.txt
{% endhighlight %}

You can also pass a list of files to check the documentation coverage, if you
already have something in mind:
{% highlight bash %}
rdoc -C1 lib/drb* > documentation_coverage.txt
{% endhighlight %}

Search for "is not documented" in "documentation_coverage.txt". Find something
that interests you, and document it. You can see how much of Ruby is documented
by looking at the stats at the bottom of the file.

<a name="Documenting"></a>
<a class="top" href="#">top</a>
## Documenting

Write your new documentation.

- For writing top-level docs in C files, look for `Document-class` or
  `rb_define_class` (may be towards the bottom of the file). For writing method
  docs, look for `rb_define_method`, and then look for the function it uses.
- To emit `<code>foo.bar</code>`, you can use `+foo.bar+`. It doesn't always
  work on operators, so use `<code>==</code>`, not `+==+`.
- Use `#foo` when writing about the foo method - it'll make a link to foo's
  documentation. `Foo.new(42)` will create a link to the constructor's docs.

Generate your new HTML documentation:
{% highlight bash %}
rdoc -o tmpdoc PATH/TO/YOUR/FILES
{% endhighlight %}

This for example will document all `drb` files:
{% highlight bash %}
rdoc -o tmpdoc lib/drb*
{% endhighlight %}

Preview your new documentation in `tmpdoc/index.html`.

> Tip: You can use the `adsf` gem to serve your files locally, similar to `gem
> server`.  Install the gem and `cd tmpdoc && adsf`. It defaults to
> port&nbsp;3000.

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

When you have something that's ready to go, you can submit a patch (see
instructions below).

<a name="CreatePatch"></a>
<a class="top" href="#">top</a>
## Instructions for creating your own patch

Create a patch to submit to ruby-core:
{% highlight bash %}
 git format-patch HEAD~1
{% endhighlight %}  

If you're having trouble generating a patch, but you have committed your
changes to GitHub, simply append `.patch` to the end of your commit. For
example, if you take the [following
commit](https://github.com/ruby/ruby/commit/071a678a156dde974d8e470b659c89cb02b07b3b)
and [add `.patch` to the
url](https://github.com/ruby/ruby/commit/071a678a156dde974d8e470b659c89cb02b07b3b.patch)
you can save this page as a patch and upload it to the bug tracker.

[Open a new ticket](https://bugs.ruby-lang.org/projects/ruby-trunk/issues/new)
on RedMine and submit your patch (will be called something like "0001-\*.patch"
in the root directory of the project). You'll need to [sign
up](https://bugs.ruby-lang.org/account/register) if you haven't before.
Assigning drbrain or zzak to the ticket should help make sure your patch is
pulled in a timely manner.

## Additional Information

[Here
is](https://github.com/ruby/ruby/commit/071a678a156dde974d8e470b659c89cb02b07b3b)
an example of a documentation patch that Eric Hodel added. You can reference
this for style.

Thanks to [Steve Klabnik](http://steveklabnik.com/) for his [excellent
post](http://blog.steveklabnik.com/2011/05/10/contributing-to-ruby-s-documentation.html)
on which most of this information is based.
