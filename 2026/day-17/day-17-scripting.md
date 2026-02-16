# 📅 Day 17 -- Shell Scripting: Loops, Arguments & Error Handling

## 🎯 Objective

Practice advanced shell scripting concepts:

-   For loops\
-   While loops\
-   Command-line arguments\
-   Installing packages via script\
-   Basic error handling

------------------------------------------------------------------------

# 🟢 Task 1 -- For Loop

## 1️⃣ `for_loop.sh`

``` bash
#!/bin/bash

fruits=("Apple" "Banana" "Mango" "Orange" "Grapes")

for fruit in "${fruits[@]}"
do
    echo "Fruit: $fruit"
done
```

### ▶ Output

    Fruit: Apple
    Fruit: Banana
    Fruit: Mango
    Fruit: Orange
    Fruit: Grapes

------------------------------------------------------------------------

## 2️⃣ `count.sh`

``` bash
#!/bin/bash

for i in {1..10}
do
    echo $i
done
```

### ▶ Output

    1
    2
    3
    4
    5
    6
    7
    8
    9
    10

------------------------------------------------------------------------

# 🟢 Task 2 -- While Loop

## 3️⃣ `countdown.sh`

``` bash
#!/bin/bash

read -p "Enter a number: " number

while (( number >= 0 ))
do
    echo $number
    ((number--))
done

echo "Done!"
```

### ▶ Output (Example)

    Enter a number: 5
    5
    4
    3
    2
    1
    0
    Done!

------------------------------------------------------------------------

# 🟢 Task 3 -- Command-Line Arguments

## 4️⃣ `greet.sh`

``` bash
#!/bin/bash

if [ $# -eq 0 ]
then
    echo "Usage: ./greet.sh <name>"
    exit 1
fi

echo "Hello, $1!"
```

### ▶ Output

    ./greet.sh Vinay
    Hello, Vinay!

If no argument:

    Usage: ./greet.sh <name>

------------------------------------------------------------------------

## 5️⃣ `args_demo.sh`

``` bash
#!/bin/bash

echo "Script Name: $0"
echo "Total Arguments: $#"
echo "All Arguments: $@"
```

### ▶ Output

    ./args_demo.sh DevOps AWS Docker
    Script Name: ./args_demo.sh
    Total Arguments: 3
    All Arguments: DevOps AWS Docker

------------------------------------------------------------------------

# 🟢 Task 4 -- Install Packages via Script

## 6️⃣ `install_packages.sh`

``` bash
#!/bin/bash

if [ "$EUID" -ne 0 ]; then
    echo "Please run as root (sudo)."
    exit 1
fi

packages=("nginx" "curl" "wget")

echo "Updating package list..."
apt update -y

for pkg in "${packages[@]}"
do
    if dpkg -s $pkg &> /dev/null
    then
        echo "$pkg is already installed. Skipping..."
    else
        echo "$pkg not installed. Installing..."
        apt install -y $pkg
        echo "$pkg installation completed."
    fi
done
```

### ▶ Output (Example)

    Updating package list...
    nginx is already installed. Skipping...
    curl is already installed. Skipping...
    wget is already installed. Skipping...

------------------------------------------------------------------------

# 🟢 Task 5 -- Error Handling

## 7️⃣ `safe_script.sh`

``` bash
#!/bin/bash

set -e

mkdir -p /tmp/devops-test
cd /tmp/devops-test
touch testfile.txt

echo "Script executed successfully!"
```

### ▶ Output

    Script executed successfully!

This script is **idempotent** and safe to run multiple times.

------------------------------------------------------------------------

# 🧠 What I Learned (3 Key Points)

1.  How to use `for` and `while` loops to automate repetitive tasks.\
2.  How to handle command-line arguments using `$1`, `$#`, and `$@`.\
3.  How to write safe and idempotent scripts using `set -e`, `mkdir -p`,
    and proper error handling.
