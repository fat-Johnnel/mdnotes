## try语句块和异常处理

**throw语句：throw _exception_ ;**

**try语句块：**

```cpp
try{
	program-statements
} catch (exception-declaration) {
	handler-statements
} catch (exception-declaration) {
	handler-statements
}
```

## 标准异常

|exception |
|---|---|
|runtime_error||
|range_error||
|overflow_error|计算上溢|
|underflow_error|计算下溢|
|logic_error|程序逻辑错误|
|domain_error|逻辑错误：参数对应的值不存在|
|invalid_argument|逻辑错误：无效参数|
|length_error|逻辑错误：试图创建一个超出该类型最大长度的对象|
|out_of_range||

### terminate函数

是一个标准库函数，当异常没有被捕捉到时调用，用于终止当前程序的进程