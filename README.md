package ;

import neko.Lib;

/**
 * ...
 * @author jexm.ma
 */

class Main 
{
	
	private var timerId:Int;

    public function new()
    {
        trace("setting up timer...");
        timerId = NekoTimer.addTimer(5000, timerCallback);
        trace(timerId);

        //idle main app
        while (true) { }
    }

    private function timerCallback():Void
    {
        trace("it's now 5 seconds later");
        //NekoTimer.delTimer(timerId);
        trace("removed timer");
    }

    //neko constructor
    static function main() 
    {
        new Main();
    }
	
}
