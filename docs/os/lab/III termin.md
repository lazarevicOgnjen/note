**procesi**
---

<br>

🚨 **BITNO**

- novi proces dobijamo tako sto **dupliramo postojeci**
- korisitmo **fork**
- novi proces **nasledjuje sav kod i vrednosti promenljivih** od roditelja


<br>

**biblioteke**
---

```c
#include <sys/types.h>
#include <unistd.h>

```

<br>

**fork**
---

kreiramo novi proces
```c

pid_t fork();

```

---

proveravamo koji se proces izvrsava
```c
switch( cpid=fork() ){

  case -1:
    perror("funkcija fork nije uspela");
    exit(1);

  case 0: // izvrsava se proces dete
    continue_child();
    break;

  default: // izvrsava se proces roditelj
    continue_parent(cpid);

}

```

---

cekamo da se svi procesi deca zavrse
```c

pid_t wait (int *status-ptr);

```

---

cekamo da se konkretan proces dete zavrsi
```c

pid_t wait (pid_t pid, int *status-ptr);

```

---


**PID**
---

PID tekuceg procesa
```c
pid_t getpid();
```

---

PID roditeljskog procesa
```c
pid_t getppid();
```



