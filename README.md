# GoUltimate
https://www.ardanlabs.com/blog/2017/06/language-mechanics-on-memory-profiling.html
## Reference
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
```go
var u user
err := json.Unmashal([]byte("..."), &u)
return &u
```
than
```go
var u &user
err := json.Unmashal([]byte("..."), &u)
return u
```
you can see this scope of function and sharing like this function finally return sharing object. it's about focusing.

- you can see data movement by...
```shell
go build -gcflags "-m -m"
```

## Escape
- compiler translation
```go 
  type X sturct{ p int }
  x2 := &X{}
  x2.p = &i2
  //translate to
  (*x2).p = &i2
```
and i2 escape to heap

- cmd to see memory movement when ... is expected function
```
go test -gcflags "-m -m" -run none -bench ... -benchmem -memprofile mem.out
```
- memory display tool
```
go tool pprof -alloc_space mem.out
```
- sometime, compiler doesn't work properly (depends on version also). sometime the escape/kickout some in-stack variable to heap.

- benchmark and memory profiling, we will get rate info like ns/op, alloc/op
```
go test -run none -bench . -benchtime 3s -benchmem -memprofile mem.out
```
- doing interface when being a argument, it'll move that argument to heap when call that function(interface-converted) like
```go
foo(Reader d){}
foo(SomeReader)
//someReader will be moved to Heap
```
- Some stuff it too large to put in to stack, so it move to heap, also dynamic stuff such as slice that have unknow size as well.
