# GoUltimate

- `new()` return reference of data
- `make()` would return data structure of reference like
```go
  v := make(map[string]string)
  v["aaa"] = this would be referenced
```
so when we have
```go
  v := make(map[string]string)
  func (v map[string]string){
    v["some string"] -> this is referenced type. it will effect v outside this function
  }(v)
```

- return value sematic
```go
  func v() type{}
```
- escape means resolution when using reference between 2 stack like
```go
  func foo() *type{}
  func main(){
    v := foo()
  }
```
so they move/escape data from memory of stack to heap, heap like common place of shared memory that used between 2 stack.

- better to write
```
var u user
err := json.Unmashal([]byte("..."), &u)
return &u
```
than
```
var u &user
err := json.Unmashal([]byte("..."), &u)
return u
```
you can see this scope of function and sharing like this function finally return sharing object. it's focused.

