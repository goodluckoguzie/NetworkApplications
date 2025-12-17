# Week 10 Lab Guide: PowerShell Scripting Fundamentals

Complete guide showing how to create, execute, and understand PowerShell scripts on Windows.

---

## Lab Overview

### What is Week 10 About:
**Introduction to PowerShell Scripting - Automating Tasks with PowerShell Scripts**

### PC Requirements:
- **Windows 10/11 or Windows Server 2019/2022** (Virtual Machine, lab PC, or LOD platform)
- PowerShell 5.1 or PowerShell 7+ installed
- Administrator privileges (for some tasks)
- Basic familiarity with Windows CLI commands from previous weeks

### What You'll Do:
- **Activity 1:** Create your first PowerShell script with user input
- **Activity 2:** Use variables and data types correctly
- **Activity 3:** Add conditional logic (if/elseif/else)
- **Activity 4:** Validate user input and handle errors
- **Activity 5:** Create logging scripts with output redirection
- **Activity 6:** Capture and explain system behaviour for AE1

---

## Important Terms

| Term | Definition | Simple Explanation |
|------|------------|-------------------|
| PowerShell | Microsoft's command-line shell and scripting language | Windows command-line interpreter and automation tool |
| Script | Automated sequence of commands | File containing commands that run automatically |
| Cmdlet | PowerShell command | Built-in PowerShell command (verb-noun format) |
| Variable | Storage for data | Named container that holds values (prefixed with $) |
| Pipeline | Passing output between commands | Using | to send one command's output to another |
| Object | Structured data in PowerShell | Data with properties and methods |
| Parameter | Input to a cmdlet or function | Options that modify command behavior |
| Module | Collection of PowerShell commands | Package of related cmdlets and functions |
| Execution Policy | Security setting for scripts | Policy that controls which scripts can run |
| PS1 | PowerShell script file extension | Files ending in .ps1 are PowerShell scripts |

---

## Background Knowledge: Why PowerShell Scripting?

**What is PowerShell Scripting?**

PowerShell scripting allows you to automate Windows administration tasks, manage systems, and create powerful workflows without manual intervention.

**Why Learn PowerShell Scripting?**

1. **Automation:** Run multiple commands with one script
2. **Efficiency:** Save time on repetitive Windows administration tasks
3. **System Administration:** Manage users, services, files, and registry
4. **Windows Management:** Native integration with Windows systems
5. **DevOps:** Essential for Windows-based CI/CD pipelines and infrastructure management
6. **Problem Solving:** Quick solutions for complex Windows tasks

**Key Concepts:**

- **Scripts run line-by-line** - Each command executes in order
- **Variables store data** - Use `$variable` to access values
- **Objects have properties** - Access data using dot notation (`$object.Property`)
- **Pipelines connect commands** - Use `|` to pass data between commands
- **Cmdlets follow verb-noun pattern** - Examples: `Get-Process`, `Set-Location`, `New-Item`

**Key Files and Directories:**
- `C:\Scripts\` - Common location for scripts
- `$PSHOME\` - PowerShell installation directory
- `$PROFILE` - PowerShell profile file (personal configuration)
- Script files typically end with `.ps1` extension

---

## SETUP: PREPARING FOR WEEK 10

**What we're doing:** We need to set up our Windows environment for PowerShell scripting.

### Prerequisites:

1. **Start your Windows system**
   - Open your Windows VM, lab PC, or LOD platform
   - Log in with your credentials

2. **Open PowerShell**
   - Press **Windows Key + X**
   - Click **Windows PowerShell (Admin)** or **Terminal (Admin)**
   - *PowerShell window opens*

3. **Check PowerShell version**
   - Type: `$PSVersionTable`
   - Press Enter
   - *Shows PowerShell version information*

4. **Create a scripts directory**
   - Type: `New-Item -Path "$HOME\Scripts" -ItemType Directory -Force`
   - Press Enter
   - Type: `Set-Location "$HOME\Scripts"`
   - Press Enter
   - *You're now in your scripts directory*

5. **Check execution policy**
   - Type: `Get-ExecutionPolicy`
   - Press Enter
   - *Shows current execution policy*

**What you should see:**
```powershell
PS C:\Users\Student> $PSVersionTable

Name                           Value
----                           -----
PSVersion                      5.1.19041.4412
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.19041.4412
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1

PS C:\Users\Student> New-Item -Path "$HOME\Scripts" -ItemType Directory -Force

    Directory: C:\Users\Student

Mode                 LastWriteTime         Length Name
----                 -------------         ------ -----
d-----        17/12/2024     10:00        Scripts

PS C:\Users\Student> Set-Location "$HOME\Scripts"
PS C:\Users\Student\Scripts> Get-ExecutionPolicy
RemoteSigned
```

**Note on Execution Policy:**
- If you see `Restricted`, you'll need to change it to run scripts
- Type: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`
- This allows running local scripts (signed or unsigned)

---

## ACTIVITY 1: CREATING YOUR FIRST POWERSHELL SCRIPT

**Do on:** Windows system

**What we're doing:** We're creating a simple PowerShell script that greets the user by name.

**Goal:** Understand script structure, user input, and script execution.

**Time:** 20 minutes

---

## TASK 1: CREATE A SIMPLE GREETING SCRIPT

**What this does:** We're creating a script called `Greet.ps1` that asks for the user's name and displays a greeting.

### CLI METHOD

1. **Navigate to scripts directory**
   - Type: `Set-Location "$HOME\Scripts"`
   - Press Enter

2. **Create the script file**
   - Type: `notepad Greet.ps1`
   - Press Enter
   - *Opens Notepad with new file*
   - Click **Yes** if prompted to create the file

3. **Type the following script:**

```powershell
# Greet.ps1 - A simple greeting script

Write-Host "Welcome to PowerShell Scripting!" -ForegroundColor Green
$username = Read-Host "Enter your name"
$city = Read-Host "Enter your city"
Write-Host "Hello, $username!" -ForegroundColor Cyan
Write-Host "Welcome from $city." -ForegroundColor Yellow
```

4. **Save and exit**
   - Press **Ctrl + S** to save
   - Close Notepad (Alt + F4)

5. **View your script**
   - Type: `Get-Content Greet.ps1`
   - Press Enter
   - *Shows the script content*

**Script Explanation:**
- `# Greet.ps1` - Comment: describes the script (ignored during execution)
- `Write-Host` - Displays text to the screen with optional colors
- `Read-Host` - Prompts user for input and **automatically stores** it in the variable
- `$username` and `$city` - Variables that store the user's input
- **Variables are automatically created** - No need to declare them first!
- `-ForegroundColor` - Parameter to change text color

---

## TASK 2: RUN YOUR FIRST SCRIPT

**What this does:** We're executing the script we just created.

### CLI METHOD

1. **Run the script**
   - Type: `.\Greet.ps1`
   - Press Enter
   - *Script executes and prompts for input*

2. **Enter your information**
   - When prompted, type your name and press Enter
   - When prompted, type your city and press Enter
   - *Script displays personalized greeting*

**What you should see:**
```powershell
PS C:\Users\Student\Scripts> .\Greet.ps1
Welcome to PowerShell Scripting!
Enter your name: Alice
Enter your city: London
Hello, Alice!
Welcome from London.
```

**Important Notes:**
- Use `.\` prefix to run scripts in current directory
- Scripts must have `.ps1` extension
- Execution policy must allow script execution

---

## ACTIVITY 2: USING VARIABLES AND DATA TYPES

**What we're doing:** We'll learn about PowerShell variables, data types, and how to use them effectively.

**Goal:** Understand variable creation, data types, and string operations.

**Time:** 25 minutes

---

## TASK 1: UNDERSTANDING VARIABLES

**What this does:** We're creating a script to demonstrate different variable types and operations.

### CLI METHOD

1. **Create a variables script**
   - Type: `notepad Variables.ps1`
   - Press Enter

2. **Type the following script:**

```powershell
# Variables.ps1 - Variable demonstration

# String variable
$name = "John Doe"
Write-Host "Name: $name" -ForegroundColor Green

# Integer variable
$age = 25
Write-Host "Age: $age" -ForegroundColor Cyan

# Boolean variable
$isStudent = $true
Write-Host "Is Student: $isStudent" -ForegroundColor Yellow

# Array variable
$fruits = @("Apple", "Banana", "Cherry")
Write-Host "Fruits: $fruits" -ForegroundColor Magenta

# Hash table (dictionary)
$person = @{
    Name = "Alice"
    Age = 30
    City = "London"
}
Write-Host "Person Info:" -ForegroundColor White
Write-Host "  Name: $($person.Name)"
Write-Host "  Age: $($person.Age)"
Write-Host "  City: $($person.City)"

# Date variable
$today = Get-Date
Write-Host "Today: $today" -ForegroundColor Blue

# String concatenation
$greeting = "Hello, " + $name + "!"
Write-Host $greeting -ForegroundColor Green

# String interpolation (substitution)
$message = "Welcome, $name. You are $age years old."
Write-Host $message -ForegroundColor Cyan
```

3. **Save and run**
   - Save the file (Ctrl + S)
   - Close Notepad
   - Type: `.\Variables.ps1`
   - Press Enter

**Key Learning Points:**
- Variables start with `$` symbol
- No need to declare variable type (PowerShell infers it)
- Use `@()` for arrays
- Use `@{}` for hash tables
- Use `$()` for complex expressions in strings
- `Get-Date` gets current date/time

---

## TASK 2: VARIABLE OPERATIONS

**What this does:** We'll perform operations on variables.

### CLI METHOD

1. **Create an operations script**
   - Type: `notepad Operations.ps1`
   - Press Enter

2. **Type the following script:**

```powershell
# Operations.ps1 - Variable operations

# Get user input
$num1 = Read-Host "Enter first number"
$num2 = Read-Host "Enter second number"

# Convert to integers
$num1 = [int]$num1
$num2 = [int]$num2

# Perform calculations
$sum = $num1 + $num2
$difference = $num1 - $num2
$product = $num1 * $num2
$quotient = $num1 / $num2

# Display results
Write-Host "`n=== Calculation Results ===" -ForegroundColor Green
Write-Host "First Number: $num1" -ForegroundColor Cyan
Write-Host "Second Number: $num2" -ForegroundColor Cyan
Write-Host "Sum: $sum" -ForegroundColor Yellow
Write-Host "Difference: $difference" -ForegroundColor Yellow
Write-Host "Product: $product" -ForegroundColor Yellow
Write-Host "Quotient: $quotient" -ForegroundColor Yellow

# String operations
$text1 = "Hello"
$text2 = "World"
$combined = $text1 + " " + $text2
Write-Host "`nCombined Text: $combined" -ForegroundColor Magenta

# String methods
$upper = $combined.ToUpper()
$lower = $combined.ToLower()
Write-Host "Uppercase: $upper" -ForegroundColor Green
Write-Host "Lowercase: $lower" -ForegroundColor Green
```

3. **Save and run**
   - Save the file
   - Close Notepad
   - Type: `.\Operations.ps1`
   - Press Enter
   - *Enter two numbers when prompted*

**Key Learning Points:**
- Use `[int]` to convert strings to integers
- Arithmetic operations work on numeric types
- Strings have methods like `.ToUpper()` and `.ToLower()`
- Use backtick `` ` `` for line continuation or escape sequences
- Use `` `n `` for newline

---

## ACTIVITY 3: ADDING CONDITIONAL LOGIC

**What we're doing:** We'll add decision-making logic to our scripts using if/elseif/else statements.

**Goal:** Understand conditional statements and comparison operators.

**Time:** 30 minutes

---

## TASK 1: BASIC IF STATEMENTS

**What this does:** We're creating a script that makes decisions based on conditions.

### CLI METHOD

1. **Create a conditional script**
   - Type: `notepad Conditionals.ps1`
   - Press Enter

2. **Type the following script:**

```powershell
# Conditionals.ps1 - Conditional logic demonstration

# Get user age
$age = Read-Host "Enter your age"
$age = [int]$age

# Simple if statement
if ($age -ge 18) {
    Write-Host "You are an adult." -ForegroundColor Green
}

# If-else statement
if ($age -lt 18) {
    Write-Host "You are a minor." -ForegroundColor Yellow
} else {
    Write-Host "You are an adult." -ForegroundColor Green
}

# If-elseif-else statement
if ($age -lt 13) {
    Write-Host "Category: Child" -ForegroundColor Cyan
} elseif ($age -lt 18) {
    Write-Host "Category: Teenager" -ForegroundColor Yellow
} elseif ($age -lt 65) {
    Write-Host "Category: Adult" -ForegroundColor Green
} else {
    Write-Host "Category: Senior" -ForegroundColor Magenta
}

# Comparison with strings
$day = Read-Host "Enter day of week (Monday-Sunday)"
$day = $day.ToLower()

if ($day -eq "saturday" -or $day -eq "sunday") {
    Write-Host "It's the weekend!" -ForegroundColor Green
} else {
    Write-Host "It's a weekday." -ForegroundColor Yellow
}
```

3. **Save and run**
   - Save the file
   - Close Notepad
   - Type: `.\Conditionals.ps1`
   - Press Enter
   - *Enter age and day when prompted*

**Key Learning Points:**
- Use `-eq` for equals (not `==`)
- Use `-ne` for not equals
- Use `-lt` for less than
- Use `-gt` for greater than
- Use `-le` for less than or equal
- Use `-ge` for greater than or equal
- Use `-and`, `-or`, `-not` for logical operations
- Conditions go in parentheses `()`
- Code blocks go in curly braces `{}`

---

## TASK 2: PRACTICAL EXAMPLE - FILE CHECKER

**What this does:** We'll create a script that checks if files exist and provides information.

### CLI METHOD

1. **Create a file checker script**
   - Type: `notepad FileChecker.ps1`
   - Press Enter

2. **Type the following script:**

```powershell
# FileChecker.ps1 - Check file existence and properties

$filePath = Read-Host "Enter file path to check"

# Check if file exists
if (Test-Path $filePath) {
    Write-Host "File exists!" -ForegroundColor Green
    
    # Get file information
    $fileInfo = Get-Item $filePath
    
    Write-Host "`n=== File Information ===" -ForegroundColor Cyan
    Write-Host "Name: $($fileInfo.Name)"
    Write-Host "Full Path: $($fileInfo.FullName)"
    Write-Host "Size: $($fileInfo.Length) bytes"
    Write-Host "Last Modified: $($fileInfo.LastWriteTime)"
    
    # Check if it's a directory
    if ($fileInfo.PSIsContainer) {
        Write-Host "Type: Directory" -ForegroundColor Yellow
    } else {
        Write-Host "Type: File" -ForegroundColor Yellow
    }
} else {
    Write-Host "File does not exist!" -ForegroundColor Red
    Write-Host "Would you like to create it? (Y/N)" -ForegroundColor Yellow
    $create = Read-Host
    
    if ($create -eq "Y" -or $create -eq "y") {
        New-Item -Path $filePath -ItemType File -Force | Out-Null
        Write-Host "File created successfully!" -ForegroundColor Green
    }
}
```

3. **Save and run**
   - Save the file
   - Close Notepad
   - Type: `.\FileChecker.ps1`
   - Press Enter
   - *Try checking different file paths*

**Key Learning Points:**
- `Test-Path` checks if file/directory exists
- `Get-Item` retrieves file/directory information
- `PSIsContainer` property indicates if it's a directory
- `New-Item` creates new files or directories
- `Out-Null` suppresses output

---

## ACTIVITY 4: VALIDATING USER INPUT AND HANDLING ERRORS

**What we're doing:** We'll learn to validate user input and handle errors gracefully.

**Goal:** Create robust scripts that handle invalid input and errors.

**Time:** 30 minutes

---

## TASK 1: INPUT VALIDATION

**What this does:** We're creating a script that validates user input before processing.

### CLI METHOD

1. **Create a validation script**
   - Type: `notepad Validation.ps1`
   - Press Enter

2. **Type the following script:**

```powershell
# Validation.ps1 - Input validation example

# Function to validate email
function Test-EmailAddress {
    param([string]$email)
    
    $emailPattern = '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    
    if ($email -match $emailPattern) {
        return $true
    } else {
        return $false
    }
}

# Function to validate number
function Test-Number {
    param([string]$input)
    
    try {
        [int]$input | Out-Null
        return $true
    } catch {
        return $false
    }
}

# Get and validate email
do {
    $email = Read-Host "Enter your email address"
    if (-not (Test-EmailAddress $email)) {
        Write-Host "Invalid email format. Please try again." -ForegroundColor Red
    }
} while (-not (Test-EmailAddress $email))

Write-Host "Valid email: $email" -ForegroundColor Green

# Get and validate number
do {
    $number = Read-Host "Enter a number between 1 and 100"
    $isNumber = Test-Number $number
    
    if (-not $isNumber) {
        Write-Host "Invalid number. Please enter a numeric value." -ForegroundColor Red
        continue
    }
    
    $number = [int]$number
    if ($number -lt 1 -or $number -gt 100) {
        Write-Host "Number must be between 1 and 100." -ForegroundColor Red
    }
} while ($number -lt 1 -or $number -gt 100)

Write-Host "Valid number: $number" -ForegroundColor Green
```

3. **Save and run**
   - Save the file
   - Close Notepad
   - Type: `.\Validation.ps1`
   - Press Enter
   - *Try entering invalid data to see validation in action*

**Key Learning Points:**
- Use `do-while` loops for input validation
- `-match` operator uses regular expressions
- `try-catch` blocks handle errors
- Functions help organize validation logic
- `continue` skips to next loop iteration

---

## TASK 2: ERROR HANDLING

**What this does:** We'll create a script that handles errors gracefully.

### CLI METHOD

1. **Create an error handling script**
   - Type: `notepad ErrorHandling.ps1`
   - Press Enter

2. **Type the following script:**

```powershell
# ErrorHandling.ps1 - Error handling demonstration

# Set error action preference
$ErrorActionPreference = "Continue"

# Try-catch block
Write-Host "=== Try-Catch Example ===" -ForegroundColor Cyan

try {
    # This will fail - file doesn't exist
    $content = Get-Content "NonExistentFile.txt" -ErrorAction Stop
    Write-Host "File content: $content" -ForegroundColor Green
} catch {
    Write-Host "Error occurred: $($_.Exception.Message)" -ForegroundColor Red
    Write-Host "Error type: $($_.Exception.GetType().FullName)" -ForegroundColor Yellow
}

# Check if command succeeded
Write-Host "`n=== Success Check Example ===" -ForegroundColor Cyan

$processName = Read-Host "Enter process name to check"
$process = Get-Process -Name $processName -ErrorAction SilentlyContinue

if ($process) {
    Write-Host "Process found: $($process.ProcessName)" -ForegroundColor Green
    Write-Host "  ID: $($process.Id)"
    Write-Host "  Memory: $($process.WorkingSet64) bytes"
} else {
    Write-Host "Process '$processName' not found." -ForegroundColor Red
}

# Using -ErrorAction parameter
Write-Host "`n=== ErrorAction Parameter ===" -ForegroundColor Cyan

# SilentlyContinue - suppress errors
Get-Item "NonExistent.txt" -ErrorAction SilentlyContinue
Write-Host "Script continued despite error (SilentlyContinue)" -ForegroundColor Yellow

# Continue - show error but continue
Get-Item "NonExistent.txt" -ErrorAction Continue

# Stop - throw exception
try {
    Get-Item "NonExistent.txt" -ErrorAction Stop
} catch {
    Write-Host "Error caught and handled" -ForegroundColor Green
}
```

3. **Save and run**
   - Save the file
   - Close Notepad
   - Type: `.\ErrorHandling.ps1`
   - Press Enter

**Key Learning Points:**
- `try-catch` blocks handle exceptions
- `$_.Exception.Message` contains error message
- `-ErrorAction` parameter controls error behavior
- `SilentlyContinue` suppresses errors
- `Stop` throws exception that can be caught
- `$ErrorActionPreference` sets default error behavior

---

## ACTIVITY 5: CREATING LOGGING SCRIPTS WITH OUTPUT REDIRECTION

**What we're doing:** We'll learn to save script output to files for logging and record-keeping.

**Goal:** Understand output redirection, logging, and file operations.

**Time:** 25 minutes

---

## TASK 1: BASIC OUTPUT REDIRECTION

**What this does:** We're creating a script that saves output to files.

### CLI METHOD

1. **Create a logging script**
   - Type: `notepad Logging.ps1`
   - Press Enter

2. **Type the following script:**

```powershell
# Logging.ps1 - Output redirection and logging

# Create log file with timestamp
$logFile = "script_log_$(Get-Date -Format 'yyyyMMdd_HHmmss').txt"
$timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"

# Method 1: Redirect all output
Write-Host "=== Method 1: Redirect All Output ===" -ForegroundColor Cyan
"Log entry at $timestamp" | Out-File -FilePath $logFile -Append
"System information logged" | Out-File -FilePath $logFile -Append

# Method 2: Use Start-Transcript
Write-Host "`n=== Method 2: Transcript ===" -ForegroundColor Cyan
$transcriptFile = "transcript_$(Get-Date -Format 'yyyyMMdd_HHmmss').txt"
Start-Transcript -Path $transcriptFile

Write-Host "This is being logged to transcript" -ForegroundColor Green
Get-Date
Get-ComputerInfo | Select-Object WindowsProductName, TotalPhysicalMemory

Stop-Transcript

# Method 3: Tee-Object (display AND save)
Write-Host "`n=== Method 3: Tee-Object ===" -ForegroundColor Cyan
Get-Process | Select-Object -First 5 ProcessName, Id | 
    Tee-Object -FilePath "processes.txt" | 
    Format-Table

# Method 4: Custom logging function
function Write-Log {
    param(
        [string]$Message,
        [string]$Level = "INFO"
    )
    
    $timestamp = Get-Date -Format "yyyy-MM-dd HH:mm:ss"
    $logEntry = "[$timestamp] [$Level] $Message"
    
    # Write to console
    switch ($Level) {
        "ERROR" { Write-Host $logEntry -ForegroundColor Red }
        "WARNING" { Write-Host $logEntry -ForegroundColor Yellow }
        "SUCCESS" { Write-Host $logEntry -ForegroundColor Green }
        default { Write-Host $logEntry -ForegroundColor White }
    }
    
    # Write to file
    $logEntry | Out-File -FilePath $logFile -Append
}

Write-Host "`n=== Method 4: Custom Logging Function ===" -ForegroundColor Cyan
Write-Log "Script started" "INFO"
Write-Log "Processing data" "INFO"
Write-Log "Operation completed successfully" "SUCCESS"
Write-Log "Warning: Low disk space" "WARNING"
Write-Log "Error: File not found" "ERROR"
Write-Log "Script finished" "INFO"

Write-Host "`nLog files created:" -ForegroundColor Green
Write-Host "  - $logFile"
Write-Host "  - $transcriptFile"
Write-Host "  - processes.txt"
```

3. **Save and run**
   - Save the file
   - Close Notepad
   - Type: `.\Logging.ps1`
   - Press Enter
   - *Check the created log files*

**Key Learning Points:**
- `Out-File` writes output to file
- `-Append` adds to existing file
- `Start-Transcript` / `Stop-Transcript` records all console output
- `Tee-Object` displays AND saves output
- Custom functions provide flexible logging
- Use timestamps in log entries

---

## TASK 2: SYSTEM INFORMATION LOGGER

**What this does:** We'll create a comprehensive system information logging script.

### CLI METHOD

1. **Create a system logger script**
   - Type: `notepad SystemLogger.ps1`
   - Press Enter

2. **Type the following script:**

```powershell
# SystemLogger.ps1 - System information logger

$reportFile = "system_report_$(Get-Date -Format 'yyyyMMdd_HHmmss').txt"

Write-Host "Generating system report..." -ForegroundColor Cyan

# Create report header
@"
========================================
System Information Report
Generated: $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')
========================================

"@ | Out-File -FilePath $reportFile

# System Information
Write-Host "Collecting system information..." -ForegroundColor Yellow
@"

=== System Information ===
Computer Name: $env:COMPUTERNAME
User: $env:USERNAME
OS: $((Get-CimInstance Win32_OperatingSystem).Caption)
Version: $((Get-CimInstance Win32_OperatingSystem).Version)
Architecture: $((Get-CimInstance Win32_OperatingSystem).OSArchitecture)

"@ | Out-File -FilePath $reportFile -Append

# Memory Information
Write-Host "Collecting memory information..." -ForegroundColor Yellow
$memory = Get-CimInstance Win32_ComputerSystem
$totalMemory = [math]::Round($memory.TotalPhysicalMemory / 1GB, 2)
@"
=== Memory Information ===
Total Physical Memory: $totalMemory GB

"@ | Out-File -FilePath $reportFile -Append

# Disk Information
Write-Host "Collecting disk information..." -ForegroundColor Yellow
Get-Volume | Where-Object {$_.DriveLetter} | ForEach-Object {
    $drive = $_.DriveLetter + ":"
    $size = [math]::Round($_.Size / 1GB, 2)
    $free = [math]::Round($_.SizeRemaining / 1GB, 2)
    $used = [math]::Round(($_.Size - $_.SizeRemaining) / 1GB, 2)
    $percentFree = [math]::Round(($_.SizeRemaining / $_.Size) * 100, 2)
    
    @"
Drive: $drive
  Total Size: $size GB
  Free Space: $free GB
  Used Space: $used GB
  Percent Free: $percentFree%

"@ | Out-File -FilePath $reportFile -Append
}

# Network Information
Write-Host "Collecting network information..." -ForegroundColor Yellow
@"
=== Network Information ===
"@ | Out-File -FilePath $reportFile -Append
Get-NetIPAddress -AddressFamily IPv4 | Where-Object {$_.IPAddress -notlike "127.*"} | ForEach-Object {
    @"
Interface: $($_.InterfaceAlias)
  IP Address: $($_.IPAddress)
  Subnet Mask: $($_.PrefixLength)

"@ | Out-File -FilePath $reportFile -Append
}

# Running Processes (Top 10 by Memory)
Write-Host "Collecting process information..." -ForegroundColor Yellow
@"
=== Top 10 Processes by Memory Usage ===
"@ | Out-File -FilePath $reportFile -Append
Get-Process | Sort-Object WorkingSet64 -Descending | Select-Object -First 10 | ForEach-Object {
    $memoryMB = [math]::Round($_.WorkingSet64 / 1MB, 2)
    @"
$($_.ProcessName): $memoryMB MB

"@ | Out-File -FilePath $reportFile -Append
}

Write-Host "`nReport generated successfully!" -ForegroundColor Green
Write-Host "Report saved to: $reportFile" -ForegroundColor Cyan

# Display first few lines
Write-Host "`n=== Report Preview ===" -ForegroundColor Yellow
Get-Content $reportFile -TotalCount 20
```

3. **Save and run**
   - Save the file
   - Close Notepad
   - Type: `.\SystemLogger.ps1`
   - Press Enter
   - *View the generated report file*

**Key Learning Points:**
- Use here-strings `@"..."@` for multi-line text
- `Get-CimInstance` retrieves system information
- `[math]::Round()` rounds numbers
- `$env:` variables access environment variables
- Combine multiple cmdlets with pipelines

---

## ACTIVITY 6: CAPTURING AND EXPLAINING SYSTEM BEHAVIOUR

**What we're doing:** We'll create scripts that monitor and explain system behavior for assessment purposes.

**Goal:** Create documentation-ready scripts that capture system state and behavior.

**Time:** 30 minutes

---

## TASK 1: SYSTEM MONITORING SCRIPT

**What this does:** We're creating a script that monitors system resources and explains what's happening.

### CLI METHOD

1. **Create a monitoring script**
   - Type: `notepad SystemMonitor.ps1`
   - Press Enter

2. **Type the following script:**

```powershell
# SystemMonitor.ps1 - System monitoring and explanation

Write-Host "=== System Monitoring Script ===" -ForegroundColor Cyan
Write-Host "This script monitors system resources and explains their status.`n" -ForegroundColor White

# Function to explain CPU usage
function Get-CPUStatus {
    $cpu = Get-Counter '\Processor(_Total)\% Processor Time' -ErrorAction SilentlyContinue
    
    if ($cpu) {
        $cpuValue = [math]::Round($cpu.CounterSamples.CookedValue, 2)
        
        Write-Host "CPU Usage: $cpuValue%" -ForegroundColor $(if ($cpuValue -gt 80) { "Red" } elseif ($cpuValue -gt 50) { "Yellow" } else { "Green" })
        
        if ($cpuValue -gt 80) {
            Write-Host "  Explanation: CPU usage is HIGH. The system is working hard." -ForegroundColor Yellow
            Write-Host "  Recommendation: Check for resource-intensive processes." -ForegroundColor Yellow
        } elseif ($cpuValue -gt 50) {
            Write-Host "  Explanation: CPU usage is MODERATE. System is under normal load." -ForegroundColor White
        } else {
            Write-Host "  Explanation: CPU usage is LOW. System has plenty of capacity." -ForegroundColor Green
        }
    }
}

# Function to explain memory usage
function Get-MemoryStatus {
    $memory = Get-CimInstance Win32_OperatingSystem
    $totalMemory = [math]::Round($memory.TotalVisibleMemorySize / 1MB, 2)
    $freeMemory = [math]::Round($memory.FreePhysicalMemory / 1MB, 2)
    $usedMemory = $totalMemory - $freeMemory
    $percentUsed = [math]::Round(($usedMemory / $totalMemory) * 100, 2)
    
    Write-Host "`nMemory Status:" -ForegroundColor Cyan
    Write-Host "  Total: $totalMemory MB" -ForegroundColor White
    Write-Host "  Used: $usedMemory MB ($percentUsed%)" -ForegroundColor $(if ($percentUsed -gt 80) { "Red" } elseif ($percentUsed -gt 50) { "Yellow" } else { "Green" })
    Write-Host "  Free: $freeMemory MB" -ForegroundColor White
    
    if ($percentUsed -gt 80) {
        Write-Host "  Explanation: Memory usage is HIGH. System may slow down." -ForegroundColor Yellow
        Write-Host "  Recommendation: Close unnecessary applications or add more RAM." -ForegroundColor Yellow
    } else {
        Write-Host "  Explanation: Memory usage is within normal range." -ForegroundColor Green
    }
}

# Function to explain disk usage
function Get-DiskStatus {
    Write-Host "`nDisk Status:" -ForegroundColor Cyan
    
    Get-Volume | Where-Object {$_.DriveLetter} | ForEach-Object {
        $drive = $_.DriveLetter + ":"
        $size = [math]::Round($_.Size / 1GB, 2)
        $free = [math]::Round($_.SizeRemaining / 1GB, 2)
        $percentFree = [math]::Round(($_.SizeRemaining / $_.Size) * 100, 2)
        
        Write-Host "  Drive $drive :" -ForegroundColor White
        Write-Host "    Total: $size GB | Free: $free GB ($percentFree% free)" -ForegroundColor White
        
        if ($percentFree -lt 10) {
            Write-Host "    Explanation: Disk space is CRITICALLY LOW!" -ForegroundColor Red
            Write-Host "    Recommendation: Free up space immediately." -ForegroundColor Red
        } elseif ($percentFree -lt 20) {
            Write-Host "    Explanation: Disk space is LOW." -ForegroundColor Yellow
            Write-Host "    Recommendation: Consider freeing up space soon." -ForegroundColor Yellow
        } else {
            Write-Host "    Explanation: Disk space is adequate." -ForegroundColor Green
        }
    }
}

# Function to explain network connectivity
function Get-NetworkStatus {
    Write-Host "`nNetwork Status:" -ForegroundColor Cyan
    
    $adapters = Get-NetAdapter | Where-Object {$_.Status -eq "Up"}
    
    if ($adapters) {
        Write-Host "  Active Network Adapters:" -ForegroundColor Green
        $adapters | ForEach-Object {
            Write-Host "    $($_.Name): $($_.Status)" -ForegroundColor White
            $ip = Get-NetIPAddress -InterfaceIndex $_.InterfaceIndex -AddressFamily IPv4 -ErrorAction SilentlyContinue
            if ($ip) {
                Write-Host "      IP Address: $($ip.IPAddress)" -ForegroundColor Cyan
            }
        }
        Write-Host "  Explanation: Network adapters are active and connected." -ForegroundColor Green
    } else {
        Write-Host "  Explanation: No active network adapters found." -ForegroundColor Red
        Write-Host "  Recommendation: Check network cable and adapter settings." -ForegroundColor Yellow
    }
    
    # Test connectivity
    Write-Host "`n  Testing Internet Connectivity..." -ForegroundColor Yellow
    $test = Test-Connection -ComputerName "8.8.8.8" -Count 2 -Quiet
    if ($test) {
        Write-Host "  Internet: CONNECTED" -ForegroundColor Green
        Write-Host "  Explanation: System can reach external network (Google DNS)." -ForegroundColor Green
    } else {
        Write-Host "  Internet: NOT CONNECTED" -ForegroundColor Red
        Write-Host "  Explanation: System cannot reach external network." -ForegroundColor Yellow
    }
}

# Run all monitoring functions
Get-CPUStatus
Get-MemoryStatus
Get-DiskStatus
Get-NetworkStatus

Write-Host "`n=== Monitoring Complete ===" -ForegroundColor Cyan
```

3. **Save and run**
   - Save the file
   - Close Notepad
   - Type: `.\SystemMonitor.ps1`
   - Press Enter
   - *Review the explanations for each system component*

**Key Learning Points:**
- Functions organize code into reusable blocks
- Conditional formatting based on thresholds
- Explanatory text helps understand system state
- Color coding provides visual feedback
- Combine multiple monitoring checks

---

## TASK 2: PROCESS ANALYSIS SCRIPT

**What this does:** We'll create a script that analyzes running processes and explains their purpose.

### CLI METHOD

1. **Create a process analysis script**
   - Type: `notepad ProcessAnalysis.ps1`
   - Press Enter

2. **Type the following script:**

```powershell
# ProcessAnalysis.ps1 - Process analysis and explanation

Write-Host "=== Process Analysis Script ===" -ForegroundColor Cyan
Write-Host "This script analyzes running processes and explains their behavior.`n" -ForegroundColor White

# Get all processes
$processes = Get-Process | Sort-Object WorkingSet64 -Descending

Write-Host "Total Processes Running: $($processes.Count)" -ForegroundColor Green
Write-Host "`n=== Top 10 Processes by Memory Usage ===" -ForegroundColor Cyan

$processes | Select-Object -First 10 | ForEach-Object {
    $memoryMB = [math]::Round($_.WorkingSet64 / 1MB, 2)
    $cpuTime = $_.CPU
    
    Write-Host "`nProcess: $($_.ProcessName)" -ForegroundColor Yellow
    Write-Host "  PID: $($_.Id)" -ForegroundColor White
    Write-Host "  Memory: $memoryMB MB" -ForegroundColor $(if ($memoryMB -gt 500) { "Red" } elseif ($memoryMB -gt 100) { "Yellow" } else { "Green" })
    Write-Host "  CPU Time: $cpuTime seconds" -ForegroundColor White
    
    # Explain process
    $processName = $_.ProcessName.ToLower()
    
    switch ($processName) {
        {$_ -like "*chrome*"} {
            Write-Host "  Explanation: Google Chrome browser process." -ForegroundColor Cyan
            Write-Host "  Behavior: Normal for web browsing. Multiple processes are common." -ForegroundColor White
        }
        {$_ -like "*explorer*"} {
            Write-Host "  Explanation: Windows Explorer (file manager and desktop)." -ForegroundColor Cyan
            Write-Host "  Behavior: Essential Windows process. Should always be running." -ForegroundColor White
        }
        {$_ -like "*svchost*"} {
            Write-Host "  Explanation: Service Host process (runs Windows services)." -ForegroundColor Cyan
            Write-Host "  Behavior: Normal. Multiple svchost processes are expected." -ForegroundColor White
        }
        {$_ -like "*powershell*"} {
            Write-Host "  Explanation: PowerShell scripting host." -ForegroundColor Cyan
            Write-Host "  Behavior: This script itself! Normal for running PowerShell scripts." -ForegroundColor White
        }
        default {
            Write-Host "  Explanation: Application or system process." -ForegroundColor Cyan
            Write-Host "  Behavior: Check if process is expected for your system." -ForegroundColor White
        }
    }
    
    # Memory usage explanation
    if ($memoryMB -gt 500) {
        Write-Host "  Memory Note: HIGH memory usage. May indicate memory leak or intensive operation." -ForegroundColor Yellow
    } elseif ($memoryMB -gt 100) {
        Write-Host "  Memory Note: Moderate memory usage. Normal for active applications." -ForegroundColor Green
    } else {
        Write-Host "  Memory Note: Low memory usage. Efficient process." -ForegroundColor Green
    }
}

# System summary
Write-Host "`n=== System Summary ===" -ForegroundColor Cyan
$totalMemory = ($processes | Measure-Object WorkingSet64 -Sum).Sum
$totalMemoryGB = [math]::Round($totalMemory / 1GB, 2)

Write-Host "Total Memory Used by Processes: $totalMemoryGB GB" -ForegroundColor Green
Write-Host "Average Memory per Process: $([math]::Round(($totalMemory / $processes.Count) / 1MB, 2)) MB" -ForegroundColor Green

Write-Host "`n=== Analysis Complete ===" -ForegroundColor Cyan
```

3. **Save and run**
   - Save the file
   - Close Notepad
   - Type: `.\ProcessAnalysis.ps1`
   - Press Enter
   - *Review the process explanations*

**Key Learning Points:**
- `Get-Process` retrieves running processes
- `Sort-Object` orders results
- `Select-Object` limits output
- `Measure-Object` calculates statistics
- Switch statements handle multiple conditions
- Explanatory text documents behavior

---

## LAB WRAP-UP: Week 10 Summary

### What We Learned:

1. **PowerShell Basics**
   - Creating and running `.ps1` scripts
   - Understanding execution policies
   - Using cmdlets and parameters

2. **Variables and Data Types**
   - String, integer, boolean, array, and hash table variables
   - Variable operations and conversions
   - String manipulation

3. **Conditional Logic**
   - If/elseif/else statements
   - Comparison operators (`-eq`, `-lt`, `-gt`, etc.)
   - Logical operators (`-and`, `-or`, `-not`)

4. **Input Validation and Error Handling**
   - Validating user input
   - Using try-catch blocks
   - Error action preferences

5. **Logging and Output Redirection**
   - Saving output to files
   - Creating log files with timestamps
   - Using transcripts and custom logging functions

6. **System Monitoring**
   - Capturing system information
   - Explaining system behavior
   - Creating documentation-ready scripts

### Scripts Created:

1. `Greet.ps1` - Simple greeting script
2. `Variables.ps1` - Variable demonstration
3. `Operations.ps1` - Variable operations
4. `Conditionals.ps1` - Conditional logic examples
5. `FileChecker.ps1` - File existence checking
6. `Validation.ps1` - Input validation
7. `ErrorHandling.ps1` - Error handling examples
8. `Logging.ps1` - Output redirection and logging
9. `SystemLogger.ps1` - System information logger
10. `SystemMonitor.ps1` - System monitoring with explanations
11. `ProcessAnalysis.ps1` - Process analysis and explanation

### Key Takeaways:

✓ **PowerShell scripts use `.ps1` extension**
✓ **Variables start with `$` symbol**
✓ **Cmdlets follow verb-noun pattern (Get-Item, Set-Location)**
✓ **Use comparison operators (`-eq`, `-lt`) not `==`, `<`**
✓ **Objects have properties accessible with dot notation**
✓ **Pipelines (`|`) pass data between commands**
✓ **Error handling makes scripts robust**
✓ **Logging provides audit trails and debugging information**

### Next Steps:

- Practice creating your own PowerShell scripts
- Explore more advanced PowerShell features (functions, modules)
- Learn about PowerShell remoting and automation
- Study Windows-specific cmdlets and WMI/CIM
- Contribute to PowerShell script repositories

### For AE1/AE2:

These PowerShell scripting skills are essential for:
- Automating Windows administration tasks
- Creating system monitoring and reporting scripts
- Managing Windows services and processes
- Processing and analyzing Windows logs
- Building Windows deployment automation
- Developing Windows backup and recovery scripts

---

## Additional Resources

### Official Documentation:
- **PowerShell Documentation:** https://learn.microsoft.com/powershell/
- **Get-Help:** `Get-Help Get-Process` (in PowerShell)
- **PowerShell Gallery:** https://www.powershellgallery.com/

### Practice Resources:
- Create scripts to automate your daily Windows tasks
- Practice on PowerShell scripting challenges
- Study existing PowerShell scripts in Windows
- Explore PowerShell modules and cmdlets

### Useful Commands for Reference:
```powershell
# Script execution
.\script.ps1                    # Run script in current directory
& "C:\Path\To\script.ps1"       # Run script with full path

# Variables
$variable = "value"              # Create variable
$variable                        # Use variable
Remove-Variable variable         # Delete variable

# Common cmdlets
Get-Process                      # List processes
Get-Service                      # List services
Get-Item                         # Get file/folder info
Set-Location                     # Change directory (cd)
Get-Content                      # Read file content
Out-File                         # Write to file

# Help system
Get-Help Get-Process            # Get help for cmdlet
Get-Help Get-Process -Examples   # Show examples
Get-Command                      # List all commands
```

---

**End of Week 10 Lab Guide**

Remember: **Practice makes perfect!** Create PowerShell scripts regularly and experiment with different cmdlets and techniques.

