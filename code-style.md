# Code Style

The most important thing when working on a team is **communication**. People need to be able to work together effectively and the only way to do that is by communicating. As developers, **we communicate primarily through code**. We communicate with other parts of the software through code and we communicate with other developers through code.

Always remember:  *“Programs are meant to be read by humans and only incidentally for computers to execute.”*

## Java

We use the [Google Java Code Style](https://google.github.io/styleguide/javaguide.html) with line wrap set to **120 columns** (our monitors are big!)

### Eclipse


How to import XMLs:

	Eclipse -> Preferences -> Java -> Code Style -> Code Templates/Formatter

- Import `java/eclipse/java-code-formatter-v1.xml` into `Formatter`
- Import `java/eclipse/java-code-template-v1.xml` into `Code Templates`

*Note:* If you want to generate the comments automatically, you can toggle on Option `Automatically add comments for new methods and types` under `Code Templates` tab.

### Intellij IDEA

How to import the Jar:

	File -> Import Settings
  Select `java/intellij/java-code-style-v1.jar`

Then, you will be guide to restart the Intellij IDE. All set!

## JavaScript

We use [airbnb](https://github.com/airbnb/javascript) guidelines. Specially 2 spaces indentation.

## Python

We follow the [PEP8 style guide for Python](http://www.python.org/dev/peps/pep-0008/) with a few modifications like 4 spaces -- never tabs -- for indentation. **This is a strict rule and ignoring this can (has) cause(d) bugs.**

`pylint` will help you check that you are not breaking the rule. We have some checks that are disable like 80 characters max for line length.

```none
pylint . --disable=F0401,E0611,E1124,E1004,C0111,I0011,I0012,W0704,W0142,W0212,W0232,W0613,W0702,R0201,W0614,R0914,R0912,R0915,R0913,R0903,R0904,R0801,C0301
```
