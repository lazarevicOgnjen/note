> **na kolokvijumu je potrebno da se lokalno preuzme a zatim commituje repozitorijum**

<br>

kloniranje
---

- `git clone ...`
- izaberemo **yes**
- ukucamo **password**
- `cd KOL1`
- `code .`

<br>

komitovanje
---

- `git add *`
- `git commit -m "Gotov kolokvijum"`
  
> **nakon ovog koraka terminal moze da izbaci -** <mark>"Author identity unknown"</mark> <br>

tada ukucamo sledece komande <br>
`git config --global user.email "student"` <br>
`git config --global user.name "student"` <br>
nakon cega ponovo unosimo `git commit -m "Gotov kolokvijum"`
 
- `git push`
- ukucamo **password**

<br><br>
