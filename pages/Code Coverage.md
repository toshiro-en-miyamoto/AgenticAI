id:: 6af25e5a-d01a-473c-ac46-65650446bb6b

- ## Overview
  - [Code Coverage](https://en.wikipedia.org/wiki/Code_coverage) on WikiPedia
  - In software engineering, code coverage is a percentage measure of the degree to which the source code of a program is executed when a particular test suite is run.
  - Code coverage was among the first methods invented for systematic software testing. The first published reference was by Miller and Maloney in Communications of the ACM, in 1963.
- ## Basic Code Coverage Criteria
  - | Coverage Name | IPA | Code Coverage Criteria |
    | ---- | ---- | ---- |
    | Function | | Has each function in the program been called? |
    | Statement | C0 | Has each statement in the program been executed? |
    | Edge | | Has every edge in the control-flow graph been executed? |
    | Branch | C1 | Has each branch of each control structure (for example, given an `if` statement, have both the `true` and `false` branches) been executed? |
    | Condition | C2 | Has each Boolean sub-expression evaluated both to `true` and `false`? |
  - IPA introduces three code coverages; C0/C1/C2. Refer to 「コラム: ホワイトボックステストでの網羅率 C0, C1, C2」(p.70), ソフトウェアテスト見積りガイドブック, 独立行政法人情報処理推進機構(IPA), March 2008.
    - 命令網羅率(C0): プログラムの命令を最低1回はテストする。
    - 分岐網羅率(C1): すべての命令とすべての分岐をテストする。
    - 条件網羅率(C2): 条件分岐に置いて条件の真・偽のすべての組み合わせをテストする。
  - For example consider the following C function:
    - ```
      int fun (int x, int y)
      {
          int z = 0;
          if ((x > 0) && (y > 0))
          {
              z = x;
          }
          return z;
      }
      ```
  - Assume this function is a part of some bigger program and this program was run with some test suite.
    - Function coverage will be satisfied if, during this execution, the function `fun` was called at least once.
      - {{renderer :mermaid_6af52eb6-b917-435d-be61-25b11f37b74a, 3}}
        - ```mermaid
          graph LR
          
            s(("fun(x,y)"))
            p1(z = 0)
            b1{"(x > 0) && (y > 0)"}
            p2(z = x)
            p4(return z)
            tc1@{ shape: text, label: "fun(1,1)" }
          
            s-->p1
            p1-->b1
            b1-->|true|p2
            p2-->p4
            b1-->|false|p4
            tc1-.->s
          ```
    - Statement coverage will be satisfied if it was called for example as `fun(1,1)`,
      - because in this case, every line in the function would be executed—including `z = x;`.
      - {{renderer :mermaid_6af4fd70-7854-4c7b-a0a4-9b438bb75567, 3}}
        - ```mermaid
          graph LR
          
            s(("fun(x,y)"))
            p1(z = 0)
            b1{"(x > 0) && (y > 0)"}
            p2(z = x)
            p4(return z)
            tc1@{ shape: text, label: "fun(1,1)" }
            c1@{ shape: comment, label: "T && T = T" }
          
            s==>p1
            p1==>b1
            b1==>|true|p2
            b1-.-c1
            p2==>p4
            b1-->|false|p4
            tc1-.->s
          ```
    - Branch coverage will be satisfied by tests calling `fun(1,1)` and `fun(0,1)`
      - because, in the first case, both `if` conditions are met and `z = x;` is executed, while in the second case, the first condition, `(x>0)`, is not satisfied, which prevents the execution of `z = x;`.
      - {{renderer :mermaid_6af53036-7098-4d52-8109-911cc0ddd63e, 3}}
        - ```mermaid
          graph LR
          
            s(("fun(x,y)"))
            p1(z = 0)
            b1{"(x > 0) && (y > 0)"}
            p2(z = x)
            p3(return z)
            tc2@{ shape: text, label: "fun(0,1)" }
            c2@{ shape: comment, label: "F && [] = F" }
          
            s==>p1
            p1==>b1
            b1-->|true|p2
            p2-->p3
            b1==>|false|p3
            b1-.-c2
            tc2-.->s
          ```
    - Condition coverage will be satisfied with tests that call `fun(1,1)`, `fun(0,1)`, and `fun(1,0)`.
      - In `fun(1,1)`, both `(x>0)` and `(y>0)` are evaluated to `true`.
      - In `fun(0,1)`, `(x>0)` is evaluated to `false`. But it does not evaluate `(y>0)`, because of the lazy-evaluation of the Boolean operator.
      - In `fun(1,0)`, `(y>0)` is evaluated to `false` so that every sub-expression is evaluated to both `true` and `false`.
      - {{renderer :mermaid_6af51de0-a673-4bb9-800b-53b8e3fc495e, 3}}
        - ```mermaid
          graph LR
          
            s(("fun(x,y)"))
            p1(z = 0)
            b1{"(x > 0) && (y > 0)"}
            p2(z = x)
            p3(return z)
            tc3@{ shape: text, label: "fun(1,0)" }
            c3@{ shape: comment, label: "T && F = F" }
          
            s==>p1
            p1==>b1
            b1-->|true|p2
            p2-->p3
            b1==>|false|p3
            b1-.-c3
            tc3-.->s
          ```
- ## Modified Condition/Decision Coverage
  - A combination of function coverage and branch coverage is sometimes also called **decision coverage**. This criterion requires that every point of entry and exit in the program has been invoked at least once, and every decision in the program has taken on **all possible outcomes** at least once. In this context, the decision is a Boolean expression comprising conditions and zero or more Boolean operators.
  - | Coverage Name | Code Coverage Criteria |
    | ---- | ---- |
    | C/DC | Condition/Decision Coverage, which requires that both condition and decision coverage be satisfied. |
    | MC/DC | Modified Condition/Decision Coverage, which extends condition/decision criteria with requirements that each condition should affect the decision outcome independently. |
    | Multiple Condition | This criterion requires that all combinations of conditions inside each decision are tested. |
  - For example, consider the following code:
    - ```
      if (a or b) and c then
      ```
  - The C/DC criteria will be satisfied by the following set of tests:
    - | expression | TC1 | TC2 | TC3 | TC4 | TC5 | TC6 | TC7 | TC8 |
      |---|---|---|---|---|---|---|---|---|
      | a | true | false |---|---|---|---|---|---|
      | b | true | false |---|---|---|---|---|---|
      | c | true | false |---|---|---|---|---|---|
      | (a or b) and c | true | false |---|---|---|---|---|---|
  - However, the above tests set will not satisfy MC/DC, since in the first test, the value of `b` and in the second test the value of `c` would not influence the output.
  - So, the following test set is needed to satisfy MC/DC:
    - | expression | TC1 | TC2 | TC3 | TC4 | TC5 | TC6 | TC7 | TC8 |
      |---|---|---|---|---|---|---|---|---|
      | a | **false** | false | false | **true** |---|---|---|---|
      | b | **false** | **true** | **true** | false |---|---|---|---|
      | c | true | **false** | **true** | **true** |---|---|---|---|
      | (a or b) and c | false | false | true | true |---|---|---|---|
  - Multiple Condition criterion requires that all combinations of conditions inside each decision are tested. For example, the code fragment from the previous section will require eight tests:
    - | expression | TC1 | TC2 | TC3 | TC4 | TC5 | TC6 | TC7 | TC8 |
      |---|---|---|---|---|---|---|---|---|
      | a | false | false | false | false | true | true | true | true |
      | b | false | false | true | true | false | false | true | true |
      | c | false | true | false | true | false | true | false | true |
- ## Linear Code Sequence and Jump Coverage
  - [Linear code sequence and jump (LCSAJ)](https://en.wikipedia.org/wiki/Linear_code_sequence_and_jump) , in the broad sense, is a software analysis method used to identify structural units in code under test.
  - In a narrower sense, an LCSAJ is a well-defined linear region of a program's code. When used in this sense, LCSAJ is also called Jump-toJump Path (JJ-Path).
  - An LCSAJ is a software code path fragment consisting of a sequence of code (a linear code sequence) followed by a control flow Jump, and consists of the following three items:
    - the start of the linear sequence of executable statements,
    - the end of the linear sequence, and
    - the target line to which control flow is transferred at the end of the linear sequence.
- ## Other Coverage Criteria
  - | Coverage Name | Code Coverage Criteria |
    | ---- | ---- |
    | PVC | Parameter Value Coverage, which requires that in a method taking parameters, all the common values for such parameters be considered. |
    | Path | Has every possible route through a given part of the code been executed? |
    | Loop | Has every possible loop been executed, zero times, once, and more than once? |
    | State | Has each state in a finite-state machine been reached and explored? |
- ## Popular Code Coverage Tools
  - | Language | Open-source Tools | Supported Criteria |
    | ---- | ---- | --- |
    | JavaScript & TS | [vitest/v8](https://vitest.dev/guide/coverage#v8-provider) | Function, Block | 
    | JavaScript & TS | [vitest/istanbul](https://vitest.dev/guide/coverage#istanbul-provider) | Line, Statement, Branch, Function | 
    | Python | [Coverage.py](https://coverage.readthedocs.io/en/latest/) | Statement, Branch | 
    | Java | [JaCoCo](https://www.jacoco.org/jacoco/trunk/doc/counters.html) | Byte-code, Stetement, Branch, CCN, Method, Class |
    | Java | [OpenClover](https://openclover.org/doc/manual/latest/general--about-code-coverage.html) | Statement, Branch, Method |
    |  PostgreSQL pgSQL | [plpgsql_check](https://github.com/okbob/plpgsql_check#coverage-metrics) | Statement, Branch |
    | MySQL stored proc | [cover_me](https://github.com/verizonconnect/cover_me) | Instrumented point, Branch, Method, Class, Module |
    | GCC C++ | [gcovr](https://gcovr.com/en/stable/output/txt.html) | Line, Branch |
    | LLVM C++ | [llvm-cov](https://llvm.org/docs/CommandGuide/llvm-cov.html) | Line, Branch, MC/DC, Region |
-