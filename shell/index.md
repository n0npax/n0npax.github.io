## Dotfile

keeping [dotfiles](https://github.com/n0npax/dotfiles) somewhere is an good practice. Once you need setup new machine, It's easier to bring back favourite aliases and configuration.

## ZSH

## Any SHELL

### Hide command from `.${SHELL}_history`

If you want to hide your action from being logged in `~/.bash_history` you can put a space before it. Please checkout the difference between: `echo llama` & ` echo beer`.

```bash
$ echo 'llama'      
llama
$ history | tail -n2
10118  pwd
10119  echo 'llama'
$  echo 'beer' # <--- please focus on additional space here    
beer
$ history | tail -n2
10118  pwd
10119  echo 'llama'

```

### Bang Bang

`!!` can be used to execute previous command. Please check the snippet. Once you press `enter` shell will autofill previous command and you can press `enter` one more time to execute it. 

```bash
$ echo 'llama'
llama
$ !! # <--- bang bang
$ echo 'llama'
llama
$ 
```