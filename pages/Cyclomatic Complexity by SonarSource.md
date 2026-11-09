id:: 6af034e2-1b26-4d21-a222-087c75e544a9

- ## Overview
  - [Cyclomatic complexity: developer's guide](https://www.sonarsource.com/resources/library/cyclomatic-complexity/)
  - Cyclomatic complexity serves as a vital gauge in software development, quantifying a program's complexity by counting its independent paths. Monitoring this metric enables the pinpointing of problematic code sections prone to errors, facilitating easier maintenance and overall robustness of software applications.
    - Cyclomatic complexity is a software metric introduced by Thomas J. McCabe in 1976 that quantifies the number of independent execution paths in code.
    - Higher scores indicate more execution paths, making code harder to test, maintain, and understand; lower scores signal cleaner, more readable code.
    - The metric is calculated using a control-flow graph formula, helping developers identify overly complex functions that need refactoring.
    - Monitoring cyclomatic complexity helps teams prioritize code modifications and reduces the likelihood of bugs in large codebases.
  - Modern software development today spans distributed teams, interconnected services, and rapidly evolving requirements—and at the center of it all is the code holding these systems together. When that code is structured with lower complexity, it becomes far easier to understand, reason about, and maintain. A clear grasp of your codebase directly contributes to higher quality, better testability, and fewer long-term issues.
  - Cyclomatic complexity offers a quantitative way to measure this structural complexity by counting the number of linearly independent paths through a program’s control flow. Monitoring this metric helps you pinpoint areas that may be overly intricate or error-prone, allowing you to refactor early and reduce maintenance risks. In this post, we’ll break down what cyclomatic complexity is, how it’s calculated, why it matters, and which tools can help you analyze and manage it effectively.
- ## What is cyclomatic complexity?
  - Cyclomatic complexity is a software engineering metric that was introduced by Thomas J. McCabe in 1976. This metric is a numerical score that represents the number of different paths a program can take during its execution. A higher score means more execution paths (higher complexity), while a lower score means fewer paths (lower complexity).
  - Programs with high cyclomatic complexity tend to be more prone to errors and harder to test and maintain. On the other hand, a lower cyclomatic complexity value suggests that the program is highly readable and easier to understand, test, and modify.
  - By quantifying the complexity of a program, developers are better prepared to approach and prioritize code modifications, refactoring, and testing. When it comes to managing larger codebases—where the risk of bugs increases with complexity—the cyclomatic complexity metric becomes especially useful.
- ## Why is cyclomatic complexity important?
  - Cyclomatic complexity is more than a software engineering metric—it’s a practical way to understand the structural complexity of code. It measures the number of linearly independent paths through a program’s control-flow graph, revealing the decision points, branches, and execution paths within a function, module, or class.
  - As cyclomatic complexity increases, code generally becomes harder to understand, test, review, and maintain. More branches create more possible execution paths and typically require more test cases for adequate path coverage or basis path testing. This makes cyclomatic complexity useful for identifying maintainability issues, potential bug hotspots, and opportunities for code refactoring.
  - | Why CC maters | What it tells you | Why it matters for software quality |
    | ---- | ---- | ---- |
    | Maintainability and readability | A lower cyclomatic complexity score generally indicates simpler control flow with fewer branches and independent paths. | Simpler code is easier for developers to read, understand, debug, review, and modify, reducing maintenance effort and technical debt. |
    | Testability and test coverage | Cyclomatic complexity indicates the number of linearly independent paths through the code and can help establish the minimum number of test cases needed for basis path testing. | Higher complexity means more execution paths may need to be verified, helping developers and QA teams identify code that requires additional testing and coverage. |
    | Defect and bug risk | Functions with many conditional branches and decision points contain more possible execution paths and interactions. | Complex control flow can make defects harder to detect and increase the chance that edge cases or paths are missed during testing and code review. |
    | Targeted code refactoring | High complexity can highlight functions, methods, or classes with convoluted control flow or too many responsibilities. | Teams can use the metric to prioritize code refactoring, break complex logic into smaller units, improve maintainability, and reduce technical debt. |
    | Code review prioritization | Complexity scores help identify areas of a codebase that may require closer inspection during code review. | Reviewers can focus attention on high-complexity code where subtle defects and maintainability problems may be more difficult to spot. |
    | Risk management | Cyclomatic complexity provides a quantitative signal for identifying structurally complex areas of a codebase. | Combined with other software quality metrics, it can help teams identify higher-risk code and prioritize testing, verification, and remediation efforts. |
- ## What is the formula for cyclomatic complexity?
  - The formula to calculate cyclomatic complexity is relatively straightforward. Start by observing the program’s control-flow graph—a graphical representation of all paths that might be traversed through a program during its execution.
  - Looking at a control-flow graph, we would represent the number of **edges** in the graph as `E`, while `N` represents the number of **nodes** in the graph. The formula for cyclomatic complexity `C` is:
    - ```
      C = E - N + 2P
      ```
    - where `P` represents the number of connected components.
  - This calculation gives us the number of linearly independent paths through the code. It indicates the **minimum number of paths that you need to test to ensure each decision point is executed at least once**. When `C` is high, then you have more complex code with more paths—meaning potentially higher maintenance and testing effort.
  - A simple conclusion we can draw from considering cyclomatic complexity is this:
    - > `if`, `while`, `for`, or `switch` statements add complexity because each condition introduces a new path in the flow of the program.
    - As a developer, you can look at this formula and quickly gauge the complexity of any part of your program.
  - Understanding and applying this formula will help you keep your code clean, efficient, and maintainable.
- ## What is an example of cyclomatic complexity?
  - To illustrate cyclomatic complexity with a practical example, let's look at a basic piece of code written in JavaScript. This example will help us understand how cyclomatic complexity is calculated and why it matters.
  - This straightforward JavaScript function checks if a user is eligible for a discount based on various conditions:
    - ```
      function checkDiscount(age, membershipDuration) {
        let discount;
        if (age > 60) {
          discount = 25; // Senior discount
        } else if (membershipDuration > 5) {
          discount = 15; // Loyalty discount
        } else {
          discount = 0; // No discount
        }
        return discount;
      }
      ```
  - We can represent these lines of code with the following control flow graph:
    - {{renderer :mermaid_6af1883d-86b9-4445-8bb3-275a35a411ea, 3}}
      - ```mermaid
        flowchart LR
        
          start((Start))
          branch1{Senior?}
          state1([discount = 25])
          branch2{Loyal?}
          state2([discount = 15])
          state3([discount = 0])
          state4([return discount])
          
          start-->branch1
          branch1-->|true|state1
          branch1-->|false|branch2
          branch2-->|true|state2
          branch2-->|false|state3
          state1-->state4
          state2-->state4
          state3-->state4
        ```
  - Let's walk through how we could calculate cyclomatic complexity.
  - As we look at the control-flow graph:
    - the total number of nodes ($N$) is 7;
      - ```
        start((Start))
        branch1{Senior?}
        state1([discount = 25])
        branch2{Loyal?}
        state2([discount = 15])
        state3([discount = 0])
        state4([return discount])
        ```
    - the total number of edges ($E$) is 8;
      - ```
        start-->branch1
        branch1-->|true|state1
        branch1-->|false|branch2
        branch2-->|true|state2
        branch2-->|false|state3
        state1-->state4
        state2-->state4
        state3-->state4
        ```
    - the total number of connected components ($P$) is 1;
      - because the entire function is a single connected component.
  - When we apply the formula, we get this:
    - ```
      C = E - N + 2P
        = 8 - 7 + 2*1
        = 3
      ```
  - This cyclomatic complexity score of 3 indicates that there are **three distinct paths** through the code. Each of these paths must be tested to ensure all scenarios are covered. By breaking down our code methodically like this, we clarify what’s needed for software testing and highlight the complexity in our code—complexity that could potentially be simplified.
- ## How to test cyclomatic complexity
  - Testing cyclomatic complexity, stated simply, means ensuring that you’ve adequately tested all of the possible paths in your program. Adequate test code coverage is important if you want to keep your code quality and reliability high.
  - After you have calculated the cyclomatic complexity of your code, break down your code to identify what those paths are. Here’s a tip: Each decision point in your code (such as conditional statements or loops) usually contributes to a new path.
  - **Once you’ve identified your paths, create test cases for each one**. The goal is to execute each path at least once. This ensures that all the functional aspects of the code are tested, and it will help you in catching potential errors in code logic. Use automated testing tools to run your test cases. After analyzing the test results, check for any failures or unexpected behaviors. Each failed test case might indicate a bug or a flaw in that path of your source code.
  - If all your tests are passing but you feel like the cyclomatic complexity is too high, then **you can consider refactoring the code to simplify it**. This might involve breaking down complex functions into simpler ones or reducing the number of decision points. After refactoring, repeat the testing process to ensure the new code maintains or improves quality.
  - By regularly determining cyclomatic complexity and then testing your code accordingly, you can maintain a manageable level of complexity. This simplifies the testing process and your codebase, leading to more reliable and maintainable software.
- ## How to do cyclomatic complexity analysis
  - Cyclomatic complexity code analysis involves reviewing your program’s source code to understand its structure and identify areas where the complexity can be reduced. To do this, you would start by gathering all the source code that needs analysis. This might be a specific module or a set of functions—or maybe even an entire application—depending on your focus.
  - You can use tools like static code analyzers (refer to [[OSS to measure CCN]] ) to automate the process of calculating cyclomatic complexity for each function or module. These tools will help you in providing a quick overview of your code complexity.
  - From there, identify any parts of code with high complexity scores.
    - These are the areas that may be potentially difficult to understand, test, and maintain.
    - Prioritize areas that should be refactored.
    - Determining which areas those are will depend on their complexity and the criticality of their functionality within the application.
    - Focus on areas where reducing complexity will yield significant benefits for maintainability and reliability.
  - As you refactor and the cyclomatic complexity decreases, you may find that your number of test cases also decreases. After refactoring, rerun your tests (which also have been rewritten) to ensure you haven’t broken anything. Then, recalculate the cyclomatic complexity to ensure that your changes have effectively reduced the complexity.
  - By regularly performing cyclomatic complexity analysis, development teams can proactively manage their codebase, ensuring that it remains clean, testable, and maintainable. This practice will help you improve code quality, and it’ll ultimately make your development processes more efficient.
- ## Language-specific considerations
  - Cyclomatic complexity follows the same formula across languages, but its meaning can vary depending on the language’s syntax and style.
    - In imperative languages like Java, C#, or Python, complexity rises mainly through explicit branching—if statements, loops, switch blocks, and exception handling. Other features, such as short-circuit logic or comprehensions, can also add hidden paths that aren’t always obvious at first glance.
    - Functional and hybrid languages introduce their own quirks. Lambdas, closures, recursion, and pattern matching may produce fewer visible branches while still encoding dense logic that’s harder to reason about. As a result, the same numeric complexity score may indicate very different levels of difficulty depending on the paradigm.
  - Understanding these differences helps teams interpret the metric accurately and decide when high complexity truly signals a maintainability risk.
- ## Conclusion
  - Understanding and managing cyclomatic complexity is important if you want to develop clean and high-quality software. This metric serves as a guide for developers to **identify areas that may be prone to errors or difficult to maintain**. By regularly measuring and analyzing cyclomatic complexity, you can ensure your code is both efficient and maintainable.