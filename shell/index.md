## Dotfile

keeping [dotfiles](https://github.com/n0npax/dotfiles) somewhere is an good practice. Once you need setup new machine, It's easier to bring back favourite aliases and configuration.

## ZSH

Please check [oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh) which brings tones of good magic.

In example:
```
alias | grep '\.\.\.' | head -n4
...=../..
....=../../..
.....=../../../..
......=../../../../..
```

## Any SHELL

### parallel processing using bash

Why ? Because we can!
In real world it's also useful, as sometimes you don't want to use `strong` programming language and loose easy access to unix shell tools.

```
find ~/workspace/my_project -print0 -name '*.go' | xargs -0 -P3 -IITEM bash -c 'echo ITEM | wc -c'
```

Please have a look at: `-print0` which means `find` will separate matched using `null`. Same null can be used by `xargs` as separator. `-P` means max processes executed by `xargs`.


### brackets

rename using brackets for name evaluation

```bash
mv myfile{.txt,.md}
```

or use brackets to eval sequence or set
```
$ for i in {1..4}; do echo $i; done  
1
2
3
4
$ for i in {1,99,4}; do echo $i; done
1
99
4
```

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