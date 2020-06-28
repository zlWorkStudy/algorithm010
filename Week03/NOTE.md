### 泛型递归模板
```java
public void recurse(int level, int param) {
    // terminator
    if (level > MAX_VALUE) {
        // process result
        return;
    }
    // process logic
    process(level, param);
    // drill down
    recurse(level + 1, new Param);
    // restore current status if needed
}
```
<br/>

### 分治代码模板
```java
public void divideConquer(int problem, int param) {
    // terminator
    if (level > MAX_VALUE) {
        // process result
        return;
    }
    // prepare data
    int data = prepareData(problem);
    Integer[] splitProblem(problem, data);
    // conquer subProblem
    int subResult1 = divideConquer(splitProblem[0], newparam);
    int subResult2 = divideConquer(splitProblem[1], newparam);
    ...
    
    // process and generate the final result
    int result = processResult(subResult1, subResult2, ...);
    
    // revert the current level status if needed
}
```