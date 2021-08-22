设计模式-状态模式



前言:

- 状态模式属于设计模式中的行为型模式。

- 行为型模式:用于描述程序在运行时复杂的流程控制，即描述多个类或对象之间怎样相互协作共同完成单个对象都无法单独完成的任务，它涉及算法与对象间职责的分配。

简介:

状态模式是一种行为型设计模式，它允许对象在其内部状态发生变化时改变其行为。这种模式和有限状态机的概念类似。状态模式可以理解为通过调用接口中定义的方法来切换策略的模式。

在计算机编程中状态模式通常用于封装同一对象的不同行为，基于其内部状态。对象可以在运行时更改行为，而无需求助于条件语句的简洁方式,可以提高代码的可维护性。

概述:

状态模式旨在解决两个问题:

- 当一个对象的内部状态改变时,应该改变其行为。
- 独立定义特定于状态的行为。也就是说，添加新状态不应影响现有状态的行为。

直接在类实现特定于状态的行为是不灵活的,这样将类赋予了特定的行为,无法在以后独立于类添加新的状态或更改状态的行为。所以该模式往往:

- 定义单独的（状态）对象，为每个状态封装特定于状态的行为。即定义一个接口（状态）来执行特定于状态的行为，并为每个状态定义实现接口的类。
- 类将特定于状态的行为委托给其当前状态对象，而不是直接实现特定于状态的行为。

这使得类独立于如何实现特定于状态的行为。可以通过定义新的状态类来添加新的状态。一个类可以在运行时通过改变它的当前状态对象来改变它的行为。

UML:

![image-20210822221339519](https://tva1.sinaimg.cn/large/008i3skNgy1gtpxay0nrwj627z0u0aie02.jpg)


在UML类图中,Context 类不直接实现特定于状态的行为。相反，Context 是指用于执行特定于状态的行为的 State 接口,这使得 Context 独立于如何实现特定于状态的行为。State1 和 State2 类实现了 State 接口，即为每个状态实现（封装）了特定于状态的行为。

类图与时序图:

![W3sDesign_State_Design_Pattern_UML](https://tva1.sinaimg.cn/large/008i3skNgy1gtpzj8pfruj60go06odg402.jpg)


例子:

    //状态类
    protocol State {
        mutating func writeName(context:StateContext,name:String)
    }
    
    struct LowerCaseState:State {
        mutating func writeName(context:StateContext,name:String) {
            print(name.lowercased())
            context.setState(newState: UpperCaseState())
        }
    }
    
    struct UpperCaseState:State {
        var count = 0;
        
        mutating func writeName(context:StateContext,name:String) {
            print(name.uppercased())
            count += 1
            if count > 1 {
                context.setState(newState: LowerCaseState())
            }
        }
    }
    
    
    
    //StateContext
    class StateContext {
        private var state:State = LowerCaseState()//内部状态
        
        func setState(newState:State)  {
            self.state = newState
        }
        public func writeName(name:String){
            state.writeName(context: self, name: name)
        }
        
    }
    
    
    //行为根据当前内部状态来决定
    var context = StateContext()
    context.writeName(name: "Monday")
    context.writeName(name: "Tuesday")
    context.writeName(name: "Wednesday")
    context.writeName(name: "Thursday")
    context.writeName(name: "Friday")
    context.writeName(name: "Saturday")
    context.writeName(name: "Sunday")
    
    //输出结果
    monday
    TUESDAY
    WEDNESDAY
    thursday
    FRIDAY
    SATURDAY
    sunday

参考:

维基百科-State pattern
