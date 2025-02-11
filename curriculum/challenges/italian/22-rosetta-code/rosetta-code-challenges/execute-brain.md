---
id: 59e0a8df964e4540d5abe599
title: Esegui Brain****
challengeType: 1
forumTopicId: 302261
dashedName: execute-brain
---

# --description--

Write a function to implement a Brain\*\*\*\* interpreter. The function will take a string as a parameter and should return a string as the output. More details are given below:

RCBF è un set di compilatori e interpreti <a href="https://rosettacode.org/wiki/Brainf***" target="_blank" rel="noopener noreferrer nofollow">Brainf\*\*\*</a> scritti per Rosetta Code in una varietà di linguaggi.

Di seguito sono riportati i link a ciascuna delle versioni di RCBF.

Un'implementazione deve solo attuare correttamente le seguenti istruzioni:

| Command                   | Descrizione                                                                                           |
| ------------------------- | ----------------------------------------------------------------------------------------------------- |
| <code>></code> | Move the pointer to the right                                                                         |
| <code>&lt;</code> | Sposta il puntatore a sinistra                                                                        |
| <code>+</code> | Incrementa la cella di memoria al puntatore                                                           |
| <code>-</code> | Decrementa la cella di memoria al puntatore                                                           |
| <code>.</code> | Esegue l'output del carattere indicato dalla cella al puntatore                                       |
| <code>,</code> | Inserisci un carattere e memorizzalo nella cella al puntatore                                         |
| <code>\[</code> | Salta oltre la corrispondente <code>]</code> se la cella al puntatore è 0                    |
| <code>]</code> | Torna indietro alla corrispondente <code>\[</code> se la cella sotto il puntatore non è zero |

Qualsiasi dimensione della cella è consentita, il supporto EOF (*E*nd-*O*f-*F*ile) è opzionale, come se tu avessi una memoria limitata o illimitata.

# --hints--

`brain(bye)` dovrebbe restituire una stringa

```js
assert(typeof brain(bye) === 'string');
```

`brain("++++++[>++++++++++++++.")` should return "A"

```js
assert.equal(brain('++++++[>++++++++++++++.'), 'A');
```

`brain(bye)` dovrebbe restituire `Goodbye, World!\r\n`

```js
assert.equal(brain(bye), 'Goodbye, World!\r\n');
```

`brain(hello)` dovrebbe restituire `Hello World!\n`

```js
assert.equal(brain(hello), 'Hello World!\n');
```

`brain(fib)` dovrebbe restituire `1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89`

```js
assert.equal(brain(fib), '1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89');
```

# --seed--

## --before-user-code--

```js
let fib=`+

++

+++++++

+>+>>

>>++++++++++++++++++++++++

+++++++++++++++++++

++++++++++++

+++++++++++++

+++++++

++++++[-<-[>>+>+<<

<-]>>>[<<<+>>>-]+<[

>[-]<[-]]>[<<[>>>+<<

<-]>>[-]]<<]>>>[>>+>+

<<<-]>>>[<<<+>>>-]+<[>

[-]<[-]]>[<<+>>[-]]<<<<

<<<]>>>>>[++++++++++++++

+++++++++++++++++++++++++

+++++++++++++++++++<[

->-<]>++++++++++++++++++++++++++++++++++++++++++++++++.

[-]<<<<<<<<<<<<[>>>+>+<<<<-]>

>>>[<<<<+>>>>-]<-[>>.>.<<<[-]]

<<[>>+>+<<<-]>>>[<<<+>>>-]<<[<+

>-]>[<+>-]<<<-]`;
let hello='++++++++[>++++++>++++++++++++.>>.<-.<.+++++++++++++[>+>+++++++>+++++++>++++++++>+++++++++++++++++++>+++++++++++++++++++++++++++++++.<++.>>>+++++++.>>>.++++++++++++.---.';
```

## --seed-contents--

```js
function brain(prog) {

}
```

# --solutions--

```js
function brain(prog){
  var output="";
    var code; // formatted code
  var ip = 0; // current instruction within code
  var nest = 0; // current bracket nesting (for Out button)
  var ahead = []; // locations of matching brackets

  var data = [0]; // data array (mod by +, -)
  var dp = 0; // index into data (mod by <, >)

  var inp = 0; // current input character (fetch with ,)
  var quit = 0;
    var commands = {
    '>':function() { if (++dp >= data.length) data[dp]=0 },
    '<':function() { if (--dp < 0) quit++ },
    '+':function() { ++data[dp] },
    '-':function() { --data[dp] },
    '[':function() { if (!data[dp]) ip = ahead[ip]; else ++nest },
    ']':function() { if ( data[dp]) ip = ahead[ip]; else --nest },
    ',':function() {
        var c = document.getElementById("input").value.charCodeAt(inp++);
        data[dp] = isNaN(c) ? 0 : c; // EOF: other options are -1 or no change
    },
    '.':function() {
            output+=String.fromCharCode(data[dp]);
            /*var s = document.getElementById("output").innerHTML)
             + String.fromCharCode(data[dp]);
            s = s.replace(/\n/g,"<br>").replace(/ /g,"&amp;nbsp;");
            document.getElementById("output").innerHTML = s;*/
        },
    };

    let ar=prog.split('');
    var st = [], back, error = -1;
    for (ip=0; ip<ar.length; ip++) {
        switch(ar[ip]) {
        case '[':
            st.push(ip);
            break;
        case ']':
            if (st.length == 0) error = ip;
            back = st.pop();
            ahead[ip] = back;
            ahead[back] = ip;
            break;
        }
    }

    for(ip=0;ip<ar.length;ip++){
    if(commands.hasOwnProperty(ar[ip]))
          commands[ar[ip]]();
    }

    return output;
}
```
