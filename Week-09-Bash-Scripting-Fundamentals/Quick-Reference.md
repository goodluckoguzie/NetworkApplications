# Week 9 Quick Reference - Bash Scripting Fundamentals

Quick command reference for Bash scripting, variables, conditionals, input validation, and redirection.

---

## Script Basics

| Task | Command | Example |
|------|---------|---------|
| Create script | `nano script.sh` | `nano greet.sh` |
| View script | `cat script.sh` | `cat greet.sh` |
| Make executable | `chmod +x script.sh` | `chmod +x greet.sh` |
| Run script | `./script.sh` | `./greet.sh` |
| Run with bash | `bash script.sh` | `bash greet.sh` |
| Check permissions | `ls -l script.sh` | `ls -l greet.sh` |
| Edit script | `nano script.sh` | `nano greet.sh` |

---

## Essential Script Components

### Shebang (Must be first line)

```bash
#!/bin/bash
```

### Comments

```bash
# This is a comment
# Comments are ignored during execution
```

### Basic Script Structure

```bash
#!/bin/bash
# script_name.sh - Description

# Your commands here
echo "Hello, World!"
```

---

## User Input Commands

| Task | Command | Example |
|------|---------|---------|
| Get input | `read variable` | `read username` |
| Prompt for input | `read -p "Prompt" var` | `read -p "Name: " name` |
| Silent input (password) | `read -sp "Password: " pass` | `read -sp "Password: " pass` |
| Read with timeout | `read -t 5 var` | `read -t 5 response` |
| Display text | `echo "text"` | `echo "Hello"` |
| Display variable | `echo "${variable}"` | `echo "${name}"` |
| Display without newline | `echo -n "text"` | `echo -n "Enter: "` |

---

## Variables

### Declaring Variables

```bash
# Variable assignment (no spaces around =)
name="John"
age=25
city="London"
```

### Using Variables

```bash
# Safe way (always use this)
echo "${name}"
echo "Hello, ${name}!"

# Unsafe way (can break with spaces)
echo $name
```

### Special Variables

| Variable | Meaning | Example |
|----------|---------|---------|
| `$0` | Script name | `echo "$0"` |
| `$1, $2, $3` | Script arguments | `./script.sh arg1 arg2` |
| `$#` | Number of arguments | `echo "$#"` |
| `$@` | All arguments | `echo "$@"` |
| `$?` | Exit status of last command | `echo "$?"` |
| `$$` | Current process ID | `echo "$$"` |
| `$USER` | Current username | `echo "$USER"` |
| `$HOME` | Home directory | `echo "$HOME"` |
| `$PWD` | Current directory | `echo "$PWD"` |

---

## Conditional Statements

### Basic IF Statement

```bash
if [ condition ]; then
    # commands
fi
```

### IF-ELSE Statement

```bash
if [ condition ]; then
    # commands if true
else
    # commands if false
fi
```

### IF-ELIF-ELSE Statement

```bash
if [ condition1 ]; then
    # commands if condition1 is true
elif [ condition2 ]; then
    # commands if condition2 is true
else
    # commands if all conditions are false
fi
```

---

## Comparison Operators

### Numeric Comparisons (using [ ])

| Operator | Meaning | Example |
|----------|---------|---------|
| `-eq` | Equal to | `[ "${age}" -eq 18 ]` |
| `-ne` | Not equal to | `[ "${age}" -ne 18 ]` |
| `-lt` | Less than | `[ "${age}" -lt 18 ]` |
| `-le` | Less than or equal | `[ "${age}" -le 18 ]` |
| `-gt` | Greater than | `[ "${age}" -gt 18 ]` |
| `-ge` | Greater than or equal | `[ "${age}" -ge 18 ]` |

### Numeric Comparisons (using (( )))

```bash
if (( age < 18 )); then
    echo "Minor"
fi

if (( age >= 18 )); then
    echo "Adult"
fi
```

### String Comparisons

| Operator | Meaning | Example |
|----------|---------|---------|
| `=` | Equal to | `[ "${name}" = "John" ]` |
| `!=` | Not equal to | `[ "${name}" != "John" ]` |
| `-z` | String is empty | `[ -z "${name}" ]` |
| `-n` | String is not empty | `[ -n "${name}" ]` |

### File Checks

| Operator | Meaning | Example |
|----------|---------|---------|
| `-e` | File exists | `[ -e "file.txt" ]` |
| `-f` | Is a regular file | `[ -f "file.txt" ]` |
| `-d` | Is a directory | `[ -d "folder" ]` |
| `-r` | File is readable | `[ -r "file.txt" ]` |
| `-w` | File is writable | `[ -w "file.txt" ]` |
| `-x` | File is executable | `[ -x "script.sh" ]` |

---

## Logical Operators

| Operator | Meaning | Example |
|----------|---------|---------|
| `&&` | AND | `[ -f file ] && [ -r file ]` |
| `\|\|` | OR | `[ "${x}" = "a" ] \|\| [ "${x}" = "b" ]` |
| `!` | NOT | `[ ! -f file ]` |

---

## Input Validation

### Check if Variable is Empty

```bash
if [ -z "${variable}" ]; then
    echo "Error: Variable is empty"
    exit 1
fi
```

### Check if Input is a Number

```bash
if ! [[ "${age}" =~ ^[0-9]+$ ]]; then
    echo "Error: Not a number"
    exit 1
fi
```

### Check if Number is in Range

```bash
if [ "${age}" -lt 0 ] || [ "${age}" -gt 120 ]; then
    echo "Error: Age out of range"
    exit 1
fi
```

### Validate Yes/No Response

```bash
response=$(echo "${response}" | tr '[:upper:]' '[:lower:]')

if [ "${response}" = "yes" ] || [ "${response}" = "y" ]; then
    echo "Confirmed"
elif [ "${response}" = "no" ] || [ "${response}" = "n" ]; then
    echo "Cancelled"
else
    echo "Invalid response"
    exit 1
fi
```

---

## Redirection and Output

### Redirection Operators

| Operator | Action | Example |
|----------|--------|---------|
| `>` | Redirect output (overwrite) | `echo "text" > file.txt` |
| `>>` | Append output | `echo "text" >> file.txt` |
| `2>` | Redirect errors | `command 2> error.log` |
| `&>` | Redirect output and errors | `command &> all.log` |
| `2>&1` | Redirect errors to output | `command > file.txt 2>&1` |
| `<` | Redirect input from file | `command < input.txt` |
| `\|` | Pipe output to another command | `cat file \| grep "text"` |

### Logging Examples

```bash
# Simple log with timestamp
echo "$(date): Script started" >> script.log

# Log disk usage
df -h >> disk.log

# Log with multiple lines
{
    echo "=== Report ==="
    echo "Date: $(date)"
    df -h
} >> report.log
```

---

## Command Substitution

### Capture Command Output

```bash
# Modern syntax (preferred)
current_date=$(date)
username=$(whoami)
directory=$(pwd)

# Old syntax (still works)
current_date=`date`
```

### Use in Echo

```bash
echo "Current date is: $(date)"
echo "You are: $(whoami)"
echo "Current directory: $(pwd)"
```

---

## Useful Commands for Scripts

### System Information

| Task | Command | Example in Script |
|------|---------|-------------------|
| Current user | `whoami` | `echo "User: $(whoami)"` |
| Hostname | `hostname` | `echo "Host: $(hostname)"` |
| Current date | `date` | `echo "Date: $(date)"` |
| Date formatted | `date '+%Y-%m-%d %H:%M:%S'` | `echo "$(date '+%Y-%m-%d %H:%M:%S')"` |
| Current directory | `pwd` | `echo "Directory: $(pwd)"` |
| Disk usage | `df -h` | `df -h >> log.txt` |
| Memory usage | `free -h` | `free -h >> log.txt` |
| CPU info | `lscpu` | `lscpu >> system.log` |
| Process list | `ps aux` | `ps aux >> process.log` |

### Text Processing

| Task | Command | Example |
|------|---------|---------|
| Display file | `cat file.txt` | `cat greet.sh` |
| First 10 lines | `head file.txt` | `head -10 log.txt` |
| Last 10 lines | `tail file.txt` | `tail -20 log.txt` |
| Follow file (live) | `tail -f file.txt` | `tail -f script.log` |
| Search in file | `grep "text" file` | `grep "Error" log.txt` |
| Count lines | `wc -l file.txt` | `wc -l script.sh` |
| Convert to lowercase | `echo "TEXT" \| tr '[:upper:]' '[:lower:]'` | In scripts |
| Convert to uppercase | `echo "text" \| tr '[:lower:]' '[:upper:]'` | In scripts |

---

## Complete Script Examples

### Example 1: Simple Greeting Script

```bash
#!/bin/bash
# greet.sh - Simple greeting script

read -p "What is your name? " username
echo "Hello, ${username}! Welcome to Bash scripting."
```

### Example 2: Age Verification Script

```bash
#!/bin/bash
# age_check.sh - Check if user is adult

read -p "Enter your age: " age

if [ -z "${age}" ]; then
    echo "Error: Age required"
    exit 1
fi

if ! [[ "${age}" =~ ^[0-9]+$ ]]; then
    echo "Error: Age must be a number"
    exit 1
fi

if [ "${age}" -ge 18 ]; then
    echo "You are an adult"
else
    echo "You are a minor"
fi
```

### Example 3: File Checker Script

```bash
#!/bin/bash
# filecheck.sh - Check if file exists

read -p "Enter filename: " filename

if [ -z "${filename}" ]; then
    echo "Error: Filename required"
    exit 1
fi

if [ -f "${filename}" ]; then
    echo "File exists"
    echo "Size: $(du -h "${filename}" | cut -f1)"
else
    echo "File does not exist"
    exit 1
fi
```

### Example 4: Disk Usage Logger

```bash
#!/bin/bash
# disk_logger.sh - Log disk usage

LOGFILE="${HOME}/disk_usage.log"

echo "=== Disk Usage Report ===" >> "${LOGFILE}"
echo "Timestamp: $(date '+%Y-%m-%d %H:%M:%S')" >> "${LOGFILE}"
df -h >> "${LOGFILE}"
echo "---" >> "${LOGFILE}"

echo "Log saved to: ${LOGFILE}"
```

### Example 5: System Information Script

```bash
#!/bin/bash
# sysinfo.sh - Display system information

echo "=== System Information ==="
echo "User: $(whoami)"
echo "Hostname: $(hostname)"
echo "Date: $(date)"
echo "Uptime: $(uptime -p)"
echo "Disk Usage:"
df -h / | grep -v "Filesystem"
echo ""
echo "Memory Usage:"
free -h | grep "Mem:"
```

### Example 6: Menu Script

```bash
#!/bin/bash
# menu.sh - Simple menu system

echo "=== Main Menu ==="
echo "1. Show date"
echo "2. Show current user"
echo "3. Show disk usage"
echo "4. Exit"
echo ""
read -p "Enter choice [1-4]: " choice

if [ "${choice}" = "1" ]; then
    echo "Current date: $(date)"
elif [ "${choice}" = "2" ]; then
    echo "Current user: $(whoami)"
elif [ "${choice}" = "3" ]; then
    echo "Disk usage:"
    df -h
elif [ "${choice}" = "4" ]; then
    echo "Goodbye!"
    exit 0
else
    echo "Invalid choice"
    exit 1
fi
```

---

## Common Task Patterns

### Pattern 1: Create and Run Script

```bash
# Create script
nano myscript.sh

# Make executable
chmod +x myscript.sh

# Run script
./myscript.sh
```

### Pattern 2: Get Input and Validate

```bash
#!/bin/bash

# Get input
read -p "Enter value: " value

# Validate not empty
if [ -z "${value}" ]; then
    echo "Error: Value required"
    exit 1
fi

# Use the value
echo "You entered: ${value}"
```

### Pattern 3: Log with Timestamp

```bash
#!/bin/bash

LOGFILE="script.log"

# Log entry
echo "[$(date '+%Y-%m-%d %H:%M:%S')] Script started" >> "${LOGFILE}"

# Your commands here

echo "[$(date '+%Y-%m-%d %H:%M:%S')] Script finished" >> "${LOGFILE}"
```

### Pattern 4: Check File Exists

```bash
#!/bin/bash

filename="data.txt"

if [ -f "${filename}" ]; then
    echo "Processing ${filename}..."
    cat "${filename}"
else
    echo "Error: ${filename} not found"
    exit 1
fi
```

---

## Debugging Scripts

### Enable Debug Mode

```bash
# Run script in debug mode
bash -x script.sh

# Or add to script
#!/bin/bash -x

# Enable debugging in script
set -x

# Disable debugging
set +x
```

### Check Exit Codes

```bash
# Run command
./script.sh

# Check exit code
echo $?

# 0 = success
# Non-zero = error
```

---

## Best Practices Checklist

- [ ] Start script with `#!/bin/bash`
- [ ] Add descriptive comments
- [ ] Use meaningful variable names
- [ ] Quote variables: `"${variable}"`
- [ ] Validate user input
- [ ] Handle errors with `exit 1`
- [ ] Make script executable: `chmod +x`
- [ ] Test with different inputs
- [ ] Add helpful echo messages
- [ ] Use consistent indentation

---

## Common Mistakes to Avoid

### ❌ Don't Do This

```bash
# No spaces around = in assignment
var = "value"  # WRONG

# Unquoted variables
echo $username  # RISKY

# No shebang
echo "Hello"  # INCOMPLETE

# Wrong permissions
./script.sh  # Permission denied if not executable
```

### ✅ Do This Instead

```bash
# No spaces around = in assignment
var="value"  # CORRECT

# Always quote variables
echo "${username}"  # SAFE

# Always include shebang
#!/bin/bash
echo "Hello"  # COMPLETE

# Make executable first
chmod +x script.sh
./script.sh  # WORKS
```

---

## Quick Troubleshooting

| Problem | Solution |
|---------|----------|
| Permission denied | `chmod +x script.sh` |
| Command not found | Use `./script.sh` not `script.sh` |
| Variable not expanding | Check spelling, use `"${var}"` |
| Syntax error near `fi` | Missing `then` after `if` condition |
| Bad substitution | Use `"${var}"` not `${var }` (no space) |
| Script stops unexpectedly | Add `echo` statements to debug |
| Conditional not working | Quote variables in conditions |
| Spaces in variable breaking code | Always quote: `"${variable}"` |

---

## Testing Scripts

### Test Cases for Age Validation Script

```bash
# Test 1: Valid age
./age_check.sh
# Input: 25
# Expected: "You are an adult"

# Test 2: Minor age
./age_check.sh
# Input: 15
# Expected: "You are a minor"

# Test 3: Empty input
./age_check.sh
# Input: [press Enter]
# Expected: Error message

# Test 4: Non-numeric
./age_check.sh
# Input: abc
# Expected: Error message

# Test 5: Negative number
./age_check.sh
# Input: -5
# Expected: Error message or handled
```

---

## File Permissions Quick Reference

```bash
# Permission format: -rwxrwxr-x
# Position: -[owner][group][others]

# r = read (4)
# w = write (2)
# x = execute (1)

# Common permissions:
chmod 755 script.sh  # rwxr-xr-x (most common for scripts)
chmod +x script.sh   # Add execute for everyone
chmod u+x script.sh  # Add execute for owner only
chmod 644 file.txt   # rw-r--r-- (common for data files)
```

---

## 📌 Remember!

- **Shebang first:** `#!/bin/bash` must be the very first line
- **Make executable:** `chmod +x script.sh` before running
- **Run with ./:** Use `./script.sh` not just `script.sh`
- **Quote variables:** Always use `"${variable}"` for safety
- **Validate input:** Check before using user data
- **Exit codes:** Use `exit 0` for success, `exit 1` for errors
- **Test thoroughly:** Try different inputs, including edge cases
- **Document well:** Add comments for complex logic

---

**💾 Save this page!** You'll use these commands throughout the course!

**🔖 Pro tip:** Keep this open during labs for quick reference!


