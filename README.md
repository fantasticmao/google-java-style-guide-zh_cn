# Google Java 编程风格指南

> 转载并翻译自 [https://google.github.io/styleguide/javaguide.html](https://google.github.io/styleguide/javaguide.html)。个人英语水平有限，应以原文档为标准。

## 简介

本文档是 Google Java 语言编程规范的 **完整** 定义。一个 Java 源文件当且仅当遵守本规范时，才可被描述为 Google 风格。

与其它编程规范指南类似，本文档讨论的不仅涉及代码对齐的美观问题，同时还包含其它类型约定和编码规范。然而，本文档侧重于讨论我们普遍遵循的 **硬性规定**，也避免提供那些无法明确执行的建议。

### 术语说明

在本文档中，除非另有说明：

1. _class_ 类 表示 _ordinary class_ 普通的类、_enum class_ 枚举类、_interface_ 接口或 _annotation_ 注解类型。
2. _member_ 成员 表示 _nested class_ 嵌套类、_field_ 属性、_method_ 方法或 _constructor_ 者构造方法，即除初始化方法和注释之外，类的所有最顶层内容。
3. _comment_ 注释 表示 _implementation comments_ 实现注释。我们不使用术语 _documentation comments_，而是使用（在 Java 中）更通用的术语 _javadoc_。

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

对任何需转义表示（`\b`、`\t`、`\n`、`\f`、`\r`、`\"`、`\'`、`\\`）的 [特殊字符](http://docs.oracle.com/javase/tutorial/java/data/characters.html)，推荐使用转义字符，而不是八进制（例如 `\012` ）或者 Unicode 转义（例如 `\u000a` ）。

#### 非 ASCII 字符

对剩余的非 ASCII 字符，取决于 **更容易阅读和理解** 的方式，选择 Unicode 字符（例如 `∞` ）或等价的 Unicode 转义字符（例如 `\u221e`），并且强烈反对在字符串和注释之外使用 Unicode 转义字符。

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

Package 语句不允许换行。单行字符限制（ [列限制：100](#列限制：100) 章节）不适用于 Package 语句。

### Import 语句

#### 不允许通配符

**不允许** 使用静态或者其它任何方式的 **通配符导入**。

#### 不允许换行

Import 语句 **不允许** 换行。单行字符限制（ [列限制：100](#列限制：100) 章节）不适用于 Import 语句。

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

**术语说明**：_block-like construct_ 块状结构 指类或者普通方法或者构造方法的主体。注意，在后续 [数组初始化](#数组初始化：可以写成块状结构) 章节中，数组的初始化语句可能被认为是一个块状结构。

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

一个空的语句块或者块状结构可以遵循 K & R 风格（正如在 [非空语句块](#非空语句块：K-&-R-风格) 章节所中描述的）。或者，当它不是 _multi-block statement_ 多块语句（一个包含多块的语句，例如：`if / else`、`try / catch / finally`）一部分的时候，可以在左花括号开始之后立即使用右花括号结束，`{}` 之中不包含任何字符或者换行符。

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

每当新写一个语句块或者块状结构时，增加 2 个空格的缩进。当语句块结束时，返回至上一级别的缩进。语句块的缩进规则适用于所有代码和注释。（代码示例请见 [非空语句块：K & R 风格](#非空语句块：K-&-R-风格) 章节）

### 一个语句占一行

每个语句的最后都有换行符。

### 列限制：100

Java 代码的列限制为 100 个字符。这儿的「字符」意味着任意的 Unicode 码位。除非另有说明，超过此限制的每行代码都必须被换行，正如在 [换行](#换行) 章节中描述的。

> 每个 Unicode 码位都算作一个字符，不论它显示得更宽或者更窄。例如，如果使用 [全角字符](https://en.wikipedia.org/wiki/Halfwidth_and_fullwidth_forms) 的话，为了遵守这条严格的要求，可以选择提前换行。

特殊情况：

- 无法遵守列限制的代码行（例如 Javadoc 中的很长的 URL，或者 JSNI 中很长的方法引用）。
- Package 语句和 Import 语句（请见 [Package 语句](#Package-语句) 和 [Import 语句](#Import-语句) 章节）。
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
     - 点分隔符（`.`）
     - 方法引用中的两个冒号（`::`）
     - 类型约束中的 & 符号（`T <extends Foo Foo & Bar>`）
     - 异常捕获中的 | 符号（`catch (FooException | BarException e)`）
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

#### 换行缩进至少 4 个空格

进行换行时，第一行（在连续换行的多行代码中）之后的每行代码至少比之前的那行多缩进 4 个空格。

当进行连续换行时，代码的缩进可以根据实际需要超过 4 个空格。一般来说，当且仅当两行代码以平级的语法元素开头时，它们才会拥有相同水平的缩进。

在 [水平对齐](#水平对齐：从不要求) 章节中阐述了不建议使用数量不确定的空格来与上一行代码中的某些单词（token）对齐。

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

没有方法和注释的枚举常量可以写成数组初始化的方式（请见 [数组初始化](数组初始化：可以写成块状结构) 章节）。

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

数组初始化可以写成「类似块状结构」的格式。例如，以下例子都是合法的（不全）：

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

**术语说明**： switch 语句的花括号里是一个或多个 _statement groups_ 语句组。每个语句组包含一个或多个 switch 标签（`case Foo:` 或 `default:`）,且标签之后是一个或多个常规语句（最后一个语句组可包含零个或多个 switch 标签）。

##### 缩进

与其它所有语句块一样，switch 语句块中的内容缩进 2 个空格。

开始 switch 标签之后应换行并增加 2 个缩进级，如语句块刚开始一样。结束 switch 标签之后应返回上级缩进，如语句块结束一样。

##### 下降执行的注释

在 switch 语句中，每个语句组都应突然终止（使用 `break`、`continue`、`return`、或者抛出异常），或者以注释形式标记指出此处可能会执行到下一个语句组。所有能传达下降执行意思的注释都是可行的（如典型的 `// fall through`）。在最后一个分组中，这个特殊的注释不是必须的。例如：

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

注意 `case 1:` 之后没有注释，只有在语句组的结尾时才可这样。

##### default 语句组是必须的

每个 switch 语句都必须包含 `default` 语句组，即使它不包含任何代码。

**注意**：如果 `enum` 类型的 switch 语句组覆盖了所有的可能性，那么 `default` 语句组可以省略。IDE 和大多数语法分析器都支持这种提示。

#### 注解

注解适用于紧随 Javadoc 之后的类、方法和构造器，且每个注解占有一行。这些行不构成换行（ 4.5 [换行](#换行) 章节），所以无需增加缩进。例如：

```java
@Override
@Nullable
public String getNameIfPresent() { ... }
```

特殊情况：单个无参数的注解可以和行首的修饰符放在一起，例如：

```java
@Override public int hashCode() { ... }
```

注解也适用于紧随 Javadoc 之后的字段，且这种情况下，多个注解（允许有参数）可以写在同一行。例如：

```java
@Partial @Mock DataLoader loader;
```

注解、局部变量或类型没有具体的格式化要求。

#### 注释

本章节介绍 _implementation comments_ 实现注释。Javadoc 在 7 [Javadoc](#Javadoc) 章节单独介绍。

换行符之前可以有任意空格，之后是实现注释。这样的注释应使用非空行。

##### 注释块风格

注释块与它关联的代码缩进相同。可以是 `/* ... */` 风格或 `// ...` 风格。对于多行的 `/* ... */` 注释，每行必须以 `*` 开始且与上一行的 `*` 对齐。

```java
/*
 * This is          // And so           /* Or you can
 * okay.            // is this.          * even do this. */
 */
```

注释不应被星号或者其它字符绘制的框所包围。

> 写多行注释的时候，如果你想在必要时能自动对齐代码，那么应使用 `/* ... */`。绝大多数编辑器不会自动对齐 `// ...` 风格的注释。

#### 修饰符

当类和成员拥有修饰符时，应以 Java 语言规范建议的顺序出现：
`public protected private abstract default static final transient volatile synchronized native strictfp`

#### 字面量数值

`long` 长整型字面量带有 `L` 后缀，且永远不为小写（避免混淆数字 `1`）。例如 `3000000000L` 而不是 `3000000000l`。

---

## 命名

### 标识符通用规则

标识符只允许使用 ASCII 字母和数字，且在少数情况中可以使用下划线。因此，每个有效的标识符都可以由正则表达式 `\w+` 匹配。

在 Google 风格中，特殊的前缀或后缀是禁止使用的，例如 `name_`、`mName`、`s_name`、`kName`。

### 标识符类型规则

#### 包名

包名全小写，连续的单词直接拼接，不使用下划线。例如 `com.example.deepspace`，而不是 `com.example.deepSpace` 或 `com.example.deep_space`。

#### 类名

类以 [大骆峰](#骆驼峰式命名) 式命名。

类名通常是名词或名词短语，如 `Character` 和 `ImmutableList`。接口名可能是名词或名词短语，如 `List`，也可能是形容词或形容词短语，如 `Readable`。

注解类型命名，没有具体的规则和成熟的约定。

测试类名以被测类的类名，加上 `Test` 结尾组成。例如 `HashTest` 或 `HashIntegrationTest`。

#### 方法名

方法以 [小骆峰](#骆驼峰式命名) 式命名。

方法名通常是动词或动词短语。例如 `sendMessage` 和 `stop`。

下划线可能为了逻辑上的分隔，而出现在 Junit 测试方法名中，并且每个测试方法都以 [小骆峰](#骆驼峰式命名) 式命名。一个典型的模式是 `<methodUnderTest>_<state>`，例如 `pop_emptyStack`。对测试方法的命名不存在唯一正确的方式。

#### 常量名

常量使用 `CONSTANT_CASE`（全大写，单词之间以下划线分隔）命名。但 Java 中的常量意味着什么？

常量是 static final 修饰的字段，是不可变的且其方法是无副作用的。包括基本元素、字符串、不可变类型和不可变类型的不可变集合。一个实例的外部状态如果是可变的，就不属于常量。但仅仅不改变实例的外部状态，又是不够的。例如：

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

常量名通常是名词或名词短语。

#### 非常量字段名

非常量字段（静态或者非静态）以 [小骆峰](#骆驼峰式命名) 式命名。

非常量字段名通常是名词或名词短语。例如：`computedValues` 或 `index`。

#### 参数名

参数以 [小骆峰](#骆驼峰式命名) 式命名。

避免在 public 方法中使用单个字符作为参数名。

#### 局部变量名

局部变量以 [小骆峰](#骆驼峰式命名) 式命名。

局部变量即使是 final 不可变的，也不被认为是常量，且不应该以常量形式命名。

#### 类型变量名

类型变量以以下两种风格之一命名：

1. 单个大写字母，后跟可选的单个数字（如 `E`，`T`，`X`，`T2`）。
2. 以类形式的命名（ 5.2.2 [类名](#类名) 章节），后跟大写字母 T（例如：`RequestT`，`FooBarT`）。

### 骆驼峰式命名

有多种情况下需要将英语短语转换为驼峰形式，如缩写的单词 iOS 或不寻常的结构 IPv6 。为了提高可预测性， Google 风格指定了以下几种明确的方案。

以散文形式的命名开始：

1. 将英语短语转换成纯 ASCII 编码，并删除所有单引号。例如，Müller's algorithm 可以转换成 Muellers algorithm。
2. 以空格或其它任何标点符号（通常为连字符）分隔单词。
   - 推荐：如果已有单词是驼峰式拼写的，则分隔它的组成部分（如 AdWords 分隔成 ad words）。注意如 iOS 这样的单词并不是真正的驼峰式拼写，它违反了所有惯例，因此不适用于本条建议。
3. 将所有字母转换为小写（包括缩写），然后将...的首字母转换为大写：
   - ... 每个单词，产生大驼峰式命名。
   - ... 除去第一个单词的其它单词，产生小驼峰式命名。
4. 最后，连接所有字母成为一个标识符。

注意，原单词的大小写应被忽略。例如：

| Prose form              | Correct                              | Incorrect         |
| ----------------------- | ------------------------------------ | ----------------- |
| "XML HTTP request"      | `XmlHttpRequest`                     | `XMLHTTPRequest`  |
| "new customer ID"       | `newCustomerId`                      | `newCustomerID`   |
| "inner stopwatch"       | `innerStopwatch`                     | `innerStopWatch`  |
| "supports IPv6 on iOS?" | supportsIpv6OnIos                    | supportsIPv6OnIOS |
| "YouTube importer"      | YouTubeImporter<br>YoutubeImporter\* |

\*是可以接受，但不推荐的。

> 在英语中，一些模糊的连字，如 nonempty 和 non-empty 都是正确的，所以方法名 `checkNonempty` 和 `checkNonEmpty` 也都是正确的。

---

## 编程实战

### @Override：尽量使用

只要是合法的方法，就应被标记 `@Override` 注解。合法的方法包括重写父类方法的类方法，实现接口方法的类方法，和重定义父接口方法的接口方法。

**特殊情况**：当父类方法为 `@Deprecated` 时，`@Override` 可以省略。

### 异常捕获：不能忽略

排除下述例子，在异常捕获中不做任何处理的做法是极少正确的。（典型的做法是打印日志，或重新抛出 `AssertionError` 异常）

当在 catch 块中确实不需要做任何处理时，应以注释说明理由。

```java
try {
  int i = Integer.parseInt(response);
  return handleNumericResponse(i);
} catch (NumberFormatException ok) {
  // it's not numeric; that's fine, just continue
}
return handleTextResponse(response);
```

**特殊情况**：在测试代码中，如果捕获的异常是以 `expected` 命名的，那么可以忽略注释。如下是一个非常常见的方法，为确保代码在测试时抛出预期 ​​ 的异常，因此可以不需要注释。

```java
try {
  emptyStack.pop();
  fail();
} catch (NoSuchElementException expected) {
}
```

### 静态成员：规范使用

应以类名引用类的静态成员，而不是以类的引用实例或者表达式。

```java
Foo aFoo = ...;
Foo.aStaticMethod(); // good
aFoo.aStaticMethod(); // bad
somethingThatYieldsAFoo().aStaticMethod(); // very bad
```

### finalize：禁用

覆盖 `Object.finalize()` 是 **非常罕见** 的行为。

> 提示：不要使用 finalize()。如果真的有必要，请先仔细阅读并深刻理解 [Effective Java 第七条](http://books.google.com/books?isbn=8131726592) Avoid Finalizers，之后也不要使用 finalize()。

---

## Javadoc

### 格式

#### 一般形式

Javadoc 的基本形式如下所示：

```java
/**
 * Multiple lines of Javadoc text are written here,
 * wrapped normally...
 */
public int method(String p1) { ... }
```

... 或者是单行形式的：

```java
/** An especially short bit of Javadoc. */
```

基本形式的 Javadoc 总是可以接受的。只有一行内容的 Javadoc 语句（包括注释标记）允许以单行形式替换。注意这种情况只能在 Javadoc 中没有任何如 `@return` 之类的标签时才可发生。

#### 段落

空行（仅包含左侧对齐 \* 号的行）出现在段落之间。若存在 @ 标记，空行则应出现在 @ 标记之前。除去第一个段落，每个段落的第一个单词之前都应有 `<p>` 标签，且 `<p>` 标签与段落的第一个单词之间没有任何空格。

#### 块标签

标准的块标签按以下顺序出现 `@param`、`@return`、`@throws`、`@deprecated`，且这四种类型不会出现在描述为空的 Javadoc 中。当无法在一行中容纳块标签时，下一行需要再缩进至少 4 个空格。

### 摘要片段

每个 Javadoc 块以一个简短的 **摘要片段** 开始。这个片段非常重要，它是在某些情况下唯一可以出现的文字，如在类和方法的索引中。

摘要片段可以是名词短语或者动词短语，但却不是一个完整的句子。它不以 `A {@code Foo} is a...` 或者 `This method returns...` 开始，也不会形成如 `Save the record.` 这样的祈使句。不过，摘要片段应是区分大小写且带有标点符号的，就好似它是个完整的句子。

> **提示**：一个常见的错误是把简短的 Javadoc 写成 `/** @return the customer ID */`。这是不正确的，应被修正为 `/** Returns the customer ID. */`。

### 尽量使用 Javadoc

Javadoc 应在每个 `public` 修饰的类，及其 `public` 和 `protected` 修饰的成员中存在，但除了下述的几个例外。

Javadoc 中也可以附加额外内容，如 7.3.4 [非必需的 Javadoc](#非必需的Javadoc) 章节所示。

#### 特殊情况：自解释的方法

对简单明了的方法，Javadoc 是可选的，如 `getFoo`。这种情况下，注释除了 Returns the foo 也确实没什么好写的了。

> 注意：不应使用本规则作为省略关键注释的理由。例如，一个名为 `getCanonicalName` 的方法，读者可能不知道方法名的含义，因此不能省略它的 Javadoc。

#### 特殊情况：重写方法

Javadoc 并不总是出现在重写方法中。

#### 非必需的 Javadoc

类或成员应根据实际需要编写 Javadoc。

当 _implementation comments_ 实现注释需要定义类或成员的目的和行为时，可以将注释 `// ...` 替换成 Javadoc `/** ... */`。

本规则不一定遵循 7.1.2、7.1.3、7.2 章节。
