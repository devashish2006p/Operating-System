# Process State & State Transition 

## 1. USER-SPACE TOOLS 
 
1. ps - Ya ek user-space command hai jo system ka current running processes ki snapshot information dikhata hai. Jab user *ps* run karta hai tab system ka ander jo processes chal rha hota hai unki info collect karta hai aur unhe table ka form ma dikha deta hai.

**Internal Mechanism**
- Command Execute hoti hai aur shell is cmd ko identify karta hai phir /bin/ps executable ko run karta hai.
- *ps* kernel sa direct baat nhi karta balki *ps* direct kernel call karke process list nhi leta instead ya "/proc" virtual filesystem ka use karta hai.
- */proc* sa data read hota hai (*/proc* ek special virtual folder hai jahan har process ka ek folder hota hai jahan sa *ps* data uthata hai).
- *ps* internally system ka sare "/proc" folders scan karta hai aur har PID ka lia state, memory, CPU usage, command name ya sab read karta hai.
- Data process hota hai jahan raw data ko format karta hai aur filter karta hai
- Output print hota hai. 

2. ps aux
3. ps -o stat
3. top
4. htop
5. atop
6. watch
9. pstree
10. watch ps
11. cat /proc/PID/status
12. cat /proc/PID/stat
13. pgrep
14. pidof
15. strace
16. lsof
17. nice
18. renice
19. time
20. uptime


## 2. USER-SPACE FUNCTIONS 

### i) New → Running

1) fork()

2) vfork()

3) clone()

4) posix_spawn()

5) execl()

6) execlp()

7) execv()

8) execvp()

9) execve()


### Running → Ready

1) sched_yield()

### Running → Waiting / Sleeping

1) sleep()

2) usleep()

3) nanosleep()

4) pause()

5) select()

6) poll()


### Waiting / Sleeping → Ready

#### No direct functions exist in user space.

### Running → Stopped

1) kill()
2) raise()
3) ctrl+z

### Stopped → Ready

1) kill()
2) raise()
3) cmd fg
4) cmd bg

### Running → Terminated (Zombie)

1) exit()
2) _exit()
3) abort()

### Zombie → Dead

1) wait()
2) waitpid()
3) waitid()
4) wait4()
