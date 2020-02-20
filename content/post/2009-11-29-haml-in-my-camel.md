---
categories:
- ruby
comments: true
date: "2009-11-29T00:00:00Z"
title: Haml in my Camel
---

Trying something new .. <a href="http://haml-lang.com/">HAML</a>... I saw it a couple years ago, I was like "ehh.. umm.. nah" ... But working on the DevChix site, it seems like it will be our choice instead of erb for templates so I thought I'd give it a try. 

So here's some straight up html + erb
```
<h1>Listing users</h1>
 
<table>
  <tr>
    <th>Username</th>
    <th>Email</th>
  </tr>
 
<% @users.each do |user| %>
  <tr>
    <td><%=h user.username %></td>
   <td><%=h user.email %></td>
    <td><%= link_to 'Show', user %></td>
    <td><%= link_to 'Edit', edit_user_path(user) %></td>
    <td><%= link_to 'Destroy', user, :confirm => 'Are you sure?', :method => :delete %></td>
  </tr>
<% end %>
</table>
 
<%= link_to 'New user', new_user_path %>
```

And same with haml:
```
%h1 Listing users

%table
  %tr
    %th Username
    %th Email

- @users.each do |user|
  %tr
    %td
      = user.username
    %td
      = user.email
    %td 
      = link_to 'Show', user
    %td
      = link_to 'Edit', edit_user_path(user)
    %td
      = link_to 'Destroy', user, :confirm => 'Are you sure?', :method => :delete

= link_to 'New user', new_user_path
```

I am not totally sold, but.. it is an interesting kind of markup!

Let me know if you see a better way to write it or if you prefer another markup language!
