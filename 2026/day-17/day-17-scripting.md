### Task 1: For Loop
1. Create `for_loop.sh` that:
   - Loops through a list of 5 fruits and prints each one
2. Create `count.sh` that:
   - Prints numbers 1 to 10 using a for loop

## for_loop.sh
#!/bin/bash 
for i in banana apple kiwi papaya strawberry; do 
    echo $i
done

## count.sh
#!/bin/bash 
for i in {1..10} ; do 
    echo $i
done
---

### Task 2: While Loop
1. Create `countdown.sh` that:
   - Takes a number from the user
   - Counts down to 0 using a while loop
   - Prints "Done!" at the end

#!/bin/bash
read -p "enter a number:" num 
while [ $num -gt -1 ] ; do 
    echo $num
    ((num--))
done
echo "Done!"
---

### Task 3: Command-Line Arguments
1. Create `greet.sh` that:
   - Accepts a name as `$1`
   - Prints `Hello, <name>!`
   - If no argument is passed, prints "Usage: ./greet.sh <name>"
vim greet.sh
#!/bin/bash
if [ $# -eq 0 ]; then
    echo "Usage: ./greet.sh <name>"
else
    echo "Hello, $1!"
fi

2. Create `args_demo.sh` that:
   - Prints total number of arguments (`$#`)
   - Prints all arguments (`$@`)
   - Prints the script name (`$0`)

vim args_demo.sh
#!/bin/bash
echo "total no of args entered are: $#"
echo "all arguments entered are: $@"
echo "script name is $0"

ubuntu@ip-172-31-46-159:~$ bash args_Demo.sh hello hi meow
total no of arg entered are: 3
all arguments entered are: hello hi meow
script name is args_Demo.sh
---

### Task 4: Install Packages via Script
1. Create `install_packages.sh` that:
   - Defines a list of packages: `nginx`, `curl`, `wget`
   - Loops through the list
   - Checks if each package is installed (use `dpkg -s` or `rpm -q`)
   - Installs it if missing, skips if already present
   - Prints status for each package

> Run as root: `sudo -i` or `sudo su`

vim install_packages.sh
#!/bin/bash

for i in nginx curl wget; do
    if dpkg -s "$i" >/dev/null 2>&1; then 
        echo "$i is already installed"
    else
        echo "$i is not installed. Installing..."
        sudo apt install -y "$i"
    fi
done

---

### Task 5: Error Handling
1. Create `safe_script.sh` that:
   - Uses `set -e` at the top (exit on error)
   - Tries to create a directory `/tmp/devops-test`
   - Tries to navigate into it
   - Creates a file inside
   - Uses `||` operator to print an error if any step fails

Example:
```bash
mkdir /tmp/devops-test || echo "Directory already exists"
```
#!/bin/bash

mkdir /tmp/devops-test || {
    echo "Failed to create directory"
    exit 1
}

cd /tmp/devops-test || {
    echo "Failed to change directory"
    exit 1
}

touch devops-file.txt || {
    echo "Failed to create file"
    exit 1
}


2. Modify your `install_packages.sh` to check if the script is being run as root — exit with a message if not.
# because root user will always have ID 0
if [ "$EUID" -ne 0 ]; then
    echo "Please run this script as root."
    exit 1
fi
---

## Hints
- For loop: `for item in list; do ... done`
- While loop: `while [ condition ]; do ... done`
- Arguments: `$1` first arg, `$#` count, `$@` all args
- Check root: `if [ "$EUID" -ne 0 ]; then echo "Run as root"; exit 1; fi`
- Check package: `dpkg -s <pkg> &> /dev/null && echo "installed"`

---


