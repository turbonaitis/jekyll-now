---
layout: post
title: "Please, learn to write great tests"
tags: interviews testing
---

While hopefully everyone in the industry already knows what automated testing is, people are doing it in many different ways. Most often people experience this when joining a new company, but I've a privilege to see a lot of different ways people approach testing during the interview process. Sadly, in many cases, testing is the weak part of the submissions. So if you need one more reason to learn to write beautiful tests, know that this will allow you to stand out during the interview!

Here are some of the problems that I see in the tests some candidates write:
* Tests are hard to follow

    This is probably the most common problem. The first thing that helps anyone understand what the test is doing is naming. "getItemsTest" is certainly better than "test1", but it still doesn't tell us much about what the test is really about. There is more than one good way to name your test, several mentioned [in this article](https://dzone.com/articles/7-popular-unit-test-naming). The key thing to keep in mind is that by reading that test name, people should be able to get some idea what actions the test might take and what outcomes it is verifying. 

    And then of course there are the test code itself. I try to keep to the AAA convention (Arrange, Act, Assert) and keep a single test to one logical assertion. If either of these three steps becomes more than a couple of lines of code - move that to a separate method - `prepareValidCustomer();` clearly communicates what this arrangement step is doing - setting up different properties on a `Customer` object - not necessarily so.

* The tests are meaningless

    Somewhat related to the first bullet point. Let's take this for example:
    ```java
    @Test
    void testLoadCustomers(){
        CustomerRepository repository = new CustomerRepository();
        assertThat(repository.loadCustomers().size() > 0).isTrue();
    }
    ```
    I'm not kidding, I've seen such tests in real code bases! What is this test trying to ensure? Are we really ok with receiving both 1 and 10^8 results? Should these results be always the same? 

    We should be very explicit about the outcome we expect and it should very rarely be "any number greater than 0".

* The test coverage is sparse

    This is something that I see both in interview code and in real life - if something is hard to test, people do not test it. I am by no means religiously advocating a 100% test coverage, but we should choose what to test based on whether the stability of that piece of code is important, not whether it's easy to test this particular bit. More importantly, if something is hard to test, that's quite often a sign of poor code design. Of course, certain things are just hard to test, period, but those are usually solved problems and you can just google them. One of the classically hard things to test is anything that has to do with the flow of time. Try Googling about it and you'll see there are many good ways to test such scenarios.

* Test failures are puzzling

    If a test fails, it should give us the general idea of what went wrong. Several factors contribute to this - having clear naming and a small number of cohesive assertions certainly helps, but other things can stand in the way of understanding failures as well. Let's take this assertion as an example:
    ```java
    assertThat(actualCustomer).equals(expectedCustomer);
    assertThat(isCustomerActive).isTrue();
    assertThat(!isCustomerDeactivated).isTrue();
    ```
    There are several issues with this assertion code:
    * You can get a failure, that reads something like this `Expected true, but got false!`. It's clear, that either the second or the third assertion failed, but we can't tell which from just this message. The solution is usually simple - every assertion framework allows you to specify some context or error message, that goes with the default error message. 
    * The third assertion is hard to read and again, makes the error message harder to attach to the actual issue. Instead of negating the actual value and asserting that it's true, let's just assert it's false straightaway. In general, you should never transform the "actual" value you're checking. 
    
        With these two recommendations, let's rewrite the last two assertions as follows:
        ```java
        assertWithMessage("Customer should be active").that(isCustomerActive).isTrue();
        assertWithMessage("Customer should not be disabled").that(isCustomerDeactivated).isFalse(); 
        ```
        Much better!

    * This last bit is not an issue for the tests you write during your interviews, but crucial for real tests. Imagine you get this error message `Expected values to be equal. Expected: CustomerClass@123, Actual: CustomerClass@321`. 

        While this clearly points us to the failed assertion, but it really doesn't help with the investigation at all. The solution depends on your assertion library, but in a lot of cases (like with the [Google Truth](https://github.com/google/truth) library used in these examples) you'll be seeing a `toString()` representation of your instances in the error messages, so to make your life easier, make sure your `toString()` method prints out all the values, that are being used in `equals()`. And we all know, that we shouldn't usually leave the default `equals()` implementation for data objects in most cases, right? :) Alternatively, you can find (or write) a library, that does a deep comparison of two objects and prints the differences, like the [Compare .NET Objects](https://github.com/GregFinzer/Compare-Net-Objects) for dotnet. In the end, we should end up with something like this `Expected values to be equal. Expected: CustomerClass{name=Jane, surname=Doe}, Actual: CustomerClass{name=Jane, surname=Ipsum}`.

The list above is by no means exhaustive and there are many more pitfalls when writing tests, implementing the suggestions will definitely make your tests better.