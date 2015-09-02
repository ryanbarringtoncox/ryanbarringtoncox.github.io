---
layout: page
title: The Art of Software Testing - by Glenford J Myers 
---
### My Notes 

Software Testing is a psychological undertaking.  It’s an intellectual challenge.
\\
\\
“Testing is the process of executing a program with the intent of finding errors.”
\\
\\
Testing is extremely creative and intellectually challenging, perhaps more than writing programs.
\\
\\
Programmers should not test their own code.
\\
\\
**We create better software when we seek to find errors.  When we seek to make our code free from bugs, we make subconsciously create tests that are more likely to pass.  This leads to weaker software.**
\\
\\
“Do not plan a testing effort under the tacit assumption that no errors will be found.”
\\
\\
Test cases must have expected output.  Thoroughly inspect the results of each test.  There are often overlooked clues to other bugs in our test cases.
\\
\\
Create test cases for invalid and unexpected conditions, as well as valid ones.
\\
\\
Never create a throwaway test case, unless the program is also a throwaway.  You’ve already created it.  Save it for later or it will be lost.  Don’t lose that time investment.
\\
\\
**“The probability of the existence of more errors in a section of a program is proportional to the number of errors already found in that section.”**  This is counterintuitive. We often think, "Oh, I've already tested there so there won't be more errors there."
\\
\\
To summarize -

  - Our testing intent should be to find errors.
  - A good test case has a high probability of detecting undiscovered errors.
  - A successful test case detects undiscovered errors.

A **Program Inspection** happens with 4 people, the author and 3 other experienced programmers.  On of these 3 should be a moderator who helps keep ego out of the equation.  Don’t publish the results anywhere.  Be gentle.  The author narrates the program statement by statement, out loud.  This alone uncovers bugs.  Program is analyzed with regard to a checklist that has these main sections.

  - Data-Reference errors
  - Data-Declaration errors
  - Computation errors
  - Comparison errors
  - Control-Flow errors
  - Interface errors
  - Input/Output errors

A **Program Walkthrough** happens with a group of people and some simple test cases.  The group walks through the program execution.  It will be far slower than a machine execution so you can only do a little bit here.
\\
\\
**Desk Checking** involves people separately checking code on their own.  It is not as effective as these other approaches because there is less collaboration and accountability.  Being in the moment together, encourages us to find bugs and “show off.”
\\
\\
A **Program Review** happens with multiple developers swapping programs, looking over them and rating the accuracy.

#### Test Case Design

“What subset of all probable test cases has the highest probability of detecting the most errors?”
\\
\\
Black Box Approaches

  - Equivalence Partitioning
  - Boundary-value Analysis
  - Cause-effect Graphing
  - Error guessing

White Box Approaches

  -  Statement coverage
  -  Decision Coverage
  -  Condition Coverage
  -  Decision/Condition Coverage
  -  Multiple Decision Coverage

**Decisions contain conditions.**
\\
\\
**Statement Coverage** is weak.  Decision (Branch) coverage is stronger.  The latter means each true/false decision must have both outcomes at least once.  In other words, each branch must be traversed at least once.
\\
\\
**Condition Coverage** is stronger than Decision Coverage.  In Condition Coverage, we have written enough test cases so that each condition has every possible output at least once.
\\
\\
**Decision and Condition Coverage** usually covers all statements, but not always.  For example, not in these cases.

  - The program has no decisions.
  - Program has multiple entry points.  Given statement might only be executed if program is entered from certain point.
  - Statements might have ON-units.  Traversing every branch may not make ON-units to be executed.

Therefore, **Decision and Condition Coverage** should be modified to say that each point of entry (including ON-units) be invoked at least once.
\\
\\
**Decision/Condition Coverage** requires that test cases such that each condition in a decision takes on all possible outcomes at least once and each decision takes on all possible outcomes at least once and each point of entry (and ON-units) are invoked at least once.
\\
\\
**Multiple-Condition Coverage** requires test cases that cover all combinations of condition outcomes in each decision are executed at least once.
\\
\\
**Equivalence Partitioning** involves chunking our inputs into subsets.  Equivalence partitioning happens with inputs and outputs.  Not just inputs.  Think about that.
\\
\\
Good test cases have the following properties.  They -

  - Reduce other test cases that must be developed by more than a count of one.
  - Cover a large set of other possible test cases.  They tell us something about the presence or absence of errors over and above this specific set of input values.

**Boundary-value analysis** deals with the test cases on the edges of partitions.  i.e.  Just over, on and below the boundary.
\\
\\
Cause-effect graphing is used for testing combinations of inputs.  It claims to help us find “high-yield test cases.”  As a side effect, it can point out incompleteness/ambiguities in spec.
\\
\\
Error guessing can be done by using intuition and experience to come up with high-yield test cases.
\\
\\
The author recommends a blend of the approaches discussed as each has its own advantages.  In particular -

  - If input spec has lots of combos, use cause-effect graphing.
  - Always use boundary-analysis and be sure to consider inputs and output boundaries.
  - Identify valid and invalid equivalence classes and construct test cases for them all.
  - Use error-guessing for additional cases.
  - Examine program logic and use decision, condition, decision/condition or multiple-condition coverage to meet coverage criterion.  The latter is the most complete and difficult to achieve.

#### Module (Unit) Testing

**Module testing is largely whitebox.**  Start with white box logic testing the supplement cases with black box.  Identify the boundaries and edges.  Create your test cases from there.
\\
\\
Myers recommends that we do incremental testing over non-incremental testing.  This means rather than test all the modules in isolation first, we test one in isolation, add another, then another.  This way interfaces are tested along the way.  Interfaces are likely to have bugs.
\\
\\
Top-down and bottom-down incremental testing is debated here too.  There are pros and cons to both.

  - If you do a top-down approach, you start testing the first program that kicks everything off.  You’ll need stubs for other modules.  Stubs are not always easy to do.  You can often get a working skeleton going faster which boosts morale.
  - In bottom-up, you start with a called module then do the parent and so on up.  You’ll need to write drivers, but not stubs in this case.

#### Higher-Order Testing

**Software is about moving from the conceptual to the concrete.  Software errors occur when the program does not do what end user reasonably expects it to do.**
\\
\\
Most errors happen due to miscommunication and mistranslation of the task at hand.  When we try to break down the user requirements, we fuck up and errors are baked in.
\\
\\
**Requirements** specify why program is needed.
\\
\\
**Objectives** specify what program should do and how well.
\\
\\
**External Specifications** define the exact representation of program to users.
\\
\\
**Documentation** specifies how program is constructed.
\\
\\
**Functional Testing** looks for discrepancies between program behavior and spec.  It’s mostly black box, describing the user’s view of behavior.  Analyze the spec and identify partitions, boundaries, error-guesses to form functional tests.

  - Keep track of which functions had the most errors.  They are likely to have many more errors.
  - Focus on unexpected and invalid inputs.

**System Testing is most misunderstood and difficult testing method.  We are testing the whole system or program against its original objectives.  Usually the most bugs are found during system testing.**
\\
\\
**User documentation is a great source of system tests.**  What should it do for the user?
\\
\\
There are many ways to go about system testing.  Here are some types of system testing.

  - Facility Testing - Scan objectives sentence by sentence and try to disprove they’re working.
  - Volume Testing - Give the program a high volume of data.  Make sure it can handle as much as it claims.
  - Stress Testing - Push it to the limits.  Cover those “never will occur” situations.
  - Usability Testing - Does all behavior of program make sense to the average user.
  - Security Testing - Study known vulnerabilities and hammer the program.
  - Performance Testing - Attempt to show that program can not meet its specified performance goals.
  - Storage Testing - Try to break main and secondary storage.
  - Configuration Testing - Test the program with various types of hardware, db, i/o, devices, etc that it should work with.
  - Compatibility/Conversion Testing - If program is a refactor or replacement of original, show that it doesn’t integrate with system like the original does.
  - Installability Testing
  - Reliability Testing
  - Recovery Testing
  - Serviceability Testing
  - Documentation Testing
  - Procedure Testing - Test procedures that humans will do.

**Acceptance Testing** compares program to initial requirements and current needs of end users.
\\
\\
Test Planning and Control is not easy.  There is so much that can be done in so little time.
\\
\\
When to stop testing?  **You will never find all bugs.**  The common criteria is Stop when -

  1. The time allocated to testing is over.
  2. All test cases pass.

Both are useless because you can do #1 without doing any work.  You can do #2 by writing one simple test case.  This encourages us to write easy tests.
