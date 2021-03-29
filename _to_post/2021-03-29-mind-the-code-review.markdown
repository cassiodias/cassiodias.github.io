---
layout: post
title:  "Mind the code review"
date:   2021-03-29 15:45
categories: coding, test, stub
---
# ****Mind the code review****

<p align="center">
    <img src="https://davidwalsh.name/demo/code-review.png">
</p>

Code review is ****knowledge sharing.**** It should primarily be presented in the form of exploratory and guiding questions, building up and improving the quality and ****team collaboration****.

Posturing thoughts as questions allow the author to express the reasonings instead of being defensive.

## ****Criticism vs Code Review by Tanner Christensen****

> **Criticism passes judgement — Code review poses questions.**

> **Criticism is personal — Code review is objective.**

> **Criticism is vague — Code review is concrete.**

> **Criticism tears down — Code review builds up.**

> **Criticism is adversarial — Code review is cooperative.**

> **Criticism belittles the person — Code review improves the code and the team.**

## ****What****/****how format****:

* What are some ways to make this code more readable?
* How can the code be optimized to parse the data only once?
* What happens when XYZ chance?

## ****I like****/****wonder****/****wish format****:

* I like how this is flexible and makes it easy to test.
* I wonder what the advantage of looping through the array every time is.
* I wish the JSON can be parsed only once since it’s expensive to read the file every time.

> **"Remember the core principles of asking exploratory questions and not making it personal."**

## ****Avoid you/your****:

* Why did ****you**** do it this way? What is ****your**** reason for this?
* I wonder what ****you**** were thinking about.
* I wish ****you**** didn’t have to parse this JSON multiple times.

## ****Avoid yes****/****no questions****:

* ****Is**** there a better way to do this?
* ****Did**** ****you**** intend this? ****Was**** this intentional?
* ****Should**** this method be broken up?

## ****Reviewer’s responsibilities****:

* Make sure both the reviewer and the author understand why and how the implementation works / might not work.
* Understand the reasoning behind the author’s implementation, regardless of whether you agree or disagree with the implementation.
* Guide the author to think about the why and how by asking questions, as opposed to making direct statements or prescribing solutions.
* Suggest reference materials as needed — examples, documentation, blogs, Wikipedia.
* It’s ok to say “everything looks good” when everything is good.

## ****Author’s responsibilities****:

* ****Review your own code**** and look at your own diffs before submitting code for review. Be confident about what you know and what you don’t know. Understand and explain any compromises you are making.
* ****Explain why and how the implementation works**** at a high level. Simple outlines or diagrams are very helpful. Timebox this effort to keep it short and high level.
* ****Give enough context to the reviewer****. Where does this fit in the big picture? What components are involved directly before/after?
* ****Point out specific areas of concern****, if any, and explain your concern and your approach. These may be edge cases or components you don’t understand that well.
* ****Ask questions if you don’t understand**** the reasoning behind the reviewer’s feedback.
* “I don’t know” and “I haven’t thought about it yet” are perfectly fine answers. ****Take some time to think about the feedback and explore****. Don’t be pressured to have an immediate answer.
* ****Acknowledge the feedback, make a decision, and explain your decision****. You don’t have to address everything at once. If you decide to address some feedback separately, clearly explain the decision and how and why you made that decision.

### ****Sources:****

****Peer Reviews in Software: A Practical Guide****
https://www.amazon.com/exec/obidos/ASIN/0201734850/

****humanizing_review****
http://www.processimpact.com/articles/humanizing_reviews.pdf

****Git creating a pull request template****
https://help.github.com/articles/creating-a-pull-request-template-for-your-repository/
