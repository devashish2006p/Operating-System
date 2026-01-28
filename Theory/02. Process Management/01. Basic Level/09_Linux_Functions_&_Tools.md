# Process State & State Transition 

## 1. USER-SPACE TOOLS 

1. ps
2. ps aux
3. ps -o stat
4. top
5. htop
6. atop
7. watch
8. pstree
9. watch ps
10. cat /proc/PID/status
11. cat /proc/PID/stat


## 2. USER-SPACE FUNCTIONS 

### i) New → Running

#### fork()

#### vfork()

#### clone()

#### posix_spawn()


### Running → Ready

#### sched_yield()

### Running → Waiting / Sleeping

#### sleep()
#### usleep()
#### nanosleep()
#### pause()

### Waiting / Sleeping → Ready

#### No direct functions exist in user space.

### Running → Stopped

#### kill()
#### raise()
#### ctrl+z

### Stopped → Ready

#### kill()
#### raise()
#### cmd fg
#### cmd bg

### Running → Terminated (Zombie)

#### exit()
#### _exit()
#### abort()

### Zombie → Dead

#### wait()
#### waitpid()
#### waitid()
