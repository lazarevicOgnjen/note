**Signali i redovi poruka**

<br>

**DEFINICIJA**
---

Signali su mehanizam koji se koristi kako bi se proces **obavestio o pojavi** nekog asinhronog dogadjaja. <br>
**Obicno operativni sistemi salju signal** procesima u slucaju greske (neocekivani dogadjaji). <br>
Svaki proces **moze da salje signal drugom procesu ukoliko ima dozvolu da to uradi.** <br>
Svaki signal ima **pridruzenu funkciju za obradu.** <br>
Kada procesu **stigne signal** (signalizira pocetak necega) proces koji se izvrsava **prekida svoj tok izvrsavanja i poziva funkciju** za obradu tog zadataka. <br>
Mozemo podesiti da neki **odredjeni signal bude ignorisan.**

<br>

🚨  **BITNO**

Mozemo koristiti samo postojece signale kao sto su: <br>
- SIGINT
- SIGTSTP
- SIGALRM

<br><br>

**Primeri**

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






