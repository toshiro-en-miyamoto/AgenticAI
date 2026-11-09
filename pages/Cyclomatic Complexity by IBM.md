id:: 6af034f5-3a3e-46a6-8896-4921aae9b627

- ## Overview
  - [What is cyclomatic complexity?](https://www.ibm.com/think/topics/cyclomatic-complexity)
    - By Shalini Harkar
    - Published 10 August 2026
- ## The invisible complexity
  - A well-designed piece of software always starts off simple. Initial versions of the software product are fun. The code is neat and organized. Adding new features is simple. Bugs get spotted, fixed and deployed with minimal effort. The development process moves fast because everyone knows how the application works. And then comes the expansion phase.
  - Customers start requesting customizations. Compliance regulations bring in new validation requirements. More edge cases are found through experience. Each new release means more business logic implemented. A method solving one problem at once has now solved ten.
  - Before anyone realizes it, the code has evolved from a straight path into an intricate maze of multiple directions. Each `if` statement creates a new direction. Each `switch` statement produces new flow paths. Each cycle and condition add more paths for a programmer to follow before making any changes.
  - Code size can remain modest. But understanding and testing have definitely become harder. An extra decision step makes the program even more difficult to understand and that is what cyclomatic complexity measures.
- ## The search for a better measure of code
  - The typical methods to evaluate the dimension of software was the calculation of lines of code, modules and file count. However, there was no approach to measure the complexity of software.
  - Taking the previously existing measurement gap into consideration, **Thomas McCabe** developed cyclomatic complexity in 1976 in his seminal paper, “A complexity measure”. His idea was to shift the focus from how much code a program contained to how many linearly independent paths it can take. He believed that instead of using the amount of code as a criterion, one should consider the number of independent paths through the program’s source code.
  - There are many decisions that a programmer must make throughout the code. Much of the complexity lies in the integration of the logic and the flow of the code as it runs and evaluates conditions.
- ## The problem it solved
  - **Opaque code complexity**: Counts the number of distinct paths of execution rather than depending upon any subjective viewpoints.
  - **Limited test coverage**: Calculates the minimum number of test cases required for executing each independent path.
  - **Potential defect hotspots**: Identifies modules that contain too many branchings that make them prone to errors.
  - **Code optimization decisions**: Identifies functions that need simplification through the division of logic.
  - **Standardized quality assessment**: Gives a way of standardizing a metric to ensure compliance with coding practices.
- ## The formula—How it's calculated?
  - McCabe derived cyclomatic complexity from graph theory. A function’s control flow is modelled as a directed graph where:
    - ```
      CC = E - N + 2P
      ```
    - Cyclomatic Complexity number (CC)
    - Nodes (N)—represent individual statements or blocks of code
    - Edges (E)—represent control flow transfers between nodes
    - Connected components (P)—usually one for a single function
- ## Complexity thresholds
  - | CC | Risk level | Recommendation |
    | ---- | ---- | ---- |
    | 1–5 | Low | Simple, easy to test. |
    | 6–10 | Moderate | Manageable. Review for refactoring. |
    | 11–20 | High | Complex. Hard to test. Refactor recommended. |
    | >20 | Critical | High defect risk. Refactor immediately. |
  - The common practice among most engineering groups is setting a maximum value of `CC ≤ 10` per function in CI/CD pipelines.
- ## Complexity tools
  - Enterprise-ready products such as SonarQube, SonarCloud, Code Climate and Codacy integrate cyclomatic complexity evaluation as part of their code quality measurements. They assess this metric alongside maintainability, technical debt, security issues and code reliability to provide a broader view of software quality.
  - Moreover, there are language-specific solutions such as Radon (for Python), PMD (for Java™), ESLint (for JavaScript/TypeScript) and gocyclo (for Go) allowing the detection of code complexity at an early stage of development. Currently, within the context of enterprise software engineering, the cyclomatic complexity is increasingly assessed together with the cognitive complexity and other measures of maintainability.
  - Also refer to [[OSS to measure CCN]].
- ## Enterprise code intelligence in IBM Bob
  - With agentic software engineering, cyclomatic complexity transforms from a software quality metric into a reasoning signal. By combining complexity metrics with contextual understanding of code, software engineering agents are now able to make autonomous code quality evaluation, task prioritization, remediation recommendations and continuous improvement of enterprise software systems.
  - In IBM Bob™, cyclomatic complexity is computed along with other code intelligence signals that include: Code dependencies, code churn and change history, critical application components to business, static analysis insights, quality measures, security risks, technical debt measures. Through a combination of these signals, IBM Bob helps engineering teams to transcend from fragmented code metrics to software modernization.
- ## Common misconceptions
  - The concept of cyclomatic complexity is often erroneously seen as an isolated measure of software quality. For example, one erroneous idea is that all programs with high cyclomatic complexity are considered poor-quality code, while complex business logic can require more decision-making paths.
  - In addition, cyclomatic complexity is perceived as a measure of the code size, while it is a measure of the number of independent execution paths and not the code lines. Some programmers mistakenly believe that the smaller complexity implies the better code quality, without considering such crucial parameters of the program as readability, modularity, coupling and maintainability.
  - Moreover, the concept of cyclomatic complexity is mistakenly associated with the execution time and speed of the program. At last, with the development of agentic and AI-assisted software solutions, cyclomatic complexity is mistakenly taken as an adequate measure of intelligent software quality. However, it cannot account for the dynamic interactions between agents, their orchestration and their decision-making processes.
- ## Best practices
  - Functions should be kept simple and cohesive.
  - Minimize the nesting of conditionals through reduction in decision-making paths.
  - Decompose highly complex functions into simpler ones.
  - Establish complexity limits that are permissible (for example, CC≤10) and evaluate deviations.
  - Regularly assess complexity through static code analysis tools incorporated in CI/CD pipelines.
- ## Conclusion
  - The future of software quality depends on intelligent assessment based on collected metrics. AI-assisted assessment and automation transform cyclomatic complexity from a mere historical metric into a tool that supports maintainability and engineering excellence.