# Week 9 Lab Guide: Bash Scripting Fundamentals

Complete guide showing how to create, execute, and understand Bash scripts on Ubuntu Linux.

---

## Lab Overview

### What is Week 9 About:
**Introduction to Bash Scripting - Automating Tasks with Shell Scripts**

### PC Requirements:
- **Ubuntu 24.04 LTS** (Virtual Machine, lab PC, or LOD platform)
  - **Download Ubuntu 24.04.3 Desktop ISO:** [Direct Download Link](https://releases.ubuntu.com/24.04/ubuntu-24.04.3-desktop-amd64.iso)
  - **Alternative:** [Ubuntu Download Page](https://releases.ubuntu.com/24.04/)
  - **File:** `ubuntu-24.04.3-desktop-amd64.iso` (5.9 GB)
- Terminal/Shell access
- Text editor (nano, vim, or gedit)
- Basic familiarity with CLI commands from previous weeks

### What You'll Do:
- **Activity 1:** Create your first Bash script with user input
- **Activity 2:** Use variables and quoting correctly
- **Activity 3:** Add conditional logic (if/elif/else)
- **Activity 4:** Validate user input and handle errors
- **Activity 5:** Create logging scripts with redirection
- **Activity 6:** Capture and explain system behaviour for AE1

---

## Important Terms

| Term | Definition | Simple Explanation |
|------|------------|-------------------|
| Bash | Bourne Again Shell | The command-line interpreter for Linux |
| Script | Automated sequence of commands | File containing commands that run automatically |
| Shebang | `#!/bin/bash` | First line that tells system which interpreter to use |
| Variable | Storage for data | Named container that holds values |
| Quoting | Using quotes around variables | Protects data from being split or misinterpreted |
| Conditional | Decision-making logic | Code that runs only if conditions are met |
| Redirection | Sending output to files | Saving command output to files instead of screen |
| Read | Input command | Gets data from user during script execution |
| Validation | Checking input correctness | Verifying data before using it |

---

## Background Knowledge: Why Bash Scripting?

**What is Bash Scripting?**

Bash scripting allows you to automate repetitive tasks, combine multiple commands, and create powerful workflows without manual intervention.

**Why Learn Bash Scripting?**

1. **Automation:** Run multiple commands with one script
2. **Efficiency:** Save time on repetitive tasks
3. **System Administration:** Monitor systems, manage processes, automate backups
4. **DevOps:** Essential skill for CI/CD pipelines and infrastructure management
5. **Problem Solving:** Quick solutions for complex tasks

**Key Concepts:**

- **Scripts run line-by-line** - Each command executes in order
- **Variables store data** - Use `$variable` or `${variable}` to access values
- **Conditionals add logic** - Scripts can make decisions based on conditions
- **Redirection saves output** - Send results to files for logging or later use

**Key Files and Directories:**
- `~/scripts/` - Common location for personal scripts
- `/usr/local/bin/` - System-wide scripts location
- `~/.bashrc` - Bash configuration file
- Script files typically end with `.sh` extension

---

## SETUP: PREPARING FOR WEEK 9

**What we're doing:** We need to set up our Ubuntu environment for Bash scripting.

### Prerequisites:

1. **Start your Ubuntu system**
   - Open your Ubuntu VM, lab PC, or LOD platform
   - Log in with your credentials

2. **Open Terminal**
   - Press **Ctrl + Alt + T**
   - *Terminal window opens*

3. **Create a scripts directory**
   - Type: `mkdir -p ~/scripts`
   - Press Enter
   - Type: `cd ~/scripts`
   - Press Enter
   - *You're now in your scripts directory*

4. **Verify you have a text editor**
   - Type: `which nano`
   - Press Enter
   - *Should show: /usr/bin/nano*

**What you should see:**
```
$ mkdir -p ~/scripts
$ cd ~/scripts
$ pwd
/home/student/scripts
$ which nano
/usr/bin/nano
```

---

## ACTIVITY 1: CREATING YOUR FIRST BASH SCRIPT

**Do on:** Ubuntu Linux system

**What we're doing:** We're creating a simple Bash script that greets the user by name.

**Goal:** Understand script structure, user input, and script execution.

**Time:** 20 minutes

---

## TASK 1: CREATE A SIMPLE GREETING SCRIPT

**What this does:** We're creating a script called `greet.sh` that asks for the user's name and displays a greeting.

### CLI METHOD

1. **Navigate to scripts directory**
   - Type: `cd ~/scripts`
   - Press Enter

2. **Create the script file**
   - Type: `nano greet.sh`
   - Press Enter
   - *Opens nano text editor*

3. **Type the following script:**

```bash
#!/bin/bash
# greet.sh - A simple greeting script

echo "Welcome to Bash Scripting!"
read -p "What is your name? " username
echo "Hello, ${username}! Nice to meet you."
```

4. **Save and exit**
   - Press **Ctrl + X**
   - Press **Y** to confirm
   - Press **Enter** to save

5. **View your script**
   - Type: `cat greet.sh`
   - Press Enter
   - *Shows the script content*

**Script Explanation:**
- `#!/bin/bash` - Shebang: tells system to use Bash interpreter
- `# greet.sh` - Comment: describes the script (ignored during execution)
- `echo` - Displays text to the screen
- `read -p` - Prompts user for input and stores it in a variable
- `${username}` - Safe way to expand/use the variable value

---

## TASK 2: MAKE THE SCRIPT EXECUTABLE

**What this does:** We're giving the script permission to run as a program.

### CLI METHOD

1. **Check current permissions**
   - Type: `ls -l greet.sh`
   - Press Enter
   - *Shows file permissions*

**What you should see:**
```
$ ls -l greet.sh
-rw-rw-r-- 1 student student 145 Dec 10 10:00 greet.sh
```

2. **Add execute permission**
   - Type: `chmod +x greet.sh`
   - Press Enter
   - *Adds execute permission*

3. **Verify permissions changed**
   - Type: `ls -l greet.sh`
   - Press Enter
   - *Should now show execute permission (x)*

**What you should see:**
```
$ ls -l greet.sh
-rwxrwxr-x 1 student student 145 Dec 10 10:00 greet.sh
```

**Permission Explanation:**
- `-rw-rw-r--` - Original: read and write only
- `-rwxrwxr-x` - After chmod: read, write, and execute (x)

---

## TASK 3: RUN THE SCRIPT

**What this does:** We're executing the script to see it in action.

### CLI METHOD

1. **Run the script**
   - Type: `./greet.sh`
   - Press Enter
   - *Script starts executing*

2. **Respond to the prompt**
   - *Script asks: "What is your name?"*
   - Type your name (e.g., `John`)
   - Press Enter

3. **View the output**
   - *Script displays: "Hello, John! Nice to meet you."*

**What you should see:**
```
$ ./greet.sh
Welcome to Bash Scripting!
What is your name? John
Hello, John! Nice to meet you.
```

**Why `./`?**
- `.` means "current directory"
- `/` is the path separator
- `./greet.sh` means "run greet.sh from current directory"

---

## ACTIVITY 2: VARIABLES AND QUOTING

**Do on:** Ubuntu Linux system

**What we're doing:** We're learning to use variables correctly with proper quoting to avoid errors.

**Goal:** Understand variable expansion, quoting, and best practices.

**Time:** 25 minutes

---

## TASK 1: CREATE A SCRIPT WITH MULTIPLE VARIABLES

**What this does:** We're creating a script that uses multiple variables and demonstrates proper quoting.

### CLI METHOD

1. **Create new script**
   - Type: `nano userinfo.sh`
   - Press Enter

2. **Type the following script:**

```bash
#!/bin/bash
# userinfo.sh - Collect and display user information

echo "=== User Information Collection ==="
read -p "Enter your first name: " firstname
read -p "Enter your last name: " lastname
read -p "Enter your age: " age
read -p "Enter your city: " city

echo ""
echo "=== Summary ==="
echo "Full Name: ${firstname} ${lastname}"
echo "Age: ${age}"
echo "City: ${city}"
echo ""
echo "Thank you, ${firstname}!"
```

3. **Save and exit**
   - Press **Ctrl + X**, **Y**, **Enter**

4. **Make executable and run**
   - Type: `chmod +x userinfo.sh`
   - Press Enter
   - Type: `./userinfo.sh`
   - Press Enter
   - *Follow the prompts*

---

## TASK 2: UNDERSTAND QUOTING BEST PRACTICES

**What this does:** We're learning why `"${variable}"` is safer than `$variable`.

### CLI METHOD

1. **Create a quoting demonstration script**
   - Type: `nano quoting_demo.sh`
   - Press Enter

2. **Type the following script:**

```bash
#!/bin/bash
# quoting_demo.sh - Demonstrates importance of quoting

read -p "Enter a sentence with spaces: " usertext

echo "=== Without Quotes ==="
echo "Result: $usertext"

echo ""
echo "=== With Quotes (Safe) ==="
echo "Result: \"${usertext}\""

echo ""
echo "Recommendation: Always use \"\${variable}\" for safety"
```

3. **Save, make executable, and run**
   - Press **Ctrl + X**, **Y**, **Enter**
   - Type: `chmod +x quoting_demo.sh`
   - Press Enter
   - Type: `./quoting_demo.sh`
   - Press Enter
   - *Try entering: "Hello World"*

**Why Quoting Matters:**
- Without quotes: `$var` can break if value contains spaces or special characters
- With quotes: `"${var}"` keeps the value intact as a single unit
- Best practice: Always use `"${variable}"` in scripts

---

## ACTIVITY 3: CONDITIONAL LOGIC (IF/ELIF/ELSE)

**Do on:** Ubuntu Linux system

**What we're doing:** We're adding decision-making logic to scripts using conditional statements.

**Goal:** Create scripts that respond differently based on conditions.

**Time:** 30 minutes

---

## TASK 1: CREATE A SIMPLE IF STATEMENT

**What this does:** We're creating a script that checks if a user is an adult.

### CLI METHOD

1. **Create age verification script**
   - Type: `nano age_check.sh`
   - Press Enter

2. **Type the following script:**

```bash
#!/bin/bash
# age_check.sh - Check if user is an adult

read -p "Enter your age: " age

if [ "${age}" -ge 18 ]; then
    echo "You are an adult."
else
    echo "You are a minor."
fi
```

3. **Save, make executable, and run**
   - Press **Ctrl + X**, **Y**, **Enter**
   - Type: `chmod +x age_check.sh && ./age_check.sh`
   - Press Enter
   - *Try different ages*

**Conditional Operators:**
- `-ge` - Greater than or equal to
- `-gt` - Greater than
- `-lt` - Less than
- `-le` - Less than or equal to
- `-eq` - Equal to
- `-ne` - Not equal to

---

## TASK 2: CREATE AN IF/ELIF/ELSE SCRIPT

**What this does:** We're creating a script with multiple conditions for different age groups.

### CLI METHOD

1. **Create age category script**
   - Type: `nano age_category.sh`
   - Press Enter

2. **Type the following script:**

```bash
#!/bin/bash
# age_category.sh - Categorize age groups

read -p "Enter your age: " age

if [ "${age}" -lt 13 ]; then
    echo "You are a child."
elif [ "${age}" -lt 18 ]; then
    echo "You are a teenager."
elif [ "${age}" -lt 65 ]; then
    echo "You are an adult."
else
    echo "You are a senior."
fi

echo "Thank you for using the age categorizer!"
```

3. **Save, make executable, and run**
   - Press **Ctrl + X**, **Y**, **Enter**
   - Type: `chmod +x age_category.sh && ./age_category.sh`
   - Press Enter

**What you should see:**
```
$ ./age_category.sh
Enter your age: 25
You are an adult.
Thank you for using the age categorizer!
```

---

## TASK 3: CREATE A NUMBER COMPARISON SCRIPT

**What this does:** We're creating a script that compares two numbers.

### CLI METHOD

1. **Create comparison script**
   - Type: `nano compare.sh`
   - Press Enter

2. **Type the following script:**

```bash
#!/bin/bash
# compare.sh - Compare two numbers

read -p "Enter first number: " num1
read -p "Enter second number: " num2

echo ""
echo "=== Comparison Results ==="

if (( num1 > num2 )); then
    echo "${num1} is greater than ${num2}"
elif (( num1 < num2 )); then
    echo "${num1} is less than ${num2}"
else
    echo "${num1} is equal to ${num2}"
fi
```

3. **Save, make executable, and run**
   - Press **Ctrl + X**, **Y**, **Enter**
   - Type: `chmod +x compare.sh && ./compare.sh`
   - Press Enter

**Arithmetic Comparison Methods:**
- `[ ]` with `-lt`, `-gt`, `-eq` (traditional, safer for older systems)
- `(( ))` with `<`, `>`, `==` (modern, more intuitive for numbers)

---

## ACTIVITY 4: INPUT VALIDATION

**Do on:** Ubuntu Linux system

**What we're doing:** We're learning to validate user input to prevent errors and security issues.

**Goal:** Create robust scripts that handle invalid input gracefully.

**Time:** 25 minutes

---

## TASK 1: VALIDATE NUMERIC INPUT

**What this does:** We're creating a script that ensures the user enters a valid number.

### CLI METHOD

1. **Create validation script**
   - Type: `nano validate_age.sh`
   - Press Enter

2. **Type the following script:**

```bash
#!/bin/bash
# validate_age.sh - Validate numeric input

read -p "Enter your age: " age

# Check if input is empty
if [ -z "${age}" ]; then
    echo "Error: You must enter a value!"
    exit 1
fi

# Check if input is a number
if ! [[ "${age}" =~ ^[0-9]+$ ]]; then
    echo "Error: Age must be a number!"
    exit 1
fi

# Check if age is realistic
if [ "${age}" -lt 0 ] || [ "${age}" -gt 120 ]; then
    echo "Error: Age must be between 0 and 120!"
    exit 1
fi

echo "Valid age entered: ${age}"
```

3. **Save, make executable, and test**
   - Press **Ctrl + X**, **Y**, **Enter**
   - Type: `chmod +x validate_age.sh`
   - Press Enter
   - Type: `./validate_age.sh`
   - Press Enter
   - *Try these test cases:*
     - Press Enter without typing anything (empty)
     - Type: `abc` (non-numeric)
     - Type: `-5` (negative)
     - Type: `25` (valid)

**Validation Techniques:**
- `-z "${var}"` - Checks if variable is empty
- `[[ "${var}" =~ ^[0-9]+$ ]]` - Checks if variable contains only digits
- `exit 1` - Exits script with error code 1 (non-zero = error)

---

## TASK 2: VALIDATE YES/NO INPUT

**What this does:** We're creating a script that validates yes/no responses.

### CLI METHOD

1. **Create yes/no validation script**
   - Type: `nano confirm.sh`
   - Press Enter

2. **Type the following script:**

```bash
#!/bin/bash
# confirm.sh - Validate yes/no input

read -p "Do you want to continue? (yes/no): " response

# Convert to lowercase for easier comparison
response=$(echo "${response}" | tr '[:upper:]' '[:lower:]')

if [ "${response}" = "yes" ] || [ "${response}" = "y" ]; then
    echo "Continuing..."
elif [ "${response}" = "no" ] || [ "${response}" = "n" ]; then
    echo "Exiting..."
    exit 0
else
    echo "Invalid response. Please enter yes or no."
    exit 1
fi
```

3. **Save, make executable, and test**
   - Press **Ctrl + X**, **Y**, **Enter**
   - Type: `chmod +x confirm.sh && ./confirm.sh`
   - Press Enter

---

## ACTIVITY 5: LOGGING AND REDIRECTION

**Do on:** Ubuntu Linux system

**What we're doing:** We're learning to save script output to log files for monitoring and troubleshooting.

**Goal:** Create scripts that log their activity automatically.

**Time:** 30 minutes

---

## TASK 1: UNDERSTAND REDIRECTION OPERATORS

**What this does:** We're learning different ways to redirect output.

### CLI METHOD

1. **Create redirection demonstration script**
   - Type: `nano redirect_demo.sh`
   - Press Enter

2. **Type the following script:**

```bash
#!/bin/bash
# redirect_demo.sh - Demonstrate redirection operators

echo "=== Redirection Examples ==="

# Example 1: Overwrite file (>)
echo "This overwrites the file" > test_output.txt
echo "Created test_output.txt with >"

# Example 2: Append to file (>>)
echo "This appends to the file" >> test_output.txt
echo "Appended to test_output.txt with >>"

# Example 3: Save command output
date >> test_output.txt
echo "Appended date to test_output.txt"

# Example 4: Show file contents
echo ""
echo "=== File Contents ==="
cat test_output.txt
```

3. **Save, make executable, and run**
   - Press **Ctrl + X**, **Y**, **Enter**
   - Type: `chmod +x redirect_demo.sh && ./redirect_demo.sh`
   - Press Enter

**Redirection Operators:**
- `>` - Redirect output to file (overwrites)
- `>>` - Append output to file (adds to end)
- `2>` - Redirect errors to file
- `&>` - Redirect both output and errors
- `2>&1` - Redirect errors to same place as output

---

## TASK 2: CREATE A DISK USAGE LOGGING SCRIPT

**What this does:** We're creating a script that logs disk usage with timestamps.

### CLI METHOD

1. **Create disk usage logger**
   - Type: `nano disk_logger.sh`
   - Press Enter

2. **Type the following script:**

```bash
#!/bin/bash
# disk_logger.sh - Log disk usage with timestamp

# Set log file location
LOGFILE="${HOME}/disk_usage.log"

# Create log entry with timestamp
echo "=== Disk Usage Report ===" >> "${LOGFILE}"
echo "Timestamp: $(date '+%Y-%m-%d %H:%M:%S')" >> "${LOGFILE}"
echo "" >> "${LOGFILE}"

# Log disk usage
df -h >> "${LOGFILE}"

echo "" >> "${LOGFILE}"
echo "---" >> "${LOGFILE}"

# Inform user
echo "Disk usage logged to: ${LOGFILE}"
echo "To view log: cat ${LOGFILE}"
```

3. **Save, make executable, and run multiple times**
   - Press **Ctrl + X**, **Y**, **Enter**
   - Type: `chmod +x disk_logger.sh`
   - Press Enter
   - Type: `./disk_logger.sh`
   - Press Enter
   - *Wait a few seconds*
   - Type: `./disk_logger.sh`
   - Press Enter
   - *Run again*
   - Type: `./disk_logger.sh`
   - Press Enter

4. **View the log file**
   - Type: `cat ~/disk_usage.log`
   - Press Enter
   - *Shows multiple timestamped entries*

**What you should see:**
```
=== Disk Usage Report ===
Timestamp: 2025-12-10 14:30:15

Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   25G   23G  53% /
---
=== Disk Usage Report ===
Timestamp: 2025-12-10 14:30:22
...
```

---

## TASK 3: CREATE A PROCESS LOGGING SCRIPT

**What this does:** We're creating a script that logs running processes periodically.

### CLI METHOD

1. **Create process logger**
   - Type: `nano process_logger.sh`
   - Press Enter

2. **Type the following script:**

```bash
#!/bin/bash
# process_logger.sh - Log top processes

LOGFILE="${HOME}/process_monitor.log"

echo "=== Process Monitor ===" >> "${LOGFILE}"
echo "Timestamp: $(date '+%Y-%m-%d %H:%M:%S')" >> "${LOGFILE}"
echo "" >> "${LOGFILE}"

# Log top 5 CPU-consuming processes
echo "Top 5 CPU Processes:" >> "${LOGFILE}"
ps aux --sort=-%cpu | head -6 >> "${LOGFILE}"

echo "" >> "${LOGFILE}"

# Log top 5 memory-consuming processes
echo "Top 5 Memory Processes:" >> "${LOGFILE}"
ps aux --sort=-%mem | head -6 >> "${LOGFILE}"

echo "" >> "${LOGFILE}"
echo "---" >> "${LOGFILE}"

echo "Process log saved to: ${LOGFILE}"
```

3. **Save, make executable, and run**
   - Press **Ctrl + X**, **Y**, **Enter**
   - Type: `chmod +x process_logger.sh && ./process_logger.sh`
   - Press Enter

4. **View the log**
   - Type: `cat ~/process_monitor.log | tail -20`
   - Press Enter

---

## ACTIVITY 6: CAPTURE SYSTEM BEHAVIOUR FOR AE1

**Do on:** Ubuntu Linux system

**What we're doing:** We're creating documentation-ready evidence of scripting skills for the AE1 assessment.

**Goal:** Produce annotated screenshots and explanations suitable for Personal Learning Record.

**Time:** 25 minutes

---

## TASK 1: CAPTURE AND ANNOTATE SCRIPT CREATION

**What this does:** We're documenting the process of creating and running a script.

### Steps:

1. **Create a demonstration script**
   - Type: `nano demo_for_ae1.sh`
   - Press Enter
   - Type:

```bash
#!/bin/bash
# demo_for_ae1.sh - Script for AE1 documentation

echo "System Information Report"
echo "========================="
echo ""
echo "Username: $(whoami)"
echo "Hostname: $(hostname)"
echo "Current Date: $(date)"
echo "Current Directory: $(pwd)"
echo ""
echo "Disk Usage:"
df -h / | grep -v "Filesystem"
```

2. **Save and make executable**
   - Press **Ctrl + X**, **Y**, **Enter**
   - Type: `chmod +x demo_for_ae1.sh`

3. **Take Screenshot 1: Script content**
   - Type: `cat demo_for_ae1.sh`
   - Press Enter
   - *Take screenshot*
   - **Caption:** "Figure 9.1 - Contents of demo_for_ae1.sh showing shebang, comments, and system information commands"

4. **Take Screenshot 2: Script execution**
   - Type: `./demo_for_ae1.sh`
   - Press Enter
   - *Take screenshot*
   - **Caption:** "Figure 9.2 - Output of demo_for_ae1.sh displaying system information including username, hostname, date, and disk usage"

5. **Write explanation (100-120 words):**

**Model Answer:**

"The script uses several important Bash concepts. The shebang `#!/bin/bash` tells the system to use the Bash interpreter. Command substitution `$(command)` captures output from commands like `whoami`, `hostname`, and `date`, storing them inline. The script demonstrates how Bash can automate information gathering tasks that would normally require multiple manual commands. Using `df -h` with `grep -v` filters out the header line to show only the data. This type of script is useful for system administrators who need to quickly gather system information for troubleshooting or documentation purposes. The script shows how Bash enables task automation and efficient system management."

---

## TASK 2: DOCUMENT ERROR HANDLING

**What this does:** We're documenting how scripts handle errors.

### Steps:

1. **Create script with error handling**
   - Type: `nano error_demo.sh`
   - Press Enter
   - Type:

```bash
#!/bin/bash
# error_demo.sh - Demonstrate error handling

read -p "Enter a file name to check: " filename

if [ -z "${filename}" ]; then
    echo "ERROR: No filename provided!"
    exit 1
fi

if [ -f "${filename}" ]; then
    echo "SUCCESS: File '${filename}' exists"
    echo "File size: $(du -h "${filename}" | cut -f1)"
else
    echo "ERROR: File '${filename}' not found"
    exit 1
fi
```

2. **Save and test both scenarios**
   - Press **Ctrl + X**, **Y**, **Enter**
   - Type: `chmod +x error_demo.sh`

3. **Test with existing file**
   - Type: `./error_demo.sh`
   - Press Enter
   - *When prompted, type: `greet.sh`*
   - *Take screenshot*
   - **Caption:** "Figure 9.3 - Successful file check showing error handling when file exists"

4. **Test with non-existing file**
   - Type: `./error_demo.sh`
   - Press Enter
   - *When prompted, type: `nonexistent.txt`*
   - *Take screenshot*
   - **Caption:** "Figure 9.4 - Error handling when file does not exist, showing exit code 1"

---

## TASK 3: WRITE REFLECTION FOR AE1

**What this does:** We're creating a reflection piece about Bash scripting learning.

### Model Reflection (100-120 words):

"During this week's practical, I learned fundamental Bash scripting concepts that enable automation of Linux system tasks. I created scripts using variables, user input with `read -p`, and conditional logic with `if/elif/else` statements. Understanding the importance of quoting variables with `"${var}"` prevents errors when handling user input with spaces. I implemented input validation to ensure scripts handle invalid data gracefully, and used redirection operators `>` and `>>` to create log files with timestamps using `$(date)`. The disk usage logger demonstrated how administrators automate system monitoring tasks. These scripting skills form the foundation for DevOps practices and system administration, enabling efficient task automation and reducing manual workload."

---

## QUICK REFERENCE: BASH SCRIPTING COMMANDS

| Task | Command | Example |
|------|---------|---------|
| Create script | `nano script.sh` | `nano greet.sh` |
| Make executable | `chmod +x script.sh` | `chmod +x greet.sh` |
| Run script | `./script.sh` | `./greet.sh` |
| View script | `cat script.sh` | `cat greet.sh` |
| Check permissions | `ls -l script.sh` | `ls -l greet.sh` |
| Get user input | `read -p "Prompt" var` | `read -p "Name: " name` |
| Display output | `echo "text"` | `echo "Hello"` |
| Use variable | `${variable}` | `echo "${name}"` |
| Redirect output | `command > file` | `echo "text" > log.txt` |
| Append output | `command >> file` | `date >> log.txt` |
| Check if number | `[[ "${var}" =~ ^[0-9]+$ ]]` | In validation scripts |
| Exit with error | `exit 1` | `exit 1` |
| Get current date | `$(date)` | `echo "$(date)"` |

---

## Troubleshooting Common Issues

### Issue: "Permission denied" when running script

**Solutions:**
1. Check permissions: `ls -l script.sh`
2. Make executable: `chmod +x script.sh`
3. Run with `./script.sh` not just `script.sh`

### Issue: "Command not found" when running script

**Solutions:**
1. Use `./script.sh` instead of `script.sh`
2. Or add script directory to PATH
3. Or run with full path: `bash ~/scripts/script.sh`

### Issue: Variables not expanding correctly

**Solutions:**
1. Use `"${variable}"` instead of `$variable`
2. Check for typos in variable names
3. Ensure no spaces around `=` in assignments: `var="value"` not `var = "value"`

### Issue: Conditional not working

**Solutions:**
1. For numbers, use `[ "${num}" -lt 10 ]` or `(( num < 10 ))`
2. For strings, use `[ "${str}" = "text" ]`
3. Always quote variables in conditionals
4. Remember `fi` to close `if` statements

### Issue: Script stops after first error

**Solutions:**
1. This is normal behavior - scripts exit on errors
2. Add error handling with `if` statements
3. Use `|| echo "Error occurred"` to continue on error

---

## Tips for Writing Good Scripts

**Best Practices:**

1. **Always start with shebang**
   ```bash
   #!/bin/bash
   ```

2. **Add comments**
   ```bash
   # This explains what the script does
   ```

3. **Use meaningful variable names**
   ```bash
   username="John"  # Good
   x="John"         # Bad
   ```

4. **Quote variables**
   ```bash
   echo "${username}"  # Good
   echo $username      # Risky
   ```

5. **Validate input**
   ```bash
   if [ -z "${input}" ]; then
       echo "Error: Input required"
       exit 1
   fi
   ```

6. **Add error handling**
   ```bash
   if [ ! -f "${file}" ]; then
       echo "Error: File not found"
       exit 1
   fi
   ```

7. **Use descriptive output**
   ```bash
   echo "Processing file: ${filename}"
   ```

---

## Important Reminders

1. **Shebang `#!/bin/bash`** must be the first line
2. **Make scripts executable** with `chmod +x`
3. **Run with `./`** when script is in current directory
4. **Quote variables** with `"${variable}"` for safety
5. **Validate input** before using it
6. **Use meaningful names** for variables and scripts
7. **Add comments** to explain complex logic
8. **Test scripts** with different inputs
9. **Use `exit 1`** to signal errors
10. **Document your work** for AE1 with screenshots and explanations

---

**Practice these concepts to become a Bash scripting pro!**

**Remember:** Bash scripting is essential for system administration, automation, and DevOps roles!

