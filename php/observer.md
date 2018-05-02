
- 意图：定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。
- 主要解决：一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作。
- 何时使用：一个对象（目标对象）的状态发生改变，所有的依赖对象（观察者对象）都将得到通知，进行广播通知。
- 使用场景： 1、拍卖场，全部买家都要知晓目前竞争信息 2、报纸，报纸新出一期，要发放给全部订阅者


```php
<?php

//被观察者
class Newspaper implements SplSubject {
    private $observers;
    private $content;
    public function __construct(){
        $this->observers = new SplObjectStorage();
    }
    public function attach(SplObserver $observer){
        $this->observers->attach($observer);
    }
    public function detach(SplObserver $observer){
        $this->observers->detach($observer);
    }
    public function notify(){
        foreach ($this->observers as $observer) {
            $observer->update($this);
        }
    }
    public function getContent(){
        return $this->content;
    }
    public function breakOutNews($content) {
        $this->content = $content;
        $this->notify();
    }
}

//观察者
class Reader implements SplObserver {
    private $name;
    public function __construct($name){
        $this->name = $name;
    }
    public function update(SplSubject $subject) {
        echo $this->name .' receive ' . $subject->getContent() . PHP_EOL;
    }
}

$newspaper = new Newspaper('times');

$a = new Reader("a");
$b = new Reader("b");
$c = new Reader("c");
//添加观察者/订阅
$newspaper->attach($a);
$newspaper->attach($b);
$newspaper->attach($c);
//移除观察者
$newspaper->detach($a);
//发布
$newspaper->breakOutNews('news 23333');
```