=
@keyIdeas
* The foundation of a computer’s ability to make decisions are control structures.
* Control structures require programmers to reduce decisions to logic that leverages Boolean logic, data comparisons, and other kinds of case-based logic.
* Loops are control structures that allow a computer to make large numbers of decisions on fixed sets of information, or even infinite amounts of information, indefinitely.
* Because control structures only approximate human decisions in the real world, their imperfections can have many unintended consequences.
* Teaching and assessing control structure knowledge requires students to learn not only the syntactic and semantic rules of a programming language, but also skills for mimicking the behavior of a program following those roles.
* Teaching control structures critically requires examining the context that is missing from the logic used in an algorithm’s use of control structures.
=

In 2006, the U.S. state of Indiana began a major digitization initiative to reduce administrative costs of its welfare system through automation and privatization<eubanks18>. Before digital systems were adopted, people applying to the federal [Supplemental Nutrition Assistance Program|https://www.fns.usda.gov/snap/supplemental-nutrition-assistance-program] (SNAP) (otherwise known as food stamps) would send in information for review by a caseworker employed by the state. This information the caseworker reviewed included things like:

* Proof of Indiana residency,
* How many people live in the applicant’s household,
* Whether the household has dependent children less than 18 years of age, and
* The gross earned monthly income of the household in the current or next month.

The case worker was available by phone for questions, to resolve any ambiguities in an application, and to help explain the state’s eligibility decisions. The process was not perfect, but the experience of applying for benefits involved interacting with a person who could guide the applicant through the process, fix any misinterpretations by the applicants about how to fill out the form, and address any exceptional circumstances not anticipated by the program, its processes, or the laws that shaped it.

Indiana’s digitization efforts sought to automate the application process, having applicants submit data through a website, then having an algorithm use that data to decide whether the applicant was eligible. The hope was that the algorithm could replicate the caseworker’s eligibility decision, but do it more consistently, more quickly, and more fairly than a case worker could do, and possibly free caseworkers to spend their time handling more complex cases. And there were good reasons to believe this: at the time, digitization efforts in private industry had already begun to transform multiple retail sectors, bringing unprecedented convenience and choice to consumers, upending assumptions about how people what wanted to shop, bank, and travel, and how much they needed other people to facilitate those transactions.

However, over the course of several years, it became very clear that while the eligibility criteria encoded into state law had not changed, something was different about the decisions that the algorithm was making: fewer people were being approved, and residents had less clarity about why they were being denied. The decisions that the algorithm was making were, in general, less flexible, less explainable, and less transparent than the decisions that caseworkers made. The result was that Indiana denied more than a million applications for food stamps and other aid, a 54% increase in denials compared to the three years before the automation was launched. From the conservative governor's standpoint, however, it was a success: the state and federal government had never spent less on food security for its citizens since the program had started.

This story, and the countless others like it, show that delegating decisions to computers is not simply a matter of productivity and time savings. Rather, it is a complex political choice, with winners (often those in charge of the design of the systems who want to save money and time) and losers (often those who rely on the systems for their wellbeing. And at the heart of automating these decisions is something called ~control structures~controlstructure in programming languages, which are how we tell computers to decide what to do next. In the rest of this chapter, we'll discuss a range of control structure types, how they form the basis of computers' ability to make decisions, and how to develop youth critical consciousness of how these control structures are used to automate human decisions.

|Chapter11_Figure01_IgnoringContext.png|A Tetris-like collection of shapes, with people, flags, and history obscured behind them.|When computers make decisions, they often ignore and overwhelm social context.|@jessie|<

# Conditionals encode decision making
The simplest control structures include CPU machine instructions like a `jump if equal` instruction (like those discussed in the @intelligence chapter), which might compare the bit sequences of two values in memory, and go to a particular instruction in a program if they are equal, and go to the next instruction in a program if they are not. CPUs have similar instructions like `jump if zero`, `jump if less than`, `jump of greater than`, all encoding basic mathematical inequalities into hardware. Somewhat surprisingly, _every_ control structure, in _every_ programming language, ultimately reduces to one of these "this or that" binary decisions about what a program to do next.

Writing a program to encode the intelligence of case worker judgments into code therefore required translating a case worker’s knowledge into into `jump if` instructions that inspected particular data values. For example, a case worker might need to check whether or not an applicant has any dependents younger than 18. This simple check can be translated into machine instructions that look something like this:

`
1. read number_of_dependents
2. jump if zero #4
3. return true
4. return false
`A simple conditional in machine instructions.

That little bit of hypothetical machine code means "_read the number of dependents from the computer’s memory_" -- which might be a number that an applicant entered into a web form, or a number computed by counting the number of entries in a list of dependents entered into the web form -- then compare the number to zero. If the number is _equal_ to zero, then the jump instruction causes the CPU to skip to step 4 of the program, returning `false`, to represent that the application does _not_ meet the eligibility requirement, and halt the program. However, if the number is _not equal_ zero, then the next step of the program, step 3, is executed, returning `true` to represent that it _does_ meet the eligibility requirement, then halt the program. This algorithm is indeed more consistent than the case worker -- it would decide the same every single time, given the same data. It is also more efficient, as a computer could make this decision a million times in a second, unlike a case worker.

In reality, though, the case worker’s decision is much more complicated. They might look at what someone had written on a paper form, notice that some of the children had different last names than the applicant, and ask the applicant to clarify this difference. Then, based on the applicant’s response, the case worker might have the applicant remove dependents from their application, or determine that they were dependents despite having different last names. The case worker might even gather more context, interpret information, interpret the eligibility rules, and make an informed decision about eligibility using information beyond the standard information that is gathered. We could attempt to write new code that captures these additional steps, but it will quickly become extremely complicated -- as complicated as the case workers' reasoning is, if not more. And, depending on the type of information the case officer might request and the format in which it might be provided, it might not even be possible to reduce the decision to simple checks. The task might be so nuanced, only a human being with training, expertise, and empathy could do it humanely.

To try to model this decision making, the programmers at IBM who were hired to build the Indiana system needed to use more complex control structures. The most basic of these is the `if` statement, found in most programming languages. Here’s how an `if` statement for the program above looks in Python:

`python
if number_of_dependents == 0:
    return False
else:
    return True
`A simple conditional in Python.

That’s easier to read than the machine instructions above, but it requires knowing a few syntactic details about Python: 

* The words `if` and `else` have special meaning to indicate two possible decisions.
* Python has named variables which refer to values in members (`number_of_dependents`).
* Python uses two consecutive equal signs (`==`) to indicate comparison and a single equals to indicate saving a value in a variable.
* Python uses _infix_ notation as in common mathematics notation, meaning that things like comparisons have a left side (`number_of_dependents`) and a right side (`0`), with an operator in the middle.
* Python also uses indentation to indicate different decision outcomes: everything indented below the `if`python is executed if the comparison is `true`python, and everything indented below the `else`python is executed if the condition is `false`python.
* `return` is a special word that takes a value calculated after the word and sends it back to whatever part of the program made the request.

Despite all of these syntactic differences from the machine code example earlier, it encodes the same logic: the Python interpreter essentially translates the above syntax into the instructions above, reading the `number_of_dependents` value, comparing it to `0`, and executing one of the return statements. So Python is just a different notation for encoding the same conditional logic that replaced the case worker.

Of course, the logic in Indiana’s system was more complicated than checking a single value. It’s program might have included code more like this:

`python
if (resident and 
    number_of_dependents > 0 and 
    number_of_people_in_household > 2 and 
    gross_monthly_income < 1000):
    return True
else:
    return False
`A conditional that uses Boolean `and`python logic ensure multiple conditions are true.

That example encodes much more logic than our earlier examples, checking not only the number of dependents, but also residency, household size, and gross income. The Python example is also much simpler than the machine code that would be required to encode all of those comparisons. Part of what makes this shorter is the use of the Boolean operator and (which we discussed earlier in @intelligence). In Python, an `and`python is evaluated by first evaluating the expression left of the `and`python, and the right, and evaluating to `true`python if both the left and the right evaluate to `true`python. That means that if _any_ of the conditions are `false`python, the entire conditional if is `false`python, resulting in the program above returning `False`python, denying the applicant. 

This comes closer to mimicking the logic that case workers might do, but of course, still fails to allow for information to be interpreted, clarified, or revised, and the comparisons to specific numbers -- 2 people, $1,000 USD -- is ruthlessly rigid. For example, a case worker might receive an application with $1,000 gross monthly income -- which is not technically less than $1,000 -- and approve it, because it’s really close to being less than $1,000. The algorithm, however, would not, because `1000`python is not less than `1000`python. Additionally, case workers had discretion to approve applications with an even higher income if there were mitigating factors, such as a large household or many dependents. These exceptions were not granted according to set rules, but based on the case workers’ experience and judgment, as allowed in state law.

To allow for more nuanced logic, programming languages include other kinds of control structures. For example, imagine the Indiana algorithm wanted to have a third possible decision, which allowed for a caseworker to review a case in more detail before deciding. It might use Python’s `elif`python syntax (which stands for _else if_) to allow for multiple cases:

`python
if number_of_rejections > 2
    return "Review"
elif (resident and 
      number_of_dependents > 0 and 
      number_of_people_in_household > 2 and 
      gross_monthly_income < 1000):
    return "Approve"
else:
    return "Deny"
`A conditional with three cases, two which check a condition, and one catch all case inside the `else`.

The algorithm above now has three possible outcomes. Imagine a policy, for example, that says that if someone has applied and been rejected more than twice, a case worker should review the application rather than an algorithm. The first condition checks that. If that condition is not true, then the `elif`python case is checked, which is the one from our earlier example that checks eligibility. If that case isn’t true, then the application is denied. The algorithm is inching closer to something resembling the logic that a caseworker might use, but again, has the same unforgiving, rigid thresholds. For example, in this algorithm, a case worker would never see an application until someone was rejected twice first{As it turns out, this is exactly what the Indiana system did, and it had an unintended consequence: it took multiple weeks to be rejected twice, and by the time an applicant got a review, they passed an undocumented time limit for case review and were required to resubmit from scratch.}.

We can combine and expand these control structures in additional ways to cover a wide range of possible situations. Consider, for example, a fourth option to indicate that additional information is needed from the applicant, perhaps due to an inconsistency in the data. This situation might be coded as follows:

`python
if number_of_rejections > 2
    return "Review"
elif (resident and 
      number_of_dependents > 0 and 
      number_of_people_in_household > 2 and 
      gross_monthly_income < 1000):
    return "Approve"
elif number_of_dependents > number_of_people_in_household:
    return "Request Information"
else:
    return "Deny"
`A conditional with four cases.

Here, the application indicates more dependents than people in the household, which would imply that some dependents were _not_ part of the household. This is certainly possible, but uncommon, so it would make sense to ask the applicant to confirm the situation. In the code, we check the first `elif`python case as described above, but if it is false, we check a second `elif`python case before proceeding to the `else`python.

As one more extension, maybe we don’t want to immediately deny the applicant if none of these conditions is true, but instead continue on with some additional code to perform further analysis. In this case, we could leave out the `else` entirely:

`python
if number_of_rejections > 2
    return "Review"
elif (resident and 
     number_of_dependents > 0 and 
     number_of_people_in_household > 2 and 
     gross_monthly_income < 1000):
    return "Approve"
elif number_of_dependents > number_of_people_in_household:
    return "Request Information"

# Add additional code here
`A conditional with no `else`python case, leaving the rest of the program to do more reasoning to make a decision.

In this example, the code will indicate that an application should be reviewed, accepted, or returned for additional information if any of those responses is appropriate. However, if _none_ of those apply, no result will be given at this time and additional work will be performed before a decision is made.{Python allows comments to be placed after lines that start with `#`python; these don't affect how a program executes, they just help the programmer make notes about the program.}

These various forms of conditionals allow us to get closer to the decision making of a human case worker, but at the cost of increasingly complex code. And, even with all this complexity, there will still be elements of human judgment that are difficult, if not impossible, to replicate. For example, what if the data the program is using is incorrect, because the person who entered it into the web form typed it in correctly or misunderstood a question? A case worker might notice that and double-check, whereas the algorithms above have no ability to inspect the accuracy of the data.

|Chapter11_Figure02_Implications.png|Five metal balls hanging from vertical wires, with one ball flying left after being hit by the other balls, cracking some glass. A small person looks on at the destruction.|Mindless repetition can break things.|@jessie|>

# Loops scale decision making

Of course, Indiana’s systems also had to process a never ending stream of applications. This is one of the great powers of software: to process large volumes of data quickly. To accomplish this, programming languages offer control structures that use the same basic machine instructions for jumping to different instructions to create *loops*. 

## Definite loops

For example, let’s expand on our program to process all of the unprocessed applications:

`python
for application in applications:
    # code to read values from application
    if number_of_rejections > 2
        status = "Review"
    elif (resident and number_of_dependents > 0 and 
          number_of_people_in_household > 2 and 
          gross_monthly_income < 1000):
        status = "Approve"
    else:
        status = "Deny"
    # code to write status to file
`A Python `for`python loop, which allows us to process a whole list of data using the same logic.

This enhanced algorithm uses the Python `for`python loop structure, which requires three additional syntactic elements:

* A variable in which to store the value currently being examined, in this case the programmer named `application`, but could have been named anything, such as `case`, `app` (or even `banana`!).
* The `in` keyword, which the [Guido van Rossum|https://en.wikipedia.org/wiki/Guido_van_Rossum], the creator of Python, could have named anything (e.g.,`from`, `of`, `inside`, but he chose the English proposition "in").
* A variable that stores a list of values, in this case `applications_file` (which again, could have been named anything).

This Python `for`python loop instructs the program to read the values in the list, one at a time, and execute the same set of instructions on each value in the list. This is what is known as a ~definite loop~definiteloop, because it has a fixed number of times that it will execute, based on the length of the list. The `for`python loop executes all of the indented{It's important to note that in Python, unlike other programming languages, indentation matters. In this loop, for example, the indentation of each line below the `for`python loop indicates that which statements are to be executed in the loop. If we were to remove the indentation, they would no longer be part of the loop, and would instead execute after the loop.} instructions for each application, one at a time. If this file of applications had 1 item, it would execute the indented instructions once, for the single item in the file; if the file had 10,000 applications, it would execute it 10,000 times, once for each application, one at a time.

Some programming languages include another kind of definite loop that offers more flexibility, but also more opportunity for errors. For example, _Javascript_, _Java_, _C_, and _C++_ have a for loop that requires a programmer to provide three parts: an initialization, a stopping condition, and an iteration step. Here's the same Python program above but in Java:

`java
for (int appIndex=0; appIndex < applications.length; appIndex++) {
    // code to read values from application
    if(number_of_rejections > 2) {
        status = "Review"
    } else if(resident and number_of_dependents > 0 &&
              number_of_people_in_household > 2 &&
              gross_monthly_income < 1000) {
        status = "Approve"
    } else {
        status = "Deny"
    }
    // code to write status to file
}
` The Python program above, but in Java, and using a Java `for`Java loop instead.

This is the exact same logic, but using a different control structure and a different syntax. For example, Java uses `{}` throughout instead of indentation. But more substantially, the for loop has the three parts we mentioned:

* It declares a variable `appIndex`java that keeps track of which application in a list of applications is being analyzed. This is nothing more than a number that starts at `0`.
* The second part, `appIndex < applications.length`java, is the stopping condition, which says when the loop should stop. In this case, this logic says that it should keep looping while the the current position in the list is less than the number of items in the list. For example, if there were 10 items, and the current item being just analyzed was 7, it would loop anther time. If it had just analyzed the last item (in this case, item 9, since we started counting at 0), then it would stop looping.
* The third part, `appIndex++`java, is code to execute after each loop. In this case, all it does is add one to the variable that keeps track of which item in the list we are analyzing.

Obviously, this is much more complicated than the Python loop, because it requires careful management of the variable `appIndex`: one simple mistake, such as initializing it to the wrong value, or forgetting to increase it by one, or having the wrong a stopping condition, might result in the program skipping over applications or other errors. Because these are so error prone, most modern programming languages do not include such control structures.{Alas, they are still present in many popular languages such as Java and JavaScript, and even appear in many standardized tests for students and teacher certification.}

Thus, `for`python loops in Python enable a powerful feature: the Indiana algorithm can process an arbitrarily large number of applications at lightning speed. A modern smartphone could process a list of millions of applications using this algorithm in less than a second. Of course, that incredible speed is at the expense of the nuanced interpretations that caseworkers can make to ensure just decisions.

## Indefinite loops

Of course, there's one limitation of the definition loop above: once it's done processing applications, it never processes them again, unless someone remembers to execute the program again. What this program needs is a way to process applications indefinitely, every time a new application arrives. For these situations, many programming languages offer a control structure called an ~indefinite loop~indefiniteloop. In Python, these are called `while`python loops:

`python
while has_another_application:
    # Add code to read the application’s data into variables
    if number_of_rejections > 2
        status = "Review"
    elif (resident and 
          number_of_dependents > 0 and 
          number_of_people_in_household > 2 and 
          gross_monthly_income < 1000):
        status = "Approve"
    else:
        status = "Deny"
    # Add code to write the status somewhere
` A python `while`python loop, which loops indefinitely until it's condition is false.

The new control structure in the code above is the `while`python statement at the top. This syntax tells the Python program to continue executing everything indented below it for as long as the variable `has_another_application`python is true. In this case, we are assuming this variable is somehow set to `True`python or `False`python depending on whether or not another application exists. Once that value becomes `False`python, the loop will stop, and not restart until someone (or some other part of the program) starts it again.

Imagine, however, that we wanted the program to process applications _forever_, never stopping for any reason. That would look like this:

`python
while True:
    # Add code to read the application’s data into variables
    if number_of_rejections > 2
        status = "Review"
    elif (resident and 
          number_of_dependents > 0 and 
          number_of_people_in_household > 2 and 
          gross_monthly_income < 1000):
        status = "Approve"
    else:
        status = "Deny"
    # Add code to write the status somewhere
`A python infinite loop, which has a condition that is always true.

This while loop will continue executing as long as `True`python is true -- in other words, forever, because when will `True`python ever be anything other than true? This type of indefinite loop is known as an infinite loop. In many cases, an infinite loop happens by accident when code is written incorrectly. But sometimes a programmer might intentionally want an algorithm to actually run forever. They might do this to indicate that the algorithm should always process applications, forever, until the program is halted by a person. This gets at the reliability goals of Indiana’s plans: computer programs can keep processing without fatigue, forever, as long as they have electricity and people to maintain them, whereas a caseworker needs to take breaks, a lunch, and eventually go home for the night. Of course, it also means that it processes applications without exception -- it doesn’t matter if there’s an earthquake, a change in law, a pandemic, or another crisis that might require reversing the decision logic. The algorithm, in this example, is ignorant of what is happening in the world, completely dependent on human beings to do the right thing in exceptional circumstances.

It might seem odd to think of a program doing something _forever_. After all, eventually computers are turned off, or run out of energy, or are rebooted, so no program really executes forever. Some programming languages offer a control structure that called ~event handlers~event, which allow programmers to express _condition-action_ rules. For example, the Scratch programming language 

|Chapter11_ScratchEvent.png|A Scratch program that says when I receive newApplication and then repeats the code in the previous code examples.|All Scratch programs are a collection of event handlers, like this one that response to a `newApplication` message.|Amy J. Ko|

One can think of event handlers like the one above an an _implicit_ loop, because they are implemented an infinite loop like the one above that repeatedly checks for new matching events from some external source (like a message from the internet or a mouse click), and one when occurs, it executes the event handler code. Most general purpose languages like Python and Java do not contain event handler features as part of the language. Instead, it is up to the programmer to use indefinite loops to implement them, or as we discuss in @abstractions, to use an API that uses indefinite loops to implement event handler patterns.


|Chapter11_Figure03_Eligibility.png|A large man at a laptop on the left looking down at the screen with a food stamp benefits form behind him, and tiny people lined up behind the laptop screen waiting for benefits.|Who decides matters, but too often, we don't discuss it.|@jessie|<

# Delegating decisions to developers

These examples reveal how control structures like machine code jump instructions and Python `if`python, `else`python, `for`python, and `while`python, statements can be quite powerful. By attempting to automate what people do, they can offer many clear benefits: the programs above will execute in the exact same way each time and they will do it without needing a break, a salary, or benefits. As long as data can faithfully represent an applicant’s life, and algorithms can faithfully execute a caseworker’s decision process, then computer programs can be a powerful aid to a caseworker, enabling them to process more applications, more consistently. It’s understandable why this would be an attractive technology to a public and government concerned with costs and fairness.

However, as we have seen, control structures, and the Boolean logic they use, can also be perilous:

* _What if the data the algorithm processes from an application is wrong?_ The algorithm will make the wrong choice, and may do nothing to try to detect errors, and alert caseworkers to the problem.
* _What if an exceptional case arises that isn’t encoded into law or the algorithm?_ There is no one to notice it, and make a different choice.
* _What if an applicant wants to understand why they were denied?_ The only person capable of fully explaining the algorithm’s decision are the software developers who wrote the algorithm -- and as any software developer can tell you, understanding the logic of a program is rarely as straightforward as the algorithm above, even when they wrote it themselves.
* _What if the decision made is unethical?_ The algorithms above, supported by conditional Boolean logic, are incapable of reconsidering its choices: they won’t pause to consider the injustice of the choice, they won’t question the morality of the law, they won’t look for ways of helping the applicant outside their logic, they won’t write the applicant for more information, and they certainly won’t rewrite their own code as it learns more about the world.

The downsides of using control structures to mimic human reasoning in particular contexts are therefore profound, to the point where the most moral and ethical choice may be to not use them at all for some decisions.

Of course, how sophisticated a program’s decisions are is purely a function of how human designers specified their requirements and how human software developers decided to implement these requirements. When Indiana awarded IBM a $10 billion contract to create the system, software developers were the ones deciding how to translate the logic of the case workers into code. How much did those developers talk to the case workers? How well were the caseworkers able to describe their logic? Did the developers talk to other other stakeholders, such as the people who had applied for SNAP benefits? How clear was Indiana state law about the legality of exceptions to eligibility requirements? If this IBM project was like most other contract-based software development projects, the software developers likely had very little knowledge of who was applying for benefits, where data came from, or what processes caseworkers used to verify data and handle edge cases. They likely just translated the ambiguous English text of Indiana state law into if statements, Boolean logic, and loops, unaware of what was lost in translation. In the case of Indiana’s SNAP system, the result was that the strict decision making of the system’s algorithms led to an increase in denials due to applicants’ inability to work with case workers to fill out forms correctly, which led to an increase in denials, and unlike the caseworkers, the denials gave no feedback about the reason for the denial. The result was that Indiana saved money, both by laying off caseworkers, but also because food insecure people in the state could no longer successfully acquire food stamps<eubanks18>.{IBM was eventually [held financially responsible|https://apnews.com/article/36ba0562a02142e5adfe39518e2e0f85] for some of these failures.}

Is it possible to ever faithfully represent human judgment with control structures? It’s certainly possible to get closer. The IBM project almost certainly had more sophisticated control logic than our examples above, checking many more conditions and having much more complex procedures for when to escalate applications to the caseworkers that remained on staff. Perhaps a robust design workshop with caseworkers would have led to logic that was more aligned with their practices, such as checking for errors and reaching out to applications to help them secure approval. Other more advanced logic might even be used, such as the statistical reasoning used in machine learning, to try to predict which applications might need human review to be successfully approved. But as we shall see in later chapters, even machine learning and other forms of artificial intelligence are limited in their ability to mimic human reasoning, especially reasoning that requires empathy, context, and moral judgment.

|Chapter11_Figure04_Observing.png|A young girl watching a traffic signal change and blink, very very closely, as she looms over traffic.|Watch programs closely; they decide quickly.|@jessie|

# Teaching control structures

As we discussed [earlier|languages:header-6], reading programs is a different skill than writing them. <nelson17,xie19>. Teaching control structures is therefore partly about helping youth understand precisely how control structures are executed by a computer, and partly about helping them decide how to use control structures to implement algorithms. *Reading* skills are the ability to accurately and reliably simulate the execution of a program in one’s head or on paper, accurately "tracing" how each part of a program executes, the order in which it executes, and how each part of a program uses or modifies data stored in memory. Once a student can reliably read programs written in a particular programming language, they are then well-prepared to learn *writing* skills for programs using control structures. Writing skills are distinct in that they require more than just understanding how programs execute, but also translating some desired behavior for a program into the syntax of the programming language. 

In this section, we'll focus on these reading skills, and save our discussion for writing for the @programming chapter that comes later.


## Teaching reading
As we discussed in the @languages chapter, reading skills involve two distinct kinds of content knowledge about a programming language: it’s ~syntax~syntax, and the corresponding ~semantics~semantics of that syntax. Teaching control structures therefore involves learning the rules about text must be formatted. One way to teach the rules of syntax is to provide a template that specifies the rules, as in this template for Python if statements:

`python
if Expression:
    Statement1
    Statement2
    ...
`The format of a Python `if`python statement.

This specifies what all valid Python statements must look like; however, the specific rules that govern this template are implicit: it must start with the keyword if, there must be a valid expression after the if (which requires knowing how to format valid expressions in Python), there must be a colon after the expression, there must be indentation for all statements that should be executed if the Expression is true. So providing templates alone often isn’t sufficient for helping students learn the rules of formatting. If any of these rules are violated, Python will give a *syntax error*. For example:

`python
if Expression
    Statement1
    Statement2
    ...
`An incorrectly formatted Python `if` statement

This Python if statement looks correct, but it violates a key rule. Can you see it? It’s missing a `:`, which will confuse Python, producing a vague `SyntaxError: invalid syntax error`, preventing the program from executing.

Here’s another example that doesn’t involve a syntax error, but does result in different behavior:

`python
if Expression
    Statement1
Statement2
`

In this case, `Statement1` will only execute if `Expression` is true, but `Statement2`, because it is not indented, will execute regardless of whether `Expression` is true, because after an `if`python statement executes, the program continues executing everything below it. This shows the important link between syntax and semantics: how something is formatted syntactically will determine what the program does.

Teaching _semantics_ requires modeling and scaffolding tracing skills for students. Without any guidance, students tend to avoid reading programs carefully, and often misinterpret how they execute, viewing program reading more as a search for what purpose code might have, rather than a document with a strict meaning dictated by a programming language's design<cunningham19>. To help students understand exactly how a machine will execute a program, one can show a program to students, and execute it, step by step, as the computer would, showing how each semantic rule is followed to interpret the meaning of the program<xie18>. For example, consider this excerpt from the earlier examples:

`python
if number_of_dependents > 0:
    return True
else:
    return False
`

To trace this example, we might execute the program twice, once with `number_of_dependents`python as `0`python, and once with a value greater than `0`python. With `0`python, we should show students, step by step, what the program does:

, Step | Action
,1. | Python starts at the first line of the program.
,2. | It checks the kind of statement that is executing, noticing that it is an `if`python statement.
,3. | Because it is an `if`python statement, it first evaluates the expression following the keyword.
,4. | To execute the expression, it follows the order of operations. But since there is only one operator -- the `>`python operator -- there is only one order possible.
,5. | To execute the inequality statement, it first evaluates the left side. The left side is a reference to a variable called `number_of_dependents`python, so Python checks its memory to see what the value of that variable currently is at this moment of execution (we decided above it would be `0`python).
,6. | Python remembers that the left side evaluated to 0, then checks the right side, which is a number, `0`python.
,7. | Now that it remembers the value of the left and the right side (`0`python and `0`python, respectively), it provides the two values to the `>`python greater-than operator. This operator resolves to `True`python if the left value is greater than the right value, and `False`python otherwise. In this case, the integer `0` is not greater than `0`python, so it resolves to `False`python.
,8. | Now that the `if`python statement’s conditional expression is resolved to False, the condition begins executing the statements indented underneath the `else`python keyword.
,9. | The first indented statement is return `False`python, which evaluates its condition, which is `False`python. The keyword return in this case tells the program to halt with the value `False`python. Notice that the statement above the `else`python keyword was skipped.

That’s a lot of steps in just that one short program! But every single rule discussed above is essential to being able to read the syntax and semantics of a program precisely. If students cannot do this reliably, they will not understand what programs they are reading will do, nor will they understand the programs they have written, leading to debugging, confusion, and ultimately eroded self-efficacy.

Of course, tracing a program’s execution can be incredibly dull -- and inevitably so, because being a computer that mindlessly follows a small set of rules over and over is inherently dull! One method for enriching tracing is to trace _critically_, raising questions about the limitations of each choice the computer is making. Consider, for example, the same trace above, but with critical reflections:

, Step | Action | Critique
,1. | Python starts at the first line of the program.| What if there’s a pandemic, and SNAP benefits should be distributed independent of eligibility criteria according to a government order? Is this a program we should even be running?
,2. | It checks the kind of statement that is executing, noticing that it is an `if`python statement. | Does the algorithm have enough information before making this decision? A human might gather more before deciding, but this program won’t.
,3. | Because it is an `if`python statement, it first evaluates the expression following the keyword. |
,4. | To execute the expression, it follows the order of operations. But since there is only one operator -- the `>`python operator -- there is only one order possible. | What counts as a dependent? What about children of divorced parents? Does this equality account for those cases?
,5. | To execute the inequality statement, it first evaluates the left side. The left side is a reference to a variable called `number_of_dependents`python, so Python checks its memory to see what the value of that variable currently is at this moment of execution (we decided above it would be `0`python). | What if this data was entered incorrectly, or is wrong? This algorithm doesn’t check the data’s validity.
,6. | Python remembers that the left side evaluated to 0, then checks the right side, which is a number, `0`python. |
,7. | Now that it remembers the value of the left and the right side (`0`python and `0`python, respectively), it provides the two values to the `>`python greater-than operator. This operator resolves to `True`python if the left value is greater than the right value, and `False`python otherwise. In this case, the integer `0` is not greater than `0`python, so it resolves to `False`python.  | What about cases where the number of dependents is changing? For example, a pregnant mother that will have a child soon, or a family that is about to adopt a foster child?
,8. | Now that the `if`python statement’s conditional expression is resolved to False, the condition begins executing the statements indented underneath the `else`python keyword. | Is there any way to reconsider this decision if new information arrived after evaluating the number of dependents? There is no going back at this point.
,9. | The first indented statement is return `False`python, which evaluates its condition, which is `False`python. The keyword return in this case tells the program to halt with the value `False`python. Notice that the statement above the `else`python keyword was skipped. | Someone was just denied benefits, without considering any of the factors above.

This method of bringing social context can be paired with having students bring in their own personal data as well. For example, rather than just choosing an arbitrary value for `number_of_dependents`python above, students could determine their own value, and trace the algorithm to see how it would decide their family’s eligibility.

After modeling tracing, then it’s time to teach students to trace programs independently. The best evidence suggests that tracing not only requires knowledge of syntax and semantics, but also strategies for scaffolding students’ attention to detail by providing them an explicit procedure<xie18>:

1. Create a memory table to remember the current values of variables.
2. Find where the program begins executing. At the start of that line, draw an arrow.
3. Execute each line according to the rules of the programming language.
3.. From the syntax, determine the rule for each part of the line. 
3.. Execute the rules.
3.. Update memory table with any new values of variables.
3.. Describe what the program did in steps (a)-(c) above.
4. Based on the rules, determine the next part of the program that will execute.
5. Return to step 2.

You might notice that following this procedure would lead to the examples of traces above. Assessing tracing skills can then involve analyzing the students’ trace for correctness; any errors suggest either a lack of vigilance in following the strategy above, or misconceptions about the programming language’s syntactic or semantic rules. In teaching these strategies, however, one must be mindful about the many different media that students might need to track the behavior of a program. Paper and pencil might be fine for non-disabled students, but those that are blind or have motor disabilities may need other ways of tracking the values of variables as a program executes, including computer-based methods.

One way of further integrating justice considerations into tracing is to have students critically reflect on the trace as well, identifying limitations of a program’s decisions. One might evaluate these by assessing the extent to which they connect the logic of the program to its consequences in the world.

## Assessing reading

While there are many things one can assess about CS knowledge, a surprising amount of both formative and summative assessment effort focuses on control structure reading skills. High stakes summative exams, including [Advanced Placement CS exams|https://apstudents.collegeboard.org/courses/ap-computer-science-a], the [Pearson|https://www.nestest.com/TestView.aspx?f=HTML_FRAG/NT315_TestPage.html] and [Praxis|https://www.ets.org/praxis/prepare/materials/5652] teacher certification exams, and many exams in higher education CS classes all focus heavily on control structure reading skills. Many researchers have developed carefully studied instruments for measuring CS knowledge<bockmon19,parker16>, but the majority of the items on those instruments are reading questions. And many curricula use formative assessment to help teachers monitor student learning, but those assessments generally focus on control structure reading skills.

Such assessments generally follow the same multiple choice format:

=
Use the pseudocode below to answer the question that follows.

`python
i = 1
n = 1
while (i <= 4)
    n = n * i
    i = i + 1
print(n)
`
What will this pseudocode print?
1. 4
2. 12
3. 16
4. 24{Try tracing this using the strategies above. Did you get it right? The correct answer is 24: the program uses `i`python to count from 1 to 4, and `n`python to compute, one number at a time, 4 factorial, the mathematical equivalent to 1 x 2 x 3 x 4.}
= 

These items generally require a student to know the syntax of the programming language being used, the semantic rules of that syntax that determine the program’s behavior, as well as having the self-regulation skills necessary for systematically and methodically following the logic of the program to determine it’s output. Unfortunately, many students do not have these self-regulation skills<loksa16b>, and instead often try to guess the behavior of the program based on the (arbitrary) variable names in the program, on previous programs that they’ve seen that look similar, or on any cues they might notice in the multiple choice options. Additionally, many assessment items require learners to combine their knowledge of multiple different programming language features (in the example above indefinite loops and inequalities), and if they poorly understand either, they might get the entire question wrong, offering little diagnostic insight into their developing conceptions of the language semantics<nelson19>. These biases, as well as gender and racial biases that are inadvertently encoded into story problems and other problem contexts<davidson21,xie21>, as well as disability biases that can make assessment content inaccessible to students who must use screen readers, can make many formative assessments of control structure reading skills poor indicators of student learning.

One approach to avoiding some of these challenges is to design items that very carefully assess precise aspects of control structure semantics, and nothing more<nelson19>. For example, whereas the item above requires knowledge of variables, indefinite loops, inequalities, multiplication, addition, and printing, the item below only assesses knowledge of variable assignments and addition:

=
Use the pseudocode below to answer the question that follows.

`js
x = 3
y = x + 5
`

What will `y` be after the program ends?

1. 3
2. 8
3. 5
4. 2
=

This item, if combined with more complex items that leverage these skills, will allow a teacher to isolate which parts of a programming language’s control structure features a student might be struggling with.

## Unit sketch: Reading control structures critically

While the methods above are enough to help students learn the syntactic and semantic rules of control structures in a language, and can even help connect control structures to issues of social justice, focusing on the abstract rules of a programming language alone may not resonate with students, and offers little insight into the ways that control structures can amplify and reinforce injustice. Here we provide an extension of the methods above that situate the learning more deeply in a social decision like the Indiana SNAP example, and then connect the automation of those decisions to control structures. As with all of the unit sketches in this book, this is not a complete lesson plan, but rather a skeleton that might inspire concrete, culturally responsive plans for your specific students.

The unit has the following learning objectives:

1. Students will be able to connect a social justice issue to control structure logic.
2. Students will be able to accurately and reliably trace control structure execution in a programming language.
3. Students will be able to critique the control structure logic of an algorithm in terms of equity and justice.
4. The first session opens the debate by starting with an issue close to students’ experiences in school.

=
### Session 1: Absence notifications
* Begin the first session by posing the problem of school absence, alluding to future sessions’ discussions of decisions that computers make.
* With everyone, brainstorm reasons why students miss school.
* Describe how your school’s absence policy interacts with parents: when and how are parents notified? Detail the conditions that must be true for parents to be notified and how the notification occurs.
* For each of the brainstormed reasons for missing school, solicit perspectives from students: should parents be notified in each case? What are the benefits? What are the potential harms?
* Identify the person who makes the decision about whether to notify parents (this may be administrative staff, teachers; if it is an automated system, identify the person who oversees the automated system). Discuss the extent to which students trust that person to account for the different contexts.
* End the session by synthesizing the nuances of the absence notifications, highlighting that context is critical in determining when a notification should occur, and that a person’s judgment is often necessary for interpreting context.
=

This session problematizes absence notifications, deepening students’ understanding of the social complexity of absence notifications. This is a critical foundation in which to situate discussions of automation and control structures in particular, as computing would be replacing whatever decision making and policies are in place in a school.

The second session builds upon this foundation, posing the possibility of automating notifications.

=
### Session 2: Automating absence notifications

* Remind students of the contextual nuances of absence notifications, then offer a hypothetical: the district is adopting a new automated system that uses software to monitor absences, and then make automated phone calls and text messages to parents or guardians about each day a student is absent.

* Provide an example Python program that shows the logic used by the software to loop through all students in the school, check their absent status, and then send notifications to parents and guardians. For example:

`python
for student in students_list:
    if student.absent == 1:
        status = "Warning to student"
    if student.absent < 3:
        status = "Warning to principal"
    else:
        status = "Automated phone call to parents"
`

* *Formative assessment*. Provide a list of the rules used to execute Python programs. Then, using the methods for teaching tracing above, have the class step through the program, attempting to follow the semantic rules and tracing the logic of the program one step at a time using example students that are and are not absent, while explaining each rule. At each step, have students pose questions about what each step will do, and how it would be different if the data or logic was different. Correct any misconceptions of the syntactic and semantic rules that surface. After each step, solicit from the class discrepancies between the context brainstormed in the previous session and the logic in the program. Is the logic of the program fair? What context or circumstances is it ignoring?
    ** This is _responsive_ in that the assessment centers students’ perceptions about the code, which may draw upon their prior experiences.
    ** This is _participatory_ in that students are generating the possible interpretations of the semantics, rather than being provided them.
    ** This is _educative_ in that students will see many possible interpretations, and be able to juxtapose them against the correct semantics.

* End the session by recapping the syntactic and semantic rules covered, and how they don’t quite capture the complexity of absence notifications.
=

This session develops students’ understanding of a programming language’s control structures, but is situated in the specific social context of absence notifications. By the end of the session, students should feel confident in their understanding of these rules, at least in the case of this one particular program.

The next session engages students in trying to address their critiques of the system.

=
### Session 3: Improving absence notification
* Remind students of the notification program and the critiques they made of it in the previous session.

* *Summative assessment*. Divide students into small groups and give them a copy of the program with the critiques embedded in it. State that their goal for this session is to design better decision making logic, but conceptually, not yet as code. They might think of a better set of conditions that cover the contexts from the first session, they might consider ways to incorporate human judgment into the algorithm, or they might argue for abandoning the automation altogether. Whatever their proposal, they need to write an English description of either the logic or process they would use instead and an argument for why it would be better than the current system. Co-construct how the descriptions will be graded, discussing the criteria you think are important and the criteria the students think are important.
    ** This is _responsive_ in that it centers students’ values and conceptions of justice in shaping the logic of the program.
    ** This is _participatory_ in that students are ensuring that the grading rubric aligns with their conceptions of justice.
    ** This is _educative_ in that students will have to work together with classmates to reconcile disagreements in what fairness means.

* Circulate amongst groups, facilitating their brainstorming, argumentation, and logic.

* End the session by having students save their proposals, and allude to the next session’s work to make their new system’s logic in terms of code.
=

This session repositions students from critic to designer, building upon their critical understanding from the first two sessions. By the end of this session, students should have an appreciation for the difficulty of conceiving of decision making logic that accounts for nuance, but also some confidence that they can do better.
In the next session, students shift from being designers to either engineers or activists, attempting to write their logic as code:

=
### Session 4: Implementing improvements

* Have student groups bring up their proposals from the previous session.

* *Summative assessment* (continued). Prompt them to translate their English proposal into code, using the syntactic and semantic rules they learned in Session 2. If there are behaviors they want to express that go beyond their knowledge, they can write them as comments. If a group’s proposal is to not use software, prompt students to develop an advocacy plan: who do they need to advocate to and how will they advocate in a way to prevent adoption of the system?

* Circulate amongst groups, facilitating their programming, helping them translate their ideas into code.

* End the session by surfacing the many translation challenges that teams faced to normalize the challenges, including challenges of translation to syntax, but also the limitations of logic in expressing the rules in their proposal.

=

This session should help students identify gaps in their understanding of the syntactic and semantic rules of the language they are learning, while also helping them identify gaps in their proposals’ original logic.

The last session keeps students in the position of engineers and activists, but reveals further nuances of their designs and activism plans.

=
### Session 5: Implementing improvements

* Have each group elect a presenter.

* Each presenter should trace the logic of their revised program or describe their activism plan. If they are tracing, the students should contribute in the same way as Session 2, identifying misalignments between the context in Session 1 and the logic in the group’s code. If they are presenting an activism plan, students should share feedback on ideas to improve their plan or concerns they have about its effectiveness.

* End the session by synthesizing the multiple layers of difficulty in writing even simple programs: understanding the social contexts in which a program will be used, identifying ideal behaviors for a program, translating those into code that may imperfectly express them, and then discovering problems with decision logic when sharing the program with a diverse audience.
=

These five sessions cover the rules of control structures in language, just as the method in the previous section, but do so by framing control structure logic as an alternative to human decision making. Importantly, it also challenges students to take multiple perspectives on the problem: stakeholder critics of the status quo and proposed change; designers of an improved process; and engineers of those improvements, subject to the limitations of code and their perspectives. This unit could be extended further, repeating this process with small groups that choose other decisions that software makes in society, and having students submit more complete working programs to reflect their proposed decision logic.

The key tradeoff of this approach is coverage of material. If the goal of a class is to help students build robust knowledge of the control structures in a programming language, the above unit will involve far less deliberate practice with reading and writing. Therefore, this unit explicitly deemphasizes that skill, prioritizing every students’ ability to relate control structures to their lives at school. The underlying hypothesis behind this trade is that students in secondary school will benefit more from a sociotechnical literacy of control structures than a purely technical one; after all, they can learn the purely technical skills elsewhere.

# Conclusion: the limits of control

Ultimately, control structures are severely limited in their ability to mimic human decisions. They lack nuance, context, reconsideration, morality, and humanity. As the Indiana case we discussed throughout this chapter, control structures and the Boolean judgments they rely upon are just brittle tools for capturing the smallest glimpses of human reasoning.

However, that does not mean that control structures are useless. While Indiana implementation of their SNAP system had the net effect of doing great harm to its food insecure residents, it didn't have to. Project leads could have carefully studied the decision making processes of the human case workers and the experiences of people applying or SNAP benefits. They could have used control structures to automate the parts of their decision making that were routine and unambiguous, but left case workers in charge of the more subjective, contextualized judgments they are frequently called to make. Framing the project as building software that supports and augments case workers' ability to help applicants by eliminating routine data management and paperwork could have _increased_ Indiana's ability to offer food security. Why didn't it? Partly because the IBM developers who wrote the software, and the politicians who commissioned it, didn't understand that control structures cannot replace human judgment. But also, Indiana's conservative leadership weren't trying to reduce food insecurity, they were trying to use code to reduce government spending -- and they were successful.

@standardsHeader
@standardsBlurb

@cstaHeader
@cstaIC
@csta2IC20
@csta3AIC24
@csta3AIC26
@csta3BIC25
@csta3BIC27
@cstaAP
@csta2AP12
@csta3AAP15

@toleranceHeader
@toleranceIdentity
@tolerance5
@toleranceDiversity
@tolerance8
@tolerance10
@toleranceJustice
@tolerance12
@tolerance14
@toleranceAction
@tolerance16
@tolerance19

@teacherHeader
@teacher1
@teacher1a
@teacher1d
@teacher1e
@teacher1f
@teacher2
@teacher2a
@teacher2b
@teacher2c
@teacher3
@teacher3b
@teacher4
@teacher4c
@teacher4e
@teacher4f
@teacher4g
@teacher5
@teacher5a
@teacher5b
@teacher5c
@teacher5d
@teacher5e
@teacher5f