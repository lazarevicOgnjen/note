**procesi**
---

<br>

🚨 **BITNO**

- novi proces dobijamo tako sto **dupliramo postojeci**
- korisitmo **fork**
- novi proces **nasledjuje sav kod i vrednosti promenljivih** od roditelja


<br>

**fork**
---

**I slucaj** kada nam treba samo jedan proces dete
```c

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>

int main(){

  pid_t pid;
  pid = fork(); // Create a new process

  if (pid < 0) {
        // Fork failed
        perror("fork failed");
        exit(1);
    } 
    else if (pid == 0) {
        // This is the CHILD process
        printf("I am the child process. PID: %d, Parent PID: %d\n", 
               getpid(), getppid());
        
        exit(0);  // Child exits
    } 
    else {
        // This is the PARENT process
        // pid contains the child's process ID
        printf("I am the parent process. PID: %d, Child PID: %d\n", 
               getpid(), pid);
        
        // Wait for child to finish (optional but recommended)
        wait(NULL);
        
        printf("Child process finished\n");
    }

}

```

**II slucaj** kada nam treba vise
```c

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    pid_t pid1, pid2;
    
    // Create first child
    pid1 = fork();
    
    if (pid1 < 0) {
        perror("fork 1 failed");
        exit(1);
    }
    else if (pid1 == 0) {
        // CHILD 1 CODE
        printf("Child 1: PID=%d, Parent=%d\n", getpid(), getppid());
        
        // Do child 1 work here
        sleep(1);
        printf("Child 1: Done!\n");
        
        exit(0);  // MUST exit so child doesn't fork again!
    }
    
    // PARENT ONLY: Create second child
    pid2 = fork();
    
    if (pid2 < 0) {
        perror("fork 2 failed");
        exit(1);
    }
    else if (pid2 == 0) {
        // CHILD 2 CODE
        printf("Child 2: PID=%d, Parent=%d\n", getpid(), getppid());
        
        // Do child 2 work here
        sleep(2);
        printf("Child 2: Done!\n");
        
        exit(0);  // MUST exit!
    }
    
    // PARENT CODE ONLY
    printf("Parent: PID=%d, Child1=%d, Child2=%d\n", getpid(), pid1, pid2);
    
    // Wait for both children to finish
    wait(NULL);  // Waits for first child to finish
    wait(NULL);  // Waits for second child to finish
    
    printf("Parent: Both children finished\n");
    
    return 0;
}

```
ovde mozemo da koristimo i petlju
```c

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    pid_t pids[2];
    
    for (int i = 0; i < 2; i++) {
        pids[i] = fork();
        
        if (pids[i] < 0) {
            perror("fork failed");
            exit(1);
        }
        else if (pids[i] == 0) {
            // CHILD: Do work then EXIT immediately
            printf("Child %d: PID=%d\n", i+1, getpid());
            sleep(i + 1);  // Different work time
            printf("Child %d: Finished\n", i+1);
            exit(0);  // CRITICAL: Exit so child doesn't fork!
        }
        // Parent continues loop to create next child
    }
    
    // PARENT ONLY
    printf("Parent: Created 2 children\n");
    
    // Wait for both
    for (int i = 0; i < 2; i++) {
        wait(NULL);
    }
    
    printf("Parent: All children done\n");
    return 0;
}

```


