***什么是依赖注入(Dependency Injection)***
   
`依赖倒置原则:高层模块不应该依赖于低层模块，两个都应该依赖于抽象（接口）。
依赖倒置是一种软件设计思想，在传统软件中，上层代码依赖于下层代码，当下层代码有所改动时，上层代码也要相应进行改动，因此维护成本较高。而依赖倒置原则的思想是，上层不应该依赖下层，应依赖接口。意为上层代码定义接口，下层代码实现该接口，从而使得下层依赖于上层接口，降低耦合度，提高系统弹性。
举个栗子：小霸王游戏机
`
```
// 如果使用单纯的面向对象来写
class XiaoBaWang
{    
    private $game;    
    public function loadGame()
    {        
    $this->game = new HunDouLuo();        
    $this->game->play();    
    }
}
class HunDouLuo{    
    public function play()    
    {        
    //玩游戏    
    }
}
$machine = new XiaoBaWang();
$machine->loadGame();

class XiaoBaWang
{    
    private $game;    
    public function loadGame()    
    {        
    $this->game = new Tank();       
     $this->game->play();    
}
}
class Tank 
{    
    public function play()    
    {        
        //玩游戏    
    }
}
$reg = new XiaoBaWang();
$reg->loadGame();
使用依赖注入的写法
小霸王游戏机：插拔式卡带

interface Game
{    
    public function play();
}
class HunDouLuo implements Game
{    
    public function play()    
{        
// 玩魂斗罗    
}
}
class Tank implements Game
{    
    public function play()    
{        
// 坦克大战    
}
}
class XiaoBaWang
{    
    private $game;    
    public function __construct(Game $game)    
    {        
        $this->game = $game;    
    }    
    public function loadGame()    
    {        
        $this->game->play();    
    }
}
$hunDouLuo = new HunDouLuo();
$tank = new Tank();
$machine = new XiaoBaWang($hunDouLuo);
$machine->loadGame();
$machine = new XiaoBaWang($tank);
$machine->loadGame();
```
`上例就使用了"注入"这个思想，就像注射器一样将一个类的实例注入到另一个类的实例中去，需要用什么就注入什么。当然"依赖倒置原则"也始终贯彻在里面。"注入"不仅可以通过构造函数注入，也可以通过属性注入，上面你可以可以通过一个"set"来动态为"game"这个属性赋值。`