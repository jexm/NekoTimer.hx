package;

    class NekoTimer 
    {
		
    private static var threadActive:Bool = false;
    private static var timersList:Array<TimerInfo> = new Array<TimerInfo>();
    private static var timerInterval:Float = 0.1;

    public static function addTimer(interval:Int, callMethod:Void->Void):Int
    {
        //setup timer thread if not yet active
        if (!threadActive) setupTimerThread();

        //add the given timer
        return timersList.push(new TimerInfo(interval, callMethod, Sys.time() * 1000)) - 1;
    }

    public static function delTimer(id:Int):Void
    {
        timersList.splice(id, 1);
    }

    private static function setupTimerThread():Void
    {
        threadActive = true;
        neko.vm.Thread.create(function() {
            while (true) {
                Sys.sleep(timerInterval);
                for (timer in timersList) {
                    if (Sys.time() * 1000 - timer.lastCallTimestamp >= timer.interval) {
                        timer.callMethod();
                        timer.lastCallTimestamp = Sys.time() * 1000;
                    }
                }
            }
        });
    }
}

private class TimerInfo
{
    public var interval:Int;
    public var callMethod:Void->Void;
    public var lastCallTimestamp:Float;

    public function new(interval:Int, callMethod:Void->Void, lastCallTimestamp:Float) {
        this.interval = interval;
        this.callMethod = callMethod;
        this.lastCallTimestamp = lastCallTimestamp;
    }
}
