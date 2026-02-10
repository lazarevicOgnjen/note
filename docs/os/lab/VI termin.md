> **Signali i redovi poruka**

<br>

**Definicija signali**
---

Signali su mehanizam koji se koristi kako bi se proces **obavestio o pojavi** nekog asinhronog dogadjaja.

**Obicno operativni sistemi salju signal** procesima u slucaju greske (neocekivani dogadjaji).

Svaki proces **moze da salje signal drugom procesu ukoliko ima dozvolu da to uradi.**

Svaki signal ima **pridruzenu funkciju za obradu.** 

Kada procesu **stigne signal** (signalizira pocetak necega) proces koji se izvrsava **prekida svoj tok izvrsavanja i poziva funkciju** za obradu tog zadataka.

Mozemo podesiti da neki **odredjeni signal bude ignorisan.**

<br>

🚨  **BITNO**

Mozemo koristiti samo postojece signale kao sto su: <br>
- SIGINT <br>
- SIGTSTP <br>
- SIGALRM

<br><br>

**Primer za signale**
---

> ALARM
```

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

char ime[10];

void alarm_f(int sig_num){

	printf("\n\nvreme je isteklo\n");
	exit(0);

}

int main(int argc, char* argv[]){


	signal(SIGALRM, alarm_f);

	printf("unesi ime: ");
	alarm(10);
	fgets(ime, sizeof(ime), stdin);

	printf("zdravo %s", ime);

	
	return 0;
}

```

<br>

ako bismo opet postavili alarm on bi se 'resetovao' na tu novu vrednost, tako da bi ovaj program ispod cekao manje od 5 sekundi

```

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>

char ime[10];

void alarm_f(int sig_num){

	printf("\n\nvreme je isteklo\n");
	exit(0);

}

int main(int argc, char* argv[]){


	signal(SIGALRM, alarm_f);

	printf("unesi ime: ");
	alarm(5);
	alarm(1); 
	fgets(ime, sizeof(ime), stdin);

	printf("zdravo %s", ime);

	
	return 0;
}

```

<br><br>

**Redovi poruka**
---

<br>

**Definicija poruke**

Redovi poruka predstavljaju **lancanu listu poruka** koji su smesteni u jezgru (u kernelu operativnog sistema) i oznaceni su **jedinstvenim indentifikatorom reda poruka.**

Svaka poruka ima isti oblik: 

- ima msg strukturu
- sadrzi polje koje oznacava tip poruke
- duzinu poruke
- bajtove sa podacima koji predstavljaju poruku navedene duzine

<br><br>

**Biblioteke**

```

#include <sys/ipc.h>
#include <sys/msg.h>

```

<br><br>

**Struktura poruke**
---

```

struct mymsgbuf{

	long mtype;
	char mtext[50];

};

```




















