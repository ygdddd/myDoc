# 编程相关杂项

## Junit 单元测试

> Junit是一个Java单元测试框架

1. 某个类通常需要测试多个方法，每次测试都需要写到main()方法中
2. 多个功能测试时来回切换操作麻烦
3. Junit支持直接运行一个方法来完成单元测试
4. Junit需要引入相关依赖，在需要测试的方法上添加注解「@Test」

```Java

@Test
public void m1(){
    sout("m1");
}

@Test
public void m2(){
    sout("m2");
}
```

# tank 568

# DB