# Three standard I/O channels

1. Standard Input (stdin)
2. Standard output (stdout)
3. Standard Error(stderr)
4. `>` for output redirection, `<` for input redirection, `2>` for stderr redirection.



- `cat fname` is the same as `cat < fname`
- `cat > fname` creates or overwrites the `fname` file.

```sh
chan@CMA:~/Downloads$ cat fname
cat: fname: No such file or directory
chan@CMA:~/Downloads$ cat > fname
Hey It's Unix
chan@CMA:~/Downloads$ cat < fname
Hey It's Unix
```

