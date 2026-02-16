# 📅 Day 18 -- Shell Scripting: Functions & Advanced Concepts

## 🎯 Objective

Write cleaner and reusable shell scripts using:

-   Functions\
-   Local variables\
-   Strict mode (`set -euo pipefail`)\
-   Real-world scripting patterns

------------------------------------------------------------------------

# 🟢 Task 1 -- Basic Functions

## functions.sh

``` bash
#!/bin/bash

greet() {
    local name="$1"
    echo "Hello, $name!"
}

add() {
    local num1="$1"
    local num2="$2"
    local sum=$((num1 + num2))
    echo "Sum: $sum"
}

greet "Vinay"
add 10 20
```

### ▶ Output

    Hello, Vinay!
    Sum: 30

------------------------------------------------------------------------

# 🟢 Task 2 -- Disk & Memory Check

## disk_check.sh

``` bash
#!/bin/bash

check_disk() {
    echo "===== Disk Usage ====="
    df -h /
    echo
}

check_memory() {
    echo "===== Memory Usage ====="
    free -h
    echo
}

main() {
    check_disk
    check_memory
}

main
```

### ▶ Output (Example)

    ===== Disk Usage =====
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/root        20G  3.5G   16G  18% /

    ===== Memory Usage =====
                   total        used        free
    Mem:           1.9Gi       400Mi       900Mi

------------------------------------------------------------------------

# 🟡 Task 3 -- Strict Mode

## strict_demo.sh

``` bash
#!/bin/bash

set -euo pipefail

echo "Strict mode enabled"

# Undefined variable (triggers set -u)
echo "$UNDEFINED_VAR"

# Command failure (triggers set -e)
ls /nonexistent-directory

# Pipeline failure (triggers pipefail)
false | true
```

### Explanation of Strict Mode

-   **set -e** → Exit immediately if any command fails.
-   **set -u** → Exit if an undefined variable is used.
-   **set -o pipefail** → Fail a pipeline if any command inside it
    fails.

Strict mode prevents silent errors and makes scripts production-safe.

------------------------------------------------------------------------

# 🟢 Task 4 -- Local Variables

## local_demo.sh

``` bash
#!/bin/bash

name="GlobalName"

change_name_global() {
    name="ChangedByFunction"
}

change_name_local() {
    local name="LocalName"
    echo "Inside function: $name"
}

echo "Before: $name"
change_name_global
echo "After global change: $name"

name="GlobalName"
change_name_local
echo "After local change: $name"
```

### ▶ Output

    Before: GlobalName
    After global change: ChangedByFunction
    Inside function: LocalName
    After local change: GlobalName

Local variables prevent accidental modification of global variables.

------------------------------------------------------------------------

# 🟢 Task 5 -- System Info Reporter

## system_info.sh

``` bash
#!/bin/bash
set -euo pipefail

print_system_info() {
    echo "===== System Info ====="
    hostname
    uname -a
    echo
}

print_uptime() {
    echo "===== Uptime ====="
    uptime
    echo
}

print_disk_usage() {
    echo "===== Top 5 Disk Usage ====="
    du -ah / 2>/dev/null | sort -rh | head -5
    echo
}

print_memory() {
    echo "===== Memory Usage ====="
    free -h
    echo
}

print_top_processes() {
    echo "===== Top 5 CPU Processes ====="
    ps -eo pid,comm,%cpu --sort=-%cpu | head -6
    echo
}

main() {
    print_system_info
    print_uptime
    print_disk_usage
    print_memory
    print_top_processes
}

main
```

------------------------------------------------------------------------

# 🧠 What I Learned (3 Key Points)

1.  How to write reusable and structured scripts using functions and a
    `main()` pattern.
2.  Why strict mode (`set -euo pipefail`) is essential for
    production-safe scripting.
3.  How `local` variables prevent global variable conflicts and improve
    script reliability.
