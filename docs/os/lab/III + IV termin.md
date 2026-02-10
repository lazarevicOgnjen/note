**procesi**
---

<br>

🚨 **BITNO**

- novi proces dobijamo tako sto **dupliramo postojeci**
- korisitmo **fork**
- novi proces **nasledjuje sav kod i vrednosti promenljivih** od roditelja


<br><br>

**kreiranje procesa**
---

**I slucaj** kada nam treba samo jedan proces dete
```c

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

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

<br>

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
<br>

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

<br><br>

**komunikacija izmedju procesa**
---

🚨 **BITNO**

komunikacija moze da se uspostavi preko: <br>
1. datavoda <br>
2. signala <br>
3. deljene memorije

<br>

**primeri:**

- datavodi (jednosmerna komunikacija)
```c

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main() {
    int pipe_fd[2];  // pipe_fd[0] = read end, pipe_fd[1] = write end
    pid_t pid;
    char buffer[100];
    
    // Create pipe BEFORE fork
    if (pipe(pipe_fd) == -1) {
        perror("pipe failed");
        exit(1);
    }
    
    pid = fork();
    
    if (pid < 0) {
        perror("fork failed");
        exit(1);
    }
    else if (pid == 0) {
        // CHILD: Close write end, read from pipe
        close(pipe_fd[1]);  // Close unused write end
        
        read(pipe_fd[0], buffer, sizeof(buffer));
        printf("Child received: %s\n", buffer);
        
        close(pipe_fd[0]);
        exit(0);
    }
    else {
        // PARENT: Close read end, write to pipe
        close(pipe_fd[0]);  // Close unused read end
        
        const char *msg = "Hello from parent!";
        write(pipe_fd[1], msg, strlen(msg) + 1);
        
        close(pipe_fd[1]);
        wait(NULL);  // Wait for child
    }
    
    return 0;
}

```

<br>

- datavodi (dvosmerna komunikacija)
```c

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <string.h>

int main() {
    int parent_to_child[2];  // Parent writes, child reads
    int child_to_parent[2];  // Child writes, parent reads
    pid_t pid;
    char buffer[100];
    
    pipe(parent_to_child);
    pipe(child_to_parent);
    
    pid = fork();
    
    if (pid == 0) {
        // CHILD
        close(parent_to_child[1]);  // Close write end of first pipe
        close(child_to_parent[0]);  // Close read end of second pipe
        
        // Read from parent
        read(parent_to_child[0], buffer, sizeof(buffer));
        printf("Child got: %s\n", buffer);
        
        // Send response
        const char *response = "Hi parent!";
        write(child_to_parent[1], response, strlen(response) + 1);
        
        close(parent_to_child[0]);
        close(child_to_parent[1]);
        exit(0);
    }
    else {
        // PARENT
        close(parent_to_child[0]);  // Close read end of first pipe
        close(child_to_parent[1]);  // Close write end of second pipe
        
        // Send to child
        write(parent_to_child[1], "Hello child!", 13);
        
        // Read response
        read(child_to_parent[0], buffer, sizeof(buffer));
        printf("Parent got: %s\n", buffer);
        
        close(parent_to_child[1]);
        close(child_to_parent[0]);
        wait(NULL);
    }
    
    return 0;
}

```












