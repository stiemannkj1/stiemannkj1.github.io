---
layout: post
title: Technical Writing
permalink: /technical-writing/
excerpt_separator: <!--end-excerpt-->
tags: [technical-writing, documentation, computer-science, programming, software, software-engineering, coding]
comments_issue_number: 11
---

<img alt="Technical Writing: the Missing Link" src="/images/technical-writing/technical-writing.png" />

## Missing Links in Computer Science Curricula: Part 2.

Programmers are technical writers. Amazing programmers are amazing technical writers. From code to documentation, everything a software engineer does involves technical writing. Developers write [code comments](#code-comments), [source code](#source-code), [documentation (or wikis)](#documentation), [blogs](#blogs), [bug reports](#bug-reports), [feature requests](#feature-requests), [questions](#questions), [answers](#answers), [forum posts](#forum-posts), [emails](#emails), [chat messages](#chat-messages), and more. While technical writing is not completely neglected in Computer Science curricula, its importance should be emphasized due to the sheer amount of writing that developers must produce.[^1] Below I outline some guidelines, principles, and tips which I have found to be helpful for writing different technical documents during my three years as a software engineer.[^2] The guidelines are by no means exhaustive, and like most rules, they can be broken *if* there is a good reason to break them. However, I've found that these rules help make my documentation clear, concise, and comprehensible.

<!--end-excerpt-->

### Code Comments

Although programs should be as self-documenting as possible, developers often need to explain complex code in prose comments for clarity. Code comments should describe **what** the code is doing and/or **why** the code is doing it (this principle is also helpful for version control commit messages). However, comments should never explain **how** the code is doing something because the code already explicitly describes the program's execution.[^3] Occasionally, developers may also need to explain when the code will be executed or reference other pertinent documentation. Unless the project has a policy of commenting on all code, developers should use comments only when the "**what**" or "**why**" behind the code is unclear.[^4] In order to avoid writing comments explaining "**how**", developers should ensure that their comments describe the code at a high-level of abstraction. Here is an example of code with poor comments:

```
// Create a list of config files.
List<File> configFiles = new ArrayList<File>();

// Add custom.config.xml to the list.
configFiles.add(new File("custom.config.xml"));

// Add default.config.xml to the list.
configFiles.add(new File("default.config.xml"));

// Add core.config.xml to the list.
configFiles.add(new File("core.config.xml"));
```

By applying the above principles to the comments and explaining **what** is happening instead of **how** it is happening, the program becomes clearer and less redundant:

```
// Obtain the list of all the config files.
List<File> configFiles = new ArrayList<File>();
configFiles.add(new File("custom.config.xml"));
configFiles.add(new File("default.config.xml"));
configFiles.add(new File("core.config.xml"));
```

When the code is enigmatic, comments can be extremely helpful, but comments are only useful if they explain the high-level methods and reasoning behind the code, not the low-level details described by the code itself.

### Source Code

Unlike all other technical documents, developers are guaranteed to write source code. Great developers have written hundreds of blogs and books on how to create excellent source code, and I will not attempt to repeat them here.[^5] However, I will mention that simplicity and code clarity should be every software engineer's highest priority. Often developers are tempted to prioritize other aspects of a program such as performance (or even cleverness), but performance should only be considered after the code is completely correct and clear. It is more straightforward to make understandable code perform well than to translate obscure, fast code into an intelligible program. If the code is coherent, it will be easier for developers to fix bugs, improve performance, and add features. Prioritize code clarity.

### Documentation

The purpose of documentation is to demonstrate and/or explain how a project, a program, or a section of code should be used. Documentation includes examples, showcases, and descriptions of modules, classes, and methods. Documentation should always be consistent. For example, when explaining the default value of a variable or attribute, many simple sentences or phrases would be perfectly valid, including "Defaults to X.", "The default is Y.", or "Valid values include X, Y, or Z (the default)." More important than the exact wording is the consistency of the wording for all similar documentation. If one method's description says "`Parameter1` defaults to X." and another states "The default value of `Parameter2` is Y.", that is inconsistent. To make documentation more readable, use consistent words and phrases for similar concepts like defaults, preconditions, postconditions, etc. Documentation should also explain the purpose of software components, classes, and methods; for example: "This Factory is designed to generate cryptographically secure random numbers." Similarly, if there are any pitfalls to using a component in a certain way or situation, those dangers should be spelled out clearly---"`FooFactory` is not guaranteed to be thread-safe." Vague or hidden requirements for implementors should also be declared in the documentation; e.g. "`FooFactory.getFoo()` must return a `Serializable` instance of type `Foo`." Documentation should also precisely spell out multi-step processes, so that anyone can follow each step without thinking and get the same end result. In fact, multi-step explanations should be tested by blindly following the steps to ensure that the documentation is correct. Each step should be a simple, self-contained task. Finally, a clear, copy/pasteable example is often more helpful than all of a project's JavaDoc combined.

### Blogs

Blogs and documentation often have overlapping purposes---some blogs showcase examples, others proclaim new features in a release, still others demonstrate common pitfalls and workarounds. In contrast to the formality of documentation however, blogs are informal documents. Developers and users expect blogs to convey excitement, personality, passion, and fun. Certainly, bloggers should still attempt to follow [most of the rules for writing documentation](#documentation), but the colloquial nature of a blog post allows writers to hastily create blogs with little editing and lots of personality. The great strength of a blog post is the speed at which an author can deliver important information to their audience.

### Bug Reports

The initial and most important step to getting a bug fixed is writing a clear and concise bug report. Without a clear report, developers must stumble around in the dark trying to find the issue. Conversely, developers can use a clear defect description to quickly pinpoint an error.  There are several critical details and elements which every bug report should include. First, the developer should include the shortest, simplest code that reproduces the problem. Second, the bug report must specify the environment in which the bug can be reproduced; for example: "The bug occurs with Java Foo 1.1 on Firefox 47.0.1." Third, the developer should write out precise steps to reproduce the issue; e.g.:

> 1. Start up the app server and deploy the example project.
> 2. Navigate to localhost:8080/example
> 3. Click the "*Navigate via Ajax Redirect*" button.

Make sure to test the steps and reproducer to ensure that everything is correct. Fourth, the description should explain what happens as a result of the bug; for instance: "If the bug still exists, no redirect will occur and an error will appear in the console logs." Fifth, the report should describe what would or should happen if the bug did not exist---"If the bug is fixed, the browser will navigate to localhost:8080/example-success via an Ajax redirect." Sixth, the developer should title the report with the simplest description possible; for example: "Ajax redirect fails on Firefox." Usually, a title is easier to write after the rest of the report is written. Optionally, the bug report may include guesses at where the code is broken, ideas for how to fix the issue, and options for working around the bug.

#### Example Bug Report:

> #### Bug 101: Ajax redirect fails on Firefox.
>
> Attachments: example-ajax-redirect-bug-reproducer.zip
>
> Environment: The bug occurs with Java Foo 1.1 on Firefox 47.0.1.
>
> Steps to reproduce:
>
> 1. Start up the app server and deploy the example project.
> 2. Navigate to localhost:8080/example
> 3. Click the "*Navigate via Ajax Redirect*" button.
>
> If the bug still exists, no redirect will occur and an error will appear in the console logs.
>
> If the bug is fixed, the browser will navigate to localhost:8080/example-success via an Ajax redirect.

### Feature Requests

Feature requests are similar to [bug reports](#bug-reports) except that the "bug" is that the feature does not exist. A clear feature request will include an explanation of how the new feature will improve the software for users and/or developers. In order to prove the usefulness of a new feature and ensure the correctness of its implementation, a feature request should include example code which utilizes the unimplemented feature. These examples should be similar to the reproducers of a bug report and should include minimal runnable code examples that will work correctly when the feature is implemented. The feature request may also include code which accomplishes the same goal without the new feature to showcase how arduous the task currently is. Along with examples, the feature request should include any other information necessary to use the proposed feature such as the environment where the feature is useful or the steps to enable or use the feature.

### Questions

The highest goal of a question is answerability. Since technical questions already require massive amounts of expertise to answer, question writers should strive to make queries as easy to answer as possible. Answerable questions look similar to [bug reports](#bug-reports)---they should include example code (or a reproducer), the environment in which the example should be run, the steps to use the example (or reproduce the issue), any error messages, the current functionality, and the end goal (or the expected result). If the asker has tried any apparent "solutions" that don't work, the asker should list the imposter solutions as well. <a target="_blank" href="http://stackoverflow.com/help/mcve">StackOverflow</a> and <a target="_blank" href="http://sscce.org">sscce.org</a> explain these requirements excellently and in more depth, so I only want to highlight the question components which are most often neglected. First, some inquisitors do not even include example code. Without example code, 99.9% of questions are impossible to answer. Include an example. Second, many examples are not minimal. By minimizing an example, the asker makes answering the question easier, shows respect for answerers' time, and speeds up the whole process. Finally, questioners often omit key environmental information or version numbers. Programs can behave differently on different systems. Likewise, distinct versions of software may operate in dissimilar ways. Incorporate relevant version numbers and environment details to ensure that a question is answerable.

### Answers

At first glance, answers seem pretty straightforward and easy. Merely write the code solution, and the answer is complete, right? While a code dump is technically an answer, the best answers are written to help not only the questioner but anyone else who has a similar question.[^6] In order to be more generally helpful, answers should provide three pieces of information: the code, an explanation of the code, and the code in the context of the questioner's program (note that some conceptual questions only require an explanation without much or any code). Meaningful answers should certainly begin with the simplest code that answers the question. Beginning with the code solution allows hasty developers (or developers who already understand the answer) to simply copy and paste the snippet into their project without thinking. After demonstrating the code solution, explain the details of how it works and why it fixes the problem. The explanation may also include information on why the asker's attempted solutions did not work. Thirdly, the answer should include the solution in the context of the questioner's example code. People who are truly struggling to understand the solution may be helped by seeing the code used in an example. Since the questioner likely wrote some sample code, it should be easy to incorporate the solution into that example program. When a question has multiple solutions, write out one code solution, explanation, and example usage for each separate solution (if technologically possible, I recommend writing a completely separate answer for each solution).

### Forum Posts

Forum posts, like blogs, are usually informal versions of [tutorials](#documentation), [bug reports](#bug-reports), [feature requests](#feature-requests), [questions](#questions), or [answers](#answers). If a forum post is like one of the aforementioned documents, it is best to follow the guidelines for that document when creating a post. Because forum posts are informal, some technical or semi-technical information which does not belong in any other technical document may still be appropriate for a forum discussion. However, due to the informality of forums, users feel more comfortable writing harsh complaints or bitterly negative statements in them. Software can certainly be frustrating, but I strongly discourage antagonistic posts. Hostility will only cause problems and make others less willing to help or converse. <a target="_blank" href="http://meta.stackexchange.com/questions/2950/should-hi-thanks-taglines-and-salutations-be-removed-from-posts#3021">Some developers believe that "Thanks!" and other positive statements not directly pertinent to the post at hand should also be removed.</a> I understand the goal of eliminating superfluous, non-technical sentences and phrases, but occasionally a genuine "Thanks for the quick response!" or similar compliment will encourage other users to be more responsive, friendly, and diligent. Developers are people. People appreciate being appreciated.

### Emails

Keep emails brief---extremely brief. Emails are where information goes to die. Only the recipients of an email will see its contents, and if it is a useful email, it will likely contain information that many other people would benefit from. Often the best response to an email is a link to a new [bug report](#bug-reports), [blog post](#blogs), code commit, or other technical document. By creating a new technical document, the information can be preserved in a more appropriate and searchable way for an audience greater than the email's recipients.[^7]

### Chat Messages

If emails are where information goes to die, chat messages are where information goes to hell. Do not attempt to store any information in chat messages for any length of time. If a chat message includes important technical documentation, the information should immediately be moved to a more permanent and appropriate home. Also, instead of starting a chat with a salutation alone, begin chatting with a "hello" message **and** all the relevant information immediately. Commencing a chat with only a "hello" interrupts the receiver's workflow without providing any information except the promise that more relevant information will interrupt them soon.

### Conclusion

Since even the simplest programs are deeply complex, developers should strive to communicate technical information accurately, succinctly, and effectively. In order for software to be "soft," malleable, and modifiable, programmers must make it and its documentation understandable. To create clear documentation, software engineers must understand the purposes, strengths, and weaknesses of different technical documents. Developers should also consider their audience to ensure that they provide the correct amount of formality, technical detail, and information. Technical writers ought to thoroughly test and edit their documents for correctness. Ultimately, technical writing skills are not necessary to make programs, but excellent technical writing skills are integral to making programs that flourish and grow.

Thanks for reading! Was my advice helpful? Did I miss anything obvious? Do you have helpful advice for technical writing? Please let me know in the comments.

---

[^1]: Due to <a target="_blank" href="{{ site.url }}/version-control/#fn:1">my strange college career</a> I never took a class focused on technical writing, but that was my own fault since most curricula include it as far as I know.
[^2]: In my (nearly) four years at Liferay, I've written/edited <a target="_blank" href="https://github.com/liferay/liferay-docs/commit/e0ce60295937fb0021009299cf3e8c3060858dc2">9,000+ words of the documentation for the Liferay Faces Project</a>, and I've written <a target="_blank" href="https://github.com/liferay/liferay-faces-bridge-impl/commit/a4b3318f0f08a9584ab293d20ba3c2c514916538">many</a> <a target="_blank" href="https://github.com/stiemannkj1/liferay-faces-test-selenium/commit/39fcf9779d3809d1f090509081ea06fa17007e0b#diff-007ac632f4a2968b595a74c229688658R93">code</a> <a target="_blank" href="https://github.com/stiemannkj1/liferay-faces-test-selenium/commit/2919b56972daa0b960d86573cd52b516884d0cad#diff-fabe3f3c41b02283c10e2b82b355bec2R46">comments</a>, <a target="_blank" href="https://github.com/stiemannkj1">tons of source code</a>, plenty of API documentation (including <a target="_blank" href="https://github.com/liferay/liferay-faces-util/pull/46/commits/08ac6dee50927052e76eefef94da7506c1d94fe8#diff-69deaedf207ebac155ffd1473060608cR29">JavaDoc</a> and <a target="_blank" href="https://github.com/liferay/liferay-faces/blame/master/alloy/alloy-components.xml">VDLDoc</a>), 10 blog posts (<a target="_blank" href="https://web.liferay.com/web/kyle.stiemann/profile">6 Liferay blogs</a> and [4 personal technical blogs](#fn:7)), <a target="_blank" href="https://issues.liferay.com/issues/?jql=issuetype%20in%20%28Bug%2C%20%22Regression%20Bug%22%29%20AND%20reporter%20in%20%28kyle.stiemann%29">200+ bug reports </a>, <a target="_blank" href="https://issues.liferay.com/issues/?jql=issuetype%20in%20%28%22Custom%20Development%22%2C%20Documentation%2C%20%22Feature%20Request%22%2C%20Improvement%2C%20%22New%20Feature%22%2C%20Story%29%20AND%20reporter%20in%20%28kyle.stiemann%29">70+ feature requests</a>, <a target="_blank" href="https://stackoverflow.com/users/2880970/stiemannkj1?tab=topactivity">100+ StackOverflow Questions/Answers</a>, <a target="_blank" href="https://web.liferay.com/web/kyle.stiemann/profile">450+ forum posts</a>, and countless emails and chat messages.
[^3]: My boss, <a target="_blank" href="https://web.liferay.com/web/neil.griffin/blog">Neil Griffin</a>, taught me this early in my career. He phrased it this way: "Comments should explain **what** the code is doing, not **how** the code is doing it." He learned this from his boss, Beverly Shefstad.
[^4]: Steve McConnel describes a reasonable methodology for commenting on all code in *Code Complete* "Chapter 9: The Pseudocode Programming Process." I do not personally utilize this process, but I think it is worth considering.
[^5]: Steve McConnel's *Code Complete* comes to mind.
[^6]: > For every person who asks a question and gets an answer on Stack Overflow, hundreds or thousands of people will come read that conversation later. Even if the original asker got a decent answer and moved on, the question lives on and may continue to be useful for decades.
      >
      > --- <a target="_blank" href="https://blog.stackoverflow.com/2011/01/the-wikipedia-of-long-tail-programming-questions/">Joel Spolsky</a>

[^7]: This is essentially a summary of <a target="_blank" href="http://www.hanselman.com/blog/DoTheyDeserveTheGiftOfYourKeystrokes.aspx">Scott Hanselman's approach to email</a>.

