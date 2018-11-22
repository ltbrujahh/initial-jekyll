---
layout: post
title: "Introduction to Fluent Programming"
date: 2018-11-21 21:05:00 +1100
categories: experiment
tags: todayilearned fluent
tdd-youtube-id: GvAzrC6-spQ
---
I admit, it was a very frustrating day at work. It's been more than a week and I have yet to push this particular feature to `Ready for Test` beacuse I'm so bad at TDD (Test Driven Development). I could really hear the frustration from my team members because I just can't quite understand the complexity of how the Test Fixture is setup.

So ... what the heck is TDD? 

I'm glad you asked!  
Here's a fun little video from our Lord and Saviour, Uncle Bob.

{% include embed-youtube.html id=page.tdd-youtube-id %}

And so I should stop **DDT**, and really **TDD**!  
What better way of learning it than to get thrown into the deep end and not drown in the process *(as much)*

I've realised as I become more exposed to the code base that they do a lot of chaining of functions, which I'm not too familar with lately.

Traditional **object-oriented programming** has a set of strict ways on how you model an object. Let's model a `User` for example who has a first name and last name.

```java
public class User
{
    private String firstName;
    private String lastName;

    public User(String firstName, String lastName)
    {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    public String getFirstName() { return this.firstName; }
    public String getLastName() { return this.lastName; }

    public void setFirstName(String firstName)
    {
        this.firstName = firstName;
    }

    public void setLastName(String lastName)
    {
        this.lastName = lastName;
    }
}

User user = new User("Lenard", "Bernardo");
user.setFirstName("Lebnerd");
```

Oh my goodness the horror :scream:, getters and setters? Are you kidding me? And look how we have to set the property, too much typing imo...

Lets kick it up a notch and get rid of them! :smiling_imp:

```java
public class User
{
    public final String firstName;
    public final String lastName;

    public User(String firstName, String lastName)
    {
        this.firstName = firstName;
        this.lastName = lastName;
    }
}

User user = new User("Lenard", "Bernardo");
user = new User("Lebnerd", user.lastName);
```
Hmm... not quite sure if that's better or *worse*. If you want to change their `firstName` or `lastName` you have to recreate the whole user. Even though it enforces immutable properties its not scalable at all if you have like 10 other properties to copy over which is bleh :thumbsdown:.

So what can we do instead? Well we can try and use a more fluent interface, or chain some functions together!

```java
public class User
{
    public String firstName;
    public String lastName;

    public User(String firstName, String lastName)
    {
        this.firstName = firstName;
        this.lastName = lastName;
    }

    public static User create() { return new User("Lenard", "Bernardo"); }

    public User withFirstName(String firstName)
    {
        this.firstName = firstName;
        return this;
    }

    public User withLastName(String lastName)
    {
        this.lastName = lastName;
        return this;
    }
}

User.create()
    .withFirstName("Lebnerd")
    .withLastName("Bernado");
```
Look how *elegant* that looks! :sob:

But doesn't that look like the dreaded getters and setters? And the properties are no longer immutable and could cause some massive issues! :rage:

Yeah, you're definitely right. But what's the difference between having to make the developer jump through hoops just to force it to change its property, or maybe you intentionally want to do that?

Also, we didn't have to reference the user object, which means we can get rid of it altogether (a bit meaningless at this point but it has its uses, *trust me*). And if it wasn't for my coding style, it could be a one-liner!

```java
User.create().withFirstname("Lebnerd").withLastName("Bernado");
```

I mean, how sweet is that?! :grin:

That's a trade-off when considering to use a Builder pattern instead of a traditional POJO *(Plain Old Java Object)*. If you only care about constructing a user and overriding its default first and last name, then we can do it declaratively which handles semantics **a lot** better than getters and setters thats for sure...

Of course this is just a *really basic* example of what fluent programming does, but it does point you into a different direction of programming, away from **object-oriented** and into **functional** programming. I can *sort of* see why university still teaches the old school techniques of object-orientation, it does set you up so then later you can get into the more fun stuff.

If you're going to be language agnostic, you gotta appreciate the right things at the right time.

