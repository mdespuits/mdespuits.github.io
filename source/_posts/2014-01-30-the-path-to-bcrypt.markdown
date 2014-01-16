---
layout: post
title: "The Path to BCrypt"
date: 2014-01-30
comments: true
categories:
---

Ok, just for a minute, imagine that you have just inherited a large codebase. In trying to understand how it all works together, you decide to look at the `users` table. What greets your eyes?

If you are a software engineer or have been for any amount of time, you care about security. So when you find that `User` passwords are stored in a poorly encrypted format, your heart sinks. It sinks even more when you realize how many people use your app, so changing it isn't an simple drop it in and it works.

Or is it?

## The Problem

Migration millions of user's passwords to BCrypt is *not* a fast migration. In this case, it would take weeks on an average box. *(I'm not going to go over BCrypt and what it is here, so please [read up on it](http://codahale.com/how-to-safely-store-a-password/) if you are not familiar with it.)* To accomplish this, we will need to support three forms of passwords:

1. The original `SHA256` password
2. The future `BCrypt` password
3. The `BCrypt`ed `SHA256` password

Here is an example of exactly what I mean. (You will need to `gem install bcrypt-ruby`)

``` bash pry
[1] pry(main)> require 'digest/sha2'
=> true
[2] pry(main)> require 'bcrypt'
=> true
[3] pry(main)> password = "password123"
=> "password123"
[4] pry(main)> Digest::SHA256.hexdigest password
=> "ef92b778bafe771e89245b89ecbc08a44a4e166c06659911881f383d4473e94f"
[5] pry(main)> BCrypt::Password.create password
=> "$2a$10$fQqOH7vj.kfS5bo2p7hHQOZNvPwv.MxrNWG5ay0AByZWVpY9ktxsO"
[6] pry(main)> BCrypt::Password.create(Digest::SHA256.hexdigest(password))
=> "$2a$10$bB2qzrislazplghA.1nSPuHgmWTCLELC.66RQPRg2Qjai8WHR3eNu"
```

* Challenges to Overcome
* Extracting to it's own class
