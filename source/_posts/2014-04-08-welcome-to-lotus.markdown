---
layout: post
title: "Welcome to Lotus: A minimal and testable Ruby web framework"
date: 2014-04-11 16:29:47 -0500
comments: true
categories: tdd, lotus
---

If you've been developing in Rails for any period of time, you know there are certain parts of Rails that are just difficult or frustrating to work with when you are trying to test (cough, cough, controllers, cough). I have gone back and forth between not testing them, testing them, testing them via acceptance tests with capybara, etc. It's hard to do well.

This is one of the reasons the new framework [Lotus](http://lotusrb.org/) caught my eye a few months back. It's another entry in the minimal web frameworks for Ruby, but it has a few unique features that set it apart in my mind.

# So, what is Lotus?

As stated on the website...

> Lotus is an Open Source software for MVC web development. It's simple, fast, and lightweight.

I couldn't put it better. If you go to the Github [Lotus organization](https://github.com/lotus/), you will notice that several of the pieces that we normally associate with Rails are in separate gems. Controllers, routes, views, and models are all pulled apart into their own gems.

It gets better. Each gem is independent of the others. I'm not sure if very many people know how to use `actionview` or `actionpack` outside of Rails. The entire point is to make it easy to plug in only pieces that we want or need for our web app. Need a router? Awesome. Need views? Nope, we don't need them.

Testing is also fast. Very fast.

> ...mind-blowingly fast. - Jeremy Clarkson

# What's the big deal?

Ok, it's another framework. It's another paradigm to learn and wrap your head around. What makes it a big deal is how minimal and how uninvasive it is to test.

Not only that, the source itself is easy to read and understand. It's extremely well-documented and has 100% unit test coverage. In short, I would find it *infinitely* easier to jump in and contribute to a Lotus framework than Rails because of how simple to follow the source really is.

Let's take a controller example:

```ruby controllers/home_controller.rb
class HomeController
  class Index
    include Lotus::Action

    expose :planet

    def call(params)
      @planet = 'World'
    end
  end

  class Create
    include Lotus::Action

    expose :planet

    def call(params)
      redirect_to '/'
    end
  end
end
```

Pretty dead simple, right? No views, not routing, just a controller with a mixin `Lotus::Action`. Doesn't even do much except return or redirect. Here are my specs that over all that I care about.


```ruby spec/controllers/home_controller_spec.rb
require 'spec_helper'

class HomeController
  describe Index do
    it "calls an action" do
      action = Index.new
      response = action.call({})

      expect(response[0]).to eq 200
      expect(response[1]).to eq({"Content-Type" => "application/octet-stream"})
      expect(action.exposures).to eq({planet: "World"})
    end
  end

  describe Create do
    it "redirects back home" do
      response = Create.new.call({})
      expect(response[0]).to eq 302
      expect(response[1]["Location"]).to eq "/"
    end
  end
end
```

And to make sure I'm not pulling any punches, here is the source for my `spec_helper.rb`.

```ruby spec_helper.rb
require 'lotus/controller'
require_relative '../controllers/home_controller'
```

That's it! All I need is the gem and my file and I can run my specs...

```bash
$ rspec spec/controllers/home_controller_spec.rb
..

Finished in 0.00353 seconds
2 examples, 0 failures

Randomized with seed 48373
```

Success!

And if you aren't sure how to test something, copy a spec from Lotus itself. They are that simple and that straightforward. You can test each part of your Lotus application without being too coupled to another part.

# So you've intrigued me. What's it good for?

Well, given how small it is, it would be ideal for a small website, a basic blog, a marketing front-page, or maybe an API endpoint. Given how new it is, I wouldn't go trying to convince your boss to switch to it from a large Rails app, especially since it is so new. But given enough time it may work well

# Is it ready?

In short, yes and no.

[Luca Guidi](https://github.com/jodosha) has been releasing one piece of the Lotus framework each month. At the time of this writing (April 8, 2014), `Lotus::Model` has not been released, but who needs models when in the meantime you have POROs!
