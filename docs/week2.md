## Variables, Numbers and Strings

The PowerShell language is a weak typed, interpreted language.

Insert lengthy explanation here &rarr;

---

#### Variables

Variables are denoted by a dollar sign preceeding a name, ex.

```powershell
$hello = "Hello"
$world = " World!"
$hello + $world
# Hello World!
```

Having good descriptive varible names is crucial to writing good scripts, and is a practice that carries over to every programming language.

![Variable Name Meme](https://i.pinimg.com/originals/e6/73/bd/e673bdd42a9ae12000c59df5dfcae440.jpg)

When writing a script, try to follow a common naming case scheme like **snake_case** or **camelCase** [Link to Aricle](https://medium.com/better-programming/string-case-styles-camel-pascal-snake-and-kebab-case-981407998841)

From [about_Variables](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_variables):

```powershell
<#
A variable is a unit of memory in which values are stored.
In PowerShell, variables are represented by text strings
that begin with a dollar sign ($), such as $a, $process, or $my_var.

Variable names aren't case-sensitive, and can include spaces and special characters.

PowerShell variables are loosely typed, which means that they
aren't limited to a particular type of object. A single variable
can even contain a collection, or array, of different types of objects at the same time.
#>

ex:
$a = 12                         # System.Int32
$a = "Word"                     # System.String
$a = 12, "Word"                 # array of System.Int32, System.String
$a = Get-ChildItem C:\Windows   # FileInfo and DirectoryInfo types
```

---

#### Numbers

Numbers in PowerShell can either be used implicitly, or type cast into a more specific number type.

```powershell
# Single line comments are denoted by a # at the start of the line
# Anything written in a comment is ignored
5+5
# Returns 10
[int]"5"+5
# Also returns 10, because the string 5 is type cast to become an integer
```

> PowerShell will attempt to type cast all data following a data type to be the same, ex:

```powershell
function add5([int]$number) {
    return $number + 5;
}

add5(10)
# Returns 15

add5("hello")
<# Returns:

add5 : Cannot process argument transformation on parameter 'number'. Cannot convert value "hello" to type
"System.Int32". Error: "Input string was not in a correct format."
At line:5 char:5
+ add5("hello")
+     ~~~~~~~~~
    + CategoryInfo          : InvalidData: (:) [add5], ParameterBindingArgumentTransformationException
    + FullyQualifiedErrorId : ParameterArgumentTransformationError,add5

#>

function addString5([string]$number) {
    return $number + 5;
}

addString5(10)
# Returns 105, because 10 is type cast to be a string, and 5 is type cast to be a string

# If the following data is unable to be cast to the first data type, PowerShell will throw an error
```

---

#### Strings

Strings are sequences of characters, usually arranged in words. In PowerShell you can _usually_ tell something is a string if it is encased in double quotes, "", single quotes '', or backticks \`\`.

This article - [Variable Substitution](https://powershellexplained.com/2017-01-13-powershell-variable-substitution-in-strings/) has great examples on variable substituion within strings in PowerShell

This article from the MS Docs explains all [special_Characters](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_special_characters)

---

#### Booleans

A boolean is either _true_ or _false_. To a computer this is represented by a single bit. If true, the bit is _1_. If false the bit is _0_.

In PowerShell a boolean is represented by `$true` or `$false`, null is represented by `$null`. [All about Null](https://docs.microsoft.com/en-us/powershell/scripting/learn/deep-dives/everything-about-null?view=powershell-7)

![Differences Between Values](https://i.redd.it/vy1n3ionqgg51.png)

> This picture is in relation to JavaScript, but the main idea still transfers
