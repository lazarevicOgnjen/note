**biblioteke**
---

- stdio.h
- stdlib.h
- string.h

<br><br>

**argumenti**
---

```c

int main(int argc, char* argv[]){


}

```

<br><br>

**konverzija**
---

> **ASCII to float**

```

tmp = atof(argv[i]);

```

<br><br>

**rad sa stringovima**
---

> **poredi 2 stringa i vraca 0 ako su stringovi isti**

```c

strcmp(s1, "KRAJ")

```

> **proverava da li se dati text nalazi u buff**

```c

if(strstr(buff, "Hello world")){

}

```

> **kopira s2 u s1**

```c

strcpy(s1, s2);

```

<br><br>

**kastovanje**
---

```c

int x = 10;
double y;

y = (double) x;

```

<br><br>

**fajlovi**
---

> **kreiramo pokazivac**

```c

FILE * f;

```

> **pristupanje fajlu**

```c

f = fopen("file.txt", "r");
f1 = fopen("file1.txt", "w");

```

> **upisujemo u fajl**

```c

fprintf(f1, "broj: %d", x);

```

> **proveravamo da li je fajl otvoren**

```c

 if(!f){
   printf("doslo je do greske");
   return -1;
 }

```

> **zatvaranje fajla**

```c

fclose(f);

```

> **ako zelimo da prodjemo kroz ceo fajl**

```c

while( !feof(f) ){

}

```

> **citamo rec po rec**

```c

fscanf(f, "%s", buff);

```

> **citamo ceo sadrzaj velicine MAX_BUFF**

```c

fgets(buff, MAX_BUFF, f2);

```

<br><br>

**buffer**
---

```c

#define MAX_BUFF 10

int main(int argc, char* argv[]){

  char buff[MAX_BUFF];

  for(int i=0; i<MAX_BUFF; i++){
    buff[i] = '\0';
  }

}


```



