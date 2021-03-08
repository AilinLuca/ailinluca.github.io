---
layout: post
title: Clean Code - Writing Code for Humans
categories: clean-code
permalink: pretty
---

Course URL: [https://app.pluralsight.com/library/courses/writing-clean-code-humans/table-of-contents](https://app.pluralsight.com/library/courses/writing-clean-code-humans/table-of-contents)

---

## INTRO

We'll walk through three core clean coding practices:

1. Select the right tool for the job.
2. Optimize the signal to noise ratio.
3. Create self-documenting logic.

Good programmers write code that humans can understand.

Clean Code helps you avoid technical debt.

Clean Code is the foundation of:

- SOLID Code outlined by Bob Martin.
  - More info: Steve Smith [http://bit.ly/13sEdjV](http://bit.ly/13sEdjV)
- Automated Testing
  - Legacy code has been defined as any code without tests.
  - Clean Code helps you write code that is modular and easy to test.
  - More info: Julie Lerman [http://bit.ly/18me0am](http://bit.ly/18me0am)
- Refactoring
- Design Patterns
  - More info: Steve Smith, et al. [http://bit.ly/15qxUwK](http://bit.ly/15qxUwK)
- TDD
  - More info: Mark Seeman [http://bit.ly/14FnXm0](http://bit.ly/14FnXm0)
- DDD

Resources:

- Steve McConnell Code Complete stevemcconnell.com
- Robert C. Martin Clean Code objectmentor.com
- Andrew Hunt, David Thomas The Pragmatic Programmer pragprog.com

---

## SUMMARY

Select the right tool for the job. Common web app tools:

- HTML -- semantic markup
- CSS -- separates styling from market
- Javascript -- provides behavior
- Serverside language (C#, Ruby, Python, etc) -- manages business logic
- SQL -- relational database, data access, and manipulation

Stay native. Respect the boundaries between tools

- Do not put HTML in a Javascript string. Do not put Javascript directly into HTML; eliminates caching and reuse opportunities. Javascript belongs in .js files. HTML belongs in .html files.
- Storing HTML in SQL binds presentation and data storage together, making reuse difficult.
- HTML mixed with CSS (inline styles) eliminates caching and reuse of CSS.
- Dynamic SQL in C# strings is not as elegant or safe.
- Using a server-side language to generate dynamic JS e.g. Dynamic JS in C# strings is not needed--JS can remain in a .js file and JSON can be returned from the server to provide dynamism.

Pros of staying native:

- Cached
- Code colored -- lose coloring when written within a string
- Syntax checked -- lose syntax checking when written within a string
- Separation of concerns. Separate files for each language are easier to check and maintain.
- Reusable
- Avoids string parsing on each request -- serve up the file once
- Can minify & obfuscate

Stay native. Avoid using one language to write another language/format via strings. E.g. Using strings in C#, Java, PHP, etc to create Javascript, XML, HTML, JSON, CSS. Creation of these format is a solved problem -- use libraries.

Strive for one language per file.

Every tech is potential evil

- Linq-to-sql are bad for massive queries, outer joins
- Flash/Silverlight are bad when native alternatives are an option
- Javascript is bad as the sole method of validation and should not be responsible for propriety logic

Signal to Noise Ratio
Signal -- Any logic that follows the TED rule:

- Terse
- Expressive
- Do one thing

The rule of 7. Only 7 things in short-term memory at a time.

- Apply to the number of parameters you assign a method.

Entropy is natural, so we need to meticulous.

DRY Principle. Don't repeat yourself.

- Many of the same principles of relational database normalization.
- Copy and paste is a sign of a design problem.
  - FIX: create a reusable function, add a parameter to an existing function.
- Every line of code is a liability.

Self-Documenting Code

1. Express intent clearly
2. Layers of abstraction can be used, so the problem domain can be walked through in different layers of detail
3. Format for readability
4. Favor code over comments

Self-documenting code can eliminate the need for wikis and javadocs.

---

## NAMING

Class names:

- A well-defined class should have a SINGLE RESPONSIBILITY
- Nouns
- Be specific. Be cohesive
  - Instance variables should all be used by a large number of the members of the class. If not, the class may be doing too many things.
- Avoid lame and generic suffixes that add no value "Product manager/info - bad. Project - ok"

Method names should be:

1. Descriptive -- no need to read body of method to understand what the method does
2. Verb phrases
3. Warning signs that you need to create two methods, not one:
   a. And
   b. If
   c. Or
4. Watch for side effects
5. Methods should not lie to your readers. They should only do what the name says they do.
6. Avoid abbreviations because there are no standards and are hard to say out loud

Boolean names:

1. Should sound like they are asking a true/false question
   a. E.g. isOpen, done, isActive, loggedIn

Variable names:

1. When dealing with states that toggle, consistently use matching pairs.
   a. E.g. on/off, fast/slow, lock/unlock, min/max

When struggling with a name, verbalize to a friend or a duck.

---

## CONDITIONALS

1. Good conditionals clearly convey intent -- why are we making the choice? How would we choose between options?
2. Use the right tool
3. Bite-sized logic
4. Sometimes code isn't the answer. Check when you can remove the conditional entirely.

### Write like English.

```
# BAD:
if(loggedIn == True)

# GOOD:
if(loggedIn)
```

The second is more like English.

### Assign booleans implicitly:

```
# BAD:
bool goingToChipotleForLunch;

if (cashInWallet > 6.00)
{
goingToChipotleForLunch = true;
} else {
goingToChipotleForLunch = false;
}

# GOOD:
bool goingToChipotleForLunch = cashInWallet > 6.00;
```

### Don't be anti-negative. Use positive conditionals.

```
# BAD:
if(isNotLoggedIn)

# GOOD:
if(loggedIn)
```

### Ternary is elegant

```
# BAD:
int registrationFee;

if (isSpeaker)
{
registrationFee = 0;
} else {
registrationFee = 50;
}

GOOD:
int registrationFee = isSpeaker ? 0 : 50;
```

(Though you also may have to switch to if else if there are more options…but YAGNE. Do not introduce complexity in the code just because you think you may need it in the future.)

Avoid chaining multiple ternary operators.

These good examples are all DRY.

### Avoid being "Stringly" typed.

```
# BAD:
if (employeeTYpe == "manager")

# GOOD:
if (employee.Type == EmployeeType.Manager)
```

- Strongly typed --> No typos!
- IntellisenseSupport helps fill in so you don't have to type as much
- Documents states
- Searchable

### Don't use numbers by themselves without context.

```
# BAD:
if (age > 21)

# GOOD:
const int legalDrinkingAge = 21;
if (age > legalDrinkingAge)


# BAD
if (status = 2)

# GOOD
if (status == Status.Active)
```

### To tame complex conditionals:

1. Use intermediate variables

```
# BAD:
If (employee.Age > 55
&& employee.YearsEmployed > 10
&& employee.IsRetired == true)

# GOOD:
bool eligibleForPension = employee.Age > MinRetirementAge && employee.YearsEmployed > minPensionEmploymentYears && employee.IsRetired;
```

2. Encapsulate complex conditionals. Favor expressive code over comments.

```
# BAD:
//Check for valid file extensions. Confirm admin or active.
if (fileExtension == "mp4" ||
    fileExtension == "mpg" ||
    fileExtension == "avi")
    && (isAdmin || isActiveFile);

# GOOD:
if (ValidFileRequest(fileExtension, isActiveFile, isAdmin))

private bool ValidFileRequest(string fileExtension, bool isActiveFile, isAdmin)
{
    return (fileExtension == "mp4" ||
        fileExtension == "mpg" ||
        fileExtension == "avi")
        && (isAdmin || isActiveFile);
}
    return validFileType && userIsAllowedToViewFile;
}
```

Could refactor, because method is doing 2 things, checking valid file extension and checking user access to file.

3. Favor polymorphism over enums for behavior, to avoid redundant switch statements.

```
# BAD:
private void LoginUser(User user)
{
    switch (user.Status)
        case Status.Active:
            //logic for active users
            break;
        case Status.Inactive:
            //logic for inactive users
            break;
        case Status.Locked:
            //logic for locked users
            break;
    }
}

# GOOD:
private void LoginUser(User user)
{
    user.Login():
}

private abstract class User
{
    public string FirstName;
    public string LastName;
    public Status Status;
    public int AccountBalance;

    public abstract void Login();
}

private class ActiveUser : User
{
    public void Login()
    {
        //Active user logic here
    }
}

private class InactiveUser : User
{
    public void Login()
    {
        //Active user logic here
    }
}

private class InactiveUser : User
{
    public void Login()
    {
        //Inactive user logic here
    }
}

private class LockedUser : User
{
    public void Login()
    {
        //Locked user logic here
    }
}
```

So you can then have a switch statement that determines which one of these users is instantiated, but the details of these user states are contained in the classes.

4. Be declarative if possible (may not be an option in your language)

```
# BAD:
List<User> matchingUsers = new List<User>();

foreach (var user in users)
{
    if (user.AccountBalance < minimumAccountBalance
        && user.Status == Status.Active)
        {
            matchingUsers.Add(user);
        }
}

return matchingUsers;

# GOOD:
return users
    .Where(u => u.AccountBalance < minimumAccountBalance)
    .Where(u => u.Status == Status.Active);

Here use C#s linked objects, similar to lambdaJ for Java, jlink for javascript, pink for Python.
```

5. Table Driven Methods

```
# BAD: (Hard-coding below)
if (age < 20)
{
    return 345.60m;
}
else if (age < 30)
{
    return 419.50m;
}
else if  (age < 40)
{
    return 476.38m;
}
else if (age < 50)
{
    return 516.25m;
}

# GOOD:
Replace with a table in the database to be queried.

return Repository.GetInsuranceRate(age);
```

Good for insurance rate, pricing structures, complex and dynamic business rules.

---

## FUNCTIONS

Function vs method?
The only difference is that methods are associated with an object.

When to create a function

- To avoid duplication
- Indentation is a sign of complexity -- so to avoid indenting too deep (also called "Arrow Code")
- To clarify intent
- To cut down functions that do more than 1 task

To eliminate excessively deep indentation:

1. Extract method
   a. Extract the deepest code to a separate function. Work from the inside out.
2. Return early
   a. If you have nothing more to do, return.
   i. Even if there are multiple return values, it enhances readability.
3. Fail fast
   a. If there's nothing more that you can do in a method, bail.
   b. Put the guard clauses first. They are a contract for what is required for the method to run.
   i. Can be turned into a named method if there are a large number of checks.
   c. With switch statements -- every switch statement should have a default value so you know exactly what would happen if requirements are not met.

Write functions to convey intent (e.g. through function name)

Do one thing because:

1. Aids the reader
2. Promotes reuse
3. Eases naming and testing
4. Avoids side-effects

Mayfly variables:
Initializing variables at the top is confusing. Strive to give variables mayfly lifetimes. Intialize variables in time and remove when no longer needed.

Strive for 0-2 parameters.
Helps assure function does one thing.

Watch for flag arguments (boolean parameters). A sign that the function is doing two things.

```
# BAD:
private void SaveUser(User user, bool emailUser)
{
//save user
if (emailUser)
{
//email user
}
}

# GOOD:
private void SaveUser(User user)
{
//save user
}

private void EmailUser(User user)
{
//email user
}
```

What is too long?

- You have a lot of whitespace & comments
- Scrolling is required
- You're having a hard time coming up with a name describing what the function does
- There are multiple conditionals
- It's hard to digest -- too many layers of abstraction
  - High numbers of parameters and variables

Bob Martin, Clean Code suggests:

- Rarely over 20 lines
- Hardly ever over 100 lines
- No more than 3 parameters

Linux style guide suggests simple functions should be shorter, more complex should be longer.

Kinds of Exceptions:

- Unrecoverable:
  - Null reference
  - File not found
  - Access denied
- Recoverable
  - Retry connection
  - Try different file
  - Wait and try again--but ultimately giving up
- Ignorable
  - E.g. Logging click, or other data collection failure that doesn't impact system function

To handle exceptions:

- Do not catch exceptions you can't handle intelligently. Let it bubble up. All exceptions must be caught and handle.
- Throw an unchecked exception is best safety net.

Logging an error is not enough. Try/Catch/Log can be failing slow. Code should stop executing if the error means the system cannot reliably move on. Here, logging the RegisterSpeaker() error and proceeding to EmailSpeaker() is bad because the speaker thinks they are registered, when they are not.

```
# BAD
try
{
RegisterSpeaker();
}
Catch(Exception e)
{
LogError(e);
}

EmailSpeaker();

# GOOD:
RegisterSpeaker();
EmailSpeaker();
```

To keep Try/Catch blocks readable, keep the body of the try in a function.

```
# BAD:
try
{
//many lines of logic
}
catch (ArgumentOUtOfRangeException)
{
//do something
}

# GOOD:
try
{
SaveThePlanet();
}
catch (ArgumentOutOfRangeException)
{
//do something here
}

private void SaveThePlanet()
{
//many lines of logic
}
```

---

## CLASSES

OOP References (see above)

- SOLID Principles
- Design Patterns

When to create a class:

- Model a new concept -- both abstract or real world
- Split up classes with low cohesion -- methods in classes should relate, or the classes should be split so they do
- Promote reuse -- small and targeted results in more reuse
- Reduce complexity -- solve once, and then hide away complexity
- Clarify parameters -- help identify a group of data; especially if you end up frequently passing the same variables and functions

Cohesion
Class responsibilities should be strongly related.

- Enhances readability
- Increases likelihood of reuse
- Avoids attracting lazy developers. Keep class names descriptive.

To avoid creating low-cohesion classes:

- Watch for methods that don't interact with the rest of the class
- Watch out for fields that are only used by one method
- Classes that change often

Example of a Low Cohesion Class:
Vehicle:

- Edit vehicle options
- Update pricing
- Schedule maintenance
- Send maintenance remind-
- Select financing
- Calculate monthly payment

Example of a High Cohesion Class:
Vehicle

- Edit vehicle options
- Update pricing
  VehicleMaintenance
- Schedule maintenance
- Send maintenance reminder
  VehicleFinance
- Select financing
- Calculate monthly payment

Sniffing out lack of cohesion:
BAD NAMES LIKE

- Utility
- Common
- MyFunctions
- JimmysObject

Signs a class is too small:

- Inappropriate intimacy -- classes call a large portion of each other's methods
- Feature envy -- one class relies heavily on another class
- Too many places -- if the classes are too small it's hard to understand how the features fit together
  Generally too small classes are rare.

Primitive Obsession:
If you pass off too many disparate parameters a lot, they may need to be lumped into a class.
EXCEPTION: If the method only takes one or two parameters from user class, then showing these parameters makes clear what from the class is needed.
BAD:
private void SaveUser(string firstName, string lastName, string state, string zip, string eyeColor, string phone, string fax, string maidenName)

GOOD:
private void SaveUser(User user)

Benefits: 1. Helps reader conceptualize 2. Makes the implicit explicit 3. Encapsulates -- so if there are any changes to the object, the method is not broken 4. Aids in code maintenance

Principle of Proximity

- Strive to make code read top to bottom
- Keep related actions together

The Outline Rule
Collapsed code should read like an outline.
Strive for multiple layers of abstraction.

---

## COMMENTS

Overreliance on comments is not good. Comments must have a good reason to exist.

    1. Prefer expressive code over comments
    	a. Code is more likely to be kept updated
    2. Use comments when code alone is insufficient

Avoid the following comments:
• Redundant -- do not repeat what the code says
• Intent -- clarify intent in code with:
○ Improved function naming
○ Intermediate variable
○ Constant or enum
• Apology
○ Fix it or add a to-do marker
• Warning
○ Just refactor
• Zombie code -- commented out code
○ Bury it
○ Common causes:
§ Risk aversion
§ Hoarding mentality
○ Bad:
§ Damages comprehension
§ Creates ambiguity -- should this code exist or not?
□ Takes time and hinders refactoring
○ Remember, code isn't lost. There is source control.
○ About to comment out code? Ask yourself:
§ When, if ever, would this be uncommented?
§ Can I just get it from source control later?
§ Is this incomplete work that should be worked via a branch?
§ Is this a feature that should be enabled/disabled via configuration?
§ Did I refactor out the need for this code?
• Dividers and brace trackers
○ Comments used as dividers, just refactor
○ Brace trackers are when the code gets so long, a comment is added to explain what the brace is attached to. Refactor.
• Bloated header
○ Avoid line endings (e.g. asterisk formatting is hard)
○ Don't repeat yourself. E.g. filename, author, created date should be visible in other places
○ Follow your language's comment style conventions.
• Defect log
○ Change meta-data like defect fixes should be saved in source control

Clean comments:
• TODOs for improvements in the future, HACK
○ Can be quickly jumped through VSC
○ Standardize them
○ Watch out for apologies or warnings in disguise -- TODOs should be short-term plans
• Summary comments
○ Describes intent at general level higher than the code
○ Often useful to provide high level overview of classes
○ Risk: Don't use to simply augment poor naming/code level intent
○ E.g. //Encapsulates logic for calculating retiree benefits or //Generates custom newsletter emails.
• Documentation
○ E.g. //See www.facebook.com/api for documentation

Before you write a comment, ask yourself: 1. Could I express what I'm about to write in code?
a. Intermediate variable, eliminate magic number, utilize enum
b. Refactor to a well-named method
i. Separate scope
ii. More likely to stay updated
iii. Better testability 2. Am I explaining bad code I've just written instead of refactoring? 3. Should this simply be a message in a source control commit?

---

## DEMO

Visual studio provides code metrics that evaluate the maintainability of the code. To run, right click on the project and select Calculate Code Metrics.
• Maintainability index -- 0 is bad, 100 is good.
• Cyclomatic Complexity -- number of paths
• Lines of code compared to actually lines occupied, if significant difference indicates lots of spaces so formatting improvements are available

Automated tests
First step: Make sure you have tests ready to ensure your edits don't cause regressions.

Then deleted unnecessary comments.

Then started pulling down variables closer to where they are actually used.

---

When to refactor: 1. When you need to work with the code. If it's working and no one needs to look at it, don't refactor. 2. When you find the code difficult to comprehend or change. 3. When you have sufficient test coverage to protect from regression first.

Broken windows: Accept no broken windows.

Code reviews and pair programming helps prevent broken windows.
• Real-time code review
• Naming and refactoring is easier

### The Boy Scout Rule

Always leave the code you're editing a little better than you found it.

```

```
