## PowerShell Best Practices
<img src="./Images/BestPracticesKey.jpg" alt="Description of Image" width="20%" height="20%">

## Table of Contents

- [Introduction](#Introduction)
  - [Why are best practices important in PowerShell](#Introduction-why)
- [Naming Conventions](#NamingConventions)
  - [Capitalization Conventions](#Capitalization)
  - [PowerShell Naming Conventions](#PowerShellNamingConventions)
- [Bracing Style](#BracingStyleinPowerShell)
- [Indentation: Crafting Readable and Maintainable PowerShell Code](#Indentation)
  - [Why is Indentation Important?](#Indentation-why)
  - [Handling Indentation for Long Lines](#Indentation-LongLines)
- [PowerShell functions](#PowerShellfunctions)
  - [Function Names](#FunctionNames)
  - [Function Parameters](#FunctionParameters)
  - [Function Output](#FunctionOutput)
  - [Function Comment-Based Help](#FunctionCommentBasedHelp)
  - [Advanced Functions](#AdvancedFunctions)
  - [Begin Process End](#BeginProcessEnd)
- [Documenting and Comments in PowerShell](#DocumentingandCommentsinPowerShell)


### Introduction <a id="Introduction"></a>

PowerShell, as a powerful task automation and configuration management framework, has seen significant adoption across many IT environments. From managing on-premises servers to orchestrating cloud environments and even handling routine administrative tasks, its versatility and ubiquity are undeniable. 

However, like any other scripting or programming language, how one writes and organizes PowerShell code can deeply impact not just the functionality, but also the maintainability, readability, and scalability of the solutions built.

**Why are best practices important in PowerShell?** <a id="Introduction-why"></a>

1. **Maintainability**: PowerShell scripts and modules often don't remain in a vacuum. They evolve, adapting to new requirements or changes in the environment. Well-structured, standardized code ensures that when modifications are needed, they can be implemented with minimal friction and less likelihood of introducing errors.

2. **Collaboration**: In many organizations, scripts are not written by a single individual. They are shared, reviewed, and improved upon by teams. Best practices ensure that code is consistently readable to all members, regardless of the original author.

3. **Error Minimization**: By following certain standards and practices, one can avoid common pitfalls and mistakes. This is particularly vital in PowerShell, where a misstep can lead to significant system changes or even outages.

4. **Efficiency and Performance**: Some best practices directly influence the performance of your scripts. Efficient code runs faster and consumes fewer resources, a crucial factor especially when automating large-scale tasks or operations.

5. **Security**: PowerShell, given its potent capabilities, can be a double-edged sword. Adhering to best practices can help prevent security lapses, ensuring that scripts don't inadvertently expose sensitive data or become vulnerabilities themselves.

In this blog, we will delve into several key best practices for PowerShell. By adopting these standards and methodologies, you not only elevate the quality of your scripts but also ensure that they stand the test of time, adaptability, and collaborative scrutiny.


## **Naming Conventions** <a id="NamingConventions"></a>
Clear and consistent naming makes PowerShell scripts much easier to read and maintain. Using standardized conventions improves readability, prevents confusion, and enables smooth collaboration. First, let's explore the capitalization conventions. Then, we'll see how to apply them in PowerShell.

### Naming Convention (Capitalization Conventions) <a id="Capitalization"></a>

**lowercase**  - all lowercase, no word seperation
```powershell
$myvariable = 'Hello World'
```
**UPPERCASE**  - all capitals, no word seperation

```powershell
$MYVARIABLE = 'Hello World'
```
**PascalCase** - capitalize the first letter of each word

```powershell
$MyVariable = 'Hello World'
```

**carmelCase** - capitalize the first letter of each work except the first

```powershell
$myVariable = 'Hello World'
```
<br>

In PowerShell, **PascalCase** is the most commonly used convention, especially for variables, functions, and parameters.

### PowerShell Naming Conventions <a id="PowerShellNamingConventions"></a>
#### 1. **Variables** 
- Use **PascalCase** for variable names
```powershell
$UserProfile = "John"

$ServerList = @("Server1", "Server2")
```

#### 2. Functions
* Follow the **Verb-Noun** naming convention for functions
```powershell
function Get-MyFunction {
    # Function content
}
```
#### 3. Parameters
* Use **PascalCase** for parameter names
```powershell
function Get-MyFunction {
    param(
        [string]$FirstName,
        [string]$LastName
    )

    # Function content
}
```
#### 4. Aliases
* Avoid aliases in script for clarity, though they often use **lowercase**

❌ Wrong: ls | ? { $_.Name -eq "example.txt" }

✅ Right: Get-ChildItem | Where-Object { $_.Name -eq "example.txt" }

#### 5. Modules and Scripts
* **PascalCase** for module and script file names
  - `UserManagement.psm1` for a module
  - `Update-UserDetails.ps1` for a script

#### 6. Constants
* Use **UPPERCASE** for constants
```powershell
$MAX_USERS = 100

$CONFIG_PATH = "C:\config\settings.json"
```
<br>

There is a *speacial* case for **two-lettered** in which both letters are acronyms, e.g $PSBoundParameters or Get-PSDrive, you can find this method for example in Active Directory Module Get-ADUser, Get-ADGroup, Move-ADDirectoryServerOperationMasterRole ect.

### Conclusion

While PowerShell mainly favors PascalCase, understanding the range of naming conventions is beneficial. This ensures not just individual clarity, but a more harmonious collaborative scripting environment. Embrace these conventions and elevate the quality and clarity of your PowerShell scripts.

<br>

## **Bracing Style**<a id="BracingStyleinPowerShell"></a>

Bracing style greatly impacts readability and maintainability of code in PowerShell. While there are multiple approaches, this guide recommends One True Brace Style (OTBS) as an idiomatic standard for PowerShell.

**One True Brace Style (OTBS)**

Derived from the classic [K&R style](https://en.wikipedia.org/wiki/Indentation_style#K&R_style), OTBS has two rules:

* Opening braces go at the end of a line
* Closing braces start a new line

For example:
```powershell
enum Color {
  Black,
  White  
}

function Test-Code {
  [CmdletBinding()]
  param (
    [int]$ParameterOne
  )
  
  PROCESS {
    if (10 -gt $ParameterOne) {
      "Greater" 
    } else {
      "Lesser"
    }
  }
}
```

**The Exeption**

There is one exeption - small scriptblocks passed as parameters can be single line:
```powershell
Get-ChildItem | Where-Object { $_.Length -gt 10mb }
```

**Why OTBS for PowerShell?**

Using OTBS offers practical benifits:

* **Consistency** - No exceptions needed, universal style
* **Minimizes Errors** - Can add lines without disrupting statement blocks
* **Historical Precedence** - Early PS codeconstraints led to thie style
* **Aligned with ScriptBlocks** - Functions talking scriptblocks (like `ForEach-Object` already follow this style

Though initially controversial, OTBS improves readability and reduces errors - making it a compelling standard for PowerShell.

<br>

## Indentation: Crafting Readable and Maintainable PowerShell Code <a id="Indentation"></a>
Indentation, simply put, is the spacing at the beginning of a line of code. It is used to indicate the structure of a script, such as loops, conditionals, and functions.<br>
While seemingly a minor aspect of coding, is a pillar of writing clean, readable, and maintainable scripts.<br>
In PowerShell, indentation is done using the `[TAB]` key which uses four spaces.

```powershell
function Test-Code {
    foreach ($exponent in 1..10) {
        [Math]::Pow(2, $exponent)
    }
}
```

### Handling Indentation for Long Lines <a id="Indentation-LongLines"></a>

When lines exceed the standard length for readability (usually 115 characters), they may need to wrap onto multiple lines.

In these cases, indenting more than the typical 4 spaces is acceptable when it improves clarity.

The goal should be logical alignment with the preceding line. This might involve:

Indenting multiple levels
Using odd spaces to align with a method call or parameter set
For example:
```powershell
function Test-Code {
    foreach ($base in 1,2,4,8,16) {
        foreach ($exponent in 1..10) {
            [System.Math]::Pow($base,
                               $exponent)
        }
    }
}
```
Here the wrapped Pow call is indented further to align with the start of the command.

This indentation helps show the visual connection to the beginning of the line, improving readability of long chained lines.

So feel free to use more than 4 space indents when it creates cleaner multi-line code. Clarity should be the top priority.

## PowerShell functions <a id="PowerShellfunctions"></a>

Before we start writing our function we need to think about the following:
1. What is the purpose of the function?
2. What parameters does the function need?
3. What output does the function need to return?
4. How will the function be used? 
5. Will the function be used in a pipeline?
6. Will the function be used interactively?

80% of the work of writing a function is designing it. Once you have a clear idea of what you want to do, the actual code is easy.

### Function Names <a id="FunctionNames"></a>
Adopting best practices in naming PowerShell functions ensures clarity, consistency, and ease of use. Here are some best practices for naming PowerShell functions:

1. `Verb-Noun Convention`: PowerShell functions should follow a Verb-Noun naming convention, like Get-Content, Set-Item, or Invoke-Command.

2. `Use Approved Verbs`: Use verbs from the list of standard PowerShell verbs. You can get this list by running Get-Verb in a PowerShell session. This ensures consistency across functions and reduces confusion for users.

3. `Be Descriptive`: The name should clearly indicate the function's purpose. For instance, Get-ActiveUsers is more descriptive than Get-Users.

4. `Avoid Abbreviations`: Unless it's a universally understood abbreviation, spell things out. For example, ConvertTo-Json is preferable over ConvertTo-Jsn.

5. `Avoid Prefixes`: Unlike cmdlets in modules where a prefix might be standard (like AD for Active Directory cmdlets), functions generally don't require prefixes unless there's a specific reason.

6. `PascalCase`: Use PascalCase (capitalizing the first letter of each word) for function names, as it's the standard convention in PowerShell.

7. `Avoid Special Characters`: Stick to letters and numbers. Hyphens are the exception due to the Verb-Noun format.

8. `Singular Nouns`: Prefer singular nouns unless the function always deals with multiple items. For example, use Get-Item instead of Get-Items.

9. `Keep It Short`: While being descriptive is crucial, it's also essential to keep names reasonably short for ease of typing and readability.

10. `Consistency`: If you're developing a suite of related functions, maintain a consistent naming pattern. This helps users predict function names based on their understanding of one function.

### Function Parameters <a id="FunctionParameters"></a>

Parameters allow PowerShell functions to be flexible, interactive, and respond dynamically. Follow these practices for optimal parameters:

1. Use descriptive names - Choose intuitive names that indicate the parameter's purpose, like `-FilePath` rather than `-Path`.
2. Set data types - Always specify data types like `[string] `or `[int]` so the function receives expected input. This will improve error handling and prevent unexpected behavior and also helps users understand what to pass in.
3. Make mandatory parameters required - Use `[Parameter(Mandatory)]` for parameters that are essential for the function.
4. Default optional values - Provide sensible defaults for optional parameters like `$Parameter = 'Default'`.
5. Limit positional parameters - Only make frequently used parameters positional to avoid confusion.
6. Validate inputs - Use validation attributes like `[ValidateSet()]` to validate correctness.
7. Validate inputs - If applicable to your function, use  `ValueFromPipeline` and `ValueFromPipelineByPropertyName` to enable pipeline input. You should use it if your function is designed to work with pipeline input.
8. Avoid parameter overload - If you have too many parameters, consider simplifying or splitting the function.
9. Switch parameters - Do not set a default value for switch parameters or make it mandatory. 
10. Document thoroughly - Use comment-based help to describe parameters in detail.

### Parameter Validateion <a id="ParameterValidateion"></a>

Parameter validation is a critical technique for enhancing the reliability and robustness of PowerShell functions. By validating parameter input using attributes like `[ValidateNotNull]`, `[ValidatePattern]`, and `[ValidateSet]`, you can catch errors and incorrect data early before it causes issues downstream in your code.

Validation also serves as documentation, making it clear to users what data is expected for each parameter. Implementing parameter validation improves user experiences, prevents bugs, and makes functions more resilient to bad input data.

Overall, validating parameters is a best practice that pays dividends in stability and usability for any non-trivial PowerShell function.

```powershell
function Get-Greeting {
    [CmdletBinding()]
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [string]$Name
    )

    process {
        "Hello, $Name!"
    }
}

# Usage
Get-Greeting -Name "Alice"  # Returns: Hello, Alice!
Get-Greeting -Name ""       # Throws a validation error
Get-Greeting                # Throws a mandatory parameter missing error
```

Use these official Microsoft documentation to learn more about [Validating Parameter Input](https://learn.microsoft.com/en-us/powershell/scripting/developer/cmdlet/validating-parameter-input?view=powershell-5.1) and [How to Validate an Argument Pattern](https://learn.microsoft.com/en-us/powershell/scripting/developer/cmdlet/how-to-validate-an-argument-pattern?view=powershell-5.1)

### Function Output <a id="FunctionOutput"></a>

There are two main ways to return output from a PowerShell function:

**`Write-Output`**

The `Write-Output` cmdlet writes objects to the pipeline. It is the default output method for PowerShell functions. If you don't specify an output method, PowerShell will use `Write-Output` by default.
```powershell
# Do not use this
function Get-Something {
  $path = "C:\Reports\MyReport.xlsx"
  $path
}

# Use this instead
function Get-Something {
  $path = "C:\Reports\MyReport.xlsx"
  Write-Output $path
}
```

Although `Write-Output` is the default, it is my personal preference to always use it explicitly. This makes it clear that the function is returning output, and also makes it easier to change the output method later if needed.

**`return`**

The return keyword exits the current scope and passes values back to the caller. It is commonly used to return a single value from a function.
```powershell
function Get-Something {
  $path = "C:\Reports\MyReport.xlsx"
  return $path
}
```

**`return` vs `Write-Output`**

* `return` is used to return a single value from a function. `Write-Output` is used to return multiple values from a function.
* `return` exits the current scope. `Write-Output` does not exit the current scope.

So, when should I use `return` and when should I use `Write-Output`?

We should prefer to use `Write-Output` over `return` in most cases. 
We should use `return` when we want to exit the current scope and return a single value from a function.

### Function Comment-Based Help <a id="FunctionCommentBasedHelp"></a>

PowerShell `functions` are foundational to creating `reusable` and efficient scripts, `encapsulating` complex or repetitive operations into modular, callable routines.<br>
**But** as with any piece of code, `clarity and understandability` are vital for maintainability and collaboration. Herein lies the value of *`comment-based help`*.

By incorporating comment-based help directly in the function (although above the function is also supported), developers can provide a comprehensive overview of the function's purpose, parameters, outputs, examples, and more. This self-contained documentation ensures that anyone using or reviewing the function has immediate access to its intended usage and behavior.

### Advanced Functions <a id="AdvancedFunctions"></a>

`CmdletBinding` is an attribute that, when applied to a function or script, transforms it into an advanced function or script. Advanced functions/scripts mimic the behavior of cmdlets, and this similarity is intentional, providing a uniform experience for users.

#### **Why CmdletBinding is Essential**

1. **Enhanced Features**: Using `CmdletBinding` unlocks numerous advanced features for functions and scripts, such as common parameters (`-Verbose`, -`WhatIf`, etc.), enhanced parameter validation, and control over pipeline input processing.

2. **Pipeline Input**: By starting with the `CmdletBinding` attribute, you're already considering the importance of pipeline input. PowerShell's pipeline is one of its most potent features, allowing for streamlined data processing.

3. **Flexibility**: Even if you don't need all the features of an advanced function immediately, starting with `CmdletBinding` makes it easier to add those features later without restructuring your code.

4. **Common parameters**: Common parameters are a set of parameters that are available to all cmdlets and advanced functions. They provide a consistent user experience and are a key part of PowerShell's discoverability and usability.

5. **Verbose and Debug Output**: By default, advanced functions and scripts support the `-Verbose` and `-Debug` common parameters. This allows you to provide additional information to users when needed.

6. **Positional Binding**: By default, our parameters are positional by default. This option dictates whether parameters in the advanced function can be bound by position. If you set `PositionalBinding=$false`, the function will only accept parameters that are explicitly named. You can always enable PositionalBinding at the parameter level if needed.

### **A Starter Template**

All your scripts or functions should ideally begin like the following snippet:

```powershell
[CmdletBinding()]
param ()
process {
}
end {
}
```

From this template:

- You can always omit or add blocks (`begin`, `process`, or `end`) as required.
  
- Introducing parameters, validation, and other enhancements becomes straightforward.

<br>

## **Begin Process End** <a id="BeginProcessEnd"></a>

In PowerShell, the `begin`, `process`, and `end` blocks are integral components of advanced functions that handle pipeline input. Each block serves a specific purpose when processing data, especially from the pipeline. Here's when and why you might use each:

1. **`begin` Block**:
    - **Purpose**: Executed once at the beginning of the function call, before any pipeline input is processed.
    - **When to use**:
        - To initialize variables or resources.
        - To open connections or files.
        - To validate prerequisites or set up logging.
        - If you're preparing something that will be used or needed in each `process` iteration.

2. **`process` Block**:
    - **Purpose**: Executed once for each item passed through the pipeline to the function.
    - **When to use**:
        - If your function is designed to handle or operate on each item from the pipeline individually.
        - For actions or calculations that need to occur on every piece of data/input.
        - Most common operations, especially filtering or modifying data, will occur here.

3. **`end` Block**:
    - **Purpose**: Executed once after all pipeline input has been processed.
    - **When to use**:
        - For cleanup tasks, such as closing connections or files.
        - To summarize or provide output after all processing is complete.
        - To release resources or handles.
        - If you need to consolidate and present results after processing all the pipeline data.



**Follow Param (), Begin, Process, End Order**

Scripts should define parameters, then blocks, in the order they execute:
```powershell
param() 

begin {}

process {}

end {}
```
**Why follow this structure?**

* **Clear Intent** - Order matches execution flow. Easy to follow.
* **Explicit is Better** - Omitting block names like `end` reduces clarity.
* **More Maintainable** - Out of order blocks confuse debugging/maintenance.
* **Avoid Confusion** - Features like `filter` hide the `process` block.

**In summary:**

Though parameter and block order is flexible in PowerShell, following the order of execution improves readability and maintainability. Stick to the `param()`, `begin`, `process`, `end` structure.

<br>

## **Documenting and Comments in PowerShell** <a id="DocumentingandCommentsinPowerShell"></a>

Proper documentation is the bridge between complex scripts and human understanding. Here's how to ensure your PowerShell scripts are both well-documented and easily understood:

1. **Self-Explanatory Code**: Ideally, `your code should explain itself`. While comments can be valuable, they shouldn't narrate every step. Comment only on segments that might be ambiguous or that employ non-obvious logic.

2. **Embrace Comment Based Help**: Every script and function you pen should have comment-based help. It's PowerShell's native method for embedding detailed, user-friendly documentation directly in your code. The time spent crafting comment-based help is *`the best investment`* in the future of your script.

3. **Inline Comments**: Use inline comments judiciously. They're most beneficial for succinct explanations spanning multiple, concise lines of code. 

4. **Simplicity in Language**: The goal is understanding, not showcasing vocabulary. Keep your comment-based help straightforward and jargon-free.

5. **Location Matters**: Aim to house your Comment Based Help within functions. This way, users can employ `Get-Help` directly on your function to retrieve detailed guidance.

6. **Detail in Notes**: The Notes section in comment-based help is where you can elaborate further. Offer additional context, background, or clarifications here.

7. **Examples with Clarity**: When documenting with `.Example`, accompany it with a brief description. Illuminate the purpose of the example, and where possible, mention the expected output. This not only instructs but also sets the right expectations.

8. **Detailed Parameter Documentation**: Every parameter should be documented thoroughly. Use inline `# Param` comments for a quick reference. However, for more extensive descriptions, rely on the `.Parameter` tag in comment-based help. It offers readers a deeper understanding of each parameter's purpose and expected input.

In essence, a well-documented script minimizes misunderstandings, simplifies maintenance, and ensures that your code remains a valuable asset to others both now and in the future.
