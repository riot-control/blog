---
layout: post
author: Paulo Silva
title: "Refactoring Views with Presenters (Ruby on Rails)"
date: 2013-11-27
comments: true
categories: [software development]
tags: presenters ruby rails
---

In a **Ruby on Rails** application, views can get really messy over time. They tend to grow into large files with **if else blocks** scattered all over the place.

Lately I've been working on an existing application with views like these. My goal was to remove all the logic from the views into **presenters**, making the views smaller, more readable and easier to maintain.

<!-- more -->

**Let's look at an example:**

{% codeblock lang:erb %}

<%# api.html.erb (before refactoring) %>

<div class="span9 no-margin">
  <% if @user.current_membership.api_is_enabled? %>
    <p>
      <span class="api_key">
        <%= @user.current_membership.api_key %>
      </span>
    </p>

    <div class="mButton float_right">
      <%= link_to t('settings.api_regenerate'), api_key_path, :method => :post, :class => 'aButton' %>
    </div>

    <div class="mButton float_right">
      <%= link_to t('settings.api_disable'), api_key_path, :method => :delete, :class => 'aButton' %>
    </div>
    
  <% else %>
      <div class="mButton float_right">
        <%= link_to(t('settings.api_generate'), api_key_path, :method => :post, :class => 'aButton') %>
      </div>
  <% end %>
</div>

{% endcodeblock %}

This view renders different html depending on the condition **@user.current_membership.api_is_enabled?**. What if in a near future another condition is needed? Probably an **elsif** would be added here and the view would grow even more. At some point it gets hard to understand the structure of the html. And when you know it, there's already repeated code in the view. Yes, you can always put that code in a helper. But I'm not a fan of that approach either. Why? Because when calling helper methods in the view, it's not obvious where they come from. It feels like you're calling a function, not a method. Also, if multiple helpers are included in the view and there are methods with the same name, you'll get unexpected behavior.

Now, by placing the logic in a presenter you gain two things. First, the view contains only the html structure of the page. There is no decision making. The view acts simply as a template that shows data. Second, the code is placed in methods of the presenter. And by giving meaningful names, it becomes clear what the code does.

**Here's how the presenter looks like:**

{% codeblock lang:ruby %}

# api_key_presenter.rb

class ApiKeyPresenter

  def initialize(controller, view, user)
    @controller = controller
    @view = view
    @user = user
  end

  def api_key
    if api_key_enabled?
      "#{t('settings.api_description')}<span class='apiKey'>#{current_api_key}</span>"
    end
  end

  def generate_api_key_button
    @view.link_to t('settings.api_regenerate'), path(:api_key_path), :method => :post, :class => 'smallPrimaryButton rightButton'
  end

  def disable_api_key_button
    if api_key_enabled?
      @view.link_to t('settings.api_disable'), path(:api_key_path), :method => :delete, :class => 'smallDeleteButton'
    end
  end

private

  def api_key_enabled?
    @user.current_membership.api_is_enabled?
  end

  def current_api_key
    @user.current_membership.api_key
  end

  def path(sym, args=nil)
    @controller.send(sym, args)
  end
end

{% endcodeblock %}

The **presenter** has three public methods that can be used in the view: **api_key**, **generate_api_key_button** and **disable_api_key_button**. These method's names are clear and it's easy to understand what they do. The logic is in the presenter and the code is reusable and more maintainable.

**Here's how you can create the presenter in the controller:**

{% codeblock lang:ruby %}

# users_controller.rb

class UsersController < ApplicationController

  def api
    user = current_account.users.find(params[:id])
    @presenter = ApiKeyPresenter.new(self, @template, user)
  end

end

{% endcodeblock %}

As you can see, three arguments are passed to the presenter: **self** (the controller), **@template** (the view) and the **user**.

(Note: In Rails 2 you can use **@template** to access the view. In Rails 3, **@template** would be replaced by **view_context**.)

**Let's see what the view looks like when using the presenter:**

{% codeblock lang:erb %}

<%# api.html.erb (after refactoring) %>

<div class="span9 no-margin">
  <p class="description">
    <%= @presenter.api_key %>
  </p>

  <div id="containerBodyActions">
    <%= @presenter.generate_api_key_button %>
    <%= @presenter.disable_api_key_button %>
  </div>
</div>

{% endcodeblock %}

The view looks a lot nicer now. The html structure is clear, there's no logic in the view and there's no repeated code.

<h1>Conclusion</h1>

This example illustrates one of the many views I refactored. Not all of them were this simple but I think it's easier to show my point of view with a simple case.

These are the three major benefits I think this approach offers:

* Cleaner view
* DRY code (presenter methods)
* Separation of concerns

Next time I start a new Ruby on Rails app, I will definitely use **presenters**.
