---
title: "Google Java 编程风格指南"
date: 2020-05-06T00:00:00+08:00
lastmod: 2020-05-06T00:00:00+08:00
categories: ["编程"]
tags: ["Java", "Google", "Code Style"]
---

> 转载并翻译自 [https://google.github.io/styleguide/javaguide.html](https://google.github.io/styleguide/javaguide.html)。个人英语水平有限，应以原文档为标准。<!--more-->

## 简介

本文档是 Google Java 语言编程规范的 **完整** 定义。一个 Java 源文件当且仅当遵守本规范时，才可被描述为 Google 风格。

与其它编程规范指南类似，本文档讨论的不仅涉及代码对齐的美观问题，同时还包含其它类型约定和编码规范。然而，本文档侧重于讨论我们普遍遵循的 **硬性规定**，也避免提供那些无法明确执行的建议。

### 术语说明

在本文档中，除非另有说明：

1. _class_ 类 表示 _ordinary class_ 普通的类、_enum class_ 枚举类、_interface_ 接口或 _annotation_ 注解类型。
2. _member_ 成员 表示 _nested class_ 嵌套类、_field_ 字段、_method_ 方法或 _constructor_ 者构造方法，即除初始化方法和注释之外，类的所有最顶层内容。
3. _comment_ 注释 表示 _implementation comments_ 实现注释。我们不使用术语 _documentation comments_，而是使用（在 Java 中）更通用的术语 _Javadoc_。

其它出现在本文档中的术语将另作说明。

### 指南说明

本文档中的示例代是 **不规范** 的。也就是说，虽然示例代码是属于 Google 风格，但并不意味着这是编写优雅代码的唯一方式。示例中代码的风格不应被作为执行的准则。

---

## 源文件准则

### 文件名

源文件的名称包含了区分大小写的（并且是 [唯一](#有且仅有一个顶级类的声明) 的）顶级类的类名和 `.java` 扩展名组成。

### 文件编码：UTF-8

源文件使用 **UTF-8** 编码。

### 特殊字符

#### 空格字符

除了换行符，**ASCII 水平空格字符（0x20）** 是源文件中唯一允许出现的空格字符，这意味着：

1. 字符串和字符字面量中的所有非空格字符都要进行转义。
2. 不允许使用制表符缩进。

#### 特殊转义字符

对任何需转义表示（ `\b`、`\t`、`\n`、`\f`、`\r`、`\"`、`\'`、`\\` ）的 [特殊字符](http://docs.oracle.com/javase/tutorial/java/data/characters.html)，推荐使用转义字符，而不是八进制（例如 `\012` ）或者 Unicode 转义（例如 `\u000a` ）。

#### 非 ASCII 字符

对剩余的非 ASCII 字符，取决于 **更容易阅读和理解** 的方式，选择 Unicode 字符（例如 `∞` ）或等价的 Unicode 转义字符（例如 `\u221e` ），并且强烈反对在字符串和注释之外使用 Unicode 转义字符。

> **提示**：在使用 Unicode 转义字符的情况下，或者偶尔使用实际的 Unicode 字符时，添加解释性的注释是非常有帮助的。

例如：

| Example                                                  | Discussion                                         |
| -------------------------------------------------------- | -------------------------------------------------- |
| `String unitAbbrev = "μs";`                              | 最好：没有注释也十分清晰                           |
| `String unitAbbrev = "\u03bcs"; // "μs"`                 | 允许：但没理由这么做                               |
| `String unitAbbrev = "\u03bcs"; // Greek letter mu, "s"` | 允许：但比较笨拙和易出错                           |
| `String unitAbbrev = "\u03bcs";`                         | 较差：可读性太差                                   |
| `return '\ufeff' + content; // byte order mark`          | 很好：转义字符用于非打印字符时，注释是非常有必要的 |

> **提示**：不要担心因为一些程序可能不能正确地处理非 ASCII 字符，而使你的代码可读性变差。如果真的发生这种情况，那程序会直接 **报错**，并需要被 **修复**。

---

## 源文件结构

源文件按以下 **顺序** 包括：

1. License 或者 Copyright（如果需要的话）
2. Package 语句
3. Import 语句
4. 有且只有一个的顶级 Class

以上每个部分间隔 **一个空行**。

### License 或者 Copyright 信息

如果文件中包含许可证和版权信息，应当至于此处。

### Package 语句

Package 语句不允许换行。单行字符限制（ [列限制：100](#列限制100) 章节）不适用于 Package 语句。

### Import 语句

#### 不允许通配符

**不允许** 使用静态或者其它形式的 **通配符导入**。

#### 不允许换行

Import 语句 **不允许** 换行。单行字符限制（ [列限制：100](#列限制100) 章节）不适用于 Import 语句。

#### 顺序和间隔

Import 语句应按以下方式排序：

1. 所有静态导入归一组。
2. 所有非静态导入归一组。

如果同时存在静态导入和非静态导入，则应使用空行分隔它们。除此之外，在 Import 语句中不允许使用其它空行。

每组中的 Import 语句以 ASCII 编码顺序先后出现。（**注意**：因为 `.` 符号的 ASCII 编码排在 `;` 符号之前，所以这与单纯的按 ASCII 编码排序略有不同。）

#### 不允许类的静态导入

静态内部类以常规方式导入，而不是使用静态导入。

### Class 定义

#### 有且仅有一个顶级类的声明

每个顶级类都定义在它们的源文件中。

#### 类内容顺序

类的成员和初始化方法的顺序对代码可读性有着很重要的影响。然而，对此并没有一个统一正确的标准：不同的类可能有不同的排序内容的方式。

重要的是，每个类都应该使用该类的维护者可以解释清楚的 **逻辑排序**。例如，新的方法不是习惯性地添加到类的最后，因为「按时间顺序添加」并不是一种逻辑顺序。

##### 方法重载：不应被分离

当一个类拥有多个构造方法或者多个同名方法时，它们应该按顺序出现，中间没有其它代码（甚至是私有成员）。

---

## 格式化

**术语说明**：_block-like construct_ 块状结构 指类或者普通方法或者构造方法的主体。注意，在后续 [数组初始化](#数组初始化可以写成块状结构) 章节中，任何数组的初始化可以选择被认为是一个块状结构。

### 花括号

#### 花括号在被需要的地方使用

花括号被使用于 `if`、`else`、`for`、`do`、`while` 语句，即使它们的语句主体是空或者仅包含一条语句。

#### 非空语句块：K & R 风格

对于非空语句块和块状结构，花括号的使用方式遵循 Kernighan & Ritchie 风格（[Egyptian brackets](http://www.codinghorror.com/blog/2012/07/new-programming-jargon.html)）：

- 左花括号之前不能换行。
- 左花括号之后换行。
- 右花括号之前换行。
- 仅在右花括号结束一条语句或者方法 / 构造方法 / 类的主体时，右花括号之后才换行。例如 `else` 和逗号之后的花括号不能换行。

例如：

```java
return () -> {
  while (condition()) {
    method();
  }
};

return new MyClass() {
  @Override public void method() {
    if (condition()) {
      try {
	something();
      } catch (ProblemException e) {
	recover();
      }
    } else if (otherCondition()) {
      somethingElse();
    } else {
      lastThing();
    }
  }
};
```

关于枚举类的一些特殊情况，将在 [枚举类](#枚举类) 章节说明。

#### 空语句块：可以简洁

一个空的语句块或者块状结构可以遵循 K & R 风格（正如在 [非空语句块](#非空语句块k--r-风格) 章节所中描述的）。或者，当它不是 _multi-block statement_ 多块语句（一个包含多块的语句，例如：`if / else`、`try / catch / finally` ）一部分的时候，可以在左花括号开始之后立即使用右花括号结束，`{}` 之中不包含任何字符或者换行符。

例如：

```java
// This is acceptable
void doNothing() {}

// This is equally acceptable
void doNothingElse() {
}
```

```java
// This is not acceptable: No concise empty blocks in a multi-block statement
try {
  doSomething();
} catch (Exception e) {}
```

### 块缩进：+2 个空格

每当新写一个语句块或者块状结构时，增加 2 个空格的缩进。当语句块结束时，返回至上一级别的缩进。语句块的缩进规则适用于所有代码和注释。（代码示例请见 [非空语句块：K & R 风格](#非空语句块k--r-风格) 章节）

### 一条语句占一行

每条语句的最后都有换行符。

### 列限制：100

Java 代码的列限制为 100 个字符。这儿的「字符」意味着任意的 Unicode 码位。除非另有说明，任何超过此限制的代码行都必须被换行，正如在 [换行](#换行) 章节中所描述的。

> 每个 Unicode 码位都算作一个字符，不论它显示得更宽或者更窄。例如，如果使用 [全角字符](https://en.wikipedia.org/wiki/Halfwidth_and_fullwidth_forms) 的话，为了遵守这条严格的要求，可以选择提前换行。

特殊情况：

- 无法遵守列限制的代码行（例如 Javadoc 中的很长的 URL，或者 JSNI 中很长的方法引用）。
- Package 语句和 Import 语句（请见 [Package 语句](#package-语句) 和 [Import 语句](#import-语句) 章节）。
- 注释中可能直接被复制粘贴执行的 Shell 命令。

### 换行

**术语说明**：将原本可以合法写在一行的代码拆分成多行，这种行为称作 _line-wrapping_ 换行。

没有全面和明确的准则，可以准确描述每种场景下该如何进行换行。对于同一段代码，通常会有多种有效可行的换行方法。

> **注意**：换行的典型原因是为了避免代码超出了列数的限制，不过即使符合列限制的一行代码，也可以依据作者的决定而换行。

> **提示**：提取方法或者局部变量或许可以避免换行的问题。

#### 在何处换行

换行指令的主要内容是：倾向于在 **较高语法级别** 处中断一行代码。并且：

1. 当一行代码的中断发生在 _non-assignment_ 非赋值运算符时，需要在该运算符之前换行。（注意这与其它语言的 Google 编程风格不同，例如 C++ 和 JavaScript）
   - 这条规则也适用于以下「类似操作符」的符号：
     - 点分隔符（ `.` ）
     - 方法引用中的两个冒号（ `::` ）
     - 类型约束中的 & 符号（ `T <extends Foo Foo & Bar>` ）
     - 异常捕获中的 | 符号（ `catch (FooException | BarException e)` ）
2. 当一行代码的中断发生在 _assignment_ 赋值运算符时，需要在该运算符之后换行，但在之前换行也可以接受。
   - 这条规则也适用于 foreach 语句中「类似赋值操作符」的冒号。
3. 方法或者构造方法的名称紧随着与它相连的开括号 `(`。
4. 逗号 `,` 紧随着它之前的内容。
5. 在 lambda 语句中，和箭头符号相邻的那行代码永远不会换行，除非 lambda 语句的主体仅是一个不带括号的表达式，并且能紧随着 lambda 语句的箭头立即出现的情况下。例如：

   ```java
   MyLambda<String, Long, Object> lambda =
       (String label, Long value, Object obj) -> {
          ...
       };

   Predicate<String> predicate = str ->
       longExpressionInvolving(str);
   ```

> **注意**：换行的主要目的是为了拥有清晰的代码，总代码的行数 **不必** 是越少越好的。

#### 换行缩进至少 +4 个空格

进行换行时，第一行（在连续换行的多行代码中）之后的每行代码至少比之前的那行多缩进 +4 个空格。

当进行连续换行时，代码的缩进可以根据实际需要超过 +4 个空格。一般来说，当且仅当两行代码以平级的语法元素开头时，它们才会拥有相同级别的缩进。

在 [水平对齐](#水平对齐从不要求) 章节中介绍了不建议使用数量不确定的空格来与上一行代码中的某些单词（token）对齐。

### 空格

#### 垂直空格

单个空行总是出现在以下情况中：

1. 类中连续的成员或初始化方法之间，包括：字段、构造方法、方法、内部类、静态初始化代码块、实例初始化代码块。
   - **特殊情况**：两个连续字段（它们之间没有其它代码）之间的空行是可选的。可以根据实际需要，用空行去创建字段之间的 _logical groupings_ 逻辑分组。
   - **特殊情况**：枚举常量之间的空行在 [枚举类](#枚举类) 章节中介绍。
2. 本文档其它章节中所要求的（例如 [源文件结构](#源文件结构) 章节和 [Import 语句](#import-语句) 章节）

单个空行也可以出现在任何需要提高代码可读性的地方，例如在将代码组织成一小块逻辑的语句之间。不鼓励也不反对将单个空行出现在类的第一个成员或初始化方法的之前，或者最后一个成员或初始化方法的之后。

多个连续的空行是允许的，但这不是本文档所要求的（或者是鼓励的）。

#### 水平空格

除了编程语言或者编程规范的要求之外，除了在字面量、注释和 Javadoc 之外，单个 ASCII 空格 **仅** 在以下位置出现：

1. 分隔任何的保留关键字，例如 `if`、`for`、`catch` ，与它们之后的开括号 `(`。
2. 分隔任何的保留关键字，例如 `else`、`catch`，与它之前的右花括号 `}`。
3. 任何的左花括号 `{` 之前，除了以下两种特殊情况：
   - `@SomeAnnotation({a, b})`（没有使用空格）
   - `String[][] x = {{"foo"}};`（ `{{` 之间不需要使用空格，请见往下的第 8 条规则）
4. 任何的二元或三元操作符的两侧。这条规则也适用于以下「类似操作符」的符号：

   - 类型约束中的 & 符：`<T extends Foo & Bar>`
   - 捕获多个异常中的 | 符：`catch (FooException | BarException e)`
   - foreach 语句中的冒号（ `:` ）
   - lambda 表达式中的箭头：`(String str) -> str.length()`

     但除了：

   - 方法引用中的两个冒号（ `::` ），写法类似于 `Object::toString`
   - 点分隔符（ `.` ），写法类似于 `Object.toString()`

5. 在 `,:;` 符号或者类型转换的闭括号 `)` 之后。
6. 在行尾注释中开头的双斜杠（ `//` ）两侧。此处多个空格也是允许的，但不是必须的。
7. 在类型和变量的定义之间：`List<String> list`。
8. 在数组初始化的两个花括号的内侧。这条规则不是必须的。
   - `new int[] {5, 6}` 和 `new int[] { 5, 6 }` 都是有效的
9. 在注解和 `[]` 或 `...` 之间。

这条规则不会被解释为要求或者禁止在行首或者行尾使用额外的空格，它只针对于行内的空格。

#### 水平对齐：从不要求

**术语说明**：_Horizontal alignment_ 水平对齐 是为了使某些单词（token）出现在上一行代码中的另一些单词的正下方，从而在代码中添加额外的数量不确定的空格的做法。

这种做法是允许的，但在 Google 编程规范中却不是必须的。甚至不要求在已经水平对齐的地方继续保持水平对齐。

如下是一个首先未对齐，然后再对齐的例子：

```java
private int x; // this is fine
private Color color; // this too

private int   x;      // permitted, but future edits
private Color color;  // may leave it unaligned
```

> **提示**：水平对齐有助于阅读代码，但却难以日后维护。考虑这样一种情况：日后的改动需要调整一行代码，这个改动可能会破坏原本令人愉悦的代码格式，不过这种改动是 **允许** 的。（IDE）通常会提示编码人员（也许是你自己）调整附近代码行中的空格，但这可能会触发一系列的代码格式化，于是这个一行代码的改动就导致了一个「范围爆炸」。在最坏的情况下，这可能会导致大量毫无意义的工作。在最好的情况下，这依然会混淆代码版本中的历史信息、减缓代码评审的速度、加速代码合并的冲突。

### 分组括号：推荐

只有当开发人员和评审人员都同意，没有分组括号时代码的阅读者不会想当然地错误理解，或者分组括号不会更有助于阅读代码的情况下，才可以省略可选的分组括号。假定每个阅读者都熟记整个 Java 运算符的优先级是不合理的。

### 特定结构

#### 枚举类

每个枚举常量的逗号之后可以选择性地换行，同时也允许添加额外的空行（通常只有一个）。以下是一种可能的例子：

```java
private enum Answer {
  YES {
    @Override public String toString() {
      return "yes";
    }
  },

  NO,
  MAYBE
}
```

没有方法和注释的枚举常量可以写成数组初始化的方式（请见 [数组初始化](#数组初始化可以写成块状结构) 章节）。

```java
private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }
```

由于枚举类也是类，因此所有对于类的格式化规则也适用于枚举类。

#### 变量声明

##### 每次声明一个变量

每次变量声明（字段或局部变量）只声明一个变量：例如 `int a, b;` 形式的变量声明是不允许的。

**特殊情况**：可以在 `for` 循环的头部中声明多个变量。

##### 在需要时声明

局部变量 **不是** 习惯性地声明在它们所属的代码块或者块状结构的起始位置。相反地，局部变量声明在它们第一次被使用的地方（在合理范围之内），这样做是为了最小化局部变量的作用域。局部变量声明通常具有初始值，或者会在声明之后立即初始化。

#### 数组

##### 数组初始化：可以写成块状结构

任何数组的初始化可以选择写成「类似块状结构」的格式。例如，以下例子都是合法的（不全）：

```java
new int[] {           new int[] {
  0, 1, 2, 3            0,
}                       1,
                        2,
new int[] {             3,
  0, 1,               }
  2, 3
}                     new int[]
                          {0, 1, 2, 3}
```

##### 拒绝使用 C 语言式的声明

方括号是类型而非变量的一部分：`String[] args` 是合法的，`String args[]` 是非法的。

#### switch 语句

**术语说明**： switch 语句块的花括号内是一个或多个 _statement groups_ 语句组。每个语句组都包含了一个或多个 switch 标签（ `case Foo:` 或者 `default:` ），switch 标签之后跟随着一条或多条语句（或者，对于最后一个语句组，它可以包含零条或多条语句）。

##### 缩进

和其它任何语句块一样，switch 语句块中的内容缩进 +2 个空格。

switch 标签之后会有一个换行，并增加 +2 个缩进级别，就好像是在开始一段新的代码块。之后的 switch 标签返回到上一个缩进级别，好像是结束了一段代码块。

##### 匹配失败：需要注释

在 switch 语句块中，每个语句组会突然终止（使用 `break`、`continue`、`return` 关键字、或者抛出异常），或者会有注释标记着程序可能会继续执行到下一个语句组。任何可以表达匹配失败意思的注释都是可行的（例如典型的 `// fall through` ）。在最后一个语句组中，这个特殊的注释不是必须的。例如：

```java
switch (input) {
  case 1:
  case 2:
    prepareOneOrTwo();
    // fall through
  case 3:
    handleOneTwoOrThree();
    break;
  default:
    handleLargeNumber(input);
}
```

注意在 `case 1:` 之后不需要注释，只有在语句组之后才需要使用注释。

##### default 分支是必须的

每个 switch 语句都包含了一个 `default` 语句组，即使它不包含任何代码。

**特殊情况**：如果 `enum` 类型的 switch 语句明确包含了覆盖所有可能性的枚举值，那么它可以省略 `default` 语句组。如果遗漏了任何情况，IDE 和静态分析工具可以发出警告。

#### 注解

用于类、方法和构造方法的注解出现在文档之后，并且每个注解都写在它们所属的行上（这意味着，每个注解占用一行）。这些注解所占用的行不构成换行（请见 [换行](#换行) 章节），因此缩进级别不会增加。例如：

```java
@Override
@Nullable
public String getNameIfPresent() { ... }
```

**特殊情况**：一个单独的没有参数的注解可以相反地和方法签名的第一行一起出现，例如：

```java
@Override public int hashCode() { ... }
```

用于字段的注解也出现在文档之后，但这种情况下，多个注解（可能会有参数）可以写在同一行上。例如：

```java
@Partial @Mock DataLoader loader;
```

用于参数、局部变量和类型的注解没有特定的格式化规则。

#### 注释

本章节介绍 _implementation comments_ 实现注释。Javadoc 将在 [Javadoc](#javadoc) 章节单独介绍。

任何换行符之前都可以有任意数量的跟随着实现注释的空格。这样的注释使该行成为非空白的。

##### 注释块样式

注释块与它周围的代码拥有相同的缩进级别。注释块可以是 `/* ... */` 样式或 `// ...` 样式。对于多行注释 `/* ... */`，后续的行中必须以 `*` 开头并且与上一行中的 `*` 保持对齐。

```java
/*
 * This is          // And so           /* Or you can
 * okay.            // is this.          * even do this. */
 */
```

注释不会被包含在由星号或者其它字符绘制的框中。

> **提示**：当写多行注释的时候，如果你希望能为了在必要的时候重新包装每行代码而自动格式化（段落样式），那么应使用 `/* ... */` 样式。大多数格式化程序不会重新包装 `// ...` 样式中的注释块。

#### 修饰符

类和成员如果存在修饰符的话，应以 Java 语言规范建议的顺序出现：

`public protected private abstract default static final transient volatile synchronized native strictfp`

#### 数字字面量

`long` 数值的整数字面量会使用大写的 `L` 后缀，永远不要使用小写（避免与数字 `1` 混淆）。例如，使用 `3000000000L` 而不是 `3000000000l`。

---

## 命名

### 适用于所有标识符的通用规则

标识符只允许使用 ASCII 字母和数字，并且在少数情况中可以使用下划线。因此，每个有效的标识符都可以由正则表达式 `\w+` 匹配。

在 Google 风格中，**不** 会使用特殊的前缀或后缀，例如，这些命名不是 Google 风格的：`name_`、`mName`、`s_name` 和 `kName`。

### 各种类型的标识符的规则

#### 包名

包名全是小写的，连续的单词只需要简单地串联在一起（不使用下划线）。例如，使用 `com.example.deepspace`，而不是 `com.example.deepSpace` 或者 `com.example.deep_space`。

#### 类名

类名以 [大骆峰](#骆驼峰形式定义) 方式编写。

类名通常是名词或者名词短语。例如 `Character` 或者 `ImmutableList`。接口名可能也是名词或名词短语（例如 `List` ），但有时候也可能是形容词或形容词短语（例如 `Readable` ）。

注解类型的命名没有特定的规则或者完善的约定。

测试类以被测试类的名称开头，并且以 `Test` 结尾。例如 `HashTest` 或者 `HashIntegrationTest`。

#### 方法名

方法名以 [小骆峰](#骆驼峰形式定义) 方式编写。

方法名通常是动词或者动词短语。例如 `sendMessage` 或者 `stop`。

下划线可能为了分离命名上的逻辑组件，而出现在 Junit 测试方法名中，每个组件都以 [小骆峰](#骆驼峰形式定义) 方式编写。一个典型的模式是 `<methodUnderTest>_<state>`，例如 `pop_emptyStack`。测试方法的命名没有一种唯一正确的方式。

#### 常量名

常量使用 `CONSTANT_CASE` 的格式命名：全大写，单词之间以下划线分隔。但常量究竟意味着什么？

常量是 static final 修饰的字段，常量的内容是深不可变的（deeply immutable)，并且常量的方法是没有副作用的。这包括了原始类型、字符串、不可变类型、不可变类型的不可变集合。如果实例的任何外在状态是可变的，那它就不属于常量。仅仅保证实例引用的不可变属性是不够的。例如：

```java
// Constants
static final int NUMBER = 5;
static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
static final ImmutableMap<String, Integer> AGES = ImmutableMap.of("Ed", 35, "Ann", 32);
static final Joiner COMMA_JOINER = Joiner.on(','); // because Joiner is immutable
static final SomeMutableType[] EMPTY_ARRAY = {};
enum SomeEnum { ENUM_CONSTANT }

// Not constants
static String nonFinal = "non-final";
final String nonStatic = "non-static";
static final Set<String> mutableCollection = new HashSet<String>();
static final ImmutableSet<SomeMutableType> mutableElements = ImmutableSet.of(mutable);
static final ImmutableMap<String, SomeMutableType> mutableValues =
    ImmutableMap.of("Ed", mutableInstance, "Ann", mutableInstance2);
static final Logger logger = Logger.getLogger(MyClass.getName());
static final String[] nonEmptyArray = {"these", "can", "change"};
```

常量名通常是名词或者名词短语。

#### 非常量字段名

非常量字段名（静态或者其它形式）以 [小骆峰](#骆驼峰形式定义) 方式编写。

非常量字段名通常是名词或者名词短语。例如：`computedValues` 或者 `index`。

#### 参数名

参数名以 [小骆峰](#骆驼峰形式定义) 方式编写。

public 方法中应该避免使用一个字符的参数名。

#### 局部变量名

局部变量名以 [小骆峰](#骆驼峰形式定义) 方式编写。

即使是 final 和不可变的，局部变量也不被认为是常量，并且不应该以常量的风格命名。

#### 类型变量名

类型变量名以如下两者之一方式编写：

- 一个大写字母，可选地跟随着一个数字（例如 `E`，`T`，`X`，`T2` ）。
- 以类名的方式命名（请见 [类名](#类名) 章节），跟随着大写字母 T（例如：`RequestT`，`FooBarT` ）。

### 骆驼峰形式：定义

有时候会有多种合理的方式用于将英语短语转换为驼峰形式，例如当英语短语里出现首字母缩略词或者不寻常结构的单词（例如「iOS」和「IPv6」）时。为了提高代码的可预测性， Google 编程风格指定了以下（近乎）明确的方案。

从命名的文字构成开始：

1. 将短语转换为纯 ASCII 编码，并且删除任何的撇号。例如，「Müller's algorithm」可以转换为「Muellers algorithm」。
2. 以空格和任何剩余的标点符号（通常是连字符），将短语划分为单词。
   - 推荐：如果任何单词在普遍用法中已经具有常规的驼峰形式，那就将它分解成它的组成部分（例如，将「AdWords」变成「ad words」）。注意例如「iOS」之类的单词本身并不是驼峰形式。它违反了一些约定，因此这条规则并不适用。
3. 现在将所有内容转换为小写（包括首字母缩略词），然后只将以下内容的第一个字符转换为大写：
   - ... 每个单词，用于产生大驼峰形式，或者
   - ... 除了第一个以外的每个单词，用于产生小驼峰形式
4. 最后，将所有单词合并为一个标识符。

注意，原始单词的大小写几乎完全被忽略。例如：

| Prose form              | Correct                              | Incorrect         |
| ----------------------- | ------------------------------------ | ----------------- |
| "XML HTTP request"      | `XmlHttpRequest`                     | `XMLHTTPRequest`  |
| "new customer ID"       | `newCustomerId`                      | `newCustomerID`   |
| "inner stopwatch"       | `innerStopwatch`                     | `innerStopWatch`  |
| "supports IPv6 on iOS?" | supportsIpv6OnIos                    | supportsIPv6OnIOS |
| "YouTube importer"      | YouTubeImporter<br>YoutubeImporter\* |                   |

\* 表示可以接受，但不推荐的。

> **注意**：一些带连字符的单词在英语中含糊不清的：例如「nonempty」和「non-empty」都是正确的，所以方法名 `checkNonempty` 和 `checkNonEmpty` 也都是正确的。

---

## 编程实践

### `@Override`：总是使用

只要是合法的，方法总会被标记 `@Override` 注解。这包括了一个类的方法重写了父类的方法、一个类的方法实现了接口的方法、一个接口的方法重新定义了父接口的方法。

**特殊情况**：当父类方法是 `@Deprecated` 的时候，`@Override` 可以省略。

### 捕获异常：不能忽略

除非另有说明，对捕获的异常不做任何响应是很少正确的。（典型的响应是打印日志，或者如果打印日志是「不可能」的，就重新抛出一个作为 `AssertionError` 的异常。）

当对 catch 语句块中的任何内容不做处理确实是合适的时候，应该在注释中说明正当的理由。

```java
try {
  int i = Integer.parseInt(response);
  return handleNumericResponse(i);
} catch (NumberFormatException ok) {
  // it's not numeric; that's fine, just continue
}
return handleTextResponse(response);
```

**特殊情况**：在测试代码中，如果捕获的异常名称是 `expected` 或者以此为开头，那么它可以不加注释地被忽略。以下是一个很常见的惯用语法，用于确认被测试的代码确实抛出了预期类型的异常，所以此处的注释是不必要的。

```java
try {
  emptyStack.pop();
  fail();
} catch (NoSuchElementException expected) {
}
```

### 静态成员：限定使用类

当对静态类的成员的引用必须是有所限定的时候，那它是以该类的名称作为限定，而不是该类的类型的引用或者表达式。

```java
Foo aFoo = ...;
Foo.aStaticMethod(); // good
aFoo.aStaticMethod(); // bad
somethingThatYieldsAFoo().aStaticMethod(); // very bad
```

### Finalizers：禁用

重写 `Object.finalize` 方法是 **非常罕见** 的。

> **提示**：禁止这么做。如果你真的需要，请先仔细阅读和理解 [《Effective Java》第七章](http://books.google.com/books?isbn=8131726592) —— 避免使用 Finalizer，然后禁止这么做。

---

## Javadoc

### 格式化

#### 一般形式

Javadoc 语句块的基本格式如这个例子所示：

```java
/**
 * Multiple lines of Javadoc text are written here,
 * wrapped normally...
 */
public int method(String p1) { ... }
```

... 或者如这个单行例子所示：

```java
/** An especially short bit of Javadoc. */
```

基本格式总是可以接受的。当整个 Javadoc 语句块（包括注释标记）可以写在一行的时候，单行格式可以被替换。注意这仅适用于没有类似于 `@return` 之类块标签的情况。

#### 段落

一个空行 -- 这意味着，仅包含用于对齐的前导星号（\*）的行 -- 会出现在段落之间，和块标签组（如果有的话）之前。除了第一个以外的每个段落，在第一个单词之前有一个 `<p>` 标签，标签与单词之间没有空格。

#### 块标签

使用到的任何标准「块标签」按以下的顺序出现 `@param`、`@return`、`@throws`、`@deprecated`，并且这四种类型的块标签不会与空的描述一起出现。当块标签不能写在一行的时候，后续的行从 `@` 的位置缩进四个（或者更多）空格。

### 摘要片段

每个 Javadoc 语句块以一个简短的 **摘要片段** 开头。这个片段非常重要：它是在某些情况下唯一可以出现的文本，例如在类和方法的索引中。

这是一个片段 -- 是名词短语或者动词短语，而不是一个完整的句子。它 **不** 以 `A {@code Foo} is a...` 或者 `This method returns...` 开头，也不会形成例如 `Save the record.` 这样的祈使句。然而，这个片段是用大写字母书写的并且会有标签符号，就好像它是个完整的句子。

> **提示**：一个常见的错误是用以下形式编写简单的 Javadoc `/** @return the customer ID */`。这是不正确的，并且应该被修正为 `/** Returns the customer ID. */`。

### 在何处使用 Javadoc

至少，Javadoc 应该出现在每个 `public` 类，和这个类的每个 `public` 或者 `protected` 成员，但除了以下提及的几个例外。

额外的 Javadoc 内容也可以出现，正如在 [非必需的 Javadoc](#非必需的-javadoc) 章节中所描述的。

#### 特殊情况：自解释的方法

对于类似 `getFoo` 之类的「简单、明显」的方法，Javadoc 是可选的，在这种情况下，除了「Returns the foo」也确实真的没什么值得好说了。

> **重要**：引用这个特殊情况来证明省略典型的阅读者可能需要知道的相关信息是不合适的。例如，对于名为 `getCanonicalName` 的方法，一个典型的阅读者可能不知道术语「canonical name」是什么意思，所以不要省略它的文档（以它只会说 `/** Returns the canonical name. */` 的理由）。

#### 特殊情况：重写

Javadoc 不会总是出现在一个重写了父类方法的方法中。

#### 非必需的 Javadoc

其它的类和成员根据实际需要或者期望来编写 Javadoc。

每当使用实现注释来定义一个类或者成员的总体目的或者行为的时候，这个注释改为用 Javadoc 来编写（使用 `/**` ）。

非必需的 Javadoc 内容不是严格要求遵守 [段落](#段落) 章节、[块标签](#块标签) 章节以及 [摘要片段](#摘要片段) 章节的格式化规则，尽管这当然是推荐的。
