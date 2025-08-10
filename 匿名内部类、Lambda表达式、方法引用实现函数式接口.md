## 函数式接口
- 函数式接口主要指只包含一个抽象方法的接口，如：java.lang.Runnable、java.util.Comparator接口等。
- Java8提供@FunctionalInterface注解来定义函数式接口，若定义的接口不符合函数式的规范便会报错。
- Java8中增加了java.util.function包，该包包含了常用的函数式接口，具体如下：
<img width="754" height="233" alt="image" src="https://github.com/user-attachments/assets/7ebe9735-48ec-4c04-8fc9-c4ff73f6fafb" />

## Lambda表达式
- Lambda 表达式是实例化函数式接口的重要方式，使用 Lambda 表达式可以使代码变的更加简洁紧凑。
- lambda表达式：参数列表、箭头符号->和方法体组成，而方法体中可以是表达式，也可以是语句块。
- 语法格式：(参数列表) -> { 方法体; } 语法格式：(参数列表) -> { 方法体; } - 其中()、参数类型、{} 以及return关键字 可以省略。

## 方法的引用
- 方法引用主要指通过方法的名字来指向一个方法而不需要为方法引用提供方法体，该方法的调用交给函数式接口执行。
- 方法引用使用一对冒号 :: 将类或对象与方法名进行连接，通常使用方式如下：
<img width="494" height="141" alt="image" src="https://github.com/user-attachments/assets/74ce40dd-3521-4f6d-88d7-94da73e67754" />

- 方法引用是在特定场景下lambda表达式的一种简化表示，可以进一步简化代码的编写使代码更加紧凑简洁，从而减少冗余代码。
