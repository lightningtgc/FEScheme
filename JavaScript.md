##Navigator.vibrate()

*[介绍](https://developer.mozilla.org/en-US/docs/Web/API/Navigator.vibrate)

*[Demo](http://davidwalsh.name/vibration-api)

技能：在FirefoxOS中可以使手机震动


"""javascript

var supportsVibrate = "vibrate" in navigator;

// Vibrate once for one second
navigator.vibrate(1000);

// Vibrate multiple times for multiple durations
// Vibrate for three seconds, wait two seconds, then vibrate for one second
navigator.vibrate([3000, 2000, 1000]);

// Either of these stop vibration
navigator.vibrate(0);
navigator.vibrate([]);

"""

###Math.random()

值：[0,1)  包括0，小于1
