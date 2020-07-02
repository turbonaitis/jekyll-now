---
layout: post
title: Representing workflow state
---
It's been a (very) long while, since I've blogged anything here, so now, that I have a bit more time, I've decided to revisit my to-blog list and this is a topic that I wanted to cover for a while. 

When programming workflows it might seem natural to use an enum to represent progress, but it is usually not the right solution and you should track each step individually instead.

## The situation
Imagine you have a workflow, let's say a user registration workflow on a financial website, which has these steps:
 - Enter personal details
 - Verify email

 It might seem natural to represent this as an enum
 {% highlight c# %}
public enum UserState
 {
     PersonalDetailsCompleted,
     EmailVerified
 }
{% endhighlight %}

This means, that if someone has state `PersonalDetailsEntered`, they've completed the personal details page and if they're in `EmailVerified` - they've completed the whole workflow.

## The first problem
While the solution above might have seemed nice and simple, now imagine you want to also collect financial details immediately after the personal details page and so you modify the enum to look like this:
 {% highlight c# %}
public enum UserState
 {
     PersonalDetailsCompleted,
     FinancialDetailsCompleted,
     EmailVerified
 }
{% endhighlight %}

Anyone see the bug yet? As we've just added a new enum member in the middle of the list, we have a problem, if we're using the integer representation of values anywhere in our program. As you probably know, the c# compiler generates integer values for enum members without explicit values defined - starting with 0. So our first enum effectively looked like this
{% highlight c# %}
public enum UserState
 {
     PersonalDetailsCompleted = 0,
     EmailVerified = 1
 }
{% endhighlight %}

and after the update, it now looks like this
 {% highlight c# %}
public enum UserState
 {
     PersonalDetailsCompleted = 0,
     FinancialDetailsCompleted = 1,
     EmailVerified = 2
 }
{% endhighlight %}

To fix this, we should always specify enum integer values explicitly (and starting with 1, if we don't want the first member to be the default), like this
 {% highlight c# %}
public enum UserState
 {
     PersonalDetailsCompleted = 1,
     FinancialDetailsCompleted = 3,
     EmailVerified = 2
 }
{% endhighlight %}

## The bigger issue
Now, the enum above assumes, that anyone, who is in the `FinancialDetailsCompleted` state, has not verified their email yet, while everyone with `EmailVerified` has completed the whole workflow.

What happens if we decide, that we only want to collect the financial details when the email address has been verified? Well, suddenly the assumptions above don't make sense anymore and we're in a bit of trouble. 

We could of course just update everyone with the old final state of `EmailVerified` to the new final state of `FinancialDetailsCompleted`, but imagine a longer workflow, where there are more states and they're being reshuffled in a more complex way - we can't migrate from that.

## The right way to do this
The correct solution is instead of having a single value represent the whole workflow, define a full workflow state
 {% highlight c# %}
public class UserRegistrationState
 {
     public bool PersonalDetailsCompleted {get;set;}
     public bool FinancialDetailsCompleted {get;set;}
     public bool EmailVerified {get;set;}
 }
{% endhighlight %}

Now, instead of assuming which steps have been completed, you'll explicitly have that information. To retain even more information, replace `bool` with `DateTime?`, to know when each step has been completed!

Have you ever used enums to represent workflow state? Did you face any similar issues?