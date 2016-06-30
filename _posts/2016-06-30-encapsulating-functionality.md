---
layout: post
title: Encapsulating Functionality
permalink: /encapsulating-functionality/
tags: programming coding encapsulation abstraction functions information-hiding
---

Computer science authors and professors often discuss the concepts of <a
target="_blank"
href="https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)">encapsulation</a>,
<a target="_blank"
href="https://en.wikipedia.org/wiki/Abstraction_(software_engineering)">abstraction</a>,
and <a target="_blank"
href="https://en.wikipedia.org/wiki/Information_hiding">information hiding</a>.
These concepts are fundamental to programming, and they are required in order to
manage complex systems. By encapsulating information into separate components,
programmers can reason about those subsystems alone. Instead of understanding
and mentally grasping the whole system at once, coders can partition systems
into coherent sections. Unfortunately, encapsulation, abstraction, and
information hiding are usually discussed with respect to objects and
object-oriented programming (as you can see in the links above). But
encapsulation is equally important in the lower-level tools of functions and
methods.

Fundamentally, functions are designed to encapsulate related functionality.
Because the functionality and information are abstracted away in a function, the
programmer can avoid thinking about those lower-level details when they are
reasoning about the calling code. If the function is well designed, coders can
also modify it more easily because they will not need to think about any
external code when doing so.

Here is an oversimplified example of a function with poor encapsulation:

```
// There are three configuration files:
// custom.config.xml, default.config.xml, and core.config.xml
public List<File> getConfigFiles() {
    ArrayList<File> configFiles = new ArrayList<File>();
    configFiles.add(new File("default.config.xml"));
    configFiles.add(new File("core.config.xml"));
    return Collections.unmodifiableList(configFiles);
}
```

Because the `getConfigFiles()` method requires the developer to know which of
the three configuration files it returns, the method has poor encapsulation.
When the developer wants to use this method, they must think about the
internals. Then they must obtain the **`custom.config.xml`** separately.
Therefore the method does not properly encapsulate the functionality of getting
the configuration files.

In order to fix this, the method must return all three configuration files:

```
// There are three configuration files:
// custom.config.xml, default.config.xml, and core.config.xml
public List<File> getConfigFiles() {
    ArrayList<File> configFiles = new ArrayList<File>();
    configFiles.add(new File("custom.config.xml"));
    configFiles.add(new File("default.config.xml"));
    configFiles.add(new File("core.config.xml"));
    return Collections.unmodifiableList(configFiles);
}
```

Now the details are hidden or abstracted within the method. When the developer
calls `getConfigFiles()`, he or she does not need to know each configuration
file that the method obtains by name. The reverse is also true. The developer
only needs to consider the configuration filenames when modifying the
`getConfigFiles()` method. By ensuring that `getConfigFiles()` truly
encapsulates its named functionality, the developer has made the whole program
easier to understand.

For further reading on constructing excellent functions, check out Chapter 7 of
<a target="_blank" href="http://cc2e.com/">Steve McConnell's excellent book,
*Code Complete*</a>---"High-Quality Routines" (pp. 161-186).

## <a href="https://github.com/stiemannkj1/stiemannkj1.github.io/issues/8" target="_blank">Comments</a>
