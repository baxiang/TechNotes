什么是角色
当说到程序的权限管理时，人们往往想到角色这一概念。角色是代表一系列可执行的操作或责任的实体，用于限定你在软件系统中能做什么、不能做什么。用户帐号往往与角色相关联，因此，一个用户在软件系统中能做什么取决于与之关联的各个角色。
例如，一个用户以关联了”项目管理员”角色的帐号登录系统，那这个用户就可以做项目管理员能做的所有事情――如列出项目中的应用、管理项目组成员、产生项目报表等。
从这个意义上来说，角色更多的是一种行为的概念：它表示用户能在系统中进行的操作。
基于角色的访问控制（Role-Based Access Control）
既然角色代表了可执行的操作这一概念，一个合乎逻辑的做法是在软件开发中使用角色来控制对软件功能和数据的访问。你可能已经猜到，这种权限控制方法就叫基于角色的访问控制(Role-Based Access Control)，或简称为RBAC。

有两种正在实践中使用的RBAC访问控制方式：隐式(模糊)的方式和显示(明确)的方式。

今天依旧有大量的软件应用是使用隐式的访问控制方式。但我肯定的说，显示的访问控制方式更适合于当前的软件应用。

 

隐式的访问控制

前面提到，角色代表一系列的可执行的操作。但我们如何知道一个角色到底关联了哪些可执行的操作呢？

答案是：目前的大多数应用，你并能不明确的知道一个角色到底关联了哪些可执行操作。可能你心里是清楚的（你知道一个有”管理员”角色的用户可以锁定用户帐号、进行系统配置；一个关联了”消费者”这一角色的用户可在网站上进行商品选购），但这些系统并没有明确定义一个角色到底包含了哪些可执行的行为。

拿”项目管理员”来说，系统中并没有对”项目管理员”能进行什么样的操作进行明确定义，它仅是一个字符串名词。开发人员通常将这个名词写在程序里以进行访问控制。例如，判断一个用户是否能查看项目报表，程序员可能会编码如下：

 

代码块1. 隐式地基于角色的权限控制:
if (user.hasRole("Project Manager") ) {
    //show the project report button
} else {
    //don't show the button
}

 

在上面的示例代表中，开发人员判断用户是否有”项目管理员”角色来决定是否显示查看项目报表按钮。请注意上面的代码，它并没有明确语句来定义”项目管理员”这一角色到底包含哪些可执行的行为，它只是假设一个关联了项目管理员角色的用户可查看项目报表，而开发人员也是基于这一假设来写 if/else 语句。

 

脆弱的权限策略

像上面的权限访问控制是非常脆弱的。一个极小的权限方面的需求的变动都可能导致上面的代码需要重新修改。

举例来说，假如某一天这个开发团队被告知：“哦，顺便说一下，我们需要一个’部门管理员’角色，他们也可以查看项目报表。请做到这一点。”

这种情况下，开发人员需要找到上面的代码块并将其修改为：

 

代码块2. 修改过的隐式的基于角色的权限控制:
if (user.hasRole("Project Manager") || user.hasRole("Department Manager") ) {
    //show the project report button
} else {
    //don't show the button
}

 

随后，开发人员需要更新他的测试用例、重新编译系统，还可能需要重走软件质量控制(QA)流程，然后再重新部署上线。这一切仅仅是因为一个微小的权限方面的需求变动！

后面如果需求方又回来告诉你说我们又有另一个角色可查看报表，或是前面关于”部门管理员可查看报表”的需求不再需要了，岂不把人累死了。

如果需求方要求动态地创建、删除角色以便他们自己配置角色，又该如何应对呢？

像上面的情况，这种隐式的(静态字符串)形式的基于角色的访问控制方式难以满足需求。理想的情况是如果权限需求变动不需要修改任何代码。怎样才能做到这一点呢？

 

显式地访问控制：更好的选择

从上面的例子我们看到，当权限需求发生变动时，隐式的权限访问控制方式会给程序开发带来沉重的负担。如果能有一种方式在权限需求发生变化时不需要去修改代码就能满足需求那就好了。理解的情况是，即使是正在运行的系统，你也可以修改权限策略却又不影响最终用户的使用。当你发现某些错误的或危险的安全策略时，你可以迅速地修改策略配置，同时你的系统还能正常使用，而不需要重构代码重新部署系统。

怎样才能达到上面的理想效果呢？我们可以通过显式的(明确的)界定我们在应用中能做的操作来进行。

回顾上面隐式的权限控制的例子，思考一下这些代码最终的目的，想一下它们最终是要做什么样的控制？

从根本上说，这些代码最终是在保护资源(项目报表)，是要界定一个用户能对这些资源进行什么样的操作(查看/修改)。当我们将权限访问控制分解到这种最原始的层次，我们就可以用一种更细粒度(更富有弹性)的方式来表达权限控制策略。

我们可以修改上面的代码块，以基于资源的语义来更有效地进行权限访问控制：

 

代码块3. 显式的权限控制:
if (user.isPermitted("projectReport:view:12345")) {
    //show the project report button
} else {
    //don't show the button
}

 

上面的例子中，我们可明确地看到我们是在控制什么。不要太在意冒号分隔的语法，这仅是一个例子，重点是上面的语句明确地表示了“如果当前用户允许查看编号为12345的项目报表，则显示项目报表按钮”。也就是说，我们明确地说明了一个用户帐号可对一个的资源实例进行的具体的操作。

 

为什么说这种方式更好

上面最后的示例代码块与前面的代码的主要区别：最后的代码块是基于什么是受保护的， 而不是谁可能有能力做什么。看似简单的区别，但后者对系统开发及部署有着深刻的影响：

 

l 更少的代码重构：我们是基于系统的功能(系统的资源及对资源的操作)来进行权限控制，而相应来说，系统的功能需求一旦确定下来后，一段时间内对它的改动相应还是比较少的。只是当系统的功能需求改变时，才会涉及到权限代码的改变。例如上面提到的查看项目报表的功能，显式的权限控制方式不会像传统隐式的RBAC权限控制那样因不同的用户/角色要进行这个操作就需要重构代码；只要这个功能存在，显式的方式的权限控制代码是不需要改变的。

 

l 更直观：保护资源对象、控制对资源对象的操作(对象及对象的行为)，这样的权限控制方式更符合人们的思想习惯。正因为符合这种直观的思维方式，面向对象的编辑思想及REST通信模型变得非常成功。

 

l 更有弹性：上面的示例代码中没有写死哪些用户、组或角色可对资源进行什么操作。这意味着它可支持任何安全模型的设计。例如，可以将操作（权限）直接分配给用户 ，或者他们可以被分配到一个角色，然后再将角色与用户关联，或者将多个角色关联到组(group)上，等等。你完全可以根据应用的特点定制权限模型。

 
l 外部安全策略管理：由于源代码只反映资源和行为，而不是用户、组和角色，这样资源/行为与用户、组、角色的关联可以通过外部的模块或专用工具或管理控制台来完成。这意味着在权限需求变化时，开发人员并不需要花费时间来修改代码，业务分析师甚至最终用户就可以通过相应的管理工具修改权限策略配置。


l 可在运行环境做修改：因为基于资源的权限控制代码并不依赖于行为的主体(如组、角色、用色)，你并没有将行为的主体的字符名词写在代码中，所以你甚至可以在程序运行的时候通过修改主体能对资源进行的操作这样一些方式，通过配置的方式就可应对权限方面需求的变动，再也不需要像隐式的RBAC方式那样需要重构代码。


RBAC新解：Resource-Based Access Control
对于上面列出的诸多好处，我重点要说是这种显式的机制带给我们的富有弹性的权限模型。

如果你仍想保留或模拟传统的基于角色的权限访问控制，你可以将权限(可执行的操作)直接分配给某个角色。这种情况下，你依旧是在使用基于角色的权限访问控制方式(不同之处在于你需要明确地界定角色中的权限，而不是传统的使用角色字符串隐式地进行权限控制)。

但在这种新的模型下，已不必再局限于角色了。你可以将权限直接分配给用户、组或其它你觉得可以的对象。

因为上面显式地、基于资源的权限访问控制的诸多好处，或许可以给RBAC一个新的定义：“Resource-Based Access Control”。只是我的一个想法：）

 

现实世界的例子：Apache Shiro

如果你好奇现实世界有没有被多个系统使用的基于资源的权限控制框架，你可以了解一下Apache Shiro。它是一个java平台的现代权限管理框架。通过它的权限(Permission)概念，Shiro很好地支持基于资源的权限访问控制。

当然，并不需要借用Shiro来理解本文所说的一些概念，但Shiro对理解本文所讨论的概念及示例有一定的帮助。