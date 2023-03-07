#设计模式

##5大设计原则 S·O·L·I·D
###单一职责原则（Single Responsibility Principle）
**对于一个类的修改原因，应该只有一个**，如果出现多个因素会影响到类的更新，则需要考虑用新的类。职责的划分对应开发的抽象能力，结合实际的业务场景对职责的粗粒度分割。

###开放关闭原则（Open Close Principle）
**类应该对扩展开放，对修改关闭。**

> 设计符合开闭原则的代码需要花费大量的精力，在最有可能改变的地方使用开闭原则

###里式替换原则（Liskov Substitution Principle）
一个子类必须能够替换所有的父类对象。

###接口隔离原则（Interface Segregation Principle）
不应该强迫客户依赖他们不需要的方法。多个专门的接口比一个总的接口好。

###依赖倒置原则（Dependency Inversion Principle）
高层模块不应该依赖低层模块，二者都应该依赖于抽象。抽象不能依赖于细节，细节依赖于抽象。

##OOD设计思想