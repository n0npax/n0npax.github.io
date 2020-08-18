# golang

## default argument
In theory it's not supported feature. In theory.

Let's consider function `foo`
```
func foo(bar ...string) int {
    myVal := 0
    if len(bar) > 0 {
        myVal = bar[0]
    }
    return myVal
}
```

If function was invoked as `foo()` it will return 0. In case it was invoked as `foo(5)`, it will return 5.

Is it used like that? Yes. In example in [kubernetes/minikube](https://github.com/kubernetes/minikube/blob/master/pkg/minikube/machine/client.go#L60)