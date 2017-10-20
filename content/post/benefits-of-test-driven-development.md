title: "Implement the Commanding Pattern"
date: 2016-05-11
tags: ["OOP", "TDD"]
draft: false
---

## The Benefits of Test-Driven Development

Many developers distance themselves from Test-Driven Development (TDD). There is a notion that writing tests is a waste of time. In my experience, I have encountered and written code that I found hard to trust at some points. As a developer, you need to be able to have faith in your code, otherwise you find yourself having sleepless nights thinking about a failing code deployment. I have found Test-Driven Development to be of great value in software development, and here are some benefits that I found:

#### Decoupling

Test-Driven Development forces you to write tests before implementation. This in turn forces you to be short and concise since you must define your input and expectations before writing any code. When your code is derived from tests, you inevitably separate the different pieces as much as possible. Decoupled code is easily maintainable. 

#### Up to date documentation

While it seems like a good idea to only rely on a living document, as time goes on and developers add more features or fix bugs, that document usually becomes neglected. However, enforcing TDD means developers have to add new tests for new features or write regression tests for any bugs. As a result, tests can act as documentation that cannot go out-of-date. 

#### Refactor with confidence

Have you ever been unable to refactor code because of the fear that something will break? I have seen code that cries out loud for refactoring (including some code I have written in the past). I have sat there and watched classes grow into monster objects with duplicates because refactoring would be like opening a pandora box. With tests, you can refactor code with confidence because you are always going to run tests after each refactor and if the test fails you can immediately revert it back.

#### Minimize deployment errors

If you often find yourself afraid to deploy to production because the result is unknown, TDD can help you allay those fears. Writing code without bugs doesn’t mean that it works. Adding new features to a project won't be a daunting task down the road. I have worked on projects where I have come back a few months or years later to add features. Without written tests, I always had to keep my fingers crossed that nothing breaks. Well written tests will eliminate this headache as the tests will provide constant feedback that each component is still working. 

Originally posted on <a href="http://majestykapps.com/">Majestyk</a>