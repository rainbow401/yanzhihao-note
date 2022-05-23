方法注释

```java
*
 * @author yanzhihao
 * @since $date$
 * @param $param$
 * @return $return$
 **/
    
    
    groovyScript("def result=''; def params=\"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); for(i = 0; i < params.size(); i++) {result+='' + params[i] + ((i < params.size() - 1) ? '\\r\\n ' : '')}; return result", methodParameters())groovyScript("def result=''; def params=\"${_1}\".replaceAll('[\\\\[|\\\\]|\\\\s]', '').split(',').toList(); for(i = 0; i < params.size(); i++) {result+='* @param ' + params[i] + ((i < params.size() - 1) ? '\\n\\b' : '')}; return result", methodParameters()) 
    
    
```

类注释

```JAVA
/**
 * 
 * @author yanzhihao
 * @since ${DATE}
 */
```

