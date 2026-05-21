# create a file
root@ubuntu-host ~/test-codes ➜  touch notes.txt

# write to file 
root@ubuntu-host ~/test-codes ➜  echo "line1" > notes.txt

# append to file 
root@ubuntu-host ~/test-codes ➜  echo "line2" >> notes.txt

# print file contents
root@ubuntu-host ~/test-codes ➜  cat notes.txt 
line1
line2

root@ubuntu-host ~/test-codes ➜  echo "line3" >> notes.txt 
root@ubuntu-host ~/test-codes ➜  echo "line4" >> notes.txt 

# print first 3 lines in file from top
root@ubuntu-host ~/test-codes ➜  head -n 3 notes.txt 
line1
line2
line3

# print last 3 lines in file from top
root@ubuntu-host ~/test-codes ➜  tail -n 3 notes.txt 
line2
line3
line4

# using > overwrites the file - it is write only , not append 
root@ubuntu-host ~/test-codes ➜  echo "line4" > notes.txt 
root@ubuntu-host ~/test-codes ➜  cat notes.txt 
line4

# using tee to write and display added content at the same time
root@ubuntu-host ~/test-codes ➜  echo "Line 3" | tee -a notes.txt
Line 3
root@ubuntu-host ~/test-codes ➜  head -n 3 notes.txt 
line1
line2
Line 3