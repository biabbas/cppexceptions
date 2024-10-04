Important notes
1. Call site headers
```
void foo() {
    L0:
        try {
            do_something();
    L1:
        } catch (const Exception1& ex) {
            ...
        } catch (const Exception2& ex) {
            ...
        } catch (const ExceptionN& ex) {
            ...
        } catch (...) {
        }
    L2:
}
```

`lp`: the offset from the start of the function where the landing pad starts. The value of lp for the following example would be `L1 - addr_of(foo)`

`action`: an offset into an action table. This is used to run the cleanup actions while unwinding the stack. We haven't reached this point yet, we can ignore it for now.

`start`: the offset from the start of the function where the try block begins: in the example this would be `L0 - addr_of(foo)`
`len`: the length of the try block. On the example this would be `L1 - L0`

The interesting fields now are start and len: in a function with multiple try/catch blocks we can know whether we should handle an exception by checking if the instruction pointer for the current frame is between `start` and `start + len`.

------------------------------------------------------------------------------------------------------------------------------------------

