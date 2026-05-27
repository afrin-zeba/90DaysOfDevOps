
### Task 1: Your First Script
1. Create a file `hello.sh`
2. Add the shebang line `#!/bin/bash` at the top
3. Print `Hello, DevOps!` using `echo`
4. Make it executable and run it

```bash
chmod +x hello.sh
./hello.sh
```

#!/bin/bash
echo "Hello, DevOps!"

chmod 764 hello.sh
./hello.sh

**Document:** What happens if you remove the shebang line? - i was still able to execute it - no change in output 

---

### Task 2: Variables
1. Create `variables.sh` with:
   - A variable for your `NAME`
   - A variable for your `ROLE` (e.g., "DevOps Engineer")
   - Print: `Hello, I am <NAME> and I am a <ROLE>`
2. Try using single quotes vs double quotes — what's the difference? --> double qoutes takes the value whereas single qoutes just prints whatever is written eg:- echo 'hello $name' will literally print hello $name

#!/bin/bash 
name="afrin"
role="devops engineer"
echo "Hi I am $name and i am a $role"
---

### Task 3: User Input with read
1. Create `greet.sh` that:
   - Asks the user for their name using `read`
   - Asks for their favourite tool
   - Prints: `Hello <name>, your favourite tool is <tool>`

#!/bin/bash
read -p "enter your name:" name
read -p "enter your fav tool:" tool
echo "hello $name your fav tool is $tool"
---

### Task 4: If-Else Conditions
1. Create `check_number.sh` that:
   - Takes a number using `read`
   - Prints whether it is **positive**, **negative**, or **zero**

#!/bin/bash
read -p "enter a number:" num 
if [ $num -gt 0 ]; then 
	echo "positive number"
elif [ $num -lt 0 ]; then
       echo "negative number"
elif [ $num == 0 ]; then
	echo "number entered is 0"
fi
---

2. Create `file_check.sh` that:
   - Asks for a filename
   - Checks if the file **exists** using `-f`
   - Prints appropriate message
!#/bin/bash
read -p "enter file path:" file
if [ -f $file ]; then 
	echo "file path exists"
else 
	echo "file path doesnt exist"
fi
---

### Task 5: Combine It All
Create `server_check.sh` that:
1. Stores a service name in a variable (e.g., `nginx`, `sshd`)
2. Asks the user: "Do you want to check the status? (y/n)"
3. If `y` — runs `systemctl status <service>` and prints whether it's **active** or **not**
4. If `n` — prints "Skipped."

#!/bin/bash
service=nginx
read -p "do you want to check $service status?" flag
if [ $flag == 'y' ]; then
	systemctl status $service 
elif [ $flag == 'n' ]; then 
	echo "skipped"
fi
---