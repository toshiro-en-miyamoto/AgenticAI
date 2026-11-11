id:: 6af25e5a-d01a-473c-ac46-65650446bb6b

- ## Overview
  - [Code Coverage](https://en.wikipedia.org/wiki/Code_coverage) on WikiPedia
  - In software engineering, code coverage is a percentage measure of the degree to which the source code of a program is executed when a particular test suite is run.
  - Code coverage was among the first methods invented for systematic software testing. The first published reference was by Miller and Maloney in Communications of the ACM, in 1963.
- ## Basic Code Coverage Criteria
  - | Coverage Name | Code Coverage Criteria |
    | ---- | ---- |
    | Function | Has each function in the program been called? |
    | Statement | Has each statement in the program been executed? (C0:命令網羅 by IPA) |
    | Edge | Has every edge in the control-flow graph been executed? |
    | Branch | Has each branch of each control structure (for example, given an `if` statement, have both the `true` and `false` branches) been executed? (C1:分岐網羅 by IPA) |
    | Condition | Has each Boolean sub-expression evaluated both to `true` and `false`? (C2:条件網羅 by IPA) |
  - For C0/C1/C2 by IPA, refer to 「コラム: ホワイトボックステストでの網羅率 C0, C1, C2」(p.70), ソフトウェアテスト見積りガイドブック, 独立行政法人情報処理推進機構(IPA), March 2008.
    - 命令網羅率(C0): プログラムの命令を最低1回はテストする。
    - 分岐網羅率(C1): すべての命令とすべての分岐をテストする。
    - 条件網羅率(C2): 条件分岐に置いて条件の真・偽のすべての組み合わせをテストする。
  - For example consider the following C function:
    - ```
      int foo (int x, int y)
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
    - Function coverage will be satisfied if, during this execution, the function `foo` was called at least once.
    - Statement coverage for this function will be satisfied if it was called for example as `foo(1,1)`,
      - because in this case, every line in the function would be executed—including `z = x;`.
    - Branch coverage will be satisfied by tests calling `foo(1,1)` and `foo(0,1)`
      - because, in the first case, both `if` conditions are met and `z = x;` is executed, while in the second case, the first condition, `(x>0)`, is not satisfied, which prevents the execution of `z = x;`.
    - Condition coverage will be satisfied with tests that call `foo(1,0)`, `foo(0,1)`, and `foo(1,1)`.
      - These are necessary because in the first case, `(x>0)` is evaluated to `true`, while in the second, it is evaluated to false. At the same time, the first case makes `(y>0)` `false`, the second case does not evaluate `(y>0)` (because of the lazy-evaluation of the Boolean operator), the third case makes it `true`.
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
  - The Condition/Decision criteria will be satisfied by the following set of tests:
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
- ## Other Coverage Criteria
  - | Coverage Name | Code Coverage Criteria |
    | ---- | ---- |
    | PVC | Parameter Value Coverage, which requires that in a method taking parameters, all the common values for such parameters be considered. The idea is that all common possible values for a parameter are tested. For example, common values for a string are: 1) null, 2) empty, 3) whitespace (space, tabs, newline), 4) valid string, 5) invalid string, 6) single-byte string, 7) double-byte string. It may also be appropriate to use very long strings. |
    | Path | Has every possible route through a given part of the code been executed? |
    | Loop | Has every possible loop been executed, zero times, once, and more than once? |
    | State | Has each state in a finite-state machine been reached and explored? |