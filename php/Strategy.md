``1.策略模式概念``

策略模式针对一组算法，将每一个算法封装到具有共同接口的独立的类中,此模式让算法的变化独立于使用算法的客户。从而让程序结构更灵活，具有更好的扩展性和维护性

``2.策略模式角色说明``
抽象策略(Strategy）角色：定义所有支持的算法的公共接口。通常是以一个接口或抽象来实现。Context使用这 
个接口来调用其ConcreteStrategy定义的算法。
具体策略(ConcreteStrategy)角色：以Strategy接口实现某具体算法
上下文(Context)角色：持有一个Strategy类的引用，用一个ConcreteStrategy对象来配置

``3.策略模式实例``

```php
<?php
/**
* 抽象策略角色，以接口实现
*/
interface Strategy {  
  /**
   * 接口函数
   */
  public function action();
}  
/**
 * 具体策略角色A
 */
class ConcreteStrategyA implements Strategy {  
  public function action() {
    echo 'action A<br />';
  }
}  
/**
 * 具体策略角色B
 */
class ConcreteStrategyB implements Strategy {  
  public function action() {
    echo 'actionB<br />';
  }
}  
/**
 * 具体策略角色C
 */
class ConcreteStrategyC implements Strategy {

  public function action() {
    echo 'action C<br />';
  }
}

/**
 * 上下文角色
 */
class Context {
  /* 引用的策略 */
  private $_strategy;

  public function __construct(Strategy $strategy) {
    $this->_strategy = $strategy;
  }
  public function contextInterface() {
    $this->_strategy->action();
  } 
}  
/**
 * 调用端
 */
class CallClient {

  /**
   * Main program.
   */
  public static function main() {
    $strategyA = new ConcreteStrategyA();
    $context = new Context($strategyA);
    $context->contextInterface();

    $strategyB = new ConcreteStrategyB();
    $context = new Context($strategyB);
    $context->contextInterface();

    $strategyC = new ConcreteStrategyC();
    $context = new Context($strategyC);
    $context->contextInterface();
  }
}  
CallClient::main();
?>

```