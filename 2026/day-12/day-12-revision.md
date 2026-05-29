## What to Review (pick at least one per section)
- **Mindset & plan:** revisit your Day 01 learning plan—are your goals still right? any tweaks?  - no 
- **Processes & services:** rerun 2 commands from Day 04/05 (e.g., `ps`, `systemctl status`, `journalctl -u <service>`); jot what you observed today.  
- **File skills:** practice 3 quick ops from Days 06–11 (e.g., `echo >>`, `chmod`, `chown`, `ls -l`, `cp`, `mkdir`).  
- **Cheat sheet refresh:** skim your Day 03 commands—highlight 5 you’d reach for first in an incident.  
- **User/group sanity:** recreate one small scenario from Day 09 or Day 11 (create a user or change ownership) and verify with `id`/`ls -l`.

## Mini Self-Check (write short answers in `day-12-revision.md`)
1) Which 3 commands save you the most time right now, and why?  
df -kh --> helps pinpoint where disk space issue is originating from 
ps -aux --> lists all processes 
grep --> makes it easier to search within docs 
2) How do you check if a service is healthy? List the exact 2–3 commands you’d run first.  
systemctl status service-name 
3) How do you safely change ownership and permissions without breaking access? Give one example command.  
chown owner:group file_name
chmod 755 file_name 
4) What will you focus on improving in the next 3 days?
remembering syntax for loops , differentiating when to use which networking command 


