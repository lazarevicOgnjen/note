**biblioteke**
---

- stdio.h
- stdlib.h
- string.h

<br>

**argumenti**
---

```c

int main(int argc, char* argv[]){


}

```

<br>

**konverzija**
---

> **ASCII to float**

```

tmp = atof(argv[i]);

```

<br>

**rad sa stringovima**
---

```c

strcmp(s1, "KRAJ")

```
**vraca 0** ako su stringovi isti

<br>

**kastovanje**
---

```c

int x = 10;
double y;

y = (double) x;

```

<br>

**fajlovi**
---

> **kreiramo pokazivac**

```c

FILE * f;

```

> **pristupanje fajlu**

```c

f = fopen("/proc/self/status", "r");

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

> **citamo ceo sadrzaj**

```c

fgets(buff, MAX_BUFF, f2);

```

