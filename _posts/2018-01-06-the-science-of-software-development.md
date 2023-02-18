---
layout: post
title: "The Science Of Software Development"
date: 2018-02-28 06:30
categories: [English, Coding]
keywords: software development, science, benchmark tests, cyclomatic complexity, halstead complexity
author: Özgür Adem Işıklı
post_img: 06.jpg
post_img_link: https://pixabay.com/en/space-center-spacex-control-center-693251
lang: en
---

I am not **_Uncle Bob_** or **_Martin Fowler_**. I am just a developer who develop web applications in all day by using another frameworks and libraries. I am not creating a new approach about how to use your classes or your patterns in cases you have. Nevertheless I am sick of reading somethings which is not scientific about software development. In this article, I want to talk about that.

### About Software Development

Software development is not like mechanical engineering or other engineering branches. It is new like a baby and it needs time to improve its discipline. But I think that we confuse science and advertisement. Read all slogans of major frameworks.

> "X makes it painless to create interactive UIs"

> "Achieve the maximum speed possible.."

> "An amazing ORM, painless routing, powerful queue library, and simple authentication give you the tools..."

...

These are not about science and don't prove anything. They are just about advertising. And using a framework is not the only or the most important indicator how fast you code. If we want to compare languages or frameworks each other, we should use scientific metrics which we have.

### Benchmark Tests

Most popular metric which we can use to compare softwares is benchmark test. But I disagree that it is the answer of all questions you have. Because if you want to compare two different things, you have to define the borders and you must consider the results under that borders.

You can review the fastest web framework benchmark results. [\*](https://www.techempower.com/benchmarks/) But you should remember that the borders. You can use fastest framework but it doesn't mean that the application you develop will be fastest application in the world. All conditions can be change and the benchmark test can't be fit for your wishes. And your wishes can be change too by your customer's wishes. So, remember that benchmark tests are limited. They are just trailers of the movie and they are all about advertising.

### Cyclomatic Complexity [1]

There is another method to understand the code quality; cyclomatic complexity. This method can help us to measure two different framework or library. I have copied following division from [tutorialspoint.com](http://tutorialspoint).

Cyclomatic complexity is a source code complexity measurement that is being correlated to a number of coding errors. It is calculated by developing a Control Flow Graph of the code that measures the number of linearly-independent paths through a program module. Lower the Program's cyclomatic complexity, lower the risk to modify and easier to understand.

It can be represented using the below formula:

```
Cyclomatic complexity = E - N + 2*P

where,
E = number of edges in the flow graph.
N = number of nodes in the flow graph.
P = number of nodes that have exit points
```

![Flow Graph](https://www.tutorialspoint.com/software_testing_dictionary/images/cyclomatic_complexity.jpg)

The Cyclomatic complexity is calculated using the above control flow diagram that shows seven nodes(shapes) and eight edges (lines), hence the cyclomatic complexity is 8 - 7 + 2 = 3

### Halstead Complexity Measures [2]

Halstead Complexity Measure is another method to understand the code quality. I have copied following division from [tutorialspoint.com](http://tutorialspoint).

In 1977, Mr. Maurice Howard Halstead introduced metrics to measure software complexity. Halstead’s metrics depends upon the actual implementation of program and its measures, which are computed directly from the operators and operands from source code, in static manner. It allows to evaluate testing time, vocabulary, size, difficulty, errors, and efforts for C/C++/Java source code.

According to Halstead, “A computer program is an implementation of an algorithm considered to be a collection of tokens which can be classified as either operators or operands”. Halstead metrics think a program as sequence of operators and their associated operands.

He defines various indicators to check complexity of module.

| Paramater | Meaning                                 |
| --------- | --------------------------------------- |
| n1        | Number of unique operators              |
| n2        | Number of unique operands               |
| N1        | Number of total occurrence of operators |
| N2        | Number of total occurrence of operands  |

When we select source file to view its complexity details in Metric Viewer, the following result is seen in Metric Report:

| Paramater | Meaning      | Mathematical Representation             |
| --------- | ------------ | --------------------------------------- |
| n         | Vocabulary   | n1 + n2                                 |
| N         | Size         | N1 + N2                                 |
| V         | Volume       | Length \* Log2 Vocabulary               |
| D         | Diffuculty   | (n1/2) \* (N1/n2)                       |
| E         | Efforts      | Difficulty \* Volume                    |
| B         | Errors       | Volume / 3000                           |
| T         | Testing time | Time = Efforts / S, where S=18 seconds. |

### Conclusion

I have talked about how you can measure a framework or a library and I referred some links about it. Benchmark Tests, Cyclomatic Complexity or Halstead Complexity Measure are just methods which you can use to understand of your code's reactions about contant situation. They can't say everything but they say something scientific. Choosing a technology, a framework or a library can't solve all problems which you have and you can't compare them directly.

The methods which I mentioned can be insufficient. But you are free to fine a new method to measure something. There is only one rule that you should follow, which is it must be scientific.

### References

[[1] Cyclomatic Complexity](https://www.tutorialspoint.com/software_testing_dictionary/cyclomatic_complexity.htm)

[[2] Halstead Complexity Measures](https://www.tutorialspoint.com/software_testing_dictionary/cyclomatic_complexity.htm)
