#### IOC

application.xml  (xml解析)

bean节点   (反射)

容器 Map



ioc的核心就说bean工厂--->负责创建、管理Bean、对外指出bean实例

1、它应该具备什么行为(对外接口)?

对外提供bean实例，getBean()方法

2、这个getBean()方法是否需要参数?需要几个参数、什么类型的参数？

简单工厂模式中，当工厂能创建很多类产品时，如要创建某类产品，需要告诉工厂。

3、getbean（）返回值应是什么类型的

各种类型bean,只能是object



beanfactory -->所有bean的实例

factorybean -->本身是bean会注册到bean工厂

BeanFactory是接口，提供了IOC容器最基本的形式，给具体的IOC容器的实现提供了规范。

FactoryBean也是接口，为IOC容器中Bean的实现提供了更加灵活的方式，FactoryBean在IOC容器的基础上，给Bean的实现加上了一个简单工厂模式和装饰模式。

BeanFactory是个Factory，也就是IOC容器或对象工厂。FactoryBean是个Bean，在Spring中，所有的Bean都是由BeanFactory来进行管理的，但对FactoryBean而言，这个Bean不是简单的Bean，而是一个能生产或者修饰对象生成的工厂Bean，它的实现与设计模式中的工厂模式和修饰器模式类似。

1、 BeanFactory

　　BeanFactory，以Factory结尾，表示它是一个工厂类(接口)， 它负责生产和管理bean的一个工厂。在Spring中，BeanFactory是IOC容器的核心接口，它的职责包括：实例化、定位、配置应用程序中的对象及建立这些对象间的依赖。BeanFactory只是个接口，并不是IOC容器的具体实现，但是Spring容器给出了很多种实现，DefaultListableBeanFactory、XmlBeanFactory、ApplicationContext等，其中XmlBeanFactory就是常用的一个，该实现将以XML方式描述组成应用的对象及对象间的依赖关系**。



2、FactoryBean

一般情况下，Spring通过反射机制利用<bean>的class属性指定实现类实例化Bean，在某些情况下，实例化Bean过程比较复杂，如果按照传统的方式，则需要在<bean>中提供大量的配置信息。配置方式的灵活性是受限的，这时采用编码的方式可能会得到一个简单的方案。Spring为此提供了一个org.springframework.bean.factory.FactoryBean的工厂类接口，用户可以通过实现该接口定制实例化Bean的逻辑。FactoryBean接口对于Spring框架来说占用重要的地位。