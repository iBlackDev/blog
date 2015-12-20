---
layout: post
title:  "Objective-C编码规范"
author: 郭燕豪
categories: iOS
tags : [iOS]
---

### 参考资料：

- [Apple官方Cocoa代码规范](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
- [Github Objective-C style guide](https://github.com/github/objective-c-style-guide)
- [Google Objective-C style guide](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)

### 目的

统一编程风格，提高的可读性与编码效率，避免团队开发可能带来的混乱。

### 适用范围

本规范适用于公司运用Objective-C作为开发语言的所有产品代码。

### 定义

- **匈牙利命名法**：是一种编程时的命名规范。基本原则是：变量名＝属性＋类型＋对象描述，其中每一对象的名称都要求有明确含义，可以取对象名字全称或名字的一部分。
- **驼峰命名法**：就是当变量名或函式名是由一个或多个单词连结在一起，而构成的唯一识别字时，驼峰命名法第一个单词以小写字母开始；第二个单词的首字母大写或每一个单词的首字母都采用大写字母。

#### 类的命名

- Objective-C中类的命名每个独立意思的单词都以大写字母开始，不要加“_”和“-”等分隔符，每个单词不要使用缩写，可以让类名有更强的自解释性，通过类名就可以知道这个类的作用。
- 在对类进行命名的时候，为了统一，也是为了不至于类名发生冲突，统一添加前缀，前缀可以是缩写词，例如母婴之家可以使用MY作为类的前缀。
- 类别的命名类似于类命名，由前缀＋类别名称组成。比如要创建一个NSString 的类别，对类别命名为MYParsingAdditions,那么类别就放在NSString+MYParsingAdditions.h文件下。
- ViewController、View、Model的命名遵循统一的标准，比如MYHomeViewController、MYOrderItemView、MYOrderModel。其他类可以按照统一的规范制定标准。

#### 方法的命名

方法的命名示例如下：
 
    - (void)deleteOrderWithOrderId:(NSString *)orderId；
    
    - (void)getOrderListWithStatus:(NSString *)status
                        pageNumber:(NSInteger)pageNumber
                         pagetSize:(NSInteger)pageSize
                        onComplete:(void(^)(OrderItemListModel *orderList, NSError *error))block;

- 方法-后面留一个空格，每个参数中间留一个空格。
- 如果方法名字过长或者参数过多，对齐参数。
- 一般来说，不要对名称进行缩写，把他们全部拼写出来，即使可能会比较长。
- 对方法，变量命名的时候，如无特别说明，全部使用驼峰命名规则。
- 不要用and来连接两个参数。

#### 属性和变量命名

- 声明实例变量第一个单词全部小写，第二个及以后的单词首字母大写，其他字母小写
- 属性按功能命名成名词或者动词
- 局部变量不要公开声明到头文件中去，可以使用extension放在.m文件中，同样的，私有方法也放到.m文件中，头文件暴漏的信息越少越好，暴露出来的方法和属性都是可以被访问的。

#### 常量命名

- 枚举常量和const类型的常量命名规范类似与函数命名规范，同样有个前缀。比如，const float NSLightGray;
- 不要使用#define预编译命令来声明常量。可以使用枚举或const类型来声明。宏使用大写字母声明。注意，有编译器声明的宏前后都有两个下划线“_”，比如，__MACH__。

#### 通知和异常命名

- 通知一般是由全局的字符串对象来标识，一般由以下几个部分组成：
  
  ` [Name of associated class] + [Did | Will] + [UniquePartOfName] + Notification`, 比如，**NSApplicationDidBecomeActiveNotification**等。
  
- 异常一般也是由全局的字符串对象来标识，由以下几个部分组成：
  
  ` *[Prefix] + [UniquePartOfName] + Exception*`, 比如**NSColorListIOException**，**NSColorListNotEditableException**等。

#### 前缀

- 前缀在软件接口命名中是非常重要的一部分。它区别于软件的功能性部分。一面它便于统一命名规范，另一方面，它能很好的避免命名冲突。尤其是在集成进第三方库的时候。
- 前缀一般是有两到三个大写字母组成。一般在定义类，协议，函数(function)，常量，以及结构体的时候要使用前缀。但是在命名方法（method,区别于函数）和结构体中变量的时候不能使用前缀。
- 前缀尽量不要使用下划线”_”（这里是指对方法的命名不能使用下划线做前缀，对变量使用下划线作为前缀是允许的）。因为命名方法的时候使用下划线作为前缀意味着该方法是私有方法，而这个命名规则是属于苹果保留规则。如果这么使用，很容易造成冲突或者覆盖苹果的私有方法。

#### 缩写

所有的类，类别，方法，变量如果包括缩写，那么缩写的部分全部使用大写。比如，URL,TIFF以及EXIF等。

#### 注释

注释写起来可能会很痛苦，但是它是保持代码可阅读性的极为重要的方式，优秀的代码一定不会少。尽管注释如此重要，最好的代码是自说明的，良好的命名往往比注释更有效果。写注释的过程中，一定要注意到注释是写给人看的，要让本来一无所知的人能快速的了解到你的思想，设计。这个人也完全有可能是你自己，如果注释不够清楚，那么一段时间之后，你自己也不一定能看懂当初的设计初衷，或者就算能看到，也要花费更多的时间。

注释分为文件注释，类注释，属性注释，方法注释，变量注释等。文件注释主要用来进行版权声明，以及内容描述，这个一般在使用xcode创建文件的时候会自动创建；类注释主要用来对类进行描述，该类有什么作用，用在什么地方，如何使用等等；；属性注释，方法注释，变量注释用来表明属性，方法，变量都有哪些含义，用在什么场景，返回值如何等等。

注释常用的有两种：行注释，块注释。行注释一般用来注释一行，使用“//”开始注释；块注释使用如下方式进行：
 
    /*!
        @method(@property,@protocol,@class,…)
        @abstract         功能描述
        @discussion       使用场景，使用方法等描述
        @params           参数，如果有多个参数，可以继续添加
        @return           返回值描述
        @result
        @see
    */

使用这种方式注释可能会麻烦一点，但是有两个好处：1.清晰明了；2.利于生成文档。具体使用哪种方式看具体情况。

推荐使用自动注释插件VVDocumenter来生成注释。

#### 分界符对齐

分界符（如大括号“{”  “}”）对齐，在函数体的开始、类和接口的定义、以及if、for、do、while、switch、case语句中的程序都要采用应该使用如下方式对齐：
 
    for (…) {
        … // program code
    }
    
    void example_fun( void ) {
        … // program code
    }

如下例子不符合规范：
 
    for (…)
    {
        … // program code
    }
    
    if (…)
        {
        … // program code
        }
    
    void example_fun( void )
        {
        … // program code
        }

#### 排版

* 不允许把多个语句写在一行中，即使一行只写一条语句。比如，以下书写不规范：

    NSString * a = nil; NSString * b = nil;

应如下书写：

    NSString * a = nil;
    NSString * b = nil;

* if, for, do, while, case, switch, default 等语句自占一行，且if, for, do, while等语句的执行语句无论多少都要加括号{}。比如，以下不合规范

    if(!a)  a = @“hello”;

应如下书写：

    if(!a){
       a = @“hello”;
    }


* 相对独立的程序块之间、变量说明之后必须加空行。
* 方法之间留一个空行，没有特殊说明的话，不允许两个空行一起出现。
* 建议使用“#pragma mark”，将相同类型的代码放在一起，方便阅读代码。

#### nil检查

对一个对象做nil检查是一个良好的习惯。不仅可以防止程序crash，更可以检查相关的逻辑。

#### 点表示法

点表示法其实是调用了set或get方法。在等号左边的时候，是为赋值，调用的是set方法；在等号右边的时候，是为取值，调用的是get方法。

#### BOOL类型的陷阱

BOOL 在Objective-C里被定义为unsigned char，这意味着它不仅仅只有YES (1)和NO (0)两个值。不要直接把整形强制转换为BOOL 型。常见的错误发生在把数组大小，指针的值或者逻辑位运算的结果赋值到BOOL型中，而这样就导致BOOL 值的仅取决于之前整形值的最后一个字节，有可能出现整形值不为0但被转为NO的情况。应此把整形转为BOOL型的时候请使用ternery操作符，保证返回YES 或NO 值。

在BOOL，_BOOL 以及bool (见C++ Std 4.7.4, 4.12以及C99 Std 6.3.1.2)之间可以安全的交换值或转型。但BOOL 和Boolean 之间不可，所以对待Boolean 就像上面讲的整形一样就可以了。在Objective-C函数签名里仅使用BOOL 。

对BOOL值使用逻辑运算都是有效的，返回值也可以安全的转为BOOL型而不需要ternery操作符。

    // AVOID
    - (BOOL)isBold {
      return [self fontTraits] & NSFontBoldTrait;
    }
    - (BOOL)isValid {
      return [self stringValue];
    }

    // GOOD
    - (BOOL)isBold {
      return ([self fontTraits] & NSFontBoldTrait) ? YES : NO;
    }
    - (BOOL)isValid {
      return [self stringValue] != nil;
    }
    - (BOOL)isEnabled {
      return [self isValid] && [self isBold];
    }


不要把BOOL 型变量直接与YES 比较。

    // AVOID
    BOOL great = [foo isGreat];
    if (great == YES)
      // …be great!
    
    // GOOD
    BOOL great = [foo isGreat];
    if (great)  {
      // …be great!
    }
