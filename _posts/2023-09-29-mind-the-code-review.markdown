---
layout: post
title:  "Mind the code review"
date:   2023-09-29 12:47
categories: best practices
---

# "Mind the Code Review": A Guide to Effective Collaboration

![Code Review](https://davidwalsh.name/demo/code-review.png)

Date: Sep 29, 2023
Categories: best practices

## Introduction

Code review is more than just checking lines of code. It's a practice that promotes knowledge sharing, enhances code quality, and fosters team collaboration. 
Let's explore the art of code review, focusing on how to turn it into a constructive and collaborative process rather than a critical one.

## The Power of Questions

One of the key aspects of effective code review is asking questions. By posing inquiries, you invite the author to explain their reasoning instead of putting them on the defensive. Let's explore the power of questions by referencing Tanner Christensen's insightful comparison between criticism and code review:

- **Criticism passes judgment — Code review poses questions.**
- **Criticism is personal — Code review is objective.**
- **Criticism is vague — Code review is concrete.**
- **Criticism tears down — Code review builds up.**
- **Criticism is adversarial — Code review is cooperative.**
- **Criticism belittles the person — Code review improves the code and the team.**

## Effective Question Formats

### What/How Format

- **What are some ways to make this code more readable?**
- **How can the code be optimized to parse the data only once?**
- **What happens when XYZ changes?**

### I Like/I Wonder/I Wish Format

- **I like how this is flexible and makes it easy to test.**
- **I wonder what the advantage of looping through the array every time is.**
- **I wish the JSON could be parsed only once since it’s expensive to read the file every time.**

Remember, the core principle is to ask exploratory questions rather than making it personal.

## Steering Clear of "You/Your" and Yes/No Questions

Avoid using "you/your" and yes/no questions, as they can come across as confrontational or restrictive:

- **Why did you do it this way? What is your reason for this?**
- **I wonder what you were thinking about.**
- **I wish you didn't have to parse this JSON multiple times.**
- **Is there a better way to do this?**
- **Did you intend this? Was this intentional?**
- **Should this method be broken up?**

To improve, consider the following options:

- **Provide Context and Explain the Why**: Instead of asking "Why did you do it this way?" or "What is your reason for this?", frame your feedback in a way that seeks to understand the reasoning behind the code. For example, you could say, "I'd like to understand the thought process behind this approach. Could you please explain your reasoning?"
- **Use Neutral Language**: Avoid using judgmental or accusatory language such as "I wonder what you were thinking about." Instead, opt for a more neutral tone that focuses on the code and its functionality. For instance, "Let's discuss the rationale behind choosing this specific implementation."
- **Suggest Improvements Positively**: Instead of saying, "I wish you didn't have to parse this JSON multiple times," you can rephrase it as, "Optimizing this code to parse the JSON only once could enhance its efficiency."
- **Encourage Discussion and Alternatives**: Rather than asking, "Is there a better way to do this?" or "Should this method be broken up?", encourage a discussion by saying, "Let's explore potential alternatives for improving this section of code. Do you think breaking up this method might make it more maintainable?"
- **Express Intent and Seek Clarification**: Instead of asking, "Did you intend this? Was this intentional?", express your understanding of the code's purpose and seek clarification if needed. For example, "I assume this code is meant to handle XYZ. Could you confirm if this was the intended behavior?"
- **Offer Solutions and Explanations**: If you identify issues or suggest changes, provide clear explanations and, when possible, offer potential solutions. This helps the developer understand the problem and how to address it.
- **Use "We" Instead of "You"**: Replace "you" with "we" to make it more collaborative. For instance, "Can we discuss the reasoning behind this approach?" implies a joint effort to understand and improve the code.
- **Focus on Objectivity**: Keep your comments focused on objective code quality, maintainability, and performance rather than personal preferences or opinions.
- **Acknowledge Positive Aspects**: Don't forget to acknowledge positive aspects of the code as well. If you see good practices or well-written sections, it's constructive to highlight them.

Remember that the goal of a code review is to improve code quality and promote learning and collaboration. Using more neutral and collaborative language can lead to more productive discussions and better outcomes for the team.

## Responsibilities of Reviewers and Authors

### Reviewer’s Responsibilities

- Ensure both the reviewer and the author understand how the implementation works or might not work.
- Understand the reasoning behind the author’s implementation, regardless of personal agreement.
- Guide the author to think about the why and how by asking questions instead of prescribing solutions.
- Suggest reference materials when necessary, such as examples, documentation, or blogs.
- It's perfectly fine to say “everything looks good” when it genuinely does.

### Author’s Responsibilities

- Review your own code and inspect your own changes before submitting for review.
- Explain why and how the implementation works at a high level, using simple outlines or diagrams if helpful.
- Provide enough context to the reviewer, explaining where this code fits in the big picture and what components are involved directly before/after.
- Point out specific areas of concern, if any, and explain your concerns and approach.
- Don’t hesitate to ask questions if you don’t understand the reasoning behind the reviewer’s feedback.
- It’s okay to respond with “I don’t know” or “I haven’t thought about it yet.” Take time to ponder the feedback and explore before making a decision.
- Acknowledge the feedback, make a decision, and explain your choice. You don’t have to address everything at once. If you decide to address some feedback separately, clearly explain your decision and rationale.

## Additional Resources

For those interested in diving deeper into the world of code reviews, here are some valuable resources:

- **"Peer Reviews in Software: A Practical Guide"** - [Link](https://www.amazon.com/exec/obidos/ASIN/0201734850/)
- **"Humanizing Review"** - [Link](http://www.processimpact.com/articles/humanizing_reviews.pdf)
- **GitHub's Guide to Creating a Pull Request Template** - [Link](https://help.github.com/articles/creating-a-pull-request-template-for-your-repository/)

Effective code review is not just about finding bugs; it's about nurturing a culture of collaboration and continuous improvement. By embracing the power of questions and following the principles outlined here, your code reviews can become a cornerstone of excellence in your software development journey.