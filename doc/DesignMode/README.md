## 设计模式

### 01.为什么要尽早学？

- 夯实基础，才能走得更远。
- 基础的知识确实很难直接转化成开发“生产力”，但是它能潜移默化地、间接地提高你对技术的理解。
- 应对面试中的设计模式相关问题。
- 写出高质量的好代码。
- 提高复杂代码的设计和开发能力
- 让读源码、学框架事半功倍
- 为职场发展做铺垫。想成长为技术专家、大牛、技术leader，希望在职场有更高的成就、更好的发展，那就要重视基本功的训练、基础知识的积累。
- 投资要趁早，这样才能尽早享受复利。有些能力，要早点锻炼；有些东西，要早点知道；有些书，要早点读。

### 02.代码质量的评价

#### 1、如何评价代码质量的高低？

**描述代码质量的所有常用词汇：**

灵活性（flexibility）、可扩展性（extensibility）、可维护性（maintainability）

可读性（readability）、可理解性（understandability）、易修改性（changeability）

可复用性（reusability）、可测试性（testability）、模块化（modularity）

高内聚低耦合（high cohesion loose coupling）、高效（high efficiency）、高性能（high performance）

安全性（security）、兼容性（compatibility）、易用性（usability）

整洁（clean）、清晰（clarity）、简单（simple）

直接（straightforward）、少即是多（less code is more）、文档详尽（well-documented）

分层清晰（well-layered）、正确性（correctness、bug free）、健壮性（robustness）

鲁棒性（robustness）、可用性（reliability）、可伸缩性（scalability）

稳定性（stability）、优雅（elegant）、好（good）、坏（bad）....

**评价代码质量的一些因素：**

- 并不能通过单一维度去评价一个段代码写的好坏。比如，一段代码的可扩展性高，但可读性很差，不能说这段代码质量高。
- 不同的评价维度也并不是完全独立的，有些是具有包含关系、重叠关系或者可以相互影响的。比如，代码的可读性好、可扩展性好，就意味着代码的可维护性好。
- 各种评价维度也不是非黑即白的。比如，不能简单地将代码分为可读与不可读。如果用数字来量化代码的可读性，它应该是一个连续的区间值，而非0、1这样的离散值。
- 对一段代码的质量评价，常常有很强的主观性。比如，怎么样的代码才算可读性好，每个人的评价标准都不大一样。越是有经验的工程师，给出的评价也就越准确。有的时候，自己觉得代码写得已经够好了，但实际上并不是。如果没有人指导，自己一个人闷头写代码，即便写再多的代码，代码能力也可能一直没有太大提高。

#### 2、常见的七个评价标准

其中，可维护性、可读性、可扩展性是最重要的三个评价标准。

**1、可维护性（maintainability）**

维护：修改bug、修改老的代码、添加新的代码。

易维护：在不破坏原有代码设计、不引入新的bug的情况下，能够快速地修改或者添加代码。

不易维护：修改或添加代码需要冒着极大的引入新bug的风险，并且需要长时间才能完成。

代码的可读性好、简洁、可扩展性好，就会使得代码易维护。更细化地讲，如果代码分层清晰、模块化好、高内聚低耦合、遵从基于接口而非实现编程的设计原则等等，那就可能意味着代码易维护。

**2、可读性（readability）**

软件设计大师 Martin Fowler 曾经说过：“Any fool can write code that a computer can understand. Good programmers write code that humans can understand.”

翻译成中文就是：“任何傻瓜都会编写计算机能理解的代码。好的程序员能够编写人能够理解的代码。”

Google内部专门有个认证就叫做Readability，只有拿到这个认证的工程师，才有资格在code review的时候，批准别人提交代码。

如何评价一段代码的可读性？

如果你的同事可以轻松地读懂你写的代码，那么说明你的代码可读性很好；如果同事在读你的代码时，有很多疑问，那就说明你的代码可读性有待提高了。

**3、可扩展性（extensibility）**

在不修改或少量修改原有代码的情况下，通过扩展的方式添加新的功能代码。

也就是说，代码预留了一些功能扩展点，可以把新功能代码，直接插到扩展点上，而不需要因为要添加一个功能而大动干戈，改动大量的原始代码。

”对修改关闭，对扩展开放“设计原则。

**4、灵活性（flexibility）**

当添加一个新的功能代码的时候，原有的代码已经预留好了扩展点，不需要修改原有的代码，只要在扩展点上添加新的代码即可。这个时候，我们除了可以说代码易扩展，还可以说代码写得好灵活。

当要实现一个功能的时候，发现原有代码中，已经抽象出了很多底层可以复用的模块、类等代码，可以直接拿来使用。这个时候，我们除了可以说代码易用之外，还可以说代码写得好灵活。

当使用某组接口的时候，如果这组接口可以应对各种使用场景，满足各种不同的需求，我们除了可以说接口易用之外，还可以说这个接口设计得好灵活或者代码写得好灵活。

**5、简洁性（simplicity）**

有一条非常著名的设计原则，那就是KISS 原则（Keep It Simple and Stupid）。意思就是说，尽量保持代码简单。代码简单、逻辑清晰，也就意味着易读、易维护。

思从深而行从简

**6、可复用性（reusability）**

尽量减少重复代码的编写。

面向对象中，继承、多态存在的目的之一，就是为了提高代码的可复用性。

设计原则中，单一职责原则也跟代码的可复用性相关。

重构技巧中，解耦、高内聚、模块化等都能提高代码的可复用性。

**7、可测试性（testability）**

代码可测试性的好坏，能从侧面上非常准确地反应代码质量的好坏。

代码的可测试性差，比较难写单元测试，那基本上就能说明代码设计得有问题。

#### 3、如何写出高质量的代码？

要写出满足这些评价标准的高质量代码，就需要掌握一些更加细化、更加落地的编程方法论，包括面向对象设计思想、设计原则、设计模式、编码规范、重构技巧等。

- 面向对象中的继承、多态能够让我们写出可复用的代码。

- 编程规范能让我们写出可读性好的代码。
- 设计原则中的单一职责、DRY、基于接口而非实现、里式替换原则等，可以让我们写出复用、灵活、可读性好、易扩展、易维护的代码。
- 设计模式可以让我们写出易扩展的代码。
- 持续重构可以时刻保持代码的可维护性。

### 03.提高代码质量的方法论

#### 1、面向对象

​		现在，主流的编程范式或者是编程风格有三种，分别是面向过程、面向对象和函数式编程。

面向对象又是这其中最主流的：

- 现在比较流行的编程语言大部分都是面向对象编程语言。
- 大部分项目也都是基于面向对象编程风格开发的。
- 面向对象因为具有丰富的特性，可以实现很多复杂的设计思路，是很多设计原则、设计模式编程实现的基础。

面向对象编程需要掌握相关的知识点：

- 面向对象的四大特性：封装、抽象、继承、多态。
- 面向对象编程和面向过程编程的区别和联系。
- 面向对象分析、面向对象设计、面向对象编程。
- 接口和抽象类的区别以及各自的应用场景。
- 基于接口而非实现编程的设计思想。
- 多用组合少用继承的设计思想。
- 面向过程的贫血模型和面向对象的充血模型。

#### 2、设计原则

​		设计原则是指导代码设计的一些经验总结。同一条设计原则，不同的人会有不同的解读。对于每一种设计原则，需要掌握它的设计初衷，能解决哪些编程问题，有哪些应用场景。只有这样，才能在项目中灵活恰当地应用这些原则。

面向对象相关书籍：深入浅出面向对象分析与设计。

需要掌握的几个常用设计原则：

- SOLID原则 -SRP单一职责原则
- SOLID原则 -OCP开闭原则
- SOLID原则 -LSP里式替换原则
- SOLID原则 -ISP接口隔离原则
- SOLID原则 -DIP依赖倒置原则
- DRY原则、KISS原则、YAGNI原则、LOD法则

#### 3、设计模式

​		设计模式是针对软件开发中经常遇到的一些设计问题，总结出来的一套解决方案或者设计思路。相对于设计原则来说，没有那么抽象，而且大部分都不难理解，代码实现也并不复杂。

- 大部分设计模式要解决的都是代码的可扩展性问题。
- 设计模式的学习难点是了解它们都能解决哪些问题，掌握典型的应用场景，并且懂得不过度应用。
- 经典的设计模式有23种。随着编程语言的演进，一些设计模式（比如Singleton）也随之过时，甚至成了反模式，一些则被内置在编程语言中（比如Iterator），另外还有一些新的模式诞生（比如Monostate）。

设计模式相关书籍：设计模式、Head Frist设计模式、java与模式。

23种设计模式又可以分为三大类：创建型、结构型、行为型。

**创建型**

常用的有：单例模式、工厂模式（工厂方法和抽象工厂）、建造者模式。

不常用的有：原型模式。

**结构型**

常用的有：代理模式、桥接模式、装饰者模式、适配器模式。

不常用的有：门面模式、组合模式、享元模式。

**行为型**

常用的有：观察者模式、模板模式、策略模式、职责链模式、迭代器模式、状态模式。

不常用的有：访问者模式、备忘录模式、命令模式、解释器模式、中介模式。

#### 4、编程规范

​		编程规范主要解决的是代码可读性问题。相对于设计原则、设计模式，更加具体、更加偏重代码细节。每条编码规范都非常简单、非常明确，比较偏向于记忆。

编码规范相关书籍：代码大全、代码整洁之道、编写可读代码的艺术。

#### 5、代码重构

​		只要软件在不停地迭代，就没有一劳永逸的设计。随着需求的变化，代码的不停堆砌，原有的设计必定会存在这样那样的问题。针对这些问题，就需要进行代码重构。重构是软件开发中非常重要的一个环节。持续重构是保证代码质量不下降的有效手段。

需要掌握的几个重构知识点：

- 重构的目的（why）、对象（what）、时机（when）、方法（how）。
- 保证重构不出错的技术手段：单元测试和代码的可测试性。
- 两种不同规模的重构：大重构（大规模高层次）和小重构（小规模低层次）。

代码重构相关书籍：重构、重构与模式、修改代码的艺术。

#### 6、五者之间的联系

- 面向对象编程因为具有丰富的特性（封装、抽象、继承、多态），可以实现很多复杂的设计思路，是很多设计原则、设计模式等编码实现的基础。

- 设计原则是指导我们代码设计的一些经验总结，对于某些场景下，是否应用某种设计模式，具有指导意义。比如，“开闭原则“是很多设计模式（策略、模板等）的指导原则。

- 设计模式是针对软件开发中经常遇到的一些设计问题，总结出来的一套解决方案或者设计思路。应用设计模式的主要目的是提高代码的可扩展性。从抽象程度上来讲，设计原则比设计模式更抽象。设计模式更加具体、更加可执行。

- 编程规范主要解决的是代码的可读性问题。编码规范相对于设计原则、设计模式，更加具体、更加偏重代码细节、更加落地。持续的小重构依赖的理论基础主要就是编程规范。

- 重构作为保持代码质量不下降的有效手段，利用的就是面向对象、设计原则、设计模式、编程规范这些理论。

### 04.面向对象概念

#### 1、面向对象编程及语言

面向对象编程（Object Oriented Programming，OOP）是一种编程规范或编程风格。它以类或对象作为组织代码的基本单元，并将封装、抽象、继承、多态四个特性，作为代码设计和实现的基石。

面向对象编程语言（Object Oriented Programming Language，OOPL）是支持类或对象的语法机制，并有现成的语法机制，能方便地实现面向对象编程四大特性的编程语言。

面向对象编程都是通过使用面向对象编程语言来进行的，但是，不用面向对象编程语言，也能进行面向对象编程。

即便是使用面向对象编程语言，写出来的代码也不一定是面向对象编程风格的，也可能是面向过程编程风格的。

面向对象编程从字面上，按照最简单、最原始的方式来理解，就是将对象或类作为代码组织的基本单元，来进行编程的一种编程范式或者编程风格，并不一定要封装、抽象、继承、多态这四大特性的支持。

只不过，在进行面向对象编程的过程中，人们不停地总结发现，有了这四大特性，就能更容易地实现各种面向对象的代码设计思路。

只要某种编程语言支持类或对象的语法概念，并且以此作为组织代码的基本单元，那就可以粗略地认为它就是面向对象编程语言。

#### 2、面向对象分析及设计

面向对象分析（Object Oriented Analysis，OOA）考虑的是要做什么？从问题域的词汇表中找到类和对象来分析需求。

面向对象设计（Object Oriented Design，OOD）考虑的是怎么做？比如，程序被拆解为哪些类，每个类有哪些属性方法，类与类之间如何交互等。

面向对象编程（Object Oriented Programming，OOP）则是将分析和设计的结果翻译成代码的过程。

#### 3、统一建模语言

UML（Unified Model Language），统一建模语言。很多讲解面向对象或设计模式的书籍，常用它来画图表达面向对象或这几模式的设计思路。

UML是一种非常复杂的东西，它不仅仅包含类图，还有用例图、顺序图、活动图、状态图、组件图等。就类于类之间的关系，UML就定义了很多种，比如泛化、实现、、关联、聚合、组合、依赖等。

要想完全掌握UML，就需要花很多的学习精力。而且，即便是你能完全按照UML规范来画图，可对于不熟悉的人来说，看懂的成本也还是很高的。所以说，大部分情况下，随手画个草图，能够达意，方便沟通就够了。

### 05.面向对象特性

#### 1、封装

封装（Encapsulation）也叫**信息隐藏或者数据访问保护**。类通过暴露有限的访问接口，授权外部仅能通过类提供的方式（或者叫函数）来访问内部信息或者数据。

java实现封装的语法机制，就是访问权限控制。通过private、public等关键字对类中属性的访问做出限制，类仅仅通过有限的方法暴露必要的操作。

如果类属性都暴露给类的调用者，调用者想要正确地操作这些属性，就势必要对业务细节有足够的了解。这对于调用者来说也是一种负担。

What：隐藏信息，数据访问保护。

How：暴露有限接口和数据，需要编程语言提供访问控制的语言。

Why：提高代码可维护性；降低接口复杂度，提高类的易用性。

#### 2、抽象

抽象（Abstraction）就是**隐藏方法的具体实现**，让调用者只需关心方法提供了哪些功能，并不需要知道这些功能是如何实现的。

java实现抽象的语法机制，就是interface和abstract关键字，利用接口类和抽象类实现对方法细节的隐藏。一般来说，只要编程语言支持方法或者函数这一语法机制，就可以实现抽象。

在面对复杂系统的时候，人脑能承受的信息复杂程度是有限的，所有必须忽略掉一些非常关键性的实现细节。而抽象作为一个非常宽泛的设计思想，在代码设计中，起到非常重要的指导作用。

很多设计原则都体现了抽象这种设计思想，比如基于接口而非实现编程、开闭原则（对扩展开发、对修改关闭）、代码解耦（降低代码的耦合性）等。

What：隐藏具体实现，使用者只需关心功能，无需关心实现。

How：通过接口类或者抽象类实现，特殊语法机制非必须。

Why：提高代码的扩展性、维护性；降低复杂度，减少细节负担。

#### 3、继承

继承（Inheritance）是用来表示类之间的is-a关系，比如猫是一种哺乳动物。从继承关系上来讲，继承可以分为两种模式，单继承和多继承。单继承表示一个子类只能继承一个父类，多继承表示一个子类可用继承多个父类，比如猫既是哺乳动物，又是爬行动物。

java实现继承的语法机制，是extends关键字，并且只支持单继承。

继承最大的一个好处就是代码复用，比如两个类有一些相同的属性和方法，我们就可以将这些相同的部分，抽取到父类中，让两个子类继承父类。这样，就避免了代码重复。

上升到思维层面，继承是一种is-a关系，反应真实世界中的继承关系，符合人类的认知，而且，从设计的角度来说，也有一种结构美感。

不过，过度使用继承，继承层次过深过复杂，就会导致代码可读性、可维护性变差。所以继承这个特性也是非常有争议的特性，很多人觉得继承是一种反模式，我们应该尽量少用，甚至不用。

What：表示is-a关系，分为单继承和多继承。

How：需要编程语言提供特殊语法机制。例如java的“extends”，C++的“:"。

Why：解决代码复用问题。

#### 4、多态

多态（Polymorphism）是指子类可用替换父类，在实际的代码运行过程中，调用子类的方法实现。

java实现多态的语法机制，要支持父类对象可以引用子类对象，要支持继承父类或实现接口，要支持重写父类中的方法。

多态能提高代码的可扩展性和复用性。多态也是很多设计模式、设计原则、编程技巧的代码实现基础，比如策略模式、基于接口而非实现编程、依赖倒置原则、里式替换原则、利用多态去掉冗长的if-else语句等。

What：子类替换父类，在运行时调用子类的实现。

How：需要编程语言提供特殊的语法机制。比如继承、接口类、duck-typing。

Why：提高代码扩展性和复用性。

### 06.对比面向过程

#### 1、面向过程概念

面向过程编程是一种编程范式或者编程风格。它以过程（方法、函数、操作）作为组织代码的基本单元，以数据（成员变量、属性）与方法相分离为最主要的特点。面向过程风格是一种流程化的编程风格，通过拼接一组顺序执行的方法来操作数据完成一项功能。

面向过程编程语言是一种编程语言。它最大的特点是不支持类和对象两个语法概念，不支持丰富的面向对象编程特性，仅支持面向过程编程。

#### 2、面向对象的优势

- OOP更加能够应对大规模复杂程序的开发。
  - 对于大规模复杂程序的开发来说，整个程序的处理流程错综复杂，并非只有一条主线。如果把整个程序的处理流程画出来的话，会是一个网状结构。
  - 如果用面向过程编程这种流程化、线性的思维方式，去翻译这网状结构，去思考如何把程序拆解为一组顺序执行的方法，就会比较吃力。
  - 面向过程编程考虑的是如何将复杂的流程拆解为一个个方法和结构。而面向对象编程则是考虑如何给业务建模，如何将需求翻译为类，如何给类之间建立交互关系。
  - 类是一种非常好的组织这些函数和数据结构的方式，是一种将代码模块化的有效手段。
  - 面向过程的编程语言照样可以写出面向对象风格的代码，只不过付出的代价要高些。
  - 面向过程的编程语言，也都有借鉴面向对象编程的一些优点。
- OOP风格的代码更易复用、易扩展、易维护。
  - 封装特性更有利于提高代码的易维护性。
  - 基于接口实现的抽象，可以轻松替换新的实现逻辑，提高了代码的可扩展性。
  - 继承特性避免了代码重复写多遍，提高了代码的复用性。
  - 多态特性，在修改一个功能实现的时候，可以通过实现一个新的子类，重写原来的功能逻辑，用子类替换父类。在实际的代码运行过程中，调用子类的新的功能逻辑，而不是在原有代码上做修改。这就遵从了“对修改关闭、对扩展开发”的设计原则，提高了代码的扩展性。
  - 利用多态特性，不同的类对象可以传递给相同的方法，执行不同的代码逻辑，提高了代码的复用性。
- OOP语言更加人性化、更加高级、更加智能。
  - 从二进制指令、汇编语言到面向对象编程语言，是一个非常自然的过渡，都是一种流程化的、面条式的编程风格，用一组指令顺序操作数据，来完成一项任务。
  - 而从这个过渡，很容易发现这样一条规律，那就是编程语言越来越人性化，让人跟机器打交道越来越容易。
  - 面向对象编程语言之后，面向对象编程语言的出现，也顺应了这样的发展规律，也就是说，面向对象编程语言比面向过程编程语言更加高级。
  - 二进制指令、汇编语言、面向过程编程是一种计算机思维方式，而面向对象是一种人类的思维方式。前三者考虑的是如何告诉机器去执行一组指令，而面向对象考虑的是如何给业务建模，如何将真实的世界映射为类或者对象，这让我们更加能焦距到业务本身，而不是思考如何跟机器打交道。可以说，越高级的编程语言离机器越“远”，离我们人类越”近“，越“智能”。

#### 3、面向过程代码

​		在用面向对象编程语言进行软件开发的时候，我们有时候会写出面向过程风格的代码。有些是有意为之，并无不妥；而有些是无意为之，会影响到代码的质量。

**1、滥用getter、setter方法**

之前我们在设计类的时候，往往在定义完类的属性之后，就顺手把这些属性的getter、setter方法都定义上了。然而，这就违反了面向对象编程的封装特性，相当于将面向对象编程风格退化成了面向过程编程风格。

在设计实现类的时候，除非真的需要，否则，尽量不要给属性定义setter方法。除此之外，尽管getter方法相对setter方法要安全些，但是如何返回的是集合容器，也要防范集合内部数据被修改的危险。

**2、滥用全局变量和全局方法**

在面向对象编程中，常见的全局变量有单例对象、静态成员变量、常量等，常见的全局方法有静态方法。

- 假设将所有的全局变量都放在Constants类中：
  - 影响代码的可维护性。常量越多，查找起来就会变得比较费时，而且还会增加提交代码冲突的概率。
  - 增加代码的编译时间。常量越多，依赖这个类的代码就会很多。每次的修改，都会导致依赖它的类文件重新编译，因此会浪费很多不必要的编译时间。每次运行单元测试，都会触发一次编译过程，这个编译时间就有可能影响到我们的开发效率。
  - 影响代码的复用性。如果我们在另一个项目中，复用本项目开发的某个类，而这个类又依赖Constants类。即便这个类只依赖Constants类中的一小部分常量，任然需要把整个Constants类也一并引入，也就引入了很多无关的常量到新的项目中。

- Utils类的背景：有A类和B类，它们要用到一块相同的功能逻辑，为了避免代码重复，不应该在两个类中，写两遍相同的功能逻辑。
  - 当然，也可以使用继承实现代码的复用性。但是，有的时候，从业务含义上，A类和B类并不一定具有继承关系。
  - 所以，定义一个静态的方法在Utils类中，就可以很好地解决这个问题了。
  - 不过，静态方法将方法于数据分离，破坏了封装特性，是典型的面向过程风格。在定义Utils类之前，需要思考这个方法是否可以直接定义到其他类中。
  - 此外，在设计Utils类时，针对不同的功能，设计不同的Utils类。

**3、定义数据和方法分离的类**

- 传统的MVC结构分为Model层、Controller层、View层这三层。
- 在做前后端分离之后，三层结构在后端开发，被分为Controller层、Service层、Repository层。
- 其每一层中，又会定义相应的VO（View Object）、BO（Business）、Entity。
- 一般情况下，VO、BO、Entity中只会定义数据，不会定义方法。所有操作这些数据的业务逻辑都定义在对应的Controller类、Service类、Repository类中。这就是典型的面向过程的编程风格。

​		实际上，这种开发模式叫做基于贫血模型的开发模式。贫血模型（Anemic Domain Model，由MatinFowler提出）又称失血模型，是指domain object仅有属性的getter/setter方法的纯数据类，将所有类的行为放到service层。

”贫血模型“的开发模式为什么会流行？

- 实现简单。Object仅仅作为传递数据的媒介，不用考虑过多的设计方面，将核心业务逻辑放到service层，用Hibernate之类的框架一套，完美解决任务。
- 上手快。使用贫血模式开发的web项目，新来的程序员看看代码就能“照猫画虎”干活了，不需要多高的技术水平。所以很多程序员干了几年，仅仅就会写CURD。
- 一些技术鼓励使用贫血模型。例如J2EE Entity Beans，Hibernate等。

### 07.编码实践总结

#### 1、抽象类

抽象类更多的是为了代码复用，利用继承表示一种is-a关系。

- 抽象类和抽象方法都要用abstract进行修饰。
- 抽象类不允许被实例化，但有构造方法（供给子类实例化使用），只能被继承（extends）。

- 抽象类可以包含属性和方法。

- 子类继承抽象类，必须实现抽象类中的所有抽象方法。

#### 2、接口

接口更侧重于解耦，表示一种has-a关系。接口是对行为的一种抽象，相当于一组协议或者契约。

- 接口用interface修饰。
- 接口不允许被实例化，且没有构造方法，只能被实现（implements）。
- 接口不能包含属性（成员变量），定义的都是常量（默认public static final）。

- 接口只能声明方法，方法不能包含代码实现（默认public abstract）。
- 接口中可以定义静态方法、default方法、枚举类型，接口中还可以定义接口（嵌套）。

- 类实现接口的时候，必须实现接口中所有抽象方法。

#### 3、基于接口而非实现编程

​		“基于接口而非实现编程”，这条原则的另一个表述方式，是“基于抽象而非实现编程”。后者的表述方式其实更能体现这条原则的设计初衷。

​		我们在做软件开发的时候，一定要有抽象意识、封装意识、接口意识。越抽象、越顶层、越脱离具体某一实现的设计，越能提高代码的灵活性、扩展性、可维护性。

​		我们在定义接口的时候，一方面，命名要足够通用，不能包含跟具体实现相关的字眼；另一方面，与特定实现有关的方法不能定义在接口中。

​		“基于接口而非实现编程”这条原则，不仅仅可以指导非常细节的编程开发，还能指导更加上层的架构设计、系统设计等。比如，服务端与客户端之间的“接口设计”、类库的“接口”设计。

#### 4、多用组合少用继承

继承主要有三个作用：表示is-a关系，支持多条特性，代码复用。而这三个作用都可以通过组合（composition）、接口、委托（delegation）三个技术手段来达成。此外，利用组合还能解决层次过深、过复杂的继承关系影响代码可读性的问题。

如果类之间的继承结构稳定，层次比较浅，关系不复杂，就可以使用继承。反之，尽量使用组合来替代继承。此外，还有一些设计模式、特殊的应用场景，会固定使用继承或者组合。

### 08.充血模型

#### 1、领域驱动设计

​		DDD（Domain Driven Design）领域驱动设计，主要用来指导如何解耦业务系统，划分业务模块，定义业务领域模型及其交互。领域驱动设计这个概念并不新颖，早在2004年就被提出了。它被大众熟知，还是基于另一个概念的兴起，那就是微服务。

#### 2、充血模型

​		在贫血模型中，数据和业务逻辑被分割到不同的类中。而充血模型（Rich Domain Model）正好相反，数据和对应的业务逻辑被封装到同一个类中。因此，满足面向对象的封装特性，是典型的面向对象编程风格。

​		基于充血模型的DDD开发模式实现的代码，也是按照MVC三层架构分层的。Controller层还是负责暴露接口，Repository层还是负责数据存取，Service层负责核心业务逻辑。它跟基于贫血模型的传统开发模式的区别主要在Service层。

​		在基于贫血模型的传统开发模式中，Service层包含Service类和BO类两部分，BO只包含数据，不包含具体的业务逻辑。业务逻辑集中在Service类中。

​		在基于充血模型的DDD开发模式中，Service层包含Service类和Domain类两部分。Domain就相当于贫血模型中的BO。不过，Domain与BO的区别在于它是基于充血模型开发的，既包含数据，也包含业务逻辑。而Service类变得非常单薄。

总结：基于贫血模型的传统的开发模式，重Service轻BO；基于充血模型的DDD开发模式，轻Service重Domain。

#### 3、贫血模型广泛普遍的原因

- 大部分情况下，我们的开发的系统业务可能都比较简单，简单到就是基于SQL的CRUD操作，贫血模型足以应付这种简单业务的开发工作。此外，因为业务比较简单，即便使用充血模型，设计出来的领域模型也会比较单薄，跟贫血模型差不多，没有太大意义。
- 充血模型的设计要比贫血模型更加有难度。因为充血模型是一种面向对象的编程风格。我们从一开始就要设计好针对数据要暴露哪些操作，定义哪些业务逻辑。而不是像贫血模型那样，只需要定义数据，之后有什么功能开发需求，就在Service层定义什么操作，不需要事先做太多设计。
- 思维已固化，转型有成本。

#### 4、充血模型的优势

基于充血模型的DDD开发模式的开发流程，在应对复杂系统的开发的时候更加有优势。

- 基于贫血模型的传统开发模式
  - 大部分都是SQL驱动（SQL-Driven）的开发模式。当接到后端接口的开发需求时，就去看接口需要的数据对应到数据库中，需要哪张表或者哪几张表，然后思考如何编写SQL语句来获取数据。
  - 业务逻辑包裹在一个大的SQL语句中，而Service层可以做的事情很少。SQL都是针对特定的业务功能编写的，复用性很差。
- 基于充血模型的DDD的开发模式
  - 需要事先理清楚所有的业务，定义领域模型所包含的属性和方法。领域模型相当于可复用的业务中间层。新功能需求的开发，都基于之前定义好的这些领域模型来完成。
  - 越复杂的系统，对代码的复用性、易维护性要求就越高，就越应该花更多的时间和精力在前期设计上。而基于充血模型的DDD开发模式，正好需要我们前期做大量的业务调研、领域模型设计，所以更适合这种复杂系统的开发。

### 09.SOLID原则

SOLID原则：单一职责原则（SRP）、开闭原则（OCP）、里式替换原则、接口隔离原则、依赖反转原则。

#### 1、单一职责原则

​		单一职责原则（Single Responsibility Principle，SRP）。这个原则的英文描述是：A class or module should have a single responsibility。中文意思是：一个类或者模块只负责完成一个职责（或者功能）。

​		不要设计大而全的类，要设计粒度小、功能单一的类。单一职责原则是为了实现代码高内聚、低耦合，提高代码的复用性、可读性、可维护性。同时，如果拆分得过细，实际上会适得其反，反倒会降低内聚性，也会影响代码的可维护性。

​		先写一个粗粒度的类，满足业务 需求。随着业务的发展，如果粗粒度越来越大，代码越来越多，这个时候就可以将这个粗粒度的类，差分成几个更细粒度的类。这就是所谓的持续重构。

**判断原则：**

- 类中的代码行数、函数或属性过多，会影响代码的可读性和可维护性，就需要考虑对类进行拆分。
- 类依赖的其他类过多，或者依赖类的其他类过多，不符合高内聚、低耦合的设计思想，就需要考虑对类进行拆分。
- 私有方法过多，就要考虑能否将私有方法独立到新的类中，设置为public方法，供更多的类使用，从而提高代码的复用性。
- 比较难给类起一个合适的名字，很难用一个业务名称概况，或者只能用一些笼统的Manager、Context之类的词语来命名，这就说明类的职责定义得可能不够清楚。
- 类中大量的方法都是集中操作类中的某几个属性，如果一半的方法都在操作某个属性，那就可以考虑将这个几个属性和对应的方法拆分出来。

#### 2、开闭原则

​		开闭原则（Open Closed Principle，OCP）。这个原则的英文描述是：software entities (modules, classes, functions, etc.) should be open for extension , but closed for modification。中文意思是：软件实体（模块、类、方法等）应该“对扩展开放、对修改关闭”。

​		添加一个新的功能应该是，在已有代码基础上扩展代码，而非修改已有代码。这里的代码指的是模块、类、方法等。

> 给类中添加新的属性和方法，算作“修改”还是"扩展"？

在粗代码粒度下，被认为“修改”，在细代码粒度下，又被认为“扩展”。比如，修改类中属性，相对于类来说，这是修改，而对于属性和方法，这就是扩展。

总结：只要它没有破坏原有的代码的正常运行，没有破坏原有的单元测试，这就是一个合格的代码改动。

> 如何做到“对扩展开发、修改关闭”？

​		为了尽量写出扩展性好的代码，我们要时刻具备扩展意识、抽象意识、封装意识。这些“潜意识”可能比任何开发技巧都重要。

​		多思考，如何设计代码结构，事先留好扩展点。识别出代码可变部分和不可变部分之后，将可变部分封装起来，隔离变化，提供抽象化的不可变接口，给上层系统使用。

​		唯一不变的只有变化本身，所以我们没必要为一些遥远的、不一定发生的需求去提前买单，做过度设计。

​		有些情况下，代码的扩展性会跟可读性相冲突。在某些场景下，代码的扩展性很重要，我们就可以适当地牺牲一些代码的可读性；在另一些场景下，代码的可读性更加重要，那我们就适当地牺牲一些代码的可扩展性。

#### 3、里式替换原则

​		里式替换原则（Liskov Substitution Principle，LSP）。这个原则最早是在1986年由Barbara Liskov提出，它是这么描述这条原则的：If S is a subtype of T, then objects of type T may be replaced with objects of type S, without breaking the program。

​		在 1996 年，Robert Martin 在他的 SOLID 原则中，重新描述了这个原则，英文原话是这样的：Functions that use pointers of references to base classes must be able to use objects of derived classes without knowing it。

​		综合两者的描述，中文描述是这样的：子类对象（object of subtype/derived class）能够替换程序（program）中父类对象（object of base/parent class）出现的任何地方，并且保证原来程序的逻辑行为（Behavior）不变及正确性不被破坏。

​		里式替换原则还有一个更加能落地、更有指导意义的描述，那就是“Design By Contract”，意思是：按照协议来设计。

​		解读：子类在设计的时候，要遵守父类的行为约定（或者叫协议）。父类定义了函数的行为约定，那子类可用改变函数的内部实现逻辑，但不能改变函数原有的行为约定。

​		行为约定包括：函数声明要实现的功能；对输入、输出、异常的约定；甚至包括注解中罗列的任何特殊说明。

违反里式替换原则的例子

> 子类违反父类声明要实现的功能

​		父类中提供的 sortOrdersByAmount() 订单排序函数，是按照金额从小到大来给订单排序的，而子类重写这个 sortOrdersByAmount() 订单排序函数之后，是按照创建日期来给订单排序的。那子类的设计就违背里式替换原则。

> 子类违背父类对输入、输出、异常的约定

​		在父类中，某个函数约定：运行出错的时候返回 null；获取数据为空的时候返回空集合（empty collection）。而子类重载函数之后，实现变了，运行出错返回异常（exception），获取不到数据返回 null。那子类的设计就违背里式替换原则。

​		在父类中，某个函数约定，输入数据可以是任意整数，但子类实现的时候，只允许输入数据是正整数，负数就抛出，也就是说，子类对输入的数据的校验比父类更加严格，那子类的设计就违背了里式替换原则。

​		在父类中，某个函数约定，只会抛出 ArgumentNullException 异常，那子类的设计实现中只允许抛出 ArgumentNullException 异常，任何其他异常的抛出，都会导致子类违背里式替换原则。

> 子类违背父类注释中所罗列的任何特殊说明

​		父类中定义的 withdraw() 提现函数的注释是这么写的：“用户的提现金额不得超过账户余额……”，而子类重写 withdraw() 函数之后，针对 VIP 账号实现了透支提现的功能，也就是提现金额可以大于账户余额，那这个子类的设计也是不符合里式替换原则的。

> 通过测试父类判断

​		判断子类的设计实现是否违背里式替换原则，还有一个小窍门，那就是拿父类的单元测试去验证子类的代码。如果某些单元测试运行失败，就有可能说明，子类的设计实现没有完全地遵守父类的约定，子类有可能违背了里式替换原则。

> 多态与里式替换的区别

​		多态是面向对象编程的一大特性，也是面向对象编程语言的一种语法。它是一种代码实现的思路。

​		而里式替换是一种设计原则，用来指导继承关系中子类该如何设计，子类的设计要保证在替换父类的时候，不改变原有程序的逻辑及不破坏原有程序的正确性。

#### 4、接口隔离原则

​		接口隔离原则（Interface Segregation Principle，ISP）。Robert Martin在SOLID原则中的定义：Clients should not be forced to depend upon interfaces that they do not use。中文意思是：客户端不应该强迫依赖它不需要的接口。其中的”客户端“，可以理解为接口的调用者或者使用者。

> 理解“接口”二字

- 一组API接口集合：
  - 一个接口有多个不相关函数，可以拆分为多个接口类，避免不必要的错用。
- 单个API接口或函数：
  - 一个函数中有多种功能实现，可以拆分为多个函数，避免无用功的代码。
- OOP中的接口概念
  - 接口的设计要尽量单一，不要让接口的实现类和调用者，依赖不需要的接口函数。

> 接口隔离原则与单一职责原则的区别

​		单一职责原则针对的是模块、类、接口的设计。

​		而接口隔离原则相对于单一职责原则，一方面它更侧重于接口的设计，另一方面它的思考的角度不同。它提供了一种判断接口是否职责单一的标准：通过调用者如何使用接口来间接地判定。如果调用者只使用部分接口或接口的部分功能，那接口的设计就不够职责单一。

#### 5、依赖反转

> 控制反转（IoC）

​		控制反转（Inversion of Control），这里的“控制”是对程序执行流程的控制，而“反转”指的是在没有使用框架之前，程序员自己控制整个程序的执行。在使用框架之后，整个程序的执行流程可以通过框架来控制。流程的控制权从程序员“反转”到了框架。

​		实现控制反转的方法有很多，比如依赖注入。所以，控制反转并不是一种具体的实现技巧，而是一个比较笼统的设计思想，一般用来指导框架层面的设计。

> 依赖注入（DI）

​		依赖注入（Dependency Injection），也就是不通过new的方式在类内部创建依赖类对象，而是将依赖的类对象在外部创建好之后，通过构造函数、函数参数等方式传递（或注入）给类使用。

> 依赖注入框架（DI Framework）

​		通过依赖注入框架提供的扩展点，简单配置一下所有需要创建的类对象、类于类之间的依赖关系，就可以实现由框架来自动创建对象、管理对象的生命周期、依赖注入等原本需要程序员来做的事情。

>依赖反转原则（DIP）

​		依赖反转原则（Dependency Inversion Principle，DIP）也叫依赖倒置原则。英文描述：High-level modules shouldn’t depend on low-level modules. Both modules should depend on abstractions. In addition, abstractions shouldn’t depend on details. Details depend on abstractions。

​		中文意思是：高层模块（high-level modules）不要依赖底层模块（low-level）。高层模块和底层模块应该通过抽象（abstractions）来互相依赖。除此之外，抽象不要依赖具体实现细节（details），具体实现系统依赖抽象。

例如：Tomcat是运行Java Web应用程序的容器，程序只需要部署在Tomcat容器下，便可以被Tomcat容器调用执行。

​		Tomcat是高层模块，Web应用程序代码就是底层模块。Tomcat和应用程序代码之间并没有直接依赖关系，两者都依赖同一个“抽象”，也就是Servlet规范。Servlet规范不依赖具体的Tomcat容器和应用程序的实现细节，而Tomcat容器和应用程序依赖Servlet规范。

### 10.KISS、YAGNI原则

>KISS原则

KISS 原则（Keep It Simple and Stupid）。意思就是说，尽量保持代码简单。代码简单、逻辑清晰，也就意味着易读、易维护。

- 不要使用同事可能不懂的技术来实现代码。
- 不要重复造轮子，要善于使用已有的工具类库。经验证明，自己去实现这些类库，出现bug的概率会更高，维护的成本也比较高。
- 不要过度优化。比如，位运算代替算术运算、复杂的条件语句替代if-else、使用一些过于底层的函数等。

> YAGNI原则

​		YAGNI原则（You Ain‘ t Gonna Need It），直译就是：你不会需要它。意思是：不要去设计当前用不到的功能；不要去编写当前用不到的代码。其核心思想是：不要过度设计。

比如，为了频繁地修改Maven配置文件，提前往项目立引入大量常用的library包。而实际上不一定用的到，这就违背了YAGNI原则。

> KISS原则跟YAGNI是一回事吗？

KISS原则讲的是“如何做”的问题（尽量保持简单）。

YAGNI原则讲的是“要不要做”的问题（当前不需要的就不要做）。

### 11.DRY原则

DRY原则（Don’ t Repeat Yourself），直译是：不要重复自己。可以理解为：不要写重复的代码。

> 实现逻辑重复

比如：校验用户名和密码是否有效，这两个函数的实现逻辑是一样的。

​		从代码实现逻辑上看起来是重复的，但从语义上并不重复。所谓“语义不重复”值的是：从功能上来看，这两个函数干的是完全不重复的两事情，一个是校验用户名，另一个是校验密码。

​		尽管代码的实现逻辑是相同的，但语义不同，所有它并不违反DRY原则。此外，未来可能会改变校验用户名或密码的逻辑。而对于重复代码问题，可以通过抽象成更细粒度函数的方式来解决。

> 功能语义重复

比如：两个人在不知情下，开发了两个不同代码逻辑的函数，都是用来校验IP地址。

尽管两个函数的实现逻辑不同，但语义重复，也就是功能重复，所以认为它违反了DRY原则。

> 代码执行重复

比如：登录操作，先调用（判断用户名和密码是否存在）函数，再调用（根据用户名获取用户对象）函数。

重复的地方在于，根据用户名获取对象时，同样调用了（判断用户名是否存在）函数。解决办法是，将校验逻辑合并到一个service中，也就是先调用（判断用户名和密码是否存在）函数，再根据用户名获取用户对象。

此外，还有一个地方重复，就是可以不用调用（判断用户名和密码是否存在）函数，直接调用（根据用户名获取用户对象）函数，再去判断登录。这样就减少了调用（判断用户名和密码是否存在）的I/O操作。

> 代码复用性

代码复用性（Code Reusability）、代码复用（Code Reuse）和DRY原则。

代码复用表示一种行为：在开发新功能的时候，尽量复用已经存在的代码。

代码可复用性表示一段代码可被复用的特性或能力：在编写代码的时候，让代码尽量可复用。

DRY原则是一条原则：不要写重复的代码。

首先，“不重复”并不代表“可复用”。其次，“复用”和“可复用性”关注角度不同，一个是从代码使用者的角度，另一个是从评审开发角度。

> 怎么提高代码复用性

- 减少代码耦合
  - 高度耦合的代码，想要复用其中的一个类，往往会牵一发而动全身。
- 满足单一职责原则
  - 独立的模块就像一块一块的积木，更加容易复用。
- 业务与非业务逻辑分离
  - 越是跟业务无关的代码越是容易复用，越是针对特定业务的代码越难复用。
- 通用代码下沉
  - 从分层角度来看，越底层的代码越通用、会被越多的模块调用，越应该设计得足够可复用。
- 继承、多态、抽象、封装
  - 继承，子类复用父类的属性和方法。多态，动态地替换一段代码的部分逻辑，让这段代码可复用。抽象，越抽象、越不依赖具体的实现，越容易复用。封装，隐藏可变细节、暴露不变的接口，就越容易复用。
- 应用模板等设计模式
  - 一些设计模式，也能提高代码的复用性。比如，模板模式利用了多态来实现，可以灵活地替换其中的部分代码，整个流程模板代码可复用。

>Rule of Three原则

​		如果把这个原则用在这里，那就是说，我们在第一次写代码的时候，如果当下没有复用的需求，而未来的复用需求也不是特别明确，并且开发可复用代码的成本比较高，那我们就不需要考虑代码的复用性。在之后我们开发新的功能的时候，发现可以复用之前写的这段代码，那我们就重构这段代码，让其变得更加可复用。

​		也就是说，第一次编写代码的时候，我们不考虑复用性；第二次遇到复用场景的时候，再进行重构使其复用。需要注意的是，“Rule of Three”中的“Three”并不是真的就指确切的“三”，这里就是指“二”。

### 12.LOD原则

#### 1、高内聚、松耦合

​		“高内聚、松耦合”是一个比较通用的设计思想，可以用来指导不同粒度代码的设计与开发，比如系统、模块、类，甚至是函数，也可以应用到不同的开发场景中，比如微服务、框架、组件、类库等。

- 高内聚：相近功能放到同一个类中，职责单一。（用于指导类本身的设计）
  - 容易维护，避免修改时“牵一发而动全身”。
- 松耦合：类于类之间的依赖关系简单清晰。（用于指导类于类之间依赖关系的设计）
  - 一个类的代码改动不会或者很少导致依赖类的代码改动。
- 两者关系：“高内聚”有助于“松耦合”，同理，“松耦合”有助于“高内聚”。

#### 2、迪米特法则

​		迪米特法则（Law of Demeter，LOD）又叫做最小知识原则，英文翻译为：The Least Knowledge Principle。

​		不该有直接依赖关系的类之间，不要有依赖；有依赖关系的类之间，尽量只依赖必要的接口（也就是定义中的有限知识）。





### 创建型

创建型设计模式主要解决的是“对象的创建”问题。

#### 1、单例模式

单例模式（Singleton Design Pattern），一个类只允许创建一个对象（实例）。

##### 使用场景

从业务概念上，有些数据在系统中只应该保留一份，就比较适合设计为单例类。

利用单例解决资源访问冲突的问题。

- 表示全局唯一类。
  - 配置信息类。
  - ID号码生成器。

##### 如何设计一个单例

- 构造函数私有化，避免通过外部new对象。
- 考虑对象创建时的线程安全问题。
- 考虑是否支持延迟加载。
- 考虑getInstance()性能是否高（是否加锁）。

##### 饿汉式

~~~java
public class IdGenerator { // Id生成器类
    private AtomicLong id = new AtomicLong(0); // 使用并发库提供的原子变量类型
    private static final IdGenerator instance = new IdGenerator(); // 静态初始化实例
    private IdGenerator() {} // 私有构造
    public static IdGenerator getInstance() { // 提供获取实例的方法
        return instance; 
    }
    public long getId() { // 生成自增Id的方法
        return id.incrementAndGet(); 
    }
}
~~~

- 静态实例，类加载时就创建，所以instance实例的创建过程是线程安全的。
- 不支持延迟加载。
  - 实例占用资源多（比如占用内存多）。
  - 初始化耗时长（比如加载各种配置文件）。
- 等到用户请求的时候再去初始化势必会影响体验，甚至超时。
  - 所以最好不要等到真正要用它的时候再去初始化。
- 资源占用多，如果一开始就内存不够，就需要引起重视，而不是在程序运行一段时间后突然病发。
  - fail-fast的设计原则（有问题尽早暴露）。

##### 懒汉式

~~~java
public class IdGenerator {
    private AtomicLong id = new AtomicLong(0);
    private static IdGenerator instance; // 声明实例变量
    private IdGenerator() {}
    public static synchronized IdGenerator getInstance() {// 方法加锁
        if (instance == null) { // 实例为null才初始化
            instance = new IdGenerator(); 
        } 
        return instance;
    }
    public long getId() {
        return id.incrementAndGet(); 
    }
}
~~~

- 支持延迟加载
- 考虑到并发创建实例时的线程安全问题，需要给getInstance方法加锁。
  - 如果该方法被频繁用到时，可接受的并发量是1，也就是不支持高并发，势必会造成性能瓶颈。


##### 双重检测

~~~java
public static IdGenerator getInstance() {
    if (instance == null) {
        synchronized(IdGenerator.class) {  // 此处为类级别的锁 
            if (instance == null) { 
                instance = new IdGenerator(); 
            } 
        }
    }
    return instance;
}
~~~

- 双重检测的问题
  - `instance = new IdGenerator();`这句并非原子操作。
  - 事实上在JVM中大概做了3件事
    - 1、给instance分配内存。
    - 2、调用IdGenerator()构造函数来初始化成员变量。
    - 3、将instance对象指向分配的内存空间。
  - 在JVM即时编译器中存在指令重排序的优化。
    - 也就是说步骤2和3的顺序不能保证。
    - 原本步骤1-2-3可能会是1-3-2。
  - 解决办法是将instance变量声明成`volatile`
    - volatile的一个特性是可见性，也就时可以保证线程在本地不会有instance的副本，每次都去主内存中读取。
    - 另一个特性就是java1.5禁止指令重排序优化。

##### 静态内部类

~~~java
public class IdGenerator {
    private AtomicLong id = new AtomicLong(0);
    private IdGenerator() {}
    // 静态内部类
    private static class SingletonHolder{ 
        private static final IdGenerator instance = new IdGenerator();
    } 
    public static IdGenerator getInstance() { 
        return SingletonHolder.instance; 
    } 
    public long getId() { 
        return id.incrementAndGet();
    }
}
~~~

- 比双重检测更加简单。
- 有点像饿汉式，但又做了延迟加载。
  - 外部类创建并不会创建内部类实例。
- 线程安全
  - 当调用getInstance方法时，才会创建instance。
  - instance的唯一性、创建过程的线程安全性，都由JVM来保证。

##### 枚举

~~~java
public enum IdGenerator {
    INSTANCE;
    private AtomicLong id = new AtomicLong(0);
    public long getId() {
        return id.incrementAndGet(); 
    }
}
~~~

- 利用枚举类型本身的特性，保证了实例创建的线程安全性和实例的唯一性。

##### 单例存在的问题

- 对OOP特性不友好。
  - 单例违背了基于接口而非实现的设计原则（抽象）。
  - 对继承多态不友好，理论上可以实现，但会造成可读性变差。
- 隐藏类之间的依赖关系。
- 对代码的扩展性不友好。
  - 当业务发生变化时，单例在某些情况下会影响代码的扩展性、灵活性。
- 对代码的可测试性不友好。
  - 当单例类依赖比较重的外部资源时，导致无法实现mock替换。
- 不支持有参数的构造函数。
  - 第一种方法：先调用init方法，再调getInstance方法，否则抛出异常。
  - 第二种方法：getInstance有参构造传入。
    - 问题：第二次调用时传入的参数无效，有可能会误导使用者。
  - 第三种方法（推荐）：通过一个全局变量，getInstance时初始化。

##### 替代解决方案

- 使用静态方法，并不能解决这些问题。
- 工厂模式、IOC容器来保证，有程序员自己来保证不创建两个类对象。

##### 作用范围

单例是进程唯一的。

- 进程唯一：进程内（线程间）唯一，进程间不唯一。
- 线程唯一：线程内唯一，线程间（进程内）不唯一。
- 进程唯一等同于线程内唯一，线程间唯一。
- 集群唯一：进程内唯一，进程间也唯一。
- 进程表单个，进程间表多个。同理线程也一样。

##### 类加载器

JVM预定义类加载器：

- Bootstrap启动类加载器：本地代码实现，负责将运行时`/lib`下面的类库（jar）加载到内存。无法操作。
- Extension扩展类加载器：`sum.misc.Launcher$ExtClassLoader`，负责将`/lib/ext`或由系统变量`java.ext.dir`指定位置类库加载到内存。开发者可以直接使用标准扩展类加载器。
- System系统类加载器：`sum.misc.Launcher$AppClassLoader`，负责将系统类路径`/classpath`中指定的类库加载到内存。开发者可以直接使用

##### 双亲委派机制

- 原理：
  - 当某个类加载器接收到加载类的请求时，首先将加载任务委托给父类加载器，依次递归，如果父类加载器可以完成类加载任务，就成功返回，否则自己加载。
  - 这样递归的好处是，保证了被加载的类只加载一次。
- 具体：
  - 当前线程的类加载器去加载线程中的第一个类。
    - `getContextClassLoader`获取当前线程类加载器。
    - `setContextClassLoaer`设置当前线程类加载器。
  - 如果类A中引用了类B，JVM将使用类A的类加载器去加载B。
  - `ClassLoader.loadClass()`方法指定某个类加载器去加载某个类。
- 意义：
  - 防止内存中出现多份同样的字节码。
  - 如果不用委托，那么每个类就会加载自己的字节码，这样内存中就会出现多份。
  - 如果使用委托，会递归向父类查找，先用Bootstrap尝试加载，如果找不到再向下。
    - 例如：System在Bootstrap中找到然后加载，当类B加载时，发现Boostrap已经加载过System了，那么就直接返回。

#### 2、工厂模式

##### 简单工厂（Simple Factory）

也叫静态工厂方法模式（Static Factory Method Pattern），因为其中创建对象的方法是静态的。

- 需求：根据不同的配置文件生成不同的解析类。例如：json、xml、yaml、properties。
- 将该需求封装成方法，通过if分支创建不同的解析类。
  - 方法名（开头）：create、getInstance、createInstance、newInstance、valueOf。
- 将该方法抽取到工厂类。
  - 工厂类名（结尾）：Factory。例外：DateFormat、Calender。
- 使用静态代码块初始化所有的解析类，装入到Map。
- 通过传入规则字符串获取对应的解析类。
- 应用场景：如果代码中存在if分支判断，就可以使用简单工厂方法了。
  - 将if分支创建对象抽离成方法，放到工厂类中。

##### 工厂方法（Factory Method）

- 需求：去掉if分支判断。
- 利用多态，定义一个父工厂接口。
- 不同的解析工厂类都实现父工厂接口，实现生成不同的解析类。
- 用简单工厂来创建工厂方法。
  - 创建一个简单工厂类。
  - 使用静态代码块初始化所有的解析工厂类，装入Map。
  - 定义一个获取解析工厂的方法，通过规则字符串获取对应的解析工厂类
- 应用场景：
  - 如果创建逻辑很复杂，需要去掉if分支，就可以考虑使用工厂方法了。
  - 封装变化：封装成工厂类之后，创建逻辑的变更对调用者透明。
  - 代码复用：创建代码抽离到独立的工厂类之后可以复用。
  - 隔离复杂性：封装复杂的创建逻辑，调用者无需了解如何创建对象。
  - 控制复杂度：将创建代码抽离出来，让函数或类职责更单一，代码更简洁。

##### 比较：

- 简单工厂：
  - 创建类都在一个工厂里面。
  - 通过if分支创建不同的类。
- 工厂方法：
  - 不同类的创建在不同的工厂里面。
  - 利用多态，抽象出一个公共的工厂接口，更符合开闭原则。
  - 通过简单工厂来获取不同的工厂类，再创建对应的类。

##### 抽象工厂（Abstract Factory）

- 需求：除了通过配置规则，还要可以通过系统配置来获取。
- 在公共的工厂接口上添加一个方法。
- 所有解析类的工厂都实现这个方法，用来创建不同规则的系统配置工厂。
- 同样，不同规则的系统配置工厂都实现一个公共的系统配置接口。

##### DI容器

依赖注入容器（Dependency Injection Container），简称DI容器。

- 与工厂模式的区别
  - 工厂类负责的是某个类对象或某一组相关类对象的创建，而DI容器负责的是整个应用中所有类对象的创建。
  - DI容器负责在程序启动的时候，根据配置事先创建好对象，使用时直接拿取，这才称得上容器。除此之外，还负责配置解析、对象生命周期管理等很多事情。
- DI核心功能
  - 配置解析。
    - 通过解析配置文件创建对象。
  - 对象创建。
    - 通过BeansFactory工厂，反射创建对象。
  - 对象生命周期管理。
    - scope区分单例或多例。
    - lazy-init是否支持懒加载。
    - init-method、init-destroy用于指定初始化和销毁方法。

##### DI容器最小原型实现

~~~xml
<!--beans.xml-->
<beans>   
    <bean id="rateLimiter" class="com.xzg.RateLimiter"> 
        <constructor-arg ref="redisCounter"/>
    </bean>   
    <bean id="redisCounter" class="com.xzg.redisCounter"
          scope="singleton" lazy-init="true">
        <constructor-arg type="String" value="127.0.0.1"/>
        <constructor-arg type="int" value=1234/>
    </bean>
</beans>
~~~

~~~java
public class Demo {
    public static void main(String[] args) {
        ApplicationContext applicationContext = 
            new ClassPathXmlApplicationContext("beans.xml");
        RateLimiter rateLimiter = 
            (RateLimiter) applicationContext.getBean("rateLimiter");
        rateLimiter.test();
        //...  
    }
}
~~~

~~~java
// 应用上下文容器
public interface ApplicationContext {
  Object getBean(String beanId);
}
// 根据xml获取应用上下文容器
public class ClassPathXmlApplicationContext implements ApplicationContext {
  private BeansFactory beansFactory;
  private BeanConfigParser beanConfigParser;

  public ClassPathXmlApplicationContext(String configLocation) {
    this.beansFactory = new BeansFactory();
    this.beanConfigParser = new XmlBeanConfigParser();
    loadBeanDefinitions(configLocation);
  }
  // 根据配置文件地址获取Bean的定义
  private void loadBeanDefinitions(String configLocation) {
    InputStream in = null;
    try {
      in = this.getClass().getResourceAsStream("/" + configLocation);
      if (in == null) {
        throw new RuntimeException("Can not find config file: " + configLocation);
      }
      // 解析配置文件，获得Bean定义的集合，放入到BeansFactory
      List<BeanDefinition> beanDefinitions = beanConfigParser.parse(in);
      beansFactory.addBeanDefinitions(beanDefinitions);
    } finally {
      if (in != null) {
        try {
          in.close();
        } catch (IOException e) {
          // TODO: log error
        }
      }
    }
  }
  // 根据Bean的唯一标识获取Bean对象
  @Override
  public Object getBean(String beanId) {
    return beansFactory.getBean(beanId);
  }
}
~~~

~~~java
// 配置解析器
public interface BeanConfigParser {
  List<BeanDefinition> parse(InputStream inputStream); // 根据流解析
  List<BeanDefinition> parse(String configContent); // 根据配置内容解析
}
// xml配置解析器
public class XmlBeanConfigParser implements BeanConfigParser {

  @Override
  public List<BeanDefinition> parse(InputStream inputStream) {
    String content = null;
    // TODO:...
    return parse(content);
  }

  @Override
  public List<BeanDefinition> parse(String configContent) {
    List<BeanDefinition> beanDefinitions = new ArrayList<>();
    // TODO:...
    return beanDefinitions;
  }

}
// Bean的定义类
public class BeanDefinition {
  private String id; // Bean的唯一标识
  private String className; // 类名
  private List<ConstructorArg> constructorArgs = new ArrayList<>(); // 构造参数
  private Scope scope = Scope.SINGLETON; // 默认单例
  private boolean lazyInit = false; // 默认直接new
  // 省略必要的getter/setter/constructors
 
  public boolean isSingleton() { // 判断是否是单例
    return scope.equals(Scope.SINGLETON);
  }


  public static enum Scope { // 枚举
    SINGLETON,
    PROTOTYPE
  }
  
  public static class ConstructorArg { // 构造方法的参数信息
    private boolean isRef; // 是否有依赖类
    private Class type; // 参数类型
    private Object arg; // 值
    // 省略必要的getter/setter/constructors
  }
}
~~~

~~~java
// Bean工厂
public class BeansFactory {
  // 存放单例对象
  private ConcurrentHashMap<String, Object> singletonObjects = new ConcurrentHashMap<>();
  // 存放Bean的定义类
  private ConcurrentHashMap<String, BeanDefinition> beanDefinitions =
      												new ConcurrentHashMap<>();
  // 将Bean的集合添加到工厂，同时初始化所有非懒加载且单例的对象
  public void addBeanDefinitions(List<BeanDefinition> beanDefinitionList) {
    // List转map，key为Bean的唯一标识ID
    for (BeanDefinition beanDefinition : beanDefinitionList) {
      this.beanDefinitions.putIfAbsent(beanDefinition.getId(), beanDefinition);
    }
	// 遍历Bean的定义Map集合创建对象
    for (BeanDefinition beanDefinition : beanDefinitionList) {
      // 初始化所有非懒加载且单例的对象
      if (beanDefinition.isLazyInit() == false && beanDefinition.isSingleton()) {
        createBean(beanDefinition);
      }
    }
  }
  // 根据Bean的唯一标识获取Bean的定义，再去创建对象
  public Object getBean(String beanId) {
    BeanDefinition beanDefinition = beanDefinitions.get(beanId);
    if (beanDefinition == null) {
      throw new NoSuchBeanDefinitionException("Bean is not defined: " + beanId);
    }
    return createBean(beanDefinition);
  }

  // 根据Bean的定义创建对象
  @VisibleForTesting
  protected Object createBean(BeanDefinition beanDefinition) {
    // 是单例对象，其在单例集合中存在，则直接返回
    if (beanDefinition.isSingleton() 
        && singletonObjects.contains(beanDefinition.getId())) {
      return singletonObjects.get(beanDefinition.getId());
    }
	// 创建对象
    Object bean = null;
    try {
      Class beanClass = Class.forName(beanDefinition.getClassName());
      List<BeanDefinition.ConstructorArg> args = beanDefinition.getConstructorArgs();
      if (args.isEmpty()) { // 无参构造
        bean = beanClass.newInstance(); // 无参构造创建对象
      } else { // 有参构造
        Class[] argClasses = new Class[args.size()]; // 字节码对象集
        Object[] argObjects = new Object[args.size()]; // 构造参数集
        for (int i = 0; i < args.size(); ++i) {
          BeanDefinition.ConstructorArg arg = args.get(i);
          if (!arg.getIsRef()) { // 不存在依赖类，arg.getArg()就是值
            argClasses[i] = arg.getType();
            argObjects[i] = arg.getArg();
          } else { // 有依赖类，arg.getArg()就是Bean的唯一标识
            BeanDefinition refBeanDefinition = beanDefinitions.get(arg.getArg());
            if (refBeanDefinition == null) {
              throw new NoSuchBeanDefinitionException("Bean is not defined: " 
                                                      + arg.getArg());
            }
            argClasses[i] = Class.forName(refBeanDefinition.getClassName());
            argObjects[i] = createBean(refBeanDefinition);
          }
        }
        // 有参构造创建对象
        bean = beanClass.getConstructor(argClasses).newInstance(argObjects);
      }
    } catch (ClassNotFoundException | IllegalAccessException
            | InstantiationException | NoSuchMethodException | InvocationTargetException e) {
      throw new BeanCreationFailureException("", e);
    }
	// 如果bean不为空，且是单例，说明是第一次创建
    if (bean != null && beanDefinition.isSingleton()) {
      // 添加到单例集合，并返回
      singletonObjects.putIfAbsent(beanDefinition.getId(), bean);
      return singletonObjects.get(beanDefinition.getId());
    }
    // 否则就是懒加载的对象，或者多例对象
    return bean;
  }
}
~~~

#### 3、建造者模式

Builder模式（建造者模式），又称构建者模式、生成器模式。

##### 传统方式构建

- 需求：初始化对象，校验参数。

- 使用构造函数初始化对象：
  - 会造成参数过多过长，影响可读性，造成难以维护。
  - 容易搞错各参数的顺序，传递错误的参数值，导致非常隐蔽的Bug。
- 构造函数+setter方法
  - 构造函数初始化必填参数。
  - set方法设置其他参数，有默认值。
- 使用set初始化对象：
  - 必填参数，无法校验必填的参数值。
  - 参数之间有一定依赖关系，无法校验它们的依赖关系或约束条件。
  - 不可变对象，一定创建就不可以修改，所以不能暴露set方法。

##### Builder构建

~~~java

public class ResourcePoolConfig {
  private String name;
  private int maxTotal;
  private int maxIdle;
  private int minIdle;

  // 私有化构造，通过构造器来创建
  private ResourcePoolConfig(Builder builder) {
    this.name = builder.name;
    this.maxTotal = builder.maxTotal;
    this.maxIdle = builder.maxIdle;
    this.minIdle = builder.minIdle;
  }
  //...省略getter方法...

  //我们将Builder类设计成了ResourcePoolConfig的内部类。
  //我们也可以将Builder类设计成独立的非内部类ResourcePoolConfigBuilder。
  public static class Builder {
    private static final int DEFAULT_MAX_TOTAL = 8;
    private static final int DEFAULT_MAX_IDLE = 8;
    private static final int DEFAULT_MIN_IDLE = 0;

    private String name;
    private int maxTotal = DEFAULT_MAX_TOTAL;
    private int maxIdle = DEFAULT_MAX_IDLE;
    private int minIdle = DEFAULT_MIN_IDLE;

    public ResourcePoolConfig build() {
      // 校验逻辑放到这里来做，包括必填项校验、依赖关系校验、约束条件校验等
      if (StringUtils.isBlank(name)) {
        throw new IllegalArgumentException("...");
      }
      if (maxIdle > maxTotal) {
        throw new IllegalArgumentException("...");
      }
      if (minIdle > maxTotal || minIdle > maxIdle) {
        throw new IllegalArgumentException("...");
      }

      return new ResourcePoolConfig(this);
    }

    public Builder setName(String name) {
      if (StringUtils.isBlank(name)) {
        throw new IllegalArgumentException("...");
      }
      this.name = name;
      return this;
    }

    public Builder setMaxTotal(int maxTotal) {
      if (maxTotal <= 0) {
        throw new IllegalArgumentException("...");
      }
      this.maxTotal = maxTotal;
      return this;
    }

    public Builder setMaxIdle(int maxIdle) {
      if (maxIdle < 0) {
        throw new IllegalArgumentException("...");
      }
      this.maxIdle = maxIdle;
      return this;
    }

    public Builder setMinIdle(int minIdle) {
      if (minIdle < 0) {
        throw new IllegalArgumentException("...");
      }
      this.minIdle = minIdle;
      return this;
    }
  }
}

// 这段代码会抛出IllegalArgumentException，因为minIdle>maxIdle
ResourcePoolConfig config = new ResourcePoolConfig.Builder()
        .setName("dbconnectionpool")
        .setMaxTotal(16)
        .setMaxIdle(10)
        .setMinIdle(12)
        .build();
~~~

- 避免了无效状态，也就是没有初始化完全的对象，是不能使用的。
  - 注意：这里的set返回的this是构建者对象，而非资源配置类对象。
- 有点重复，构建者类重复定义了配置类的属性。
  - 所以，不是很复杂的构建逻辑、校验逻辑，一般使用构造函数即可。

##### 区别工厂模式

- 工厂模式
  - 是为了创建不同但是相关的对象。
    - 相关：继承同一父类或者接口的一组子类。
- Builder模式
  - 是为了创建相同对象但不同参数的对象，也就是定制化创建不同的对象。
    - 不同参数：通过set方法设置不同的可选参数。

#### 4、原型模式

原型设计模式（Prototype Design Pattern），简称原型模式。

##### 介绍

- 作用：对已有对象进行复制或拷贝的方式来创建新对象。
- 使用方式：
  - 浅拷贝（Shallow Copy）：只拷贝地址，不拷贝对象。速度快。
    - `Object.clone();`
  - 深拷贝（Deep Copy）：地址和对象都拷贝，相对于浅拷贝，对内存、时间消耗大。
    - 递归拷贝对象。
    - 序列化/反序列化。
- 使用场景：
  - 创建对象包含的申请内存、给成员变量赋值这一过程，本身并不会花费太多时间，或者说，这点时间完全是可以忽略的。应用一个复杂的模式，只得到一点点的性能提升，这就是所谓的过度设计。
  - 如果对象中的数据要经过复杂的计算才能得到，或者需要从RPC、网络、数据库、文件系统等非常慢速的IO中读取，这种情况下就可以利用原型模式。

##### 浅拷贝

~~~java
public class Demo {
  private HashMap<String, SearchWord> currentKeywords=new HashMap<>();
  private long lastUpdateTime = -1;

  public void refresh() {
    // 原型模式，拷贝已有对象的数据，更新少量差值
    HashMap<String, SearchWord> newKeywords = 
        (HashMap<String, SearchWord>) currentKeywords.clone();

    // 从数据库中取出更新时间>lastUpdateTime的数据，放入到newKeywords中
    List<SearchWord> toBeUpdatedSearchWords = getSearchWords(lastUpdateTime);
    long maxNewUpdatedTime = lastUpdateTime;
    for (SearchWord searchWord : toBeUpdatedSearchWords) {
      // 最后更新时间
      if (searchWord.getLastUpdateTime() > maxNewUpdatedTime) {
        maxNewUpdatedTime = searchWord.getLastUpdateTime();
      }
      // 更新数据，包含这个key，就修改搜索词
      if (newKeywords.containsKey(searchWord.getKeyword())) {
        // 得到旧的搜索词
        SearchWord oldSearchWord = newKeywords.get(searchWord.getKeyword());
        oldSearchWord.setCount(searchWord.getCount());
        oldSearchWord.setLastUpdateTime(searchWord.getLastUpdateTime());
      } else {// 不包含，就将查询到搜索词放入
        newKeywords.put(searchWord.getKeyword(), searchWord);
      }
    }

    lastUpdateTime = maxNewUpdatedTime;
    currentKeywords = newKeywords;
  }

  private List<SearchWord> getSearchWords(long lastUpdateTime) {
    // TODO: 从数据库中取出更新时间>lastUpdateTime的数据
    return null;
  }
}
~~~

##### 递归拷贝

~~~java
public class Demo {
  private HashMap<String, SearchWord> currentKeywords=new HashMap<>();
  private long lastUpdateTime = -1;

  public void refresh() {
    // Deep copy
    HashMap<String, SearchWord> newKeywords = new HashMap<>(); // 新map
    for (HashMap.Entry<String, SearchWord> e : currentKeywords.entrySet()) {
      SearchWord searchWord = e.getValue();
      // 新关键词对象
      SearchWord newSearchWord = new SearchWord(
          searchWord.getKeyword(),
          searchWord.getCount(),
          searchWord.getLastUpdateTime());
      newKeywords.put(e.getKey(), newSearchWord);
    }

    // 从数据库中取出更新时间>lastUpdateTime的数据，放入到newKeywords中
    List<SearchWord> toBeUpdatedSearchWords = getSearchWords(lastUpdateTime);
    long maxNewUpdatedTime = lastUpdateTime;
    for (SearchWord searchWord : toBeUpdatedSearchWords) {
      if (searchWord.getLastUpdateTime() > maxNewUpdatedTime) {
        maxNewUpdatedTime = searchWord.getLastUpdateTime();
      }
      if (newKeywords.containsKey(searchWord.getKeyword())) {
        SearchWord oldSearchWord = newKeywords.get(searchWord.getKeyword());
        oldSearchWord.setCount(searchWord.getCount());
        oldSearchWord.setLastUpdateTime(searchWord.getLastUpdateTime());
      } else {
        newKeywords.put(searchWord.getKeyword(), searchWord);
      }
    }

    lastUpdateTime = maxNewUpdatedTime;
    currentKeywords = newKeywords;
  }

  private List<SearchWord> getSearchWords(long lastUpdateTime) {
    // TODO: 从数据库中取出更新时间>lastUpdateTime的数据
    return null;
  }

}
~~~

##### 序列化/反序列化

~~~java
public Object deepCopy(Object object) {
    // 序列化对象，写入流
    ByteArrayOutputStream bo = new ByteArrayOutputStream();
    ObjectOutputStream oo = new ObjectOutputStream(bo);
    oo.writeObject(object);
    // 读取流
    ByteArrayInputStream bi = new ByteArrayInputStream(bo.toByteArray());
    ObjectInputStream oi = new ObjectInputStream(bi);
    // 反序列化成对象
    return oi.readObject();
}
~~~

##### 浅/深拷贝

- 先用浅拷贝复制map集合，再对少量更新的关键词对象做深拷贝。

~~~java
public class Demo {
  private HashMap<String, SearchWord> currentKeywords=new HashMap<>();
  private long lastUpdateTime = -1;

  public void refresh() {
    // Shallow copy
    HashMap<String, SearchWord> newKeywords = (HashMap<String, SearchWord>) currentKeywords.clone();

    // 从数据库中取出更新时间>lastUpdateTime的数据，放入到newKeywords中
    List<SearchWord> toBeUpdatedSearchWords = getSearchWords(lastUpdateTime);
    long maxNewUpdatedTime = lastUpdateTime;
    for (SearchWord searchWord : toBeUpdatedSearchWords) {
      if (searchWord.getLastUpdateTime() > maxNewUpdatedTime) {
        maxNewUpdatedTime = searchWord.getLastUpdateTime();
      }
      if (newKeywords.containsKey(searchWord.getKeyword())) {
        newKeywords.remove(searchWord.getKeyword()); // 删除旧数据
      }
      // put新数据
      newKeywords.put(searchWord.getKeyword(), searchWord);
    }

    lastUpdateTime = maxNewUpdatedTime;
    currentKeywords = newKeywords;
  }

  private List<SearchWord> getSearchWords(long lastUpdateTime) {
    // TODO: 从数据库中取出更新时间>lastUpdateTime的数据
    return null;
  }
}
~~~

- 浅拷贝的如果是不可变对象，那么共享不可变对象是没问题的，但对于可变对象，就有可能出现数据被修改的风险。
- 如果不是很耗时的操作，一般情况下推荐使用深拷贝，不要为了一点点性能而使用浅拷贝。

### 结构型

结构型设计模式主要解决的是“类或对象的组合或组装”问题。

#### 1、代理模式

代理模式（Proxy Design Pattern），在不改变原始类代码的情况下，通过引入代理类来给原始类附加功能。

想想你所依赖的东西，看它是否有一天会离你而去，或是你离它而去。如果真是这样，那么请向它学习，直到能够脱离它。

##### 静态代理

- 基于接口
  - 代理类和原始类都继承同一接口，在代理类中注入原始类。
  - 请求的是代理类，实际上还是委托原始类进行的业务操作，只不过可以在调用前后做些统计之类的扩展功能。
  - 基于接口而非实现，这样我们在使用时就可以把代理类当做原始类来使用了。
- 基于继承
  - 如果原始类并没有定义接口，或者是它来自一个第三方的类库。
  - 原理和实现基本于接口一致，因为都是利用了多态。

##### 动态代理（Dynamic Proxy）

java动态代理

Spring AoP基于java动态代理，在无接口下是使用cglib

- jdk动态代理是利用反射机制生成一个实现代理接口的匿名类，在调用具体方法前调用InvokeHandler来处理。
- 而cglib动态代理是利用asm开源包，对被代理对象类的class文件加载进来，通过修改其字节码生成子类来处理。


使用场景

- 非功能性需求开发
  - 监控、统计、鉴权、限流、事务、幂等、日志。
- RPC、缓存
  - RPC框架也可以看作一种代理模式，远程代理。
  - 动态加载需要支持缓存的接口。


~~~java
public class MetricsCollectorProxy {
  private MetricsCollector metricsCollector;

  public MetricsCollectorProxy() {
    this.metricsCollector = new MetricsCollector();
  }

  public Object createProxy(Object proxiedObject) {
    // 获取该类的所有接口
    Class<?>[] interfaces = proxiedObject.getClass().getInterfaces();
    // 创建动态代理处理器
    DynamicProxyHandler handler = new DynamicProxyHandler(proxiedObject);
    // 创建java动态代理，获取类加载器，传入接口，处理器类
    return Proxy.newProxyInstance(proxiedObject.getClass().getClassLoader(), interfaces, handler);
  }

  private class DynamicProxyHandler implements InvocationHandler {
    private Object proxiedObject;

    public DynamicProxyHandler(Object proxiedObject) {
      this.proxiedObject = proxiedObject;
    }

    // 扩展功能
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
      long startTimestamp = System.currentTimeMillis();
      Object result = method.invoke(proxiedObject, args);
      long endTimeStamp = System.currentTimeMillis();
      long responseTime = endTimeStamp - startTimestamp;
      String apiName = proxiedObject.getClass().getName() + ":" + method.getName();
      RequestInfo requestInfo = new RequestInfo(apiName, responseTime, startTimestamp);
      metricsCollector.recordRequest(requestInfo);
      return result;
    }
  }
}

//MetricsCollectorProxy使用举例
MetricsCollectorProxy proxy = new MetricsCollectorProxy();
IUserController userController = (IUserController) proxy.createProxy(new UserController());
~~~

#### 2、桥接模式

桥接模式（Bridge Design Pattern），两种理解方式：

- 将抽象和实现解耦，让他们独立变化；

- 一个类存在两个（或多个）独立变化的维度，我们通过组合的方式，让这两个（或多个）维度可以独立进行扩展。

##### JDBC

- java.sql.Driver接口是标准，负责对接不同数据库厂商的驱动。
- 这个例子比较贴近第一个解释。

~~~java
package com.mysql.jdbc;
import java.sql.SQLException;
public class Driver extends NonRegisteringDriver implements java.sql.Driver {
  static {
    try {
      java.sql.DriverManager.registerDriver(new Driver());
    } catch (SQLException E) {
      throw new RuntimeException("Can't register driver!");
    }
  }
  public Driver() throws SQLException {
  }
}
~~~

- Class.forName("com.mysql.jdbc.Driver");
  - 加载驱动类。
- com.mysql.jdbc.Driver
  - 执行静态代码块里面的代码，注册了驱动。
- 由于继承自NonRegisteringDriver，所以该类拥有父类所有的功能。
- 由于实现了java.sql.Driver，所以在实际的使用中，都是通过注册的mysql驱动委托执行。

##### API告警

API 接口监控告警例子，比较贴近第二个解释。

~~~java
// 通知的紧急程度枚举
public enum NotificationEmergencyLevel {
  SEVERE, URGENCY, NORMAL, TRIVIAL
}
// 通知
public class Notification {
  private List<String> emailAddresses;
  private List<String> telephones;
  private List<String> wechatIds;

  public Notification() {}
  // 设置邮件地址
  public void setEmailAddress(List<String> emailAddress) {
    this.emailAddresses = emailAddress;
  }
  // 设置语言电话
  public void setTelephones(List<String> telephones) {
    this.telephones = telephones;
  }
  // 设置微信号
  public void setWechatIds(List<String> wechatIds) {
    this.wechatIds = wechatIds;
  }
  // 通知，根据紧急级别发送对应的消息
  public void notify(NotificationEmergencyLevel level, String message) {
    if (level.equals(NotificationEmergencyLevel.SEVERE)) {
      //...自动语音电话
    } else if (level.equals(NotificationEmergencyLevel.URGENCY)) {
      //...发微信
    } else if (level.equals(NotificationEmergencyLevel.NORMAL)) {
      //...发邮件
    } else if (level.equals(NotificationEmergencyLevel.TRIVIAL)) {
      //...发邮件
    }
  }
}

// 错误警告处理
public class ErrorAlertHandler extends AlertHandler {
  public ErrorAlertHandler(AlertRule rule, Notification notification){
    super(rule, notification);
  }


  @Override
  public void check(ApiStatInfo apiStatInfo) {
    // 如果api错误的数量大于所设置最大错误数
    if (apiStatInfo.getErrorCount() > 
        			rule.getMatchedRule(apiStatInfo.getApi()).getMaxErrorCount()) {
      // 那么就属于严重情况，发送对应的消息
      notification.notify(NotificationEmergencyLevel.SEVERE, "...");
    }
  }
}
~~~

由于发送消息的逻辑比较复杂，可以抽取出来。

- 把通知看作抽象，把消息看作实现，相分离，通过任意组合（桥梁）在一起。
- 通知与消息的对应关系可以通过配置文件动态指定。

~~~java
// 消息发送人
public interface MsgSender {
  void send(String message);
}
// 语音电话消息
public class TelephoneMsgSender implements MsgSender {
  private List<String> telephones;

  public TelephoneMsgSender(List<String> telephones) {
    this.telephones = telephones;
  }

  @Override
  public void send(String message) {
    //...
  }

}
// 邮件消息
public class EmailMsgSender implements MsgSender {
  // 省略...
}
// 微信消息
public class WechatMsgSender implements MsgSender {
  // 省略...
}
// 通知
public abstract class Notification {
  protected MsgSender msgSender;

  public Notification(MsgSender msgSender) {
    this.msgSender = msgSender;
  }

  public abstract void notify(String message);
}
// 严重通知
public class SevereNotification extends Notification {
  public SevereNotification(MsgSender msgSender) {
    super(msgSender);
  }

  @Override
  public void notify(String message) {
    msgSender.send(message);
  }
}
// 紧急通知
public class UrgencyNotification extends Notification {
  // 省略...
}
// 普通通知
public class NormalNotification extends Notification {
  // 省略...
}
// 无关紧要通知
public class TrivialNotification extends Notification {
  // 省略...
}
~~~

#### 3、装饰器模式

装饰器模式（Decorator Pattern），基于组合，对功能的增强。

##### 基于继承的缺点

- 如果要实现对文件的读取，`InputStream`的子类`FileInputStream`类可以很好的完成。
- 如果想在此基础上再按照基本数据类型（int、boolean、long等）来读取数据。
  - `DataInputStream`具备以上功能，按照继承则需要再派生出：`DataFileInputStream`、`DataPipedInputStream`等类。
- 如果还想再支持缓存读取功能，`BufferedInputStream`具备。
  - 则需要再派生出`BufferedDataFileInputStream`、`BufferedDataPipedInputStream`等类。
- 仅仅附加了两个功能，就让类继承结构变得无比复制，所以不推荐使用继承。

##### 基于组合

~~~java
public abstract class InputStream {
  //...
    
  // 读取方法
  public int read(byte b[]) throws IOException {
    return read(b, 0, b.length);
  }
  public int read(byte b[], int off, int len) throws IOException {
    //...
  }
  // 跳过n个字节
  public long skip(long n) throws IOException {
    //...
  }
  // 对于本地io：得到文件的大小，对于网络io：小于文件大小
  public int available() throws IOException {
    return 0;
  }
  // 关闭流
  public void close() throws IOException {}
  // 标记
  public synchronized void mark(int readlimit) {}
  // 回到标记位置
  public synchronized void reset() throws IOException {
    throw new IOException("mark/reset not supported");
  }
  // 是否支持标记
  public boolean markSupported() {
    return false;
  }
}
// 支持缓存读取
public class BufferedInputStream extends InputStream {
  protected volatile InputStream in;

  protected BufferedInputStream(InputStream in) {
    this.in = in;
  }
  
  //...实现基于缓存的读数据接口...  
}
// 支持基本数据类型读取
public class DataInputStream extends InputStream {
  protected volatile InputStream in;

  protected DataInputStream(InputStream in) {
    this.in = in;
  }
  
  //...实现读取基本类型数据的接口
}
~~~

##### 特殊1





- 装饰器类和原始类继承同样的父类，这样就可以对原始类“嵌套”多个装饰器类。

~~~java
InputStream in = new FileInputStream("C:/test.txt"); // 原始类
InputStream bin = new BufferedInputStream(in); // 支持缓存
DataInputStream din = new DataInputStream(bin); // 支持按照基本数据类型读取
int data = din.readInt();
~~~

##### 特殊2

装饰器类是对功能的增强。

~~~java
// 装饰器模式的代码结构(下面的接口也可以替换成抽象类)
public interface IA {
    void f();
}
public class A impelements IA {
    public void f() {
        //... 
    }
}
public class ADecorator impements IA {
    private IA a;
    public ADecorator(IA a) {
        this.a = a;
    }
    public void f() {
        // 功能增强代码
        a.f();
        // 功能增强代码 
    }
}
~~~

##### 特殊3

- 查看源码发现：`BufferedInputStream`和`DataInputStream`都继承自`FilterInputStream`
  - `FilterInputStream`继承自`InputStream`
- 如果这两个类直接继承`InputStream`，那么在那些不需要增强的方法，也需要进行委托实现，否则无法完成读取任务，因为调用的是组合的`InputStream`。
- 所以为了避免这种情况的发生，抽象出来一个装饰器类`FilterInputStream`。
  - `InputStream`的所有装饰器类都继承自这个装饰器父类。这样，装饰器类只需要实现它增强的方法。
  - 比如：`BufferedInputStream`、`DataInputStream`等

~~~java
public class BufferedInputStream extends FilterInputStream {
	public BufferedInputStream(InputStream in) {
        this(in, DEFAULT_BUFFER_SIZE);
    }
	public BufferedInputStream(InputStream in, int size) {
        super(in); // 传递给父类
        if (size <= 0) {
            throw new IllegalArgumentException("Buffer size <= 0");
        }
        buf = new byte[size];
    }
}
~~~

~~~java
// 而父类装饰了InputStream所有的方法
// 所以其子类只需要装饰需要的方法即可，无需再装饰不需要增强的方法了
public class FilterInputStream extends InputStream {
  protected volatile InputStream in;

  protected FilterInputStream(InputStream in) {
    this.in = in;
  }

  public int read() throws IOException {
    return in.read();
  }

  public int read(byte b[]) throws IOException {
    return read(b, 0, b.length);
  }
   
  public int read(byte b[], int off, int len) throws IOException {
    return in.read(b, off, len);
  }

  public long skip(long n) throws IOException {
    return in.skip(n);
  }

  public int available() throws IOException {
    return in.available();
  }

  public void close() throws IOException {
    in.close();
  }

  public synchronized void mark(int readlimit) {
    in.mark(readlimit);
  }

  public synchronized void reset() throws IOException {
    in.reset();
  }

  public boolean markSupported() {
    return in.markSupported();
  }
}
~~~

- 解决继承关系过于复杂的问题。
- 通过组合来替代继承。
- 其主要作用是给原始类添加增强功能。
- 装饰器类和原始类都继承自相同抽象类或接口，就可以嵌套使用。

#### 4、适配器模式

适配器模式（Adapter Design Pattern），USB转接头充当适配器，将两种不兼容的接口，通过转接变得可以一起工作。

##### 类适配器

~~~java
// 类适配器: 基于继承
public interface ITarget { // 目标类
  void f1();
  void f2();
  void fc();
}

public class Adaptee { // 改造类
  public void fa() { //... }
  public void fb() { //... }
  public void fc() { //... }
}
// 适配类，使Adaptee兼容ITarget
public class Adaptor extends Adaptee implements ITarget {
  public void f1() { // 直接调用父类方法
    super.fa();
  }
  
  public void f2() { // 重新实现
    //...重新实现f2()...
  }
  
  // 这里fc()不需要实现，直接继承自Adaptee，这是跟对象适配器最大的不同点
}
~~~

- 对于不适配的方法，通过直接调用父类方法，屏蔽掉方法名的歧义，同时还可以对方法进行增强。

- 可选择地实现。不实现则默认调用父类的方法，方便了开发。

##### 对象适配器

~~~java
// 对象适配器：基于组合
public interface ITarget {
  void f1();
  void f2();
  void fc();
}

public class Adaptee {
  public void fa() { //... }
  public void fb() { //... }
  public void fc() { //... }
}

public class Adaptor implements ITarget {
  private Adaptee adaptee;
  
  public Adaptor(Adaptee adaptee) {
    this.adaptee = adaptee;
  }
  
  public void f1() {
    adaptee.fa(); //委托给Adaptee
  }
  
  public void f2() {
    //...重新实现f2()...
  }
  
  public void fc() {
    adaptee.fc();
  }
}
~~~

- 基于组合的方式更加灵活，可以适配多个接口。
- 但是，需要实现所有的适配方法。

##### 如何选择

- 判断的标准有两个
  - Adaptee接口的个数。
  - Adaptee和ITarget的契合程度。
- 如果接口不多，两种实现都可以。
- 如果接口很多，且两者接口定义大部分相同，推荐使用类适配器，因为更加复用。
- 如果接口很多，且两者接口定义大部分不相同，推荐使用对象适配器，因为组合相对于继承更加灵活。

##### 应用场景

接口不兼容

- 封装有缺陷的接口设计
  - 隔离设计上的缺陷，对外部系统接口进行二次封装。

~~~java
public class CD { //这个类来自外部sdk，我们无权修改它的代码
  //...
  public static void staticFunction1() { //... }
  
  public void uglyNamingFunction2() { //... }

  public void tooManyParamsFunction3(int paramA, int paramB, ...) { //... }
  
   public void lowPerformanceFunction4() { //... }
}

// 使用适配器模式进行重构
public class ITarget {
  void function1();
  void function2();
  void fucntion3(ParamsWrapperDefinition paramsWrapper);
  void function4();
  //...
}
// 注意：适配器类的命名不一定非得末尾带Adaptor
public class CDAdaptor extends CD implements ITarget {
  //...
  public void function1() {
     super.staticFunction1();
  }
  
  public void function2() {
    super.uglyNamingFucntion2();
  }
  
  public void function3(ParamsWrapperDefinition paramsWrapper) {
     super.tooManyParamsFunction3(paramsWrapper.getParamA(), ...);
  }
  
  public void function4() {
    //...reimplement it...
  }
}
~~~

- 统一多个类的接口设计
  - 如果某功能依赖多个外部系统，则可以将它们的接口适配为统一的接口。

~~~java
public class ASensitiveWordsFilter { // A敏感词过滤系统提供的接口
  //text是原始文本，函数输出用***替换敏感词之后的文本
  public String filterSexyWords(String text) {
    // ...
  }
  public String filterPoliticalWords(String text) {
    // ...
  } 
}
public class BSensitiveWordsFilter  { // B敏感词过滤系统提供的接口
  public String filter(String text) {
    //...
  }
}
public class CSensitiveWordsFilter { // C敏感词过滤系统提供的接口
  public String filter(String text, String mask) {
    //...
  }
}

// 未使用适配器模式之前的代码：代码的可测试性、扩展性不好
public class RiskManagement {
  private ASensitiveWordsFilter aFilter = new ASensitiveWordsFilter();
  private BSensitiveWordsFilter bFilter = new BSensitiveWordsFilter();
  private CSensitiveWordsFilter cFilter = new CSensitiveWordsFilter();
  
  public String filterSensitiveWords(String text) {
    String maskedText = aFilter.filterSexyWords(text);
    maskedText = aFilter.filterPoliticalWords(maskedText);
    maskedText = bFilter.filter(maskedText);
    maskedText = cFilter.filter(maskedText, "***");
    return maskedText;
  }
}

// 使用适配器模式进行改造
public interface ISensitiveWordsFilter { // 统一接口定义
  String filter(String text);
}

public class ASensitiveWordsFilterAdaptor implements ISensitiveWordsFilter {
  private ASensitiveWordsFilter aFilter;
  public String filter(String text) {
    String maskedText = aFilter.filterSexyWords(text);
    maskedText = aFilter.filterPoliticalWords(maskedText);
    return maskedText;
  }
}
//...省略BSensitiveWordsFilterAdaptor、CSensitiveWordsFilterAdaptor...

// 扩展性更好，更加符合开闭原则，如果添加一个新的敏感词过滤系统，
// 这个类完全不需要改动；而且基于接口而非实现编程，代码的可测试性更好。
public class RiskManagement { 
  private List<ISensitiveWordsFilter> filters = new ArrayList<>();
 
  public void addSensitiveWordsFilter(ISensitiveWordsFilter filter) {
    filters.add(filter);
  }
  
  public String filterSensitiveWords(String text) {
    String maskedText = text;
    for (ISensitiveWordsFilter filter : filters) {
      maskedText = filter.filter(maskedText);
    }
    return maskedText;
  }
}
~~~

- 替换依赖的外部系统
  - 利用适配器模式，把项目中依赖的一个外部系统替换成另一个外部系统时，可以减少对代码的改动。

~~~java
// 外部系统A
public interface IA {
  //...
  void fa();
}
public class A implements IA {
  //...
  public void fa() { //... }
}
// 在我们的项目中，外部系统A的使用示例
public class Demo {
  private IA a;
  public Demo(IA a) {
    this.a = a;
  }
  //...
}
Demo d = new Demo(new A());

// 将外部系统A替换成外部系统B
public class BAdaptor implemnts IA {
  private B b;
  public BAdaptor(B b) {
    this.b= b;
  }
  public void fa() {
    //...
    b.fb();
  }
}
// 借助BAdaptor，Demo的代码中，调用IA接口的地方都无需改动，
// 只需要将BAdaptor如下注入到Demo即可。
Demo d = new Demo(new BAdaptor(new B()));
~~~

- 兼容老版本接口
  - 在做版本升级的时候，对于一些要废弃的接口，不能直接将其删除，而是暂时保留。
  - 并且标注为deprecated，并将内部实现逻辑委托为新的接口实现。
- 适配不同格式的数据
  - 如：`List stooges = Arrays.asList("Larry", "Moe", "Curly");`

##### Java日志类

- Slf4j是java提供的一套打印日志的统一接口规范。
- Slf4j只有接口，没有具体的实现，需要配合其他日志框架（log4j、logback）来使用。
- 由于Slf4j的出现晚于JUL、JCL、log4j等框架，所以Slf4j还提供了针对不同日志框架的适配器。
- 如果想从JCL切换到log4j怎么办？
  - Slf4j还提供了反向适配器，先将JCL切换为Slf4j，再将Slf4j切换为log4j。

~~~java
// slf4j统一的接口定义
package org.slf4j;
public interface Logger {
  public boolean isTraceEnabled();
  public void trace(String msg);
  public void trace(String format, Object arg);
  public void trace(String format, Object arg1, Object arg2);
  public void trace(String format, Object[] argArray);
  public void trace(String msg, Throwable t);
 
  public boolean isDebugEnabled();
  public void debug(String msg);
  public void debug(String format, Object arg);
  public void debug(String format, Object arg1, Object arg2)
  public void debug(String format, Object[] argArray)
  public void debug(String msg, Throwable t);

  //...省略info、warn、error等一堆接口
}

// log4j日志框架的适配器
// Log4jLoggerAdapter实现了LocationAwareLogger接口，
// 其中LocationAwareLogger继承自Logger接口，
// 也就相当于Log4jLoggerAdapter实现了Logger接口。
package org.slf4j.impl;
public final class Log4jLoggerAdapter extends MarkerIgnoringBase
  implements LocationAwareLogger, Serializable {
  final transient org.apache.log4j.Logger logger; // log4j
 
  public boolean isDebugEnabled() {
    return logger.isDebugEnabled();
  }
 
  public void debug(String msg) {
    logger.log(FQCN, Level.DEBUG, msg, null);
  }
 
  public void debug(String format, Object arg) {
    if (logger.isDebugEnabled()) {
      FormattingTuple ft = MessageFormatter.format(format, arg);
      logger.log(FQCN, Level.DEBUG, ft.getMessage(), ft.getThrowable());
    }
  }
 
  public void debug(String format, Object arg1, Object arg2) {
    if (logger.isDebugEnabled()) {
      FormattingTuple ft = MessageFormatter.format(format, arg1, arg2);
      logger.log(FQCN, Level.DEBUG, ft.getMessage(), ft.getThrowable());
    }
  }
 
  public void debug(String format, Object[] argArray) {
    if (logger.isDebugEnabled()) {
      FormattingTuple ft = MessageFormatter.arrayFormat(format, argArray);
      logger.log(FQCN, Level.DEBUG, ft.getMessage(), ft.getThrowable());
    }
  }
 
  public void debug(String msg, Throwable t) {
    logger.log(FQCN, Level.DEBUG, msg, t);
  }
  //...省略一堆接口的实现...
}
~~~

##### 4种模式的区别

- 代理
  - 不改变原始类接口的条件下，为原始类定义一个代理类。
- 桥接
  - 将接口和实现分离，让他们可以比较容易、也相对独立地加以改变。
- 装饰器
  - 在不改变原始类接口的情况下，对原始类进行增强，支持多个装饰器嵌套使用。
- 适配器
  - 一种事后的补救策略。适配器提供跟原始类不同的接口，而代理、装饰器模式提供的都是跟原始类相同的接口。

#### 5、门面模式

门面模式（Facade Design Pattern）也叫外观模式，它为子系统提供一组统一的接口，定义一组高层接口让子系统更易用。

##### 应用场景

- 解决易用性问题
  - 封装系统底层实现，隐藏系统的复杂性，通过更简单、更高层次的接口。
  - 如：Linux暴露给开发者的一组编程接口，它封装了底层更基础的Linux内核调用。
  - 如：Linux的Shell命令，它继续封了装系统调用，提供了更友好、简单的命令来与操作系统交互。
- 解决性能问题
  - 通过门面接口只需要调用一次，这就大大减少了网络通信的开销，提高了客户端访问的速度。
  - 组织门面接口
    - 如果接口不多，完全可以将它跟门面接口放在一起（不区分）。
    - 如果接口很多，可以在已有接口上再抽象出一层，专门放置门面接口（做区分）。
    - 如果接口特别多，并且很多时跨多个子系统的，可以将门面接口放到一个新的子系统中（划分子系统）。
- 解决分布式事务问题
  - 两个操作在同一接口中完成，通过数据库事务或者Spring框架提供的事务。

##### 总结

- 尽量保持接口的可复用性。（迪米特法则的最小知识原则、接口隔离原则）
- 针对特殊情况，允许提供冗余的门面接口。（封装、抽象设计思想）

#### 6、组合模式

组合模式（Composite Design Pattern），将一组对象组织成树形结构，以表示一种“部分-整体”的层次结构。

- 对业务场景的一种数据结构和算法的抽象。
  - 数据可以表示成树这种数据结构。
  - 业务需求可以通过在树上的递归遍历算法来实现。

##### 应用场景

- OA系统（办公自动化系统）
  - 公司的组织结构包含部门和员工两种数据类型。
    - 部门可以包含子部门和员工。

~~~java
// 资源管理
public abstract class HumanResource {
  protected long id;
  protected double salary;

  public HumanResource(long id) {
    this.id = id;
  }

  public long getId() {
    return id;
  }

  public abstract double calculateSalary();
}
// 员工
public class Employee extends HumanResource {
  public Employee(long id, double salary) {
    super(id);
    this.salary = salary;
  }

  @Override
  public double calculateSalary() {
    return salary; 
  }
}
// 部门
public class Department extends HumanResource {
  private List<HumanResource> subNodes = new ArrayList<>(); // 组合资源，包括子部门和员工

  public Department(long id) {
    super(id);
  }
  // 递归计算总薪资开支
  @Override
  public double calculateSalary() {
    double totalSalary = 0;
    for (HumanResource hr : subNodes) {
      totalSalary += hr.calculateSalary();
    }
    this.salary = totalSalary;
    return totalSalary;
  }
  // 添加子节点
  public void addSubNode(HumanResource hr) {
    subNodes.add(hr);
  }
}

// 构建组织架构的代码
public class Demo {
  private static final long ORGANIZATION_ROOT_ID = 1001; // 根id
  private DepartmentRepo departmentRepo; // 依赖注入
  private EmployeeRepo employeeRepo; // 依赖注入
  // 构建资源
  public void buildOrganization() {
    Department rootDepartment = new Department(ORGANIZATION_ROOT_ID);
    buildOrganization(rootDepartment);
  }
  // 根据部门构造其组织架构
  private void buildOrganization(Department department) {
    // 根据部门id，查询所有子部门id，并组合所以部门
    List<Long> subDepartmentIds = departmentRepo.getSubDepartmentIds(department.getId());
    for (Long subDepartmentId : subDepartmentIds) {
      Department subDepartment = new Department(subDepartmentId);
      department.addSubNode(subDepartment);
      buildOrganization(subDepartment);
    }
    // 根据部门id，查询所有员工id，并组合所有员工
    List<Long> employeeIds = employeeRepo.getDepartmentEmployeeIds(department.getId());
    for (Long employeeId : employeeIds) {
      double salary = employeeRepo.getEmployeeSalary(employeeId);
      department.addSubNode(new Employee(employeeId, salary));
    }
  }
}
~~~

#### 7、享元模式

享元模式（Flyweight Design Pattern），共享单元，复用对象，节省内存。前提是重复的对象是不可变对象，利用享元模式将对象设计成享元，在内存中只保存一份实例，供多处代码引用。

##### 棋盘

- 棋子类
  - id、text、color、positionX、positionY。
- 其中id、text、color是不变的，可以单独抽出来作为享元类。
- 假设一个棋盘最多有棋子30个，那么只需要创建30个享元类，即可复用任意个棋盘的所有棋子。

~~~java
// 享元类
public class ChessPieceUnit {
  private int id;
  private String text;
  private Color color;
  public ChessPieceUnit(int id, String text, Color color) {
    this.id = id;
    this.text = text;
    this.color = color;
  }
  public static enum Color {
    RED, BLACK
  }
  // ...省略其他属性和getter方法...
}
// 享元工厂类
public class ChessPieceUnitFactory {
  private static final Map<Integer, ChessPieceUnit> pieces = new HashMap<>();
  // 初始化享元
  static {
    pieces.put(1, new ChessPieceUnit(1, "車", ChessPieceUnit.Color.BLACK));
    pieces.put(2, new ChessPieceUnit(2,"馬", ChessPieceUnit.Color.BLACK));
    //...省略摆放其他棋子的代码...
  }
  // 获取享元对象
  public static ChessPieceUnit getChessPiece(int chessPieceId) {
    return pieces.get(chessPieceId);
  }
}
// 棋子类
public class ChessPiece {
  private ChessPieceUnit chessPieceUnit; // 组合享元类
  private int positionX;
  private int positionY;
  // 构造棋子
  public ChessPiece(ChessPieceUnit unit, int positionX, int positionY) {
    this.chessPieceUnit = unit;
    this.positionX = positionX;
    this.positionY = positionY;
  }
  // 省略getter、setter方法
}
// 棋盘类
public class ChessBoard {
  private Map<Integer, ChessPiece> chessPieces = new HashMap<>(); // 存放棋子的棋盘
  // 构造初始化
  public ChessBoard() {
    init();
  }

  // 根据id获取对应的棋子，并设置其所在位置，最后将其添加到棋盘上
  private void init() {
    chessPieces.put(1, new ChessPiece(
            ChessPieceUnitFactory.getChessPiece(1), 0,0));
    chessPieces.put(1, new ChessPiece(
            ChessPieceUnitFactory.getChessPiece(2), 1,0));
    //...省略摆放其他棋子的代码...
  }
  // 根据棋子Id移动棋子
  public void move(int chessPieceId, int toPositionX, int toPositionY) {
    //...省略...
  }
}
~~~

##### 文本编辑器

- 功能：
  - 记录文字。
  - 格式。

~~~java
// 享元类 样式
public class CharacterStyle {
  private Font font;
  private int size;
  private int colorRGB;
  // 构造样式
  public CharacterStyle(Font font, int size, int colorRGB) {
    this.font = font;
    this.size = size;
    this.colorRGB = colorRGB;
  }
  // 比较，是否有更改样式
  @Override
  public boolean equals(Object o) {
    CharacterStyle otherStyle = (CharacterStyle) o;
    return font.equals(otherStyle.font)
            && size == otherStyle.size
            && colorRGB == otherStyle.colorRGB;
  }
}
// 样式工厂
public class CharacterStyleFactory {
  private static final List<CharacterStyle> styles = new ArrayList<>(); // 样式集合
  // 获取样式
  public static CharacterStyle getStyle(Font font, int size, int colorRGB) {
    CharacterStyle newStyle = new CharacterStyle(font, size, colorRGB); // 创建样式
    // 存在就直接返回
    for (CharacterStyle style : styles) {
      if (style.equals(newStyle)) {
        return style;
      }
    }
    // 否则将添加到样式集合，并返回样式
    styles.add(newStyle);
    return newStyle;
  }
}
// 字符
public class Character {
  private char c; // 字符
  private CharacterStyle style; // 样式

  public Character(char c, CharacterStyle style) {
    this.c = c;
    this.style = style;
  }
}
// 编辑
public class Editor {
  private List<Character> chars = new ArrayList<>(); // 文本域
  // 写入字符
  public void appendCharacter(char c, Font font, int size, int colorRGB) {
    Character character = new Character(c, CharacterStyleFactory.getStyle(font, size, colorRGB)); // 构建字符及其格式
    chars.add(character); // 追加到文本域
  }
}
~~~

##### 区分

- 区分单例
  - 在单例中，一个类只能创建一个对象。
  - 在享元中，一个类可以创建多个对象。（类似多例）
    - 享元的设计意图在于对象复用。
    - 多例的设计意图在于限制对象个数。
- 区分缓存
  - 在享元中，通过工厂来“缓存”创建好的对象。这里的缓存是指“存储”的意思。
  - 在”数据库缓存“、”CPU缓存“、”MemCache缓存“中，主要是为了提高访问效率，而非复用。
- 区分对象池
  - 对象池、连接池、线程池等也是为了复用。不过这个复用可以理解为“重复使用”，避免重复创建。
  - 享元中的“复用”可以理解为“共享使用”，目的在于节省空间。

##### 包装类

~~~java
// 自动装箱
Integer i = 59; // 当值在-128~127之间，会从IntegerCache类中直接返回。
// 等同
Integer i = Integer.valueOf(59);
~~~

~~~java
// 自动拆箱
int j = i;
// 等同
int j = i.intValue();
~~~

调整最大值的方法

~~~java
//方法一：
-Djava.lang.Integer.IntegerCache.high=255
//方法二：
-XX:AutoBoxCacheMax=255
~~~

- Integer、Long等。
- String字符常量池。

### 行为型

行为型设计模式主要解决的是“类或对象之间的交互”问题。

#### 1、观察者模式

观察者模式（Observer Design Pattern）也被称为发布订阅模式（Publish-Subscribe Design Pattern）。在对象之间定义一个一对多的依赖，当一个对象状态改变的时候，所有依赖的对象都会自动收到通知。

- 一般被依赖的对象叫做被观察者（Observable），依赖的对象叫做观察者（Observer）。
- 在实际项目开发中有各种不同的叫法：
  - Subject-Observer。被观察者-观察者
  - Publisher-Subscriber。发布-订阅
  - Producer-Consumer。生产者-消费者
  - EventEmitter-EventListener。事件发射器-事件监听器
  - Dispatcher-Listener。调度-监听

将行为方法内的代码，拆分成更单一的小类，让其满足开闭原则、高内聚低耦合等特性，以此控制和应对代码的复杂性，提高代码的可扩展性。

##### 教科书实现

~~~java
// 被观察者
public interface Subject {
  void registerObserver(Observer observer);// 注册观察者
  void removeObserver(Observer observer); // 移除观察者
  void notifyObservers(Message message); // 通知观察者
}
// 观察者
public interface Observer {
  void update(Message message);
}
// 被观察者实现
public class ConcreteSubject implements Subject {
  // 组合所有观察者
  private List<Observer> observers = new ArrayList<Observer>();

  @Override
  public void registerObserver(Observer observer) {
    observers.add(observer);
  }

  @Override
  public void removeObserver(Observer observer) {
    observers.remove(observer);
  }

  @Override
  public void notifyObservers(Message message) {
    for (Observer observer : observers) { // 遍历通知
      observer.update(message);
    }
  }

}
// 观察者1
public class ConcreteObserverOne implements Observer {
  @Override
  public void update(Message message) {
    //TODO: 获取消息通知，执行自己的逻辑...
    System.out.println("ConcreteObserverOne is notified.");
  }
}
// 观察者2
public class ConcreteObserverTwo implements Observer {
  @Override
  public void update(Message message) {
    //TODO: 获取消息通知，执行自己的逻辑...
    System.out.println("ConcreteObserverTwo is notified.");
  }
}

public class Demo {
  public static void main(String[] args) {
    ConcreteSubject subject = new ConcreteSubject();
    subject.registerObserver(new ConcreteObserverOne());
    subject.registerObserver(new ConcreteObserverTwo());
    subject.notifyObservers(new Message());
  }
}
~~~

##### 注册后的事

- 注册

- 发放体验金
- 发送站内信

~~~java
// 被观察者
public interface RegObserver {
  void handleRegSuccess(long userId);
}
// 观察者实现，推销
public class RegPromotionObserver implements RegObserver {
  private PromotionService promotionService; // 依赖注入

  @Override
  public void handleRegSuccess(long userId) {
    promotionService.issueNewUserExperienceCash(userId); // 发放体验金
  }
}
// 观察者实现，通知
public class RegNotificationObserver implements RegObserver {
  private NotificationService notificationService;

  @Override
  public void handleRegSuccess(long userId) {
    notificationService.sendInboxMessage(userId, "Welcome..."); // 发送站内信
  }
}
// 用户接口类
public class UserController {
  private UserService userService; // 依赖注入
  private List<RegObserver> regObservers = new ArrayList<>();

  // 一次性设置好，之后也不可能动态的修改
  public void setRegObservers(List<RegObserver> observers) {
    regObservers.addAll(observers); // 添加所有
  }

  public Long register(String telephone, String password) {
    // 注册用户
    long userId = userService.register(telephone, password);
	// 注册成功后，通知观察者
    for (RegObserver observer : regObservers) {
      observer.handleRegSuccess(userId); // 处理注册后的事情
    }
    return userId;
  }
}
~~~

##### 异步非阻塞

~~~java
// 第一种实现方式，其他类代码不变，就没有再重复罗列
public class RegPromotionObserver implements RegObserver {
  private PromotionService promotionService; // 依赖注入

  @Override
  public void handleRegSuccess(Long userId) {
    Thread thread = new Thread(new Runnable() {
      @Override
      public void run() {
        promotionService.issueNewUserExperienceCash(userId);
      }
    });
    thread.start();
  }
}

// 第二种实现方式，其他类代码不变，就没有再重复罗列
public class UserController {
  private UserService userService; // 依赖注入
  private List<RegObserver> regObservers = new ArrayList<>();
  private Executor executor;

  public UserController(Executor executor) {
    this.executor = executor;
  }

  public void setRegObservers(List<RegObserver> observers) {
    regObservers.addAll(observers);
  }

  public Long register(String telephone, String password) {
    //省略输入参数的校验代码
    //省略userService.register()异常的try-catch代码
    long userId = userService.register(telephone, password);

    for (RegObserver observer : regObservers) {
      executor.execute(new Runnable() {
        @Override
        public void run() {
          observer.handleRegSuccess(userId);
        }
      });
    }

    return userId;
  }
}
~~~

- 第一种方式
  - 频繁地创建和销毁线程比较耗时。
  - 并发线程数无法控制，创建多的线程会导致堆栈溢出。
- 第二种方式
  - 利用线程池解决了第一种方式的问题。
  - 线程池、异步执行逻辑耦合在了register()函数中，增加了维护成本。

##### 使用EventBus

EventBus翻译为“事件总线”，它提供了实现观察者模式的骨架代码。

Google Guava EventBus就是一个著名的EventBus框架，支持异步非阻塞模式，也支持同步阻塞模式。

~~~java
// 用户相关接口类
public class UserController {
  private UserService userService; // 依赖注入

  private EventBus eventBus;
  private static final int DEFAULT_EVENTBUS_THREAD_POOL_SIZE = 20; // 观察者池的大小

  // 初始化
  public UserController() {
    //eventBus = new EventBus(); // 同步阻塞模式
    eventBus = new AsyncEventBus(
        Executors.newFixedThreadPool(DEFAULT_EVENTBUS_THREAD_POOL_SIZE)); // 异步非阻塞模式
  }

  // 注册观察者
  public void setRegObservers(List<Object> observers) {
    for (Object observer : observers) {
      eventBus.register(observer);
    }
  }

  public Long register(String telephone, String password) {
    //省略输入参数的校验代码
    //省略userService.register()异常的try-catch代码
    long userId = userService.register(telephone, password);

    eventBus.post(userId);// 发布，事件

    return userId;
  }
}
// 促销，观察者
public class RegPromotionObserver {
  private PromotionService promotionService; // 依赖注入

  // 注解订阅，也就是观察
  @Subscribe
  public void handleRegSuccess(Long userId) {
    promotionService.issueNewUserExperienceCash(userId);
  }
}
// 通知，观察者
public class RegNotificationObserver {
  private NotificationService notificationService;

  @Subscribe
  public void handleRegSuccess(Long userId) {
    notificationService.sendInboxMessage(userId, "...");
  }
}
~~~

- Guava EventBus
  - EventBus # 同步阻塞模式
    - `EventBus eventBus = new EventBus();`
  - AsyncEventBus # 异步非阻塞模式
    - `EventBus eventBus = new AsyncEventBus(Executors.newFixedThreadPool(8));`
- 函数
  - register # 注册观察者
    - `public void register(Object object);`
  - unregister # 删除观察者
    - `public void unregister(Object object);`
  - post # 发送消息
    - `public void post(Object event);`
    - 可匹配，
      - a消息，传递可以接收的a消息，a观察者接收a消息。
      - b消息是a消息的子类，那么传递可接收的b消息，b观察者接收a、b消息。
- 注解观察者
  - @Subscribe # 注解方法
  - 用于注解观察者可以接收的消息类型，如果给下面两个方法注解，该观察者就可以接收a、b消息了。
    - `public void f1(AMsg event);`
    - `public void f2(BMsg event);`
  - 原理是根据注解的方法与对应参数类型做记录，当有消息传递过来的时候，会匹配消息调用对应的方法。

##### 实现EventBus

Subscribe 注解类

~~~java
@Retention(RetentionPolicy.RUNTIME) # 运行时注解
@Target(ElementType.METHOD) # 注解方法
@Beta # 测试版，有可能更改的意思
public @interface Subscribe {}
~~~

ObserverAction 行为类，也就是被@Subscribe注解的方法

~~~java
public class ObserverAction {
  private Object target; // 观察者类
  private Method method; // 方法

  public ObserverAction(Object target, Method method) {
    this.target = Preconditions.checkNotNull(target);
    this.method = method;
    this.method.setAccessible(true);
  }

  public void execute(Object event) { // event是method方法的参数
    try {
      method.invoke(target, event);
    } catch (InvocationTargetException | IllegalAccessException e) {
      e.printStackTrace();
    }
  }
}
~~~

ObserverRegistry 注册表类

- 事件类型 -> 所有的观察者行为（set）
- 获取观察者的所有行为
  - 根据@Subscribe注解获取。
  - 按照事件类型分类行为：事件类型 -> 观察者行为（list）
- 遍历该观察者的所有行为
  - 根据事件类型（也就是被注解的方法且唯一的参数类型），将list行为统一添加到注册表。
    - 如果注册表不存在该类型，则创建一个set集合
    - 最后才根据事件类型添加到注册表。

~~~java
public class ObserverRegistry {
  private ConcurrentMap<Class<?>, CopyOnWriteArraySet<ObserverAction>> registry = 
      								new ConcurrentHashMap<>(); // 注册表
  // 注册观察者
  public void register(Object observer) {
    Map<Class<?>, Collection<ObserverAction>> observerActions = 
        						findAllObserverActions(observer); // 获取观察者的所有行为
    // 遍历观察者的所有行为
    for (Map.Entry<Class<?>, Collection<ObserverAction>> entry : 
         							observerActions.entrySet()) {
      Class<?> eventType = entry.getKey(); // 事件类型
      Collection<ObserverAction> eventActions = entry.getValue(); // 对应的所有行为
      CopyOnWriteArraySet<ObserverAction> registeredEventActions = 
          						registry.get(eventType); // 从注册表获取该事件类型的所有行为
      if (registeredEventActions == null) { // 没有就存
        registry.putIfAbsent(eventType, new CopyOnWriteArraySet<>()); // 存
        registeredEventActions = registry.get(eventType); // 取
      }
      // 给观察者添加所有行为
      registeredEventActions.addAll(eventActions); // 由于是set，不会造成有重复的行为存在
    }
  }
  // 根据事件消息，获取观察者相匹配的所有行为
  public List<ObserverAction> getMatchedObserverActions(Object event) {
    List<ObserverAction> matchedObservers = new ArrayList<>(); // 匹配观察者
    Class<?> postedEventType = event.getClass(); // 事件类型
    for (Map.Entry<Class<?>, CopyOnWriteArraySet<ObserverAction>> entry : registry.entrySet()) { // 遍历注册表
      Class<?> eventType = entry.getKey(); // 事件类型
      Collection<ObserverAction> eventActions = entry.getValue(); // 所有行为
      if (postedEventType.isAssignableFrom(eventType)) { // 匹配则添加
        matchedObservers.addAll(eventActions);
      }
    }
    return matchedObservers;
  }
  // 根据观察者获取所有行为
  private Map<Class<?>, Collection<ObserverAction>> findAllObserverActions(Object observer) {
    Map<Class<?>, Collection<ObserverAction>> observerActions = new HashMap<>();
    Class<?> clazz = observer.getClass(); // 观察者类
    for (Method method : getAnnotatedMethods(clazz)) { // 遍历所有方法
      Class<?>[] parameterTypes = method.getParameterTypes(); // 获取所有参数类型
      Class<?> eventType = parameterTypes[0]; // 获取第一个参数，也就是事件类型
      if (!observerActions.containsKey(eventType)) { // 存在就不重复存储了
        observerActions.put(eventType, new ArrayList<>());// 第一次存储这个类型
      }
      // 根据事件类型，添加对应的行为
      observerActions.get(eventType).add(new ObserverAction(observer, method));
    }
    return observerActions;
  }
  // 获取注解方法集合
  private List<Method> getAnnotatedMethods(Class<?> clazz) {
    List<Method> annotatedMethods = new ArrayList<>();
    for (Method method : clazz.getDeclaredMethods()) { // 遍历所有方法
      if (method.isAnnotationPresent(Subscribe.class)) { // 该方法是否注解了@Subscribe
        Class<?>[] parameterTypes = method.getParameterTypes(); // 获取所有形参类型
        // 必须是一个参数
        Preconditions.checkArgument(parameterTypes.length == 1,
                "Method %s has @Subscribe annotation but has %s parameters."
                        + "Subscriber methods must have exactly 1 parameter.",
                method, parameterTypes.length);
        annotatedMethods.add(method); // 添加
      }
    }
    return annotatedMethods;
  }
}
~~~

EventBus

~~~java
public class EventBus {
  private Executor executor; // 线程池
  private ObserverRegistry registry = new ObserverRegistry(); // 注册表类

  public EventBus() {
    this(MoreExecutors.directExecutor()); // 单线程
  }
  protected EventBus(Executor executor) {
    this.executor = executor;
  }
  // 委托注册
  public void register(Object object) {
    registry.register(object);
  }
  // 发送事件消息
  public void post(Object event) {
    // 获取相匹配的所有行为
    List<ObserverAction> observerActions = registry.getMatchedObserverActions(event);
    // 遍历异步执行
    for (ObserverAction observerAction : observerActions) {
      executor.execute(new Runnable() {
        @Override
        public void run() {
          observerAction.execute(event);
        }
      });
    }
  }
}
~~~

AsyncEventBus

~~~java
public class AsyncEventBus extends EventBus {
  public AsyncEventBus(Executor executor) {
    super(executor);
  }
}
~~~

##### 总结

框架的作用

- 隐藏实现细节，降低开发难度。
- 做到代码复用。
- 解耦业务与非业务代码。

#### 2、模板模式

模板模式（Template Method Design Pattern），模板方法模式在一个方法中定义一个算法骨架，并将某些步骤推迟到子类中实现。模板方法模式可以让子类在不改变算法整体结构的情况下，重新定义算法中的某些步骤。

- 算法可以理解为“业务逻辑”。
- 算法骨架就是“模板”。
  - 也就是推迟到子类中实现的方法，可以是强制子类实现的方法（abstract），或是提供默认实现方法。
- 包含算法骨架的方法就是“模板方法”。
  - 模板方法的定义是final，避免被子类重写。
- 模板模式有两大作用
  - 复用：指复用父类中提供的模板方法的代码。
  - 扩展：指通过模板模式提供的功能扩展点，定制化框架的功能。

##### 简单实现

~~~java
public abstract class AbstractClass {
  public final void templateMethod() { // 模板方法
    //...
    method1();
    //...
    method2();
    //...
  }
  protected abstract void method1(); // 抽象每一步，由子类实现
  protected abstract void method2();
}
public class ConcreteClass1 extends AbstractClass {
  @Override
  protected void method1() {
    //...
  }
  @Override
  protected void method2() {
    //...
  }
}
public class ConcreteClass2 extends AbstractClass {
  @Override
  protected void method1() {
    //...
  } 
  @Override
  protected void method2() {
    //...
  }
}
// 通过多态，调用父类模板方法
AbstractClass demo = ConcreteClass1();
demo.templateMethod();
~~~

##### 复用

- 只需要实现指定步骤的方法，即可使用父类的模板方法了。

> Java InputStream

在Java IO类库中，有很多类的设计都用到了模板模式，比如InputStream、OutputStream、Reader、Writer。

~~~java

public abstract class InputStream implements Closeable {
  //...省略其他代码...
  
  public int read(byte b[], int off, int len) throws IOException { // 模板方法
    if (b == null) {
      throw new NullPointerException();
    } else if (off < 0 || len < 0 || len > b.length - off) {
      throw new IndexOutOfBoundsException();
    } else if (len == 0) {
      return 0;
    }

    int c = read();
    if (c == -1) {
      return -1;
    }
    b[off] = (byte)c;

    int i = 1;
    try {
      for (; i < len ; i++) {
        c = read();
        if (c == -1) {
          break;
        }
        b[off + i] = (byte)c;
      }
    } catch (IOException ee) {
    }
    return i;
  }
  
  public abstract int read() throws IOException; // 步骤方法
}

public class ByteArrayInputStream extends InputStream {
  //...省略其他代码...
  
  @Override
  public synchronized int read() {
    return (pos < count) ? (buf[pos++] & 0xff) : -1;
  }
}
~~~

> Java AbstractList

~~~java
public boolean addAll(int index, Collection<? extends E> c) {
    rangeCheckForAdd(index);
    boolean modified = false;
    for (E e : c) {
        add(index++, e); // 模板方法
        modified = true;
    }
    return modified;
}

public void add(int index, E element) { // 提供默认实现，如果不实现，使用则会抛出异常
    throw new UnsupportedOperationException();
}
~~~

##### 扩展

- 在不修改框架源码的情况下，定制化框架的功能。

Java Servlet

Servlet框架提供了扩展点（doGet、doPost方法），将业务代码通过扩展点镶嵌到框架中执行。

~~~java
public class HelloServlet extends HttpServlet {
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {
    this.doPost(req, resp);
  }
  
  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp)
      throws ServletException, IOException {
    resp.getWriter().write("Hello World.");
  }
}
~~~

~~~java
// HttpService类
public void service(ServletRequest req, ServletResponse res)
    throws ServletException, IOException
{
    HttpServletRequest  request;
    HttpServletResponse response;
    if (!(req instanceof HttpServletRequest &&
            res instanceof HttpServletResponse)) {
        throw new ServletException("non-HTTP request or response");
    }
    request = (HttpServletRequest) req;
    response = (HttpServletResponse) res;
    service(request, response);
}
protected void service(HttpServletRequest req, HttpServletResponse resp)
    throws ServletException, IOException
{
    String method = req.getMethod();
    if (method.equals(METHOD_GET)) {
        long lastModified = getLastModified(req);
        if (lastModified == -1) {
            // servlet doesn't support if-modified-since, no reason
            // to go through further expensive logic
            doGet(req, resp);
        } else {
            long ifModifiedSince = req.getDateHeader(HEADER_IFMODSINCE);
            if (ifModifiedSince < lastModified) {
                // If the servlet mod time is later, call doGet()
                // Round down to the nearest second for a proper compare
                // A ifModifiedSince of -1 will always be less
                maybeSetLastModified(resp, lastModified);
                doGet(req, resp);
            } else {
                resp.setStatus(HttpServletResponse.SC_NOT_MODIFIED);
            }
        }
    } else if (method.equals(METHOD_HEAD)) {
        long lastModified = getLastModified(req);
        maybeSetLastModified(resp, lastModified);
        doHead(req, resp);
    } else if (method.equals(METHOD_POST)) {
        doPost(req, resp);
    } else if (method.equals(METHOD_PUT)) {
        doPut(req, resp);
    } else if (method.equals(METHOD_DELETE)) {
        doDelete(req, resp);
    } else if (method.equals(METHOD_OPTIONS)) {
        doOptions(req,resp);
    } else if (method.equals(METHOD_TRACE)) {
        doTrace(req,resp);
    } else {
        String errMsg = lStrings.getString("http.method_not_implemented");
        Object[] errArgs = new Object[1];
        errArgs[0] = method;
        errMsg = MessageFormat.format(errMsg, errArgs);
        resp.sendError(HttpServletResponse.SC_NOT_IMPLEMENTED, errMsg);
    }
}
~~~

JUnit TestCase

JUnit框架提供了一些功能扩展点（setUp、tearDown等），让用户可以在这些扩展点上扩展功能。

~~~java
public abstract class TestCase extends Assert implements Test {
  public void runBare() throws Throwable {  // 模板方法
    Throwable exception = null;
    setUp(); // 抽象步骤
    try {
      runTest();
    } catch (Throwable running) {
      exception = running;
    } finally {
      try {
        tearDown(); // 抽象步骤
      } catch (Throwable tearingDown) {
        if (exception == null) exception = tearingDown;
      }
    }
    if (exception != null) throw exception;
  }
  
  /**
  * Sets up the fixture, for example, open a network connection.
  * This method is called before a test is executed.
  */
  protected void setUp() throws Exception { // 默认实现，前置准备
  }

  /**
  * Tears down the fixture, for example, close a network connection.
  * This method is called after a test is executed.
  */
  protected void tearDown() throws Exception { // 默认实现，后置清扫
  }
}
~~~

##### 回调

回调是一种双向调用关系。A调用B，B反过来又调用A，这种调用机制就叫做“回调”。

- 同步回调更像模板模式
- 异步回调更像观察者模式

~~~java
public interface ICallback { // 包裹回调方法的类
  void methodToCallback();
}
public class BClass {
  public void process(ICallback callback) {
    //...
    callback.methodToCallback();
    //...
  }
}
public class AClass {
  public static void main(String[] args) {
    BClass b = new BClass();
    b.process(new ICallback() { // 回调对象
      @Override
      public void methodToCallback() {
        System.out.println("Call back me.");
      }
    });
  }
}
~~~

##### JdbcTemplate

使用JdbcTemplate

~~~java
public class JdbcTemplateDemo {
  private JdbcTemplate jdbcTemplate; // 注入

  public User queryUser(long id) {
    String sql = "select * from user where id="+id;
    return jdbcTemplate.query(sql, new UserRowMapper()).get(0);
  }
  // 结果集封装
  class UserRowMapper implements RowMapper<User> {
    public User mapRow(ResultSet rs, int rowNum) throws SQLException {
      User user = new User();
      user.setId(rs.getLong("id"));
      user.setName(rs.getString("name"));
      user.setTelephone(rs.getString("telephone"));
      return user;
    }
  }
}
~~~

JdbcTemplate实现

~~~java
@Override
public <T> List<T> query(String sql, RowMapper<T> rowMapper) throws DataAccessException {
 return query(sql, new RowMapperResultSetExtractor<T>(rowMapper));
}

@Override
public <T> T query(final String sql, final ResultSetExtractor<T> rse) throws DataAccessException {
 //...

 // 回调方法
 class QueryStatementCallback implements StatementCallback<T>, SqlProvider {
      @Override
      public T doInStatement(Statement stmt) throws SQLException {
       ResultSet rs = null;
       try {
           	// 4、执行sql
            rs = stmt.executeQuery(sql);
            ResultSet rsToUse = rs;
            if (nativeJdbcExtractor != null) {
             rsToUse = nativeJdbcExtractor.getNativeResultSet(rs);
            }
           	// 5、返回结果
            return rse.extractData(rsToUse);
           }
           finally {
            JdbcUtils.closeResultSet(rs);
           }
      }
     
      @Override
      public String getSql() {
       return sql;
      }
 }

 return execute(new QueryStatementCallback());
}
// 执行查询
@Override
public <T> T execute(StatementCallback<T> action) throws DataAccessException {
 Assert.notNull(action, "Callback object must not be null");

 // 1、获取连接
 Connection con = DataSourceUtils.getConnection(getDataSource());
 Statement stmt = null;
 try {
  Connection conToUse = con;
  if (this.nativeJdbcExtractor != null &&
    this.nativeJdbcExtractor.isNativeConnectionNecessaryForNativeStatements()) {
   conToUse = this.nativeJdbcExtractor.getNativeConnection(con);
  }
  // 2、创建预编译
  stmt = conToUse.createStatement();
  applyStatementSettings(stmt);
  Statement stmtToUse = stmt;
  if (this.nativeJdbcExtractor != null) {
   stmtToUse = this.nativeJdbcExtractor.getNativeStatement(stmt);
  }
  // 3、执行回调方法
  T result = action.doInStatement(stmtToUse); // 6、接收结果
  handleWarnings(stmt);
  return result;
 }
 catch (SQLException ex) {
  //...
 }
 finally {
  JdbcUtils.closeStatement(stmt);
  DataSourceUtils.releaseConnection(con, getDataSource());
 }
}
~~~

##### setClickListener(）

~~~java
// 安卓中给按钮设置点击事件
Button button = (Button)findViewById(R.id.button);
button.setOnClickListener(new OnClickListener() {
  @Override
  public void onClick(View v) {
    System.out.println("I am clicked.");
  }
});
~~~

##### addShutdownHook()

JVM 提供了 Runtime.addShutdownHook(Thread hook) 方法，可以注册一个 JVM 关闭的 Hook。

当应用程序关闭的时候，JVM 会自动调用ApplicationShutdownHooks的runHooks() 方法。

~~~java
public class ShutdownHookDemo {
  private static class ShutdownHook extends Thread {
    public void run() {
      System.out.println("I am called during shutting down.");
    }
  }
  public static void main(String[] args) {
    Runtime.getRuntime().addShutdownHook(new ShutdownHook());
  }
}
~~~

~~~java
public class Runtime {
  public void addShutdownHook(Thread hook) { 
    SecurityManager sm = System.getSecurityManager();
    if (sm != null) {
      sm.checkPermission(new RuntimePermission("shutdownHooks"));
    }
    ApplicationShutdownHooks.add(hook); // 添加关闭钩子函数
  }
}

class ApplicationShutdownHooks {
    /* The set of registered hooks */
    private static IdentityHashMap<Thread, Thread> hooks;
    static {
            hooks = new IdentityHashMap<>();
        } catch (IllegalStateException e) {
            hooks = null;
        }
    }

    static synchronized void add(Thread hook) {
        if(hooks == null)
            throw new IllegalStateException("Shutdown in progress");

        if (hook.isAlive())
            throw new IllegalArgumentException("Hook already running");

        if (hooks.containsKey(hook))
            throw new IllegalArgumentException("Hook previously registered");

        hooks.put(hook, hook); // 添加到一致性哈希集合
    }

    static void runHooks() {
        Collection<Thread> threads;
        synchronized(ApplicationShutdownHooks.class) {
            threads = hooks.keySet();
            hooks = null;
        }
		// 执行所有钩子函数，异步执行，同时执行
        for (Thread hook : threads) {
            hook.start();
        }
        for (Thread hook : threads) {
            while (true) {
                try {
                    hook.join(); // 等待执行完该子线程，也就是同步，依次执行
                    break;
                } catch (InterruptedException ignored) {
                }
            }
        }
    }
}
~~~

##### 模板模式 VS 回调

- 应用场景上：
  - 同步回调几乎跟模板模式一样，在算法骨架中替换其中某个步骤，起到代码复用和扩展的目的。
  - 异步回调跟模板模式有较大差别，更像观察者模式。
- 代码实现上：
  - 回调基于组合关系来实现，把一个对象传递给另一个对象，是一种对象之间的关系。
  - 模板模式基于继承关系来实现，子类重写父类的抽象方法，是一种类之间的关系。
- 回调相对于模板模式更加灵活：
  - 模板模式编写的子类，不再具有继承的能力。
  - 回调可以使用匿名类来创建回调对象，可以不用实现定义类。而模板模式针对不同的实现都要定义不同的子类。
  - 如果某个类中定义了多个模板方法，每个方法都有对应的抽象方法，即便我们只用到其中一个模板方法，子类也必须实现所有的抽象方法。而回调只需要往用到的模板方法中注入回调对象即可。

#### 3、策略模式

策略模式（Strategy Design Pattern），定义一组算法类，将每个算法分别封装起来，让他们可以互相替换。策略模式可以使算法的变化独立于使用他们的客户端（指使用算法的代码）。

- 策略的定义：基于接口而非实现编程
  - 以便于灵活地替换不同的策略类。
- 策略的创建：根据type创建策略，放到工厂类中。
  - 共享策略：实现创建好。
  - 不共享策略：每次都new。
- 策略的使用：
  - 运行时动态，加载配置文件。
  - 直接指定策略类。

##### 排序

根据文件大小，选择不同算法来排序

~~~java

public interface ISortAlg {
  void sort(String filePath);
}

public class QuickSort implements ISortAlg {
  @Override
  public void sort(String filePath) {
    //...
  }
}

public class ExternalSort implements ISortAlg {
  @Override
  public void sort(String filePath) {
    //...
  }
}

public class ConcurrentExternalSort implements ISortAlg {
  @Override
  public void sort(String filePath) {
    //...
  }
}

public class MapReduceSort implements ISortAlg {
  @Override
  public void sort(String filePath) {
    //...
  }
}

public class Sorter {
  private static final long GB = 1000 * 1000 * 1000;

  public void sortFile(String filePath) {
    // 省略校验逻辑
    File file = new File(filePath);
    long fileSize = file.length();
    ISortAlg sortAlg;
    if (fileSize < 6 * GB) { // [0, 6GB)
      sortAlg = new QuickSort();
    } else if (fileSize < 10 * GB) { // [6GB, 10GB)
      sortAlg = new ExternalSort();
    } else if (fileSize < 100 * GB) { // [10GB, 100GB)
      sortAlg = new ConcurrentExternalSort();
    } else { // [100GB, ~)
      sortAlg = new MapReduceSort();
    }
    sortAlg.sort(filePath);
  }
}
~~~

##### 策略工厂

~~~java

public class SortAlgFactory {
  private static final Map<String, ISortAlg> algs = new HashMap<>();

  static {
    algs.put("QuickSort", new QuickSort());
    algs.put("ExternalSort", new ExternalSort());
    algs.put("ConcurrentExternalSort", new ConcurrentExternalSort());
    algs.put("MapReduceSort", new MapReduceSort());
  }

  public static ISortAlg getSortAlg(String type) {
    if (type == null || type.isEmpty()) {
      throw new IllegalArgumentException("type should not be empty.");
    }
    return algs.get(type);
  }
}

public class Sorter {
  private static final long GB = 1000 * 1000 * 1000;

  public void sortFile(String filePath) {
    // 省略校验逻辑
    File file = new File(filePath);
    long fileSize = file.length();
    ISortAlg sortAlg;
    if (fileSize < 6 * GB) { // [0, 6GB)
      sortAlg = SortAlgFactory.getSortAlg("QuickSort");
    } else if (fileSize < 10 * GB) { // [6GB, 10GB)
      sortAlg = SortAlgFactory.getSortAlg("ExternalSort");
    } else if (fileSize < 100 * GB) { // [10GB, 100GB)
      sortAlg = SortAlgFactory.getSortAlg("ConcurrentExternalSort");
    } else { // [100GB, ~)
      sortAlg = SortAlgFactory.getSortAlg("MapReduceSort");
    }
    sortAlg.sort(filePath);
  }
}
~~~

##### 最小化、集中化

~~~java

public class Sorter {
  private static final long GB = 1000 * 1000 * 1000;
  private static final List<AlgRange> algs = new ArrayList<>();
  static {
    algs.add(new AlgRange(0, 6*GB, SortAlgFactory.getSortAlg("QuickSort")));
    algs.add(new AlgRange(6*GB, 10*GB, SortAlgFactory.getSortAlg("ExternalSort")));
    algs.add(new AlgRange(10*GB, 100*GB, SortAlgFactory.getSortAlg("ConcurrentExternalSort")));
    algs.add(new AlgRange(100*GB, Long.MAX_VALUE, SortAlgFactory.getSortAlg("MapReduceSort")));
  }

  public void sortFile(String filePath) {
    // 省略校验逻辑
    File file = new File(filePath);
    long fileSize = file.length();
    ISortAlg sortAlg = null;
    for (AlgRange algRange : algs) {
      if (algRange.inRange(fileSize)) {
        sortAlg = algRange.getAlg();
        break;
      }
    }
    sortAlg.sort(filePath);
  }

  private static class AlgRange {
    private long start;
    private long end;
    private ISortAlg alg;

    public AlgRange(long start, long end, ISortAlg alg) {
      this.start = start;
      this.end = end;
      this.alg = alg;
    }

    public ISortAlg getAlg() {
      return alg;
    }

    public boolean inRange(long size) {
      return size >= start && size < end;
    }
  }
}
~~~

##### 总结

策略模式的作用：

- 解耦策略的定义、创建和使用。
- 控制代码的复杂度。
- 满足开闭原则，添加新策略的时候，最小化、集中化代码改动，减少引入bug的风险。

#### 4、职责链模式

职责链模式（Chain Of Responsibility Design Pattern），将请求的发送和接收解耦，让多个接收对象都有机会处理这个请求。将这些接收对象串成一条链，并沿着这条链传递这个请求，直到链上的某个接收对象能够处理它为止。

##### 链表方式

基于继承、抽象、组合。

- 抽象的处理器类
  - 定义一个公共的抽象类，用于统一处理。
  - 组合一个自己，successor用于指向下一个处理器，形成链条。
  - 暴露一个方法设置successor。
- 处理器链
  - 头指针用于从头开始执行处理。
  - 尾指针用于添加。
  - 一个处理方法，从头执行。
  - 一个添加方法，从尾添加。

~~~java
// 处理器
public abstract class Handler {
  protected Handler successor = null; // 组合successor，指向下一个处理器

  public void setSuccessor(Handler successor) {
    this.successor = successor;
  }

  public abstract void handle(); // 处理
}

public class HandlerA extends Handler {
  @Override
  public void handle() {
    boolean handled = false; // 是否能处理
    //...
    if (!handled && successor != null) { // 不能处理，且有下一个处理器
      successor.handle(); // 则交由下一个处理器处理
    }
  }
}

public class HandlerB extends Handler {
  @Override
  public void handle() {
    boolean handled = false; // 是否能处理
    //...
    if (!handled && successor != null) { // 不能处理，且有下一个处理器
      successor.handle(); // 则交由下一个处理器处理
    } 
  }
}
// 处理器链
public class HandlerChain {
  private Handler head = null; // 头指针，用于从头遍历
  private Handler tail = null; // 尾指针，用于添加，指向下一个处理器

  public void addHandler(Handler handler) {
    handler.setSuccessor(null);

    if (head == null) {
      head = handler;
      tail = handler;
      return;
    }

    tail.setSuccessor(handler); // 添加
    tail = handler; // 指向添加的这个处理器
  }

  public void handle() {
    if (head != null) {
      head.handle();
    }
  }
}

// 使用举例
public class Application {
  public static void main(String[] args) {
    HandlerChain chain = new HandlerChain();
    chain.addHandler(new HandlerA());
    chain.addHandler(new HandlerB());
    chain.handle();
  }
}
~~~

> 重构优化

利用模板模式进行优化。

- 抽象的处理器类
  - 抽象的doHandle方法，返回boolean，表示是否处理了。

~~~java
public abstract class Handler {
  protected Handler successor = null;

  public void setSuccessor(Handler successor) {
    this.successor = successor;
  }

  public final void handle() {
    boolean handled = doHandle();
    if (successor != null && !handled) {
      successor.handle();
    }
  }

  protected abstract boolean doHandle();
}

public class HandlerA extends Handler {
  @Override
  protected boolean doHandle() {
    boolean handled = false;
    //...
    return handled;
  }
}

public class HandlerB extends Handler {
  @Override
  protected boolean doHandle() {
    boolean handled = false;
    //...
    return handled;
  }
}

// HandlerChain和Application代码不变
~~~

##### 数组方式

基于接口、组合。

~~~java

public interface IHandler {
  boolean handle();
}

public class HandlerA implements IHandler {
  @Override
  public boolean handle() {
    boolean handled = false;
    //...
    return handled;
  }
}

public class HandlerB implements IHandler {
  @Override
  public boolean handle() {
    boolean handled = false;
    //...
    return handled;
  }
}

public class HandlerChain {
  private List<IHandler> handlers = new ArrayList<>(); // 数组

  public void addHandler(IHandler handler) { // 添加
    this.handlers.add(handler);
  }

  public void handle() {
    for (IHandler handler : handlers) { // 遍历执行
      boolean handled = handler.handle(); // 处理，并返回结果
      if (handled) { // 是否处理了
        break;
      }
    }
  }
}

// 使用举例
public class Application {
  public static void main(String[] args) {
    HandlerChain chain = new HandlerChain();
    chain.addHandler(new HandlerA());
    chain.addHandler(new HandlerB());
    chain.handle();
  }
}
~~~

##### 变体

- 全部都执行一遍，无需判断是否处理了，只需要判断有没有下一个。

~~~java
public abstract class Handler {
  protected Handler successor = null;

  public void setSuccessor(Handler successor) {
    this.successor = successor;
  }

  public final void handle() {
    doHandle();
    if (successor != null) {
      successor.handle();
    }
  }

  protected abstract void doHandle();
}

public class HandlerA extends Handler {
  @Override
  protected void doHandle() {
    //...
  }
}

public class HandlerB extends Handler {
  @Override
  protected void doHandle() {
    //...
  }
}

public class HandlerChain {
  private Handler head = null;
  private Handler tail = null;

  public void addHandler(Handler handler) {
    handler.setSuccessor(null);

    if (head == null) {
      head = handler;
      tail = handler;
      return;
    }

    tail.setSuccessor(handler);
    tail = handler;
  }

  public void handle() {
    if (head != null) {
      head.handle();
    }
  }
}

// 使用举例
public class Application {
  public static void main(String[] args) {
    HandlerChain chain = new HandlerChain();
    chain.addHandler(new HandlerA());
    chain.addHandler(new HandlerB());
    chain.handle();
  }
}
~~~

##### 应用场景

UGC（User Generated Content，用户生成内容）应用（比如论坛），实现敏感词过滤功能。

~~~java

public interface SensitiveWordFilter {
  boolean doFilter(Content content);
}

public class SexyWordFilter implements SensitiveWordFilter {
  @Override
  public boolean doFilter(Content content) {
    boolean legal = true;
    //...
    return legal;
  }
}

// PoliticalWordFilter、AdsWordFilter类代码结构与SexyWordFilter类似

public class SensitiveWordFilterChain {
  private List<SensitiveWordFilter> filters = new ArrayList<>();

  public void addFilter(SensitiveWordFilter filter) {
    this.filters.add(filter);
  }

  // return true if content doesn't contain sensitive words.
  public boolean filter(Content content) {
    for (SensitiveWordFilter filter : filters) {
      if (!filter.doFilter(content)) {
        return false;
      }
    }
    return true;
  }
}

public class ApplicationDemo {
  public static void main(String[] args) {
    SensitiveWordFilterChain filterChain = new SensitiveWordFilterChain();
    filterChain.addFilter(new AdsWordFilter());
    filterChain.addFilter(new SexyWordFilter());
    filterChain.addFilter(new PoliticalWordFilter());

    boolean legal = filterChain.filter(new Content());
    if (!legal) {
      // 不发表
    } else {
      // 发表
    }
  }
}
~~~

- 应对代码的复杂性
  - 职责链模式，将各个敏感词过滤函数拆分出独立的类，简化实现过滤功能的类。
- 提高代码的扩展性
  - 直接对实现过滤功能的类修改，虽然违反开闭原则，但也可以接受。
  - 而职责链模式只需要添加一个过滤的类，将它添加到过滤链中即可，做到了最少代码的改动。
  - 添加删除过滤类容易，还可以通过配置、注解方式添加，更加灵活。

##### Servlet Filter

~~~java

public class LogFilter implements Filter {
  @Override
  public void init(FilterConfig filterConfig) throws ServletException {
    // 在创建Filter时自动调用，
    // 其中filterConfig包含这个Filter的配置参数，比如name之类的（从配置文件中读取的）
  }

  @Override
  public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
    System.out.println("拦截客户端发送来的请求.");
    chain.doFilter(request, response);
    System.out.println("拦截发送给客户端的响应.");
  }

  @Override
  public void destroy() {
    // 在销毁Filter时自动调用
  }
}

// 在web.xml配置文件中如下配置：
<filter>
  <filter-name>logFilter</filter-name>
  <filter-class>com.xzg.cd.LogFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>logFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
~~~



 ~~~java
public final class ApplicationFilterChain implements FilterChain {
  private int pos = 0; //当前执行到了哪个filter
  private int n; //filter的个数
  private ApplicationFilterConfig[] filters;
  private Servlet servlet;
  
  @Override
  public void doFilter(ServletRequest request, ServletResponse response) {
    if (pos < n) {
      ApplicationFilterConfig filterConfig = filters[pos++];
      Filter filter = filterConfig.getFilter();
      filter.doFilter(request, response, this);
    } else {
      // filter都处理完毕后，执行servlet
      servlet.service(request, response);
    }
  }
  
  public void addFilter(ApplicationFilterConfig filterConfig) {
    for (ApplicationFilterConfig filter:filters)
      if (filter==filterConfig)
         return;

    if (n == filters.length) {//扩容
      ApplicationFilterConfig[] newFilters = new ApplicationFilterConfig[n + INCREMENT];
      System.arraycopy(filters, 0, newFilters, 0, n);
      filters = newFilters;
    }
    filters[n++] = filterConfig;
  }
}
 ~~~

~~~java

  @Override
  public void doFilter(ServletRequest request, ServletResponse response) {
    if (pos < n) {
      ApplicationFilterConfig filterConfig = filters[pos++];
      Filter filter = filterConfig.getFilter();
      //filter.doFilter(request, response, this);
      //把filter.doFilter的代码实现展开替换到这里
      System.out.println("拦截客户端发送来的请求.");
      chain.doFilter(request, response); // chain就是this
      System.out.println("拦截发送给客户端的响应.")
    } else {
      // filter都处理完毕后，执行servlet
      servlet.service(request, response);
    }
  }
~~~

##### Spring Interceptor

~~~java

public class LogInterceptor implements HandlerInterceptor {

  @Override
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    System.out.println("拦截客户端发送来的请求.");
    return true; // 继续后续的处理
  }

  @Override
  public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
    System.out.println("拦截发送给客户端的响应.");
  }

  @Override
  public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    System.out.println("这里总是被执行.");
  }
}

//在Spring MVC配置文件中配置interceptors
<mvc:interceptors>
   <mvc:interceptor>
       <mvc:mapping path="/*"/>
       <bean class="com.xzg.cd.LogInterceptor" />
   </mvc:interceptor>
</mvc:interceptors>
~~~



~~~java

public class HandlerExecutionChain {
 private final Object handler;
 private HandlerInterceptor[] interceptors;
 
 public void addInterceptor(HandlerInterceptor interceptor) {
  initInterceptorList().add(interceptor);
 }

 boolean applyPreHandle(HttpServletRequest request, HttpServletResponse response) throws Exception {
  HandlerInterceptor[] interceptors = getInterceptors();
  if (!ObjectUtils.isEmpty(interceptors)) {
   for (int i = 0; i < interceptors.length; i++) {
    HandlerInterceptor interceptor = interceptors[i];
    if (!interceptor.preHandle(request, response, this.handler)) {
     triggerAfterCompletion(request, response, null);
     return false;
    }
   }
  }
  return true;
 }

 void applyPostHandle(HttpServletRequest request, HttpServletResponse response, ModelAndView mv) throws Exception {
  HandlerInterceptor[] interceptors = getInterceptors();
  if (!ObjectUtils.isEmpty(interceptors)) {
   for (int i = interceptors.length - 1; i >= 0; i--) {
    HandlerInterceptor interceptor = interceptors[i];
    interceptor.postHandle(request, response, this.handler, mv);
   }
  }
 }

 void triggerAfterCompletion(HttpServletRequest request, HttpServletResponse response, Exception ex)
   throws Exception {
  HandlerInterceptor[] interceptors = getInterceptors();
  if (!ObjectUtils.isEmpty(interceptors)) {
   for (int i = this.interceptorIndex; i >= 0; i--) {
    HandlerInterceptor interceptor = interceptors[i];
    try {
     interceptor.afterCompletion(request, response, this.handler, ex);
    } catch (Throwable ex2) {
     logger.error("HandlerInterceptor.afterCompletion threw exception", ex2);
    }
   }
  }
 }
}
~~~

- Filter可以拿到原始的http请求。
- Interceptor可以拿到请求的控制器和方法。
- Aop可以拿到方法的参数。

#### 5、状态模式

有限状态机（Finite State Machine，FSM），简称状态机。

状态机有三种状态：

- 状态（State）
- 事件（Event），也称为转移条件（Transition Condition）
  - 事件触发状态的转移及动作的执行。
- 动作（Action）
  - 动作不是必须的。

##### 分支逻辑法

包含大量if-elase或switch-case分支判断逻辑。

~~~java
// 状态枚举
public enum State {
  SMALL(0),
  SUPER(1),
  FIRE(2),
  CAPE(3);

  private int value;

  private State(int value) {
    this.value = value;
  }

  public int getValue() {
    return this.value;
  }
}
// 马里奥状态机
public class MarioStateMachine {
  private int score;
  private State currentState;

  public MarioStateMachine() {
      this.score = 0;
      this.currentState = State.SMALL;
  }
  // 获得蘑菇
  public void obtainMushRoom() {
      if (currentState.equals(State.SMALL)) {
          this.currentState = State.SUPER;
          this.score += 100;
      }
  }
  // 获得斗篷
  public void obtainCape() {
      if (currentState.equals(State.SMALL) || currentState.equals(State.SUPER) ) {
          this.currentState = State.CAPE;
          this.score += 200;
      }
  }
  // 获得火焰
  public void obtainFireFlower() {
      if (currentState.equals(State.SMALL) || currentState.equals(State.SUPER) ) { 
          this.currentState = State.FIRE;
          this.score += 300;
      }
  }
  // 遇到怪物
  public void meetMonster() {
      if (currentState.equals(State.SUPER)) {
          this.currentState = State.SMALL;
          this.score -= 100; return;
      }
      if (currentState.equals(State.CAPE)) {
          this.currentState = State.SMALL;
          this.score -= 200; return;
      }
      if (currentState.equals(State.FIRE)) {
          this.currentState = State.SMALL;
          this.score -= 300; return;
      }
  }
  // 获取分数
  public int getScore() {
    return this.score;
  }
  // 获取当前分数
  public State getCurrentState() {
    return this.currentState;
  }
}

public class ApplicationDemo {
  public static void main(String[] args) {
    MarioStateMachine mario = new MarioStateMachine();
    mario.obtainMushRoom();
    int score = mario.getScore();
    State state = mario.getCurrentState();
    System.out.println("mario score: " + score + "; state: " + state);
  }
}
~~~

##### 查表法

列表示事件、行表示状态、内容表示在该事件和状态下产生的行为。

~~~java
// 枚举事件
public enum Event {
  GOT_MUSHROOM(0),
  GOT_CAPE(1),
  GOT_FIRE(2),
  MET_MONSTER(3);

  private int value;

  private Event(int value) {
    this.value = value;
  }

  public int getValue() {
    return this.value;
  }
}
// 马里奥状态机
public class MarioStateMachine {
  private int score;
  private State currentState;
  // 转换表
  private static final State[][] transitionTable = {
          {SUPER, CAPE, FIRE, SMALL},
          {SUPER, CAPE, FIRE, SMALL},
          {CAPE, CAPE, CAPE, SMALL},
          {FIRE, FIRE, FIRE, SMALL}
  };
  // 行为表
  private static final int[][] actionTable = {
          {+100, +200, +300, +0},
          {+0, +200, +300, -100},
          {+0, +0, +0, -200},
          {+0, +0, +0, -300}
  };

  public MarioStateMachine() {
    this.score = 0;
    this.currentState = State.SMALL;
  }

  public void obtainMushRoom() {
    executeEvent(Event.GOT_MUSHROOM);
  }

  public void obtainCape() {
    executeEvent(Event.GOT_CAPE);
  }

  public void obtainFireFlower() {
    executeEvent(Event.GOT_FIRE);
  }

  public void meetMonster() {
    executeEvent(Event.MET_MONSTER);
  }
  // 触发事件
  private void executeEvent(Event event) {
    int stateValue = currentState.getValue(); // 获取状态索引
    int eventValue = event.getValue(); // 获取事件索引
    this.currentState = transitionTable[stateValue][eventValue]; // 获取事件后的状态
    this.score += actionTable[stateValue][eventValue]; // 获取事件后的分数
  }

  public int getScore() {
    return this.score;
  }

  public State getCurrentState() {
    return this.currentState;
  }

}
~~~

##### 状态模式

由查表法衍生的：

- 列（事件）是不经常变的，也是统一的，所以定义为接口

- 所有的行（状态）都继承自该接口，实现里面的事件，补充行为（不是必须的）。 

~~~java
public interface IMario { //所有状态类的接口
  State getName();
  //以下是定义的事件
  void obtainMushRoom();
  void obtainCape();
  void obtainFireFlower();
  void meetMonster();
}

public class SmallMario implements IMario {
  private MarioStateMachine stateMachine;

  public SmallMario(MarioStateMachine stateMachine) {
    this.stateMachine = stateMachine;
  }

  @Override
  public State getName() {
    return State.SMALL;
  }

  @Override
  public void obtainMushRoom() {
    stateMachine.setCurrentState(new SuperMario(stateMachine));
    stateMachine.setScore(stateMachine.getScore() + 100);
  }

  @Override
  public void obtainCape() {
    stateMachine.setCurrentState(new CapeMario(stateMachine));
    stateMachine.setScore(stateMachine.getScore() + 200);
  }

  @Override
  public void obtainFireFlower() {
    stateMachine.setCurrentState(new FireMario(stateMachine));
    stateMachine.setScore(stateMachine.getScore() + 300);
  }

  @Override
  public void meetMonster() {
    // do nothing...
  }
}

public class SuperMario implements IMario {
  private MarioStateMachine stateMachine;

  public SuperMario(MarioStateMachine stateMachine) {
    this.stateMachine = stateMachine;
  }

  @Override
  public State getName() {
    return State.SUPER;
  }

  @Override
  public void obtainMushRoom() {
    // do nothing...
  }

  @Override
  public void obtainCape() {
    stateMachine.setCurrentState(new CapeMario(stateMachine));
    stateMachine.setScore(stateMachine.getScore() + 200);
  }

  @Override
  public void obtainFireFlower() {
    stateMachine.setCurrentState(new FireMario(stateMachine));
    stateMachine.setScore(stateMachine.getScore() + 300);
  }

  @Override
  public void meetMonster() {
    stateMachine.setCurrentState(new SmallMario(stateMachine));
    stateMachine.setScore(stateMachine.getScore() - 100);
  }
}

// 省略CapeMario、FireMario类...

public class MarioStateMachine {
  private int score;
  private IMario currentState; // 不再使用枚举来表示状态

  public MarioStateMachine() {
    this.score = 0;
    this.currentState = new SmallMario(this);
  }

  public void obtainMushRoom() {
    this.currentState.obtainMushRoom();
  }

  public void obtainCape() {
    this.currentState.obtainCape();
  }

  public void obtainFireFlower() {
    this.currentState.obtainFireFlower();
  }

  public void meetMonster() {
    this.currentState.meetMonster();
  }

  public int getScore() {
    return this.score;
  }

  public State getCurrentState() {
    return this.currentState.getName();
  }

  public void setScore(int score) {
    this.score = score;
  }

  public void setCurrentState(IMario currentState) {
    this.currentState = currentState;
  }
}
~~~

> 重构优化

~~~java

public interface IMario {
  State getName();
  void obtainMushRoom(MarioStateMachine stateMachine);
  void obtainCape(MarioStateMachine stateMachine);
  void obtainFireFlower(MarioStateMachine stateMachine);
  void meetMonster(MarioStateMachine stateMachine);
}

public class SmallMario implements IMario {
  private static final SmallMario instance = new SmallMario();
  private SmallMario() {}
  public static SmallMario getInstance() {
    return instance;
  }

  @Override
  public State getName() {
    return State.SMALL;
  }

  @Override
  public void obtainMushRoom(MarioStateMachine stateMachine) {
    stateMachine.setCurrentState(SuperMario.getInstance());
    stateMachine.setScore(stateMachine.getScore() + 100);
  }

  @Override
  public void obtainCape(MarioStateMachine stateMachine) {
    stateMachine.setCurrentState(CapeMario.getInstance());
    stateMachine.setScore(stateMachine.getScore() + 200);
  }

  @Override
  public void obtainFireFlower(MarioStateMachine stateMachine) {
    stateMachine.setCurrentState(FireMario.getInstance());
    stateMachine.setScore(stateMachine.getScore() + 300);
  }

  @Override
  public void meetMonster(MarioStateMachine stateMachine) {
    // do nothing...
  }
}

// 省略SuperMario、CapeMario、FireMario类...

public class MarioStateMachine {
  private int score;
  private IMario currentState;

  public MarioStateMachine() {
    this.score = 0;
    this.currentState = SmallMario.getInstance();
  }

  public void obtainMushRoom() {
    this.currentState.obtainMushRoom(this);
  }

  public void obtainCape() {
    this.currentState.obtainCape(this);
  }

  public void obtainFireFlower() {
    this.currentState.obtainFireFlower(this);
  }

  public void meetMonster() {
    this.currentState.meetMonster(this);
  }

  public int getScore() {
    return this.score;
  }

  public State getCurrentState() {
    return this.currentState.getName();
  }

  public void setScore(int score) {
    this.score = score;
  }

  public void setCurrentState(IMario currentState) {
    this.currentState = currentState;
  }
}
~~~

##### 使用场景

- 分支逻辑法将每一个状态转移原模原样得直译成代码，这种实现方式最简单，最直接，是首选。
- 像游戏这种比较复杂的状态机，包含的状态比较多，推荐使用查表法。
  - 通过二维数据来表示状态转移图，提高代码的可读性和可维护性。
- 像电商下单、外卖下单这种类型的状态机，包含的状态并不多，状态转移也比较简单，但事件触发执行的动作包含的业务逻辑可能比较复杂，更推荐使用状态模式来实现。

#### 6、迭代器模式

迭代器模式（Iterator Design Pattern），也叫游标模式（Cursor Design Pattern）。

##### 两种实现方式

~~~java

// 接口定义方式一
public interface Iterator<E> {
  boolean hasNext();
  void next();
  E currentItem(); // 可以多次获得当前元素，而不需要移动游标。
}

// 接口定义方式二
public interface Iterator<E> {
  boolean hasNext();
  E next();
}
~~~

##### ArrayIterator

~~~java
// 不同数据结构，定义不同的迭代器类
public class ArrayIterator<E> implements Iterator<E> {
  private int cursor; // 游标，用于指向当前元素
  private ArrayList<E> arrayList; // 组合要迭代的容器

  public ArrayIterator(ArrayList<E> arrayList) {
    this.cursor = 0;
    this.arrayList = arrayList;
  }

  @Override
  public boolean hasNext() { // 是否有下一个元素
    return cursor != arrayList.size();
  }

  @Override
  public void next() { // 游标自增1
    cursor++;
  }

  @Override
  public E currentItem() { // get元素
    if (cursor >= arrayList.size()) {
      throw new NoSuchElementException();
    }
    return arrayList.get(cursor);
  }
}

public class Demo {
  public static void main(String[] args) {
    ArrayList<String> names = new ArrayList<>();
    names.add("xzg");
    names.add("wang");
    names.add("zheng");
    
    Iterator<String> iterator = new ArrayIterator(names);
    while (iterator.hasNext()) {
      System.out.println(iterator.currentItem());
      iterator.next();
    }
  }
}
~~~

~~~java

public interface List<E> {
  Iterator iterator();
  //...省略其他接口函数...
}

public class ArrayList<E> implements List<E> {
  //...
  public Iterator iterator() { // 获取ArrayList迭代器
    return new ArrayIterator(this);
  }
  //...省略其他代码
}

public class Demo {
  public static void main(String[] args) {
    List<String> names = new ArrayList<>();
    names.add("xzg");
    names.add("wang");
    names.add("zheng");
    
    Iterator<String> iterator = names.iterator();
    while (iterator.hasNext()) {
      System.out.println(iterator.currentItem());
      iterator.next();
    }
  }
}
~~~

##### 迭代器的优势

- 三种遍历方式
  - for、for-each、iterator。
  - 实际上只要两种，for-each只是一个语法糖，底层还是基于迭代器来实现的。
- 像数组和链表这种数据结构，遍历方式比较简单，用for循环来遍历就足够了。
- 如果是复杂的数据结构，如树、图等有各种复杂的遍历方式，使用for循环就不能满足了。
  - 而且由客户端自己实现，势必增加开发成本，引入复杂的代码，有可能出bug，增加维护成本。
- 使用迭代器，可以解耦遍历算法，拆分出不同的迭代器类，容器只需要引入即可使用。
  - 而且，每个迭代器都有自己的游标，所以不同的迭代器同时对容器进行遍历而互不影响。

##### 遍历时增删元素问题

- 由于ArrayList在删除元素时，所有元素都会进行向左搬移的操作，也就是删除元素后面的元素索引都会减一。
  - 遍历同时删除元素，会出现未决行为（结果不可预期行为）。
    - 删除游标左边的元素时，游标索引会指向下个元素索引加一的元素。
    - 删除游标右边的元素时，则不会有影响。
  - 遍历同时添加元素，也会出现未决行为。
    - 从游标左边添加元素时，游标会重复指向上一个元素。
    - 从游标右边添加元素时，则不会有影响。
- 应对这种未决行为。
  - 遍历时不允许增删元素。
    - 确定遍历开始和结束的时间点。
    - 通过主动调用迭代的finishIteration()方法宣告结束。
  - 增删元素之后让遍历报错。（java采用的是这种方式）
    - ArrayList中定义一个成员变量modCount，集合被修改的次数，增加或删除元素一次就会加一。
    - 当调用iterator函数创建迭代器时将modCount传递给迭代器的expectedModCount成员变量。
    - 每次调用hasNext、next、currentItem函数时，都会去检查modCount是否等于expectModCount，是否在遍历时集合发生改变。
    - 如果不相同，就抛出运行时异常，结束掉程序。

~~~java
public class ArrayIterator implements Iterator {
  private int cursor;
  private ArrayList arrayList;
  private int expectedModCount;

  public ArrayIterator(ArrayList arrayList) {
    this.cursor = 0;
    this.arrayList = arrayList;
    this.expectedModCount = arrayList.modCount; // 初始化expectedModCount
  }

  @Override
  public boolean hasNext() {
    checkForComodification();
    return cursor < arrayList.size();
  }

  @Override
  public void next() {
    checkForComodification();
    cursor++;
  }

  @Override
  public Object currentItem() {
    checkForComodification();
    return arrayList.get(cursor);
  }
  
  private void checkForComodification() {
    if (arrayList.modCount != expectedModCount) // 不相等则说明集合发生改变，抛出异常。
        throw new ConcurrentModificationException();
  }
}

//代码示例
public class Demo {
  public static void main(String[] args) {
    List<String> names = new ArrayList<>();
    names.add("a");
    names.add("b");
    names.add("c");
    names.add("d");

    Iterator<String> iterator = names.iterator();
    iterator.next();
    names.remove("a");
    iterator.next();//抛出ConcurrentModificationException异常
  }
}
~~~

> 遍历集合的同时安全地删除集合元素

- 使用iterator提供的remove()方法。
- 其原理是删除游标前一位的元素。
- 在一次迭代遍历中调用两次remove方法会报错。

~~~java

public class ArrayList<E> {
  transient Object[] elementData;
  private int size;

  public Iterator<E> iterator() {
    return new Itr();
  }

  private class Itr implements Iterator<E> {
    int cursor;       // index of next element to return
    int lastRet = -1; // index of last element returned; -1 if no such
    int expectedModCount = modCount;

    Itr() {}

    public boolean hasNext() {
      return cursor != size;
    }

    @SuppressWarnings("unchecked")
    public E next() {
      checkForComodification();
      int i = cursor;
      if (i >= size)
        throw new NoSuchElementException();
      
      Object[] elementData = ArrayList.this.elementData;
      if (i >= elementData.length)
        throw new ConcurrentModificationException();
      
      cursor = i + 1;
      return (E) elementData[lastRet = i];
    }
    
    public void remove() {
      if (lastRet < 0)
        throw new IllegalStateException();
      checkForComodification();

      try {
        ArrayList.this.remove(lastRet); // 删除元素，后面元素都会往左移一位
        cursor = lastRet; // 游标指向了前一个元素的索引
        lastRet = -1; // 重置
        expectedModCount = modCount; // 同步，免检查
      } catch (IndexOutOfBoundsException ex) {
        throw new ConcurrentModificationException();
      }
    }
  }
}
~~~

##### 支持快照

> 第一种方式

每次遍历都拷贝一份数据到迭代器中

~~~java
public class SnapshotArrayIterator<E> implements Iterator<E> {
  private int cursor;
  private ArrayList<E> snapshot; // 快照

  public SnapshotArrayIterator(ArrayList<E> arrayList) {
    this.cursor = 0;
    this.snapshot = new ArrayList<>();
    this.snapshot.addAll(arrayList); // 浅拷贝
  }

  @Override
  public boolean hasNext() {
    return cursor < snapshot.size();
  }

  @Override
  public E next() {
    E currentItem = snapshot.get(cursor);
    cursor++;
    return currentItem;
  }
}
~~~

> 第二种方式

在容器中为每个元素保存两个时间戳，标记删除。

- addTimestamp # 添加时间戳，初始化添加时时间。
- delTimestamp # 删除时间戳，添加时初始化Long.MAX_VALUE，当元素删除时更新为当前时间，表示删除。

在迭代器中保存快照的创建时间戳。

- snapshotTimestamp # 迭代器创建时间戳
- 当addTimestamp<snapshotTimestamp<delTimestamp时，元素才属于这个迭代器的快照。
- 当addTimestamp>snapshotTimestamp时，说明是在迭代器创建之后才有的元素，不属于快照。
- 当delTimestamp<snapshotTimestamp时，说明是在迭代器创建之前删除的元素，也不属于快照。

下面这种方式解决了不需要拷贝容器的迭代器，但又不支持数组的随机访问了，可以尝试使用两个数组

- 一个支持标记删除的，用来实现快照遍历功能。
- 一个不支持标记删除的，移除已删除的元素，用来支持随机访问。

~~~java
// 集合容器类
public class ArrayList<E> implements List<E> {
  private static final int DEFAULT_CAPACITY = 10;

  private int actualSize; //不包含标记删除元素
  private int totalSize; //包含标记删除元素

  private Object[] elements;
  private long[] addTimestamps; // 添加时间
  private long[] delTimestamps; // 删除时间

  public ArrayList() {
    this.elements = new Object[DEFAULT_CAPACITY];
    this.addTimestamps = new long[DEFAULT_CAPACITY];
    this.delTimestamps = new long[DEFAULT_CAPACITY];
    this.totalSize = 0;
    this.actualSize = 0;
  }

  @Override
  public void add(E obj) {
    elements[totalSize] = obj;
    addTimestamps[totalSize] = System.currentTimeMillis(); // 添加元素，当前时间
    delTimestamps[totalSize] = Long.MAX_VALUE; // 删除元素，long最大数
    totalSize++;
    actualSize++;
  }

  @Override
  public void remove(E obj) {
    for (int i = 0; i < totalSize; ++i) { // 遍历查找元素
      if (elements[i].equals(obj)) { // 匹配删除
        delTimestamps[i] = System.currentTimeMillis(); // 删除时间
        actualSize--;
      }
    }
  }

  public int actualSize() {
    return this.actualSize;
  }

  public int totalSize() {
    return this.totalSize;
  }

  public E get(int i) {
    if (i >= totalSize) {
      throw new IndexOutOfBoundsException();
    }
    return (E)elements[i];
  }

  public long getAddTimestamp(int i) {
    if (i >= totalSize) {
      throw new IndexOutOfBoundsException();
    }
    return addTimestamps[i];
  }

  public long getDelTimestamp(int i) {
    if (i >= totalSize) {
      throw new IndexOutOfBoundsException();
    }
    return delTimestamps[i];
  }
}
// 支出快照功能的迭代器类
public class SnapshotArrayIterator<E> implements Iterator<E> {
  private long snapshotTimestamp; // 迭代器创建时间
  private int cursorInAll; // 在整个容器中的下标，而非快照中的下标
  private int leftCount; // 快照中还有几个元素未被遍历
  private ArrayList<E> arrayList;

  public SnapshotArrayIterator(ArrayList<E> arrayList) {
    this.snapshotTimestamp = System.currentTimeMillis();
    this.cursorInAll = 0; // 游标初始化0
    this.leftCount = arrayList.actualSize(); // 现存元素数
    this.arrayList = arrayList;

    justNext(); // 先跳到这个迭代器快照的第一个元素
  }

  @Override
  public boolean hasNext() {
    return this.leftCount >= 0; // 注意是>=, 而非>
  }

  @Override
  public E next() {
    E currentItem = arrayList.get(cursorInAll); // 获取当前元素
    justNext(); // 游标指向下一个元素
    return currentItem;
  }
  // 会跳过已删除的元素，找到属于快照的下一个元素，游标++，剩余元素--
  private void justNext() {
    while (cursorInAll < arrayList.totalSize()) { // 遍历数组
      long addTimestamp = arrayList.getAddTimestamp(cursorInAll);
      long delTimestamp = arrayList.getDelTimestamp(cursorInAll);
      // 匹配属于快照中的元素
      if (snapshotTimestamp > addTimestamp && snapshotTimestamp < delTimestamp) {
        leftCount--;
        break;
      }
      cursorInAll++;
    }
  }
}
~~~

#### 7、访问者模式

访问者模式（Visitor Design Pattern），允许一个或者多个操作应用到一组对象上，解耦操作和对象本身。

##### 0.1提取文本功能

抽象出一个ResourceFile类，同样的文件路径。不同是抽象的extract2txt，以便实现不同的抽取内容到text的算法。

~~~java
public abstract class ResourceFile {
  protected String filePath;

  public ResourceFile(String filePath) {
    this.filePath = filePath;
  }

  public abstract void extract2txt();
}

public class PPTFile extends ResourceFile {
  public PPTFile(String filePath) {
    super(filePath);
  }

  @Override
  public void extract2txt() {
    //...省略一大坨从PPT中抽取文本的代码...
    //...将抽取出来的文本保存在跟filePath同名的.txt文件中...
    System.out.println("Extract PPT.");
  }
}

// 运行结果是：
// Extract PDF.
// Extract WORD.
// Extract PPT.
public class ToolApplication {
  public static void main(String[] args) {
    List<ResourceFile> resourceFiles = listAllResourceFiles(args[0]);
    for (ResourceFile resourceFile : resourceFiles) {
      resourceFile.extract2txt();
    }
  }

  private static List<ResourceFile> listAllResourceFiles(String resourceDirectory) {
    List<ResourceFile> resourceFiles = new ArrayList<>();
    //...根据后缀(pdf/ppt/word)由工厂方法创建不同的类对象(PdfFile/PPTFile/WordFile)
    resourceFiles.add(new PdfFile("a.pdf"));
    resourceFiles.add(new WordFile("b.word"));
    resourceFiles.add(new PPTFile("c.ppt"));
    return resourceFiles;
  }
}
~~~

- 违背开闭原则，当ResourceFile需要添加新功能时，所有类都要修改。
- 随着功能增多，每个类的代码都不断膨胀，可读性和可维护性都变差了。
- 上层业务逻辑都耦合到PdfFile/PPtFile/WordFile类中，导致这些类职责不够单一。

##### 0.2解耦功能

解耦功能，将抽取内容的方法从ResourceFile类中取出，引入一个Extractor类，重载里面的extract2txt方法，以便传入不同文件源，进行不同的抽取内容任务。

~~~java
public abstract class ResourceFile {
  protected String filePath;
  public ResourceFile(String filePath) {
    this.filePath = filePath;
  }
}

public class PdfFile extends ResourceFile {
  public PdfFile(String filePath) {
    super(filePath);
  }
  //...
}
//...PPTFile、WordFile代码省略...
public class Extractor {
  public void extract2txt(PPTFile pptFile) {
    //...
    System.out.println("Extract PPT.");
  }

  public void extract2txt(PdfFile pdfFile) {
    //...
    System.out.println("Extract PDF.");
  }

  public void extract2txt(WordFile wordFile) {
    //...
    System.out.println("Extract WORD.");
  }
}

public class ToolApplication {
  public static void main(String[] args) {
    Extractor extractor = new Extractor();
    List<ResourceFile> resourceFiles = listAllResourceFiles(args[0]);
    for (ResourceFile resourceFile : resourceFiles) {
      extractor.extract2txt(resourceFile);
    }
  }

  private static List<ResourceFile> listAllResourceFiles(String resourceDirectory) {
    List<ResourceFile> resourceFiles = new ArrayList<>();
    //...根据后缀(pdf/ppt/word)由工厂方法创建不同的类对象(PdfFile/PPTFile/WordFile)
    resourceFiles.add(new PdfFile("a.pdf"));
    resourceFiles.add(new WordFile("b.word"));
    resourceFiles.add(new PPTFile("c.ppt"));
    return resourceFiles;
  }
}
~~~

不过这里有个小问题，由于传入的是其父类，无法知道具体是哪个资源文件，会导致编译报错。

##### 0.3重载的技巧

小技巧：

ResourceFile定义了一个抽象方法，传入Extractor类。

ResourceFile的实现类则实现accept这个方法，调用Extractor的重载方法extract2txt，将自己传入，这样就传递的是子类了。

~~~java

public abstract class ResourceFile {
  protected String filePath;
  public ResourceFile(String filePath) {
    this.filePath = filePath;
  }
  abstract public void accept(Extractor extractor);
}

public class PdfFile extends ResourceFile {
  public PdfFile(String filePath) {
    super(filePath);
  }

  @Override
  public void accept(Extractor extractor) {
    extractor.extract2txt(this);
  }

  //...
}

//...PPTFile、WordFile跟PdfFile类似，这里就省略了...
//...Extractor代码不变...

public class ToolApplication {
  public static void main(String[] args) {
    Extractor extractor = new Extractor();
    List<ResourceFile> resourceFiles = listAllResourceFiles(args[0]);
    for (ResourceFile resourceFile : resourceFiles) {
      resourceFile.accept(extractor);
    }
  }

  private static List<ResourceFile> listAllResourceFiles(String resourceDirectory) {
    List<ResourceFile> resourceFiles = new ArrayList<>();
    //...根据后缀(pdf/ppt/word)由工厂方法创建不同的类对象(PdfFile/PPTFile/WordFile)
    resourceFiles.add(new PdfFile("a.pdf"));
    resourceFiles.add(new WordFile("b.word"));
    resourceFiles.add(new PPTFile("c.ppt"));
    return resourceFiles;
  }
}
~~~

##### 0.4添加新功能

添加新功能，压缩资源文件。

需要两步：

- 定义一个压缩资源文件的类，并重载对不同文件的压缩方法。

- 文件资源类重载一个压缩资源的方法，所有文件资源的子类需要实现其方法。

~~~java

public abstract class ResourceFile {
  protected String filePath;
  public ResourceFile(String filePath) {
    this.filePath = filePath;
  }
  abstract public void accept(Extractor extractor);
  abstract public void accept(Compressor compressor);
}

public class PdfFile extends ResourceFile {
  public PdfFile(String filePath) {
    super(filePath);
  }

  @Override
  public void accept(Extractor extractor) {
    extractor.extract2txt(this);
  }

  @Override
  public void accept(Compressor compressor) {
    compressor.compress(this);
  }

  //...
}
}
//...PPTFile、WordFile跟PdfFile类似，这里就省略了...
//...Extractor代码不变

public class ToolApplication {
  public static void main(String[] args) {
    Extractor extractor = new Extractor();
    List<ResourceFile> resourceFiles = listAllResourceFiles(args[0]);
    for (ResourceFile resourceFile : resourceFiles) {
      resourceFile.accept(extractor);
    }

    Compressor compressor = new Compressor();
    for(ResourceFile resourceFile : resourceFiles) {
      resourceFile.accept(compressor);
    }
  }

  private static List<ResourceFile> listAllResourceFiles(String resourceDirectory) {
    List<ResourceFile> resourceFiles = new ArrayList<>();
    //...根据后缀(pdf/ppt/word)由工厂方法创建不同的类对象(PdfFile/PPTFile/WordFile)
    resourceFiles.add(new PdfFile("a.pdf"));
    resourceFiles.add(new WordFile("b.word"));
    resourceFiles.add(new PPTFile("c.ppt"));
    return resourceFiles;
  }
}
~~~

##### 0.5隔离变化

解决违背开闭原则的问题。

- 将所有功能类根据不同资源文件提取算法的方法，抽象到Visitor接口类。

- 资源文件类的accept不需要为了不同的功能重载了，只需要传入所有功能的接口即可。

~~~java

public abstract class ResourceFile {
  protected String filePath;
  public ResourceFile(String filePath) {
    this.filePath = filePath;
  }
  abstract public void accept(Visitor vistor);
}

public class PdfFile extends ResourceFile {
  public PdfFile(String filePath) {
    super(filePath);
  }

  @Override
  public void accept(Visitor visitor) {
    visitor.visit(this);
  }

  //...
}
//...PPTFile、WordFile跟PdfFile类似，这里就省略了...

public interface Visitor {
  void visit(PdfFile pdfFile);
  void visit(PPTFile pdfFile);
  void visit(WordFile pdfFile);
}

public class Extractor implements Visitor {
  @Override
  public void visit(PPTFile pptFile) {
    //...
    System.out.println("Extract PPT.");
  }

  @Override
  public void visit(PdfFile pdfFile) {
    //...
    System.out.println("Extract PDF.");
  }

  @Override
  public void visit(WordFile wordFile) {
    //...
    System.out.println("Extract WORD.");
  }
}

public class Compressor implements Visitor {
  @Override
  public void visit(PPTFile pptFile) {
    //...
    System.out.println("Compress PPT.");
  }

  @Override
  public void visit(PdfFile pdfFile) {
    //...
    System.out.println("Compress PDF.");
  }

  @Override
  public void visit(WordFile wordFile) {
    //...
    System.out.println("Compress WORD.");
  }

}

public class ToolApplication {
  public static void main(String[] args) {
    Extractor extractor = new Extractor();
    List<ResourceFile> resourceFiles = listAllResourceFiles(args[0]);
    for (ResourceFile resourceFile : resourceFiles) {
      resourceFile.accept(extractor);
    }

    Compressor compressor = new Compressor();
    for(ResourceFile resourceFile : resourceFiles) {
      resourceFile.accept(compressor);
    }
  }

  private static List<ResourceFile> listAllResourceFiles(String resourceDirectory) {
    List<ResourceFile> resourceFiles = new ArrayList<>();
    //...根据后缀(pdf/ppt/word)由工厂方法创建不同的类对象(PdfFile/PPTFile/WordFile)
    resourceFiles.add(new PdfFile("a.pdf"));
    resourceFiles.add(new WordFile("b.word"));
    resourceFiles.add(new PPTFile("c.ppt"));
    return resourceFiles;
  }
}
~~~

##### 双分派

面试题：为什么支持双分派的语言就不需要访问者模式？

- 单分派（Single Dispatch）。
  - 执行哪个对象的方法，根据对象的**运行时类型**来决定。
  - 执行对象的哪个方法，根据方法参数的**编译时类型**来决定。
- 双分派（Double Dispatch）。
  - 执行哪个对象的方法，根据对象的**运行时类型**来决定。
  - 执行对象的哪个方法，根据方法参数的**运行时类型**来决定。

- Dispatch：可以把方法调用理解为一种消息传递（dispatch）。
- Single：执行哪个对象的哪个方法，只跟对象有关。
- Double：执行哪个对象的哪个方法，跟对象和方法参数有关。

- 目前主流的面向对象编程语言（java、C++、C#等）都只支持Single Dispatch。

~~~java
public class ParentClass {
  public void f() {
    System.out.println("I am ParentClass's f().");
  }
}

public class ChildClass extends ParentClass {
  public void f() {
    System.out.println("I am ChildClass's f().");
  }
}

public class SingleDispatchClass {
  public void polymorphismFunction(ParentClass p) {
    p.f();
  }

  public void overloadFunction(ParentClass p) {
    System.out.println("I am overloadFunction(ParentClass p).");
  }

  public void overloadFunction(ChildClass c) {
    System.out.println("I am overloadFunction(ChildClass c).");
  }
}

public class DemoMain {
  public static void main(String[] args) {
    SingleDispatchClass demo = new SingleDispatchClass();
    ParentClass p = new ChildClass();
    demo.polymorphismFunction(p);//执行哪个对象的方法，由对象的实际类型决定
    demo.overloadFunction(p);//执行对象的哪个方法，由参数对象的声明类型决定
  }
}

//代码执行结果:
I am ChildClass's f().
I am overloadFunction(ParentClass p).
~~~

- 重载方法参数
  - 编译时类型，声明类型（ParentClass）
- 多态传递参数
  - 运行时类型，实际类型（ChildClass）

#### 8、备忘录模式

备忘录模式（Memento Design Pattern），在不违背封装原则的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，以便之后恢复对象为先前的状态。

##### 文本备份

~~~java

public class InputText {
  private StringBuilder text = new StringBuilder();

  public String getText() {
    return text.toString();
  }

  public void append(String input) {
    text.append(input);
  }

  public Snapshot createSnapshot() {
    return new Snapshot(text.toString());
  }

  public void restoreSnapshot(Snapshot snapshot) {
    this.text.replace(0, this.text.length(), snapshot.getText());
  }
}

public class Snapshot {
  private String text;

  public Snapshot(String text) {
    this.text = text;
  }

  public String getText() {
    return this.text;
  }
}

public class SnapshotHolder {
  private Stack<Snapshot> snapshots = new Stack<>();

  public Snapshot popSnapshot() {
    return snapshots.pop();
  }

  public void pushSnapshot(Snapshot snapshot) {
    snapshots.push(snapshot);
  }
}

public class ApplicationMain {
  public static void main(String[] args) {
    InputText inputText = new InputText();
    SnapshotHolder snapshotsHolder = new SnapshotHolder();
    Scanner scanner = new Scanner(System.in);
    while (scanner.hasNext()) {
      String input = scanner.next();
      if (input.equals(":list")) {
        System.out.println(inputText.toString());
      } else if (input.equals(":undo")) {
        Snapshot snapshot = snapshotsHolder.popSnapshot();
        inputText.restoreSnapshot(snapshot);
      } else {
        snapshotsHolder.pushSnapshot(inputText.createSnapshot());
        inputText.append(input);
      }
    }
  }
}
~~~

##### 优化内存和事件消耗

- 全量备份。
  - 低效率的备份，直接拿来恢复。
- 增量备份。
  - 先找到最近一次全量备份，之后执行此次全量备份之后的所有增量备份，也就是对应的操作或者数据变动。

#### 9、命令模式

命令模式（Command Design Pattern），将请求（命令）封装为一个对象，这样可以使用不同的请求参数化其他对象（将不同请求依赖注入到其他对象），并且能够支持请求（命令）的排序执行、记录日志、撤销等（附加控制）功能。

- 利用多线程，一个线程接收请求后，启动一个新的线程来处理请求。
- 一个线程内轮询请求和处理请求。
  - 缺点是无法利用多线程多核处理的优势。
  - 优点是对于IO密集型的业务，避免了多线程不停切换对性能的损耗。

##### 第二种实现方式

~~~java
// 命令接口
public interface Command {
  void execute();
}
// 得到钻石的命令
public class GotDiamondCommand implements Command {
  // 省略成员变量

  public GotDiamondCommand(/*数据*/) {
    //...
  }

  @Override
  public void execute() {
    // 执行相应的逻辑
  }
}
// 启动命令/有障碍命令/归档命令
//GotStartCommand/HitObstacleCommand/ArchiveCommand类省略

public class GameApplication {
  private static final int MAX_HANDLED_REQ_COUNT_PER_LOOP = 100;
  private Queue<Command> queue = new LinkedList<>();

  public void mainloop() {
    while (true) {
      List<Request> requests = new ArrayList<>();
      
      //省略从epoll或者select中获取数据，并封装成Request的逻辑，
      //注意设置超时时间，如果很长时间没有接收到请求，就继续下面的逻辑处理。
      
      for (Request request : requests) {
        Event event = request.getEvent();
        Command command = null;
        if (event.equals(Event.GOT_DIAMOND)) {
          command = new GotDiamondCommand(/*数据*/);
        } else if (event.equals(Event.GOT_STAR)) {
          command = new GotStartCommand(/*数据*/);
        } else if (event.equals(Event.HIT_OBSTACLE)) {
          command = new HitObstacleCommand(/*数据*/);
        } else if (event.equals(Event.ARCHIVE)) {
          command = new ArchiveCommand(/*数据*/);
        } // ...一堆else if...

        queue.add(command);
      }

      int handledCount = 0;
      while (handledCount < MAX_HANDLED_REQ_COUNT_PER_LOOP) {
        if (queue.isEmpty()) {
          break;
        }
        Command command = queue.poll();
        command.execute();
      }
    }
  }
}
~~~

- 跟策略模式、工厂模式有些相似。
- 区别设计模式：应用场景（解决什么问题）、解决方案（具体实现）。
  - 设计模式的主要区别在于设计意图，也就是应用场景。
  - 策略模式侧重“策略”或“算法”。
  - 工厂模式侧重封装对象的创建过程。
  - 策略具有相同的目的，而命令具有不同的目的。

#### 10、解释器模式

解释器模式（Interpreter Design Pattern），为某个语言定义它的语法（或者叫文法）表示，并定义一个解释器用来处理这个语法。

##### 自定义语法

- 语法：+、-、*、/、数字

- 表达式：8 3 2 4 - + * 

- 结果：28 

~~~java
// 语法翻译
public class ExpressionInterpreter {
  private Deque<Long> numbers = new LinkedList<>();
  // 翻译
  public long interpret(String expression) {
    String[] elements = expression.split(" ");
    int length = elements.length;
    // 将8、3、2、4添加到队列尾部
    for (int i = 0; i < (length+1)/2; ++i) {
      numbers.addLast(Long.parseLong(elements[i]));
    }
    // 遍历-、+、*
    for (int i = (length+1)/2; i < length; ++i) {
      String operator = elements[i];
      boolean isValid = "+".equals(operator) || "-".equals(operator)
              || "*".equals(operator) || "/".equals(operator);
      if (!isValid) {
        throw new RuntimeException("Expression is invalid: " + expression);
      }

      long number1 = numbers.pollFirst();
      long number2 = numbers.pollFirst();
      long result = 0;
      if (operator.equals("+")) {
        result = number1 + number2;
      } else if (operator.equals("-")) {
        result = number1 - number2;
      } else if (operator.equals("*")) {
        result = number1 * number2;
      } else if (operator.equals("/")) {
        result = number1 / number2;
      }
      numbers.addFirst(result);
    }

    if (numbers.size() != 1) {
      throw new RuntimeException("Expression is invalid: " + expression);
    }

    return numbers.pop();
  }
}
~~~

##### 重构

~~~java
// 语法接口
public interface Expression {
  long interpret();
}
// 数字语法
public class NumberExpression implements Expression {
  private long number;

  public NumberExpression(long number) {
    this.number = number;
  }

  public NumberExpression(String number) {
    this.number = Long.parseLong(number);
  }

  @Override
  public long interpret() {
    return this.number;
  }
}
// 加法语法
public class AdditionExpression implements Expression {
  private Expression exp1;
  private Expression exp2;

  public AdditionExpression(Expression exp1, Expression exp2) {
    this.exp1 = exp1;
    this.exp2 = exp2;
  }

  @Override
  public long interpret() {
    return exp1.interpret() + exp2.interpret();
  }
}
// 减法、乘法、除法
// SubtractionExpression/MultiplicationExpression/DivisionExpression

public class ExpressionInterpreter {
  private Deque<Expression> numbers = new LinkedList<>();

  public long interpret(String expression) {
    String[] elements = expression.split(" ");
    int length = elements.length;
    for (int i = 0; i < (length+1)/2; ++i) {
      numbers.addLast(new NumberExpression(elements[i]));
    }

    for (int i = (length+1)/2; i < length; ++i) {
      String operator = elements[i];
      boolean isValid = "+".equals(operator) || "-".equals(operator)
              || "*".equals(operator) || "/".equals(operator);
      if (!isValid) {
        throw new RuntimeException("Expression is invalid: " + expression);
      }

      Expression exp1 = numbers.pollFirst();
      Expression exp2 = numbers.pollFirst();
      Expression combinedExp = null;
      if (operator.equals("+")) {
        combinedExp = new AdditionExpression(exp1, exp2);
      } else if (operator.equals("-")) {
        combinedExp = new AdditionExpression(exp1, exp2);
      } else if (operator.equals("*")) {
        combinedExp = new AdditionExpression(exp1, exp2);
      } else if (operator.equals("/")) {
        combinedExp = new AdditionExpression(exp1, exp2);
      }
      long result = combinedExp.interpret();
      numbers.addFirst(new NumberExpression(result));
    }

    if (numbers.size() != 1) {
      throw new RuntimeException("Expression is invalid: " + expression);
    }

    return numbers.pop().interpret();
  }
}
~~~

##### 自定义接口告警规则

- 功能：api异常数超过某个值时告警。
- 语法：||、&&、>、<、==，数字，字符key。
- 表达式：api_error_per_minute > 100 || api_count_per_minute > 10000
  - 当前每分钟API出错数大于100，或者每分钟API被调用数大于10000时告警。

~~~java
// 告警规则解析
public class AlertRuleInterpreter {

  // key1 > 100 && key2 < 1000 || key3 == 200
  public AlertRuleInterpreter(String ruleExpression) {
    //TODO:由你来完善
  }

  //<String, Long> apiStat = new HashMap<>();
  //apiStat.put("key1", 103);
  //apiStat.put("key2", 987);
  public boolean interpret(Map<String, Long> stats) {
    //TODO:由你来完善
  }

}

public class DemoTest {
  public static void main(String[] args) {
    String rule = "key1 > 100 && key2 < 30 || key3 < 100 || key4 == 88";
    AlertRuleInterpreter interpreter = new AlertRuleInterpreter(rule);
    Map<String, Long> stats = new HashMap<>();
    stats.put("key1", 101l);
    stats.put("key3", 121l);
    stats.put("key4", 88l);
    boolean alert = interpreter.interpret(stats);
    System.out.println(alert);
  }
}
~~~

~~~java
// 解析接口类
public interface Expression {
  boolean interpret(Map<String, Long> stats);
}
// 大于
public class GreaterExpression implements Expression {
  private String key;
  private long value;

  public GreaterExpression(String strExpression) {
    String[] elements = strExpression.trim().split("\\s+");
    if (elements.length != 3 || !elements[1].trim().equals(">")) {
      throw new RuntimeException("Expression is invalid: " + strExpression);
    }
    this.key = elements[0].trim();
    this.value = Long.parseLong(elements[2].trim());
  }

  public GreaterExpression(String key, long value) {
    this.key = key;
    this.value = value;
  }

  @Override
  public boolean interpret(Map<String, Long> stats) {
    if (!stats.containsKey(key)) {
      return false;
    }
    long statValue = stats.get(key);
    return statValue > value;
  }
}
// 小于、等于
// LessExpression/EqualExpression
// 与&&
public class AndExpression implements Expression {
  private List<Expression> expressions = new ArrayList<>();

  public AndExpression(String strAndExpression) {
    String[] strExpressions = strAndExpression.split("&&");
    for (String strExpr : strExpressions) {
      if (strExpr.contains(">")) {
        expressions.add(new GreaterExpression(strExpr));
      } else if (strExpr.contains("<")) {
        expressions.add(new LessExpression(strExpr));
      } else if (strExpr.contains("==")) {
        expressions.add(new EqualExpression(strExpr));
      } else {
        throw new RuntimeException("Expression is invalid: " + strAndExpression);
      }
    }
  }

  public AndExpression(List<Expression> expressions) {
    this.expressions.addAll(expressions);
  }

  @Override
  public boolean interpret(Map<String, Long> stats) {
    for (Expression expr : expressions) {
      if (!expr.interpret(stats)) {
        return false;
      }
    }
    return true;
  }

}
// or
public class OrExpression implements Expression {
  private List<Expression> expressions = new ArrayList<>();
  // key1 > 100 && key2 < 30 || key3 < 100 || key4 == 88
  public OrExpression(String strOrExpression) {
    // key1 > 100 && key2 < 30 --> key1 > 100、key2 < 30
    // key3 < 100
    // key4 == 88
    String[] andExpressions = strOrExpression.split("\\|\\|");
    for (String andExpr : andExpressions) {
      expressions.add(new AndExpression(andExpr));
    }
  }

  public OrExpression(List<Expression> expressions) {
    this.expressions.addAll(expressions);
  }

  @Override
  public boolean interpret(Map<String, Long> stats) {
    for (Expression expr : expressions) {
      if (expr.interpret(stats)) {
        return true;
      }
    }
    return false;
  }
}
// 告警规则解释器
public class AlertRuleInterpreter {
  private Expression expression;

  public AlertRuleInterpreter(String ruleExpression) {
    this.expression = new OrExpression(ruleExpression);
  }

  public boolean interpret(Map<String, Long> stats) {
    return expression.interpret(stats);
  }
} 
~~~

#### 11、中介模式

中介模式（Mediator Design Pattern），定义了一个单独的（中介）对象，来封装一组对象之间的交互。将这组对象之间的交互委派给与中介对象交互，来避免对象之间的直接交互。

- 将复杂的网状关系转换为一对多星状关系。

##### UI组件

~~~java
// 中介接口
public interface Mediator {
  void handleEvent(Component component, String event);
}
// 登录页对话框
public class LandingPageDialog implements Mediator {
  private Button loginButton;
  private Button regButton;
  private Selection selection;
  private Input usernameInput;
  private Input passwordInput;
  private Input repeatedPswdInput;
  private Text hintText;

  @Override
  public void handleEvent(Component component, String event) {
    if (component.equals(loginButton)) {// 登录逻辑
      String username = usernameInput.text();
      String password = passwordInput.text();
      //校验数据...
      //做业务处理...
    } else if (component.equals(regButton)) {// 注册逻辑
      //获取usernameInput、passwordInput、repeatedPswdInput数据...
      //校验数据...
      //做业务处理...
    } else if (component.equals(selection)) {// 下拉选择
      String selectedItem = selection.select();
      if (selectedItem.equals("login")) { // 登录控件显示
        usernameInput.show();
        passwordInput.show();
        repeatedPswdInput.hide();
        hintText.hide();
        //...省略其他代码
      } else if (selectedItem.equals("register")) { // 注册控件显示
        //....
      }
    }
  }
}
// UI控件
public class UIControl {
  private static final String LOGIN_BTN_ID = "login_btn";
  private static final String REG_BTN_ID = "reg_btn";
  private static final String USERNAME_INPUT_ID = "username_input";
  private static final String PASSWORD_INPUT_ID = "pswd_input";
  private static final String REPEATED_PASSWORD_INPUT_ID = "repeated_pswd_input";
  private static final String HINT_TEXT_ID = "hint_text";
  private static final String SELECTION_ID = "selection";

  public static void main(String[] args) {
    Button loginButton = (Button)findViewById(LOGIN_BTN_ID);
    Button regButton = (Button)findViewById(REG_BTN_ID);
    Input usernameInput = (Input)findViewById(USERNAME_INPUT_ID);
    Input passwordInput = (Input)findViewById(PASSWORD_INPUT_ID);
    Input repeatedPswdInput = (Input)findViewById(REPEATED_PASSWORD_INPUT_ID);
    Text hintText = (Text)findViewById(HINT_TEXT_ID);
    Selection selection = (Selection)findViewById(SELECTION_ID);

    Mediator dialog = new LandingPageDialog();
    dialog.setLoginButton(loginButton);
    dialog.setRegButton(regButton);
    dialog.setUsernameInput(usernameInput);
    dialog.setPasswordInput(passwordInput);
    dialog.setRepeatedPswdInput(repeatedPswdInput);
    dialog.setHintText(hintText);
    dialog.setSelection(selection);

    loginButton.setOnClickListener(new OnClickListener() {
      @Override
      public void onClick(View v) {
        dialog.handleEvent(loginButton, "click");
      }
    });

    regButton.setOnClickListener(new OnClickListener() {
      @Override
      public void onClick(View v) {
        dialog.handleEvent(regButton, "click");
      }
    });

    //....
  }
}
~~~

##### 对比观察者

- 在观察者模式中，一个参与者要么是观察者，要么是被观察者，参与者之间的交互关系比较有条理。
- 而中介模式正好相反，只有当参与者之间的交互关系错综复杂，维护成本很高的时候，才考虑使用中介模式。

### 应对大型项目

#### 设计原则和思想

- 封装与抽象
  - Everything is a file，翻译是一切皆文件。
  - 抽象为统一的访问方式，更高层的代码就能基于统一的访问方式来访问底层不同的实现。这样做的好处是隔离底层访问的复杂性，简化上层代码的编写，且更容易复用。
  - 封装复杂代码，隔离实现的易变性，提供简单、统一的访问接口，其他模块基于抽象的接口而非具体的实现编程，代码会更加稳定。
- 分层模块化
  - 不同的模块之间通过接口来进行通信，模块之间耦合很小，将各个模块组装起来，就是一个超级复杂的系统。
  - 分层，计算机领域的任何问题都可以通过增加一个间接的中间层来解决。

- 基于接口通信
  - 接口从命名到定义都要抽象一些，尽量少涉及具体的实现细节。
- 高内聚、松耦合
  - 聚焦小范围的代码，不需要了解太多其他模块或类，降低阅读和修改代码的难度。
- 为扩展而设计
  - 提前思考未来可能会有哪些功能需要扩展，提前预留扩展点，以便在不改动代码情况下，添加新的功能。
  - 做到代码可扩展，需要代码满足开闭原则，基于扩展而非修改来添加新功能。
  - 封装抽象、隔离变化，提供抽象的不可变接口，根据替换实现来达到代码的修改。
- KISS原则
  - 可读性好是首要，避免过度设计、过早优化，在扩展性和可读性之间做出权衡。
- 最小惊奇原则
  - The Least Surprise Principle，这个原则等同于“遵守开发规范”，意思是在做设计或者编码时，要遵守统一的开发规范，避免反直觉的设计。

#### 研发管理和开发技巧

- 严格执行编码规范。
  - 共同的想象，提高多人合作的效率。
- 编写高质量的单元测试。
  - 覆盖率高。
  - 测试要全，异常、边界等条件的测试。
- code review
  - 执行到位，不能流于形式。
- 开发未动，文档先行。
  - 成本低，易交接，易评审。
- 持续重构
  - 时刻保证代码质量，防止代码腐化。
- 对项目与团队进行拆分
  - 将一个大的团队拆分成多个小团队，分别负责一个模块或微服务的开发。

#### Code Review

- 改进的空间
  - 自己觉得写得再好的代码，在经过多人多视角的审查，总会有改进的空间。
- 团队项目
  - 项目代码的质量是团队整体代码的质量，而不是某个人的代码质量。
- 可读性
  - 代码可读性好，说明代码足够简单，出错可能性小，bug少。
  - 自己觉得容易的代码，别人看来不一定简单，所谓旁观者清，当局者迷。
- 口口相传
  - 通过代码评审，将实践技术传递给他人学习，比自己学习、摸索来得更高效。
- 不止一人熟悉
  - 通过代码评审，可以让团队的人都熟悉代码，保证同事离职，需要反复沟通。
- 技术氛围
  - 通过代码评审，可以让团队人员尽量地写出好的代码、提出好的建议，增进技术交流。

- 沟通方式
  - Talk is cheap, show me the code.翻译是：空谈是廉价的，给我看看代码。
- 自律
  - 直播写代码，坏的代码应该被指出，好的代码将得到赏识。
- 无法落实怎么办？
  - 就好比盲打敲键盘，前期可能需要不断练习，后面还不是顺手就来，花费不了多少时间。
  - 代码评审，前期需要一个checklist照着做，当熟练后，扫一遍代码就能揪出绝大部分问题。

### Google Guava

#### 通用功能模块

- 与业务无关的通用功能模块，常见有三类：
  - 类库（library）
  - 框架（framework）
  - 功能组件（component）
- 比如：
  - Google Guava属于类库，提供一组API接口。
  - EventBus、DI容器属于框架，提供骨架代码。
  - ID生成器、性能计数器属于功能组件，提供一组特殊功能的API接口，类似类库，但更聚焦和重量级。
    - 比如ID生成器可能会依赖Redis等外部系统。
- 共同特点
  - 复用。
  - 业务无关。
- 如何开发通用的功能模块?
  - 产品意识
    - 当做一个产品（技术产品）来开发，目标用户是”程序员“，解决的是他们的”开发痛点“。
  - 技术指标：bug少、性能好、易用、已集成、易插拔、文档全、易上手等。
    - 产品素质很重要，特别是那些容易忽视、不被重视的东西，会决定你的技术产品是否能在众多同类中脱颖而出。
  - 比如：Google Guava
    - Google管理、长期维护，经过充分的单元测试，代码质量有保证。
    - 可靠、性能好、高度优化，比如Immutable Collections比JDK中的unmodifiableCollection性能好。
    - 全面、完善的文档，容易上手，学习成本低。
  - 服务意识
    - 感谢他人的使用。
    - 做好客服角色的心理准备。
  - 阅读著名开源项目代码、参与开源项目来提高技术。
  - 不要重复造轮子，能复用就复用。
  - 把通用的功能作为项目的一部分来开发。
    - 做好模块化工作，通过接口、扩展点与其它模块交互。
    - 耦合度低，分离出来的成本也就不会很高。

#### 用到的设计模式

##### Builder模式



##### Wrapper模式



##### Immutable模式





### 模式索引

#### Calendar

- 工厂方法、构造者模式。

#### IO流

- 装饰器模式。

#### Collections

- 装饰器模式、适配器模式、模板模式。

#### EventBus

Google Guava

- java.util.Observable
- java.util.Observer

观察者模式

#### Runtime

- java.lang.Runtime

单例类

#### Java Servlet

模板模式

#### JUnit TestCase

模板模式

#### Java InputStream

模板模式

#### Java AbstractList

模板模式

#### Integer等包装类

享元模式

#### Java Servlet Filter

职责链模式

#### Spring Interceptor

职责链模式


