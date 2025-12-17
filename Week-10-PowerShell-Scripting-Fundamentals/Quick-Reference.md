# Week 10 Quick Reference: PowerShell Scripting Fundamentals

Fast reference guide for PowerShell scripting concepts and commands.

---

## Script Basics

### Creating and Running Scripts
```powershell
# Create script file
notepad MyScript.ps1

# Run script
.\MyScript.ps1

# Run with full path
& "C:\Scripts\MyScript.ps1"

# Check execution policy
Get-ExecutionPolicy

# Set execution policy (CurrentUser scope)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Script Structure
```powershell
# Script header (optional but recommended)
# MyScript.ps1 - Description of script
# Author: Your Name
# Date: 2024-12-17

# Script code here
Write-Host "Hello, World!"
```

---

## Variables

### Variable Creation
```powershell
# Basic variable
$name = "John"
$age = 25
$isActive = $true

# Variable types are inferred automatically
$number = 42          # Integer
$text = "Hello"       # String
$flag = $true         # Boolean
```

### Variable Operations
```powershell
# Display variable
$name
Write-Host $name

# String concatenation
$fullName = $firstName + " " + $lastName

# String interpolation
$message = "Hello, $name"

# Complex expressions in strings
$info = "Name: $($person.Name), Age: $($person.Age)"
```

### Variable Types
```powershell
# Explicit type casting
[int]$number = "42"
[string]$text = 123
[bool]$flag = "true"

# Check variable type
$number.GetType()
```

### Special Variables
```powershell
$PSVersionTable      # PowerShell version info
$HOME                 # User home directory
$PWD                  # Current directory
$PROFILE              # PowerShell profile path
$env:COMPUTERNAME     # Computer name
$env:USERNAME         # Current user
$_                    # Current pipeline object
$args                 # Script arguments
```

---

## Data Types

### Arrays
```powershell
# Create array
$fruits = @("Apple", "Banana", "Cherry")
$numbers = 1, 2, 3, 4, 5

# Access elements
$fruits[0]            # First element
$fruits[-1]           # Last element

# Add to array
$fruits += "Date"

# Array length
$fruits.Count
$fruits.Length
```

### Hash Tables (Dictionaries)
```powershell
# Create hash table
$person = @{
    Name = "Alice"
    Age = 30
    City = "London"
}

# Access values
$person.Name
$person["Name"]

# Add/Modify
$person.Email = "alice@example.com"

# Get keys/values
$person.Keys
$person.Values
```

---

## Input and Output

### Getting Input
```powershell
# Read user input
$name = Read-Host "Enter your name"
$age = Read-Host "Enter your age"

# Read secure input (password)
$password = Read-Host "Enter password" -AsSecureString
```

### Displaying Output
```powershell
# Write to console
Write-Host "Hello" -ForegroundColor Green
Write-Output "Hello"
"Hello"                # Also writes to output

# Format output
Write-Host "Name: $name" -ForegroundColor Cyan -BackgroundColor Black
```

### Output Redirection
```powershell
# Write to file
"Text" | Out-File -FilePath "file.txt"
"Text" | Out-File -FilePath "file.txt" -Append

# Suppress output
Get-Process | Out-Null

# Display and save (Tee-Object)
Get-Process | Tee-Object -FilePath "processes.txt"
```

---

## Conditional Statements

### If Statements
```powershell
# Simple if
if ($age -ge 18) {
    Write-Host "Adult"
}

# If-else
if ($age -lt 18) {
    Write-Host "Minor"
} else {
    Write-Host "Adult"
}

# If-elseif-else
if ($score -ge 90) {
    Write-Host "A"
} elseif ($score -ge 80) {
    Write-Host "B"
} else {
    Write-Host "C"
}
```

### Comparison Operators
```powershell
# Equality
-eq          # Equal
-ne          # Not equal

# Comparison
-lt          # Less than
-le          # Less than or equal
-gt          # Greater than
-ge          # Greater than or equal

# Pattern matching
-like        # Wildcard match
-notlike     # Not wildcard match
-match       # Regular expression match
-notmatch    # Not regex match

# Containment
-contains    # Array contains value
-notcontains # Array does not contain value
-in          # Value in array
-notin       # Value not in array
```

### Logical Operators
```powershell
-and         # Logical AND
-or          # Logical OR
-not         # Logical NOT
-xor         # Exclusive OR
```

### Switch Statements
```powershell
switch ($day) {
    "Monday" { Write-Host "Start of week" }
    "Friday" { Write-Host "End of week" }
    default { Write-Host "Midweek" }
}
```

---

## Loops

### For Loop
```powershell
# For loop
for ($i = 1; $i -le 10; $i++) {
    Write-Host $i
}

# ForEach loop
foreach ($item in $collection) {
    Write-Host $item
}

# ForEach-Object (pipeline)
1..10 | ForEach-Object {
    Write-Host $_
}
```

### While Loop
```powershell
# While loop
$counter = 1
while ($counter -le 10) {
    Write-Host $counter
    $counter++
}
```

### Do-While Loop
```powershell
# Do-while (runs at least once)
do {
    $input = Read-Host "Enter number (1-10)"
} while ($input -lt 1 -or $input -gt 10)
```

### Loop Control
```powershell
# Break - exit loop
foreach ($item in $items) {
    if ($item -eq "stop") {
        break
    }
}

# Continue - skip to next iteration
foreach ($item in $items) {
    if ($item -eq "skip") {
        continue
    }
    Write-Host $item
}
```

---

## Functions

### Basic Function
```powershell
# Define function
function Get-Greeting {
    param(
        [string]$Name
    )
    return "Hello, $Name!"
}

# Call function
Get-Greeting -Name "Alice"
```

### Function with Multiple Parameters
```powershell
function Add-Numbers {
    param(
        [int]$Num1,
        [int]$Num2
    )
    return $Num1 + $Num2
}

Add-Numbers -Num1 5 -Num2 3
```

### Function with Default Values
```powershell
function Write-Message {
    param(
        [string]$Message = "Default message",
        [string]$Color = "White"
    )
    Write-Host $Message -ForegroundColor $Color
}
```

---

## Error Handling

### Try-Catch
```powershell
try {
    Get-Content "file.txt" -ErrorAction Stop
} catch {
    Write-Host "Error: $($_.Exception.Message)" -ForegroundColor Red
}
```

### ErrorAction Parameter
```powershell
# SilentlyContinue - suppress errors
Get-Item "file.txt" -ErrorAction SilentlyContinue

# Continue - show error but continue
Get-Item "file.txt" -ErrorAction Continue

# Stop - throw exception
Get-Item "file.txt" -ErrorAction Stop

# Inquire - ask what to do
Get-Item "file.txt" -ErrorAction Inquire
```

### ErrorActionPreference
```powershell
# Set default error behavior
$ErrorActionPreference = "Continue"
$ErrorActionPreference = "Stop"
$ErrorActionPreference = "SilentlyContinue"
```

---

## Common Cmdlets

### File Operations
```powershell
# Get file/folder info
Get-Item "C:\Path\File.txt"
Get-ChildItem "C:\Path"        # List directory
Get-Content "file.txt"         # Read file

# Create items
New-Item -Path "file.txt" -ItemType File
New-Item -Path "folder" -ItemType Directory

# Copy/Move/Remove
Copy-Item "source.txt" "dest.txt"
Move-Item "source.txt" "dest.txt"
Remove-Item "file.txt"

# Test path
Test-Path "file.txt"            # Returns True/False
```

### Process Operations
```powershell
# Get processes
Get-Process
Get-Process -Name "notepad"

# Stop process
Stop-Process -Name "notepad"
Stop-Process -Id 1234

# Start process
Start-Process "notepad.exe"
```

### Service Operations
```powershell
# Get services
Get-Service
Get-Service -Name "Spooler"

# Start/Stop service
Start-Service -Name "Spooler"
Stop-Service -Name "Spooler"
Restart-Service -Name "Spooler"
```

### System Information
```powershell
# Computer info
Get-ComputerInfo
Get-CimInstance Win32_OperatingSystem
Get-CimInstance Win32_ComputerSystem

# Memory
Get-CimInstance Win32_PhysicalMemory

# Disk
Get-Volume
Get-PSDrive

# Network
Get-NetIPAddress
Get-NetAdapter
Test-Connection -ComputerName "8.8.8.8"
```

---

## Pipelines

### Basic Pipeline
```powershell
# Pass output to next command
Get-Process | Where-Object {$_.CPU -gt 100}
Get-Process | Sort-Object CPU -Descending
Get-Process | Select-Object ProcessName, Id
```

### Pipeline Examples
```powershell
# Filter and format
Get-Process | 
    Where-Object {$_.WorkingSet64 -gt 100MB} | 
    Sort-Object WorkingSet64 -Descending | 
    Select-Object -First 10 | 
    Format-Table

# Process each item
Get-ChildItem | ForEach-Object {
    Write-Host $_.Name
}
```

---

## String Operations

### String Methods
```powershell
$text = "Hello World"

$text.ToUpper()           # "HELLO WORLD"
$text.ToLower()           # "hello world"
$text.Length              # 11
$text.Substring(0, 5)    # "Hello"
$text.Replace("World", "PowerShell")  # "Hello PowerShell"
$text.Contains("Hello")  # True
$text.StartsWith("H")     # True
$text.EndsWith("d")       # True
```

### String Formatting
```powershell
# Format string
$name = "Alice"
$age = 30
"Name: {0}, Age: {1}" -f $name, $age

# String interpolation
"Name: $name, Age: $age"
```

---

## Date and Time

### Get Date
```powershell
# Current date/time
Get-Date

# Formatted date
Get-Date -Format "yyyy-MM-dd"
Get-Date -Format "yyyy-MM-dd HH:mm:ss"

# Date operations
(Get-Date).AddDays(7)
(Get-Date).AddMonths(1)
(Get-Date).AddYears(1)
```

---

## Logging

### Basic Logging
```powershell
# Write to log file
$logFile = "script.log"
"Log entry" | Out-File -FilePath $logFile -Append

# With timestamp
$timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
"[$timestamp] Log entry" | Out-File -FilePath $logFile -Append
```

### Transcript
```powershell
# Start transcript
Start-Transcript -Path "transcript.txt"

# Commands here
Get-Process
Get-Service

# Stop transcript
Stop-Transcript
```

### Custom Logging Function
```powershell
function Write-Log {
    param(
        [string]$Message,
        [string]$Level = "INFO"
    )
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
    $logEntry = "[$timestamp] [$Level] $Message"
    Write-Host $logEntry
    $logEntry | Out-File -FilePath "script.log" -Append
}
```

---

## Useful Operators

### Arithmetic
```powershell
+           # Addition
-           # Subtraction
*           # Multiplication
/           # Division
%           # Modulus
++          # Increment
--          # Decrement
```

### Assignment
```powershell
=           # Assign
+=          # Add and assign
-=          # Subtract and assign
*=          # Multiply and assign
/=          # Divide and assign
```

### Type Operators
```powershell
-is         # Type check
-isnot      # Not type check
-as         # Type conversion
```

---

## Common Patterns

### Input Validation Loop
```powershell
do {
    $input = Read-Host "Enter number (1-10)"
    $isValid = [int]::TryParse($input, [ref]$null)
} while (-not $isValid -or $input -lt 1 -or $input -gt 10)
```

### Check File Exists
```powershell
if (Test-Path "file.txt") {
    Write-Host "File exists"
} else {
    Write-Host "File not found"
}
```

### Process Each File
```powershell
Get-ChildItem -Filter "*.txt" | ForEach-Object {
    Write-Host "Processing: $($_.Name)"
    # Process file here
}
```

### Error Handling Pattern
```powershell
try {
    $result = Get-Content "file.txt" -ErrorAction Stop
    Write-Host "Success"
} catch {
    Write-Host "Error: $($_.Exception.Message)" -ForegroundColor Red
    exit 1
}
```

---

## Execution Policy

### Check Policy
```powershell
Get-ExecutionPolicy
Get-ExecutionPolicy -List
```

### Set Policy
```powershell
# For current user
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# Policies:
# Restricted      - No scripts can run
# AllSigned       - Only signed scripts
# RemoteSigned    - Local scripts OK, remote must be signed
# Unrestricted    - All scripts can run (not recommended)
```

---

## Help System

### Get Help
```powershell
# Basic help
Get-Help Get-Process

# Detailed help
Get-Help Get-Process -Detailed

# Examples
Get-Help Get-Process -Examples

# Full help
Get-Help Get-Process -Full

# Online help
Get-Help Get-Process -Online
```

### Find Commands
```powershell
# Find commands
Get-Command Get-*
Get-Command *Process*

# Find by verb
Get-Command -Verb Get

# Find by noun
Get-Command -Noun Process
```

---

## Script Parameters

### Define Parameters
```powershell
param(
    [string]$Name,
    [int]$Age,
    [switch]$Verbose
)

Write-Host "Name: $Name, Age: $Age"
```

### Parameter Attributes
```powershell
param(
    [Parameter(Mandatory=$true)]
    [string]$Name,
    
    [Parameter(Mandatory=$false)]
    [int]$Age = 25,
    
    [ValidateRange(1,100)]
    [int]$Score
)
```

---

## Best Practices

1. **Use meaningful variable names**
   ```powershell
   $userName = "Alice"      # Good
   $u = "Alice"             # Bad
   ```

2. **Comment your code**
   ```powershell
   # This function calculates the total
   function Get-Total { ... }
   ```

3. **Handle errors**
   ```powershell
   try {
       # Code here
   } catch {
       # Error handling
   }
   ```

4. **Use approved verbs**
   ```powershell
   # Approved: Get, Set, New, Remove, Test, etc.
   function Get-User { ... }
   ```

5. **Validate input**
   ```powershell
   if (-not $input) {
       Write-Host "Input required"
       return
   }
   ```

---

## Quick Tips

- Use `$PSVersionTable` to check PowerShell version
- Use `Get-Member` to see object properties: `$object | Get-Member`
- Use `Select-Object` to choose specific properties
- Use `Where-Object` to filter objects
- Use `Sort-Object` to sort results
- Use `Measure-Object` for statistics
- Use `Format-Table` or `Format-List` for better output
- Use `-WhatIf` parameter to preview changes
- Use `-Confirm` parameter for confirmation prompts

---

**End of Quick Reference - Week 10**

*Keep this guide handy while scripting!*

