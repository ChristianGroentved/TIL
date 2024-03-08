# Escape single quotes within a single quoted string

Escaping a single quoted string can be done in the following way
```bash
 alias rxvt='urxvt -fg '"'"'#111111'"'"' -bg '"'"'#111111'"'"
 #                     ^^^^^       ^^^^^     ^^^^^       ^^^^
 #                     12345       12345     12345       1234
```

How is `'"'"'` interpreted as jus `'`:

1. `'` End first quotation which uses single quotes.
2. `"` Start a second quotation, using double quotes.
3. `'` Quoted character.
4. `"` End a second quotation, using double quotes.
5. `'` Start a third quotation, using single quotes.

All credit goes to [liori](https://stackoverflow.com/questions/1250079/how-to-escape-single-quotes-within-single-quoted-strings) answer on stack overflow