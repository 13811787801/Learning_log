# Software Design

## Don't Repeat Yourself(DRY)

在两个或多个地方发现一些相似代码的时候，我们需要把它们的共性抽象出来形成一个唯一的新方法，并且改变现有地方的代码让它们以一些合适的参数调用这个新的方法

## Keep it simple,stupid(KISS)

复杂的东西越来越被众人所鄙视了，而简单的东西越来越被人所认可。宜家（IKEA）简约、高效的家居设计和生产思路；微软（Microsoft）“所见即所得”的理念；谷歌（Google）简约、直接的商业风格，无一例外地遵循了“KISS”原则。也正是“KISS”原则，成就了这些看似神奇的商业经典

## Program to Interface,not a implement

面向接口编程，而不是实现~  
接口是抽象是稳定的，实现则是多种多样的。在面向对象的 S.O.L.I.D 原则中会提到我们的依赖倒置原则，就是这个原则的另一种样子。还有一条原则叫 Composition over inheritance（喜欢组合而不是继承），这两条是那 23 个经典设计模式中的设计原则

## You Ain’t Gonna Need It (YAGNI)

只考虑和设计必须的功能，避免过度设计。只实现目前需要的功能，在以后你需要更多功能时，可以再进行添加。如无必要，勿增复杂性。软件开发是一场 trade-off 的博弈
