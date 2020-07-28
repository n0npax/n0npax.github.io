# Linux

## Extended attributes

`lsattr` in example shows extended file perms. Yes, you can be a root and be not allowed to modify `/etc/hosts`. Check the [man](https://man7.org/linux/man-pages/man1/lsattr.1.html)

```bash
$ touch foo      
$ _ chattr +i foo
$ _ rm foo       
rm: cannot remove 'foo': Operation not permitted
$ _ whoami
root
```

## Tricks

My favourite linux interview question:

I accidentally run `chmod -x /usr/bin/chmod` can you fix this for me? We don't have any access to the internet right now.

How many solutions can you find?

## Security

### Must have on system

* ssh only via key
* fail2ban
  * auto blocks ip who are performing some attacks (in example common passords)
* etckeeper
  * later or sooner something may get broken. Etckeeper helps with investigation as long as it's not destroyed

### Lets get crazy :)

#### Preventing core dumps

Setting the resource limit RLIMIT_CORE to 0 disables core dumps (ulimit -c 0)