---
layout: post
title: Step Through Your Code
permalink: /step-through-your-code/
tags: programming coding debugging
excerpt_separator: <!--end-excerpt-->
---

<!--end-excerpt-->

Take a look at this code:

```
public interface ResourceHandler {
	public Resource createResource(String resourceName,
		String resourceLibraryName);
}

/* ... */

String resourceId = "resourceLibrary:resourceName";

/* ... */

String[] resourceInfo = resourceId.split(":");

/* ... */

resourceName = resourceInfo[0];
resourceLibrary = resourceInfo[1];

/* ... */

resource = resourceHandler.createResource(resourceLibrary, resourceName);
```

Although the code runs correctly, it would be problematic for future maintenance
because there are two subtle errors in it. So, what is wrong with this code?

First, the `resourceName` and `resourceLibrary` are swapped:

```
resourceName = resourceInfo[0];
resourceLibrary = resourceInfo[1];
```

Since the first part of the resource I.D. is the resource's library, and the
second part is the resource's name, the code should be changed to:

```
resourceLibrary = resourceInfo[0];
resourceName = resourceInfo[1];
```

Second, `resourceName` and `resourceLibrary` are passed in the incorrect order
to `ResourceHandler.createResource(String resourceName, String
resourceLibraryName)`:

```
resource = resourceHandler.createResource(resourceLibrary, resourceName);
```

Instead, `resourceName` should be passed as the first argument, and
`resourceLibrary` should be passed as the second argument:

```
resource = resourceHandler.createResource(resourceName, resourceLibrary);
```

Because the program runs perfectly, these defects would be difficult to
discover. Testing would not reveal these issues. Additionally, only the most
careful review of this code would detect the errors. In complex production code,
a developer would find it even more challenging to identify these issues. So how
can developers detect and prevent errors like these early in the programming
process? Developers must step through their code in the debugger. As Steve
McConnell says in <a href="http://cc2e.com/" target="_blank">his fantastic book,
*Code Complete* (p. 231)</a>:

> ***Step through the code in the debugger*** Once the routine compiles, put it into the debugger and step through each line of code. Make sure each line executes as you expect it to. You can find many errors by following this simple practice.

McConnell is completely correct. By interactively debugging every line, I've
found many errors in my own code (including the bugs that inspired this post).
Stepping through your code in a debugger is an excellent way to reduce obscure
bugs in your software.

<sub><sup>*Code Complete* is copyright Steve McConnell, and the text from it is
published here under fair use for educational purposes.</sup></sub>

## <a href="https://github.com/stiemannkj1/stiemannkj1.github.io/issues/7" target="_blank">Comments</a>

