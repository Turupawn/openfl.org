---
title: Adding Animation
redirect_from: learn/npm/adding-animation/
---

_This project builds on the "features/display/DisplayingABitmap" sample (available on [GitHub](https://github.com/openfl?utf8=✓&q=openfl-samples&type=&language=)) or based on code written previously in [Getting Started](./getting-started/)._

## Using `Event.ENTER_FRAME`

One of the ways that you can begin to add animation in OpenFL is by listening to the `Event.ENTER_FRAME` event. This event occurs every time that OpenFL enters a new animation frame (which is by default the same as `requestAnimationFrame`, but is configurable).

First we can add a new import for `openfl.events.Event`:

{% capture typescript %}
```ts
import Event from "openfl/events/Event";
```
{% endcapture %}
{% capture haxe %}
```js
import openfl.events.Event;
```
{% endcapture %}
{% capture es6 %}
```js
import Event from "openfl/events/Event";
```
{% endcapture %}
{% capture es5 %}
```js
var Event = require ("openfl/events/Event").default;
```
{% endcapture %}
{% include code.md %}

_If you prefer, you can use `"enterFrame"` instead of `Event.ENTER_FRAME`, but importing `Event` is a standard way of accessing event names._

Now we can add some code to listen to the `Event.ENTER_FRAME` event. Each code sample occurs inside the `BitmapData.loadFromFile` callback, after `var bitmap` has been assigned and added as a child to make it visible.

{% capture typescript %}
```ts
this.addEventListener (Event.ENTER_FRAME, (e:Event) => {
	
	console.log ("Enter Frame");
	
});
```
{% endcapture %}
{% capture haxe %}
```js
addEventListener (Event.ENTER_FRAME, function (e) {
	
	trace ("Enter Frame");
	
});
```
{% endcapture %}
{% capture es6 %}
```js
this.addEventListener (Event.ENTER_FRAME, (e) => {
	
	console.log ("Enter Frame");
	
});
```
{% endcapture %}
{% capture es5 %}
```js
this.addEventListener (Event.ENTER_FRAME, function (e) {
	
	console.log ("Enter Frame");
	
});
```
{% endcapture %}
{% include code.md %}

All we need to do to create an animation is update a property of our `Bitmap` object (such as the `x`, `y` or `alpha` property) in our listener. This will occur repeatedly over time, making it possible to play an animation.

The following code will animate the image from it's initial `y` position of `0`, increasing by `1` each frame until it reaches `y == 200`. Then it changes direction, and moves back toward `y == 0` before finally repeating:

{% capture typescript %}
```ts
let direction = true;

this.addEventListener (Event.ENTER_FRAME, (e:Event) => {
	
	if (direction) {
		
		bitmap.y += 1;
		if (bitmap.y >= 200) direction = !direction;
		
	} else {
		
		bitmap.y -= 1;
		if (bitmap.y <= 0) direction = !direction;
		
	}
	
});
```
{% endcapture %}
{% capture haxe %}
```js
var direction = true;

addEventListener (Event.ENTER_FRAME, function (e) {
	
	if (direction) {
		
		bitmap.y += 1;
		if (bitmap.y >= 200) direction = !direction;
		
	} else {
		
		bitmap.y -= 1;
		if (bitmap.y <= 0) direction = !direction;
		
	}
	
});
```
{% endcapture %}
{% capture es6 %}
```js
let direction = true;

this.addEventListener (Event.ENTER_FRAME, (e) => {
	
	if (direction) {
		
		bitmap.y += 1;
		if (bitmap.y >= 200) direction = !direction;
		
	} else {
		
		bitmap.y -= 1;
		if (bitmap.y <= 0) direction = !direction;
		
	}
	
});
```
{% endcapture %}
{% capture es5 %}
```js
var direction = true;

this.addEventListener (Event.ENTER_FRAME, function (e) {
	
	if (direction) {
		
		bitmap.y += 1;
		if (bitmap.y >= 200) direction = !direction;
		
	} else {
		
		bitmap.y -= 1;
		if (bitmap.y <= 0) direction = !direction;
		
	}
	
});
```
{% endcapture %}
{% include code.md %}

{% capture embed %}
var Bitmap = openfl.display.Bitmap;
var BitmapData = openfl.display.BitmapData;
var Sprite = openfl.display.Sprite;
var Stage = openfl.display.Stage;
var Event = openfl.events.Event;

var App = function () {
	
	Sprite.call (this);
	
	BitmapData.loadFromFile ("{{ site.baseurl }}/learn/assets/openfl.png").onComplete (function (bitmapData) {
		
		var bitmap = new Bitmap (bitmapData);
		this.addChild (bitmap);
		
		var direction = true;
		
		this.addEventListener (Event.ENTER_FRAME, function (e) {
			
			if (direction) {
				
				bitmap.y += 1;
				if (bitmap.y >= 200) direction = !direction;
				
			} else {
				
				bitmap.y -= 1;
				if (bitmap.y <= 0) direction = !direction;
				
			}
			
		});
		
	}.bind (this));
	
}

App.prototype = Sprite.prototype;
{% endcapture %}
{% include embed.md %}

## Using `Timer`

Sometimes, you may need to update a property based upon time, rather than animation frames. In general, it is recommended that updates are made within an animation frame. In the next section, we will show you can combine `Event.ENTER_FRAME` with `getTimer` to handle time, but in the meantime, this is a short description of `openfl.utils.Timer` could be used to trigger animation based on time.

Both `setTimeout` and `setInterval` could be used to trigger an event repeatedly, as well as a final complete event, but the `Timer` class combines the two behaviors into a single utility which may be helpful for time-based behaviors.

{% capture typescript %}
```ts
import TimerEvent from "openfl/events/TimerEvent";
import Timer from "openfl.utils/Timer";
```
{% endcapture %}
{% capture haxe %}
```js
import openfl.events.TimerEvent;
import openfl.utils.Timer;
```
{% endcapture %}
{% capture es6 %}
```js
import TimerEvent from "openfl/events/TimerEvent";
import Timer from "openfl.utils/Timer";
```
{% endcapture %}
{% capture es5 %}
```js
var TimerEvent = require ("openfl/events/TimerEvent").default;
var Timer = require ("openfl/utils/Timer").default;
```
{% endcapture %}
{% include code.md %}

{% capture typescript %}
```ts
let increment = -0.005;
let timer = new Timer (1000 / 60, 200);
timer.addEventListener (TimerEvent.TIMER, (e:TimerEvent) => {
	
	bitmap.alpha += increment;
	
});
timer.addEventListener (TimerEvent.TIMER_COMPLETE, (e:TimerEvent) => {
	
	increment *= -1;
	timer.reset ();
	timer.start ();
	
});
timer.start ();
```
{% endcapture %}
{% capture haxe %}
```js
var increment = -0.005;
var timer = new Timer (1000 / 60, 200);
timer.addEventListener (TimerEvent.TIMER, function (e) {
	
	bitmap.alpha += increment;
	
});
timer.addEventListener (TimerEvent.TIMER_COMPLETE, function (e) {
	
	increment *= -1;
	timer.reset ();
	timer.start ();
	
});
timer.start ();
```
{% endcapture %}
{% capture es6 %}
```js
let increment = -0.005;
let timer = new Timer (1000 / 60, 200);
timer.addEventListener (TimerEvent.TIMER, (e) => {
	
	bitmap.alpha += increment;
	
});
timer.addEventListener (TimerEvent.TIMER_COMPLETE, (e) => {
	
	increment *= -1;
	timer.reset ();
	timer.start ();
	
});
timer.start ();
```
{% endcapture %}
{% capture es5 %}
```js
var increment = -0.005;
var timer = new Timer (1000 / 60, 200);
timer.addEventListener (TimerEvent.TIMER, function (e) {
	
	bitmap.alpha += increment;
	
});
timer.addEventListener (TimerEvent.TIMER_COMPLETE, function (e) {
	
	increment *= -1;
	timer.reset ();
	timer.start ();
	
});
timer.start ();
```
{% endcapture %}
{% include code.md %}

{% capture embed %}
var Bitmap = openfl.display.Bitmap;
var BitmapData = openfl.display.BitmapData;
var Sprite = openfl.display.Sprite;
var Stage = openfl.display.Stage;
var TimerEvent = openfl.events.TimerEvent;
var Timer = openfl.utils.Timer;

var App = function () {
	
	Sprite.call (this);
	
	BitmapData.loadFromFile ("{{ site.baseurl }}/learn/assets/openfl.png").onComplete (function (bitmapData) {
		
		var bitmap = new Bitmap (bitmapData);
		this.addChild (bitmap);
		
		var increment = -0.005;
		var timer = new Timer (1000 / 60, 200);
		timer.addEventListener (TimerEvent.TIMER, function (e) {
			
			bitmap.alpha += increment;
			
		});
		timer.addEventListener (TimerEvent.TIMER_COMPLETE, function (e) {
			
			increment *= -1;
			timer.reset ();
			timer.start ();
			
		});
		timer.start ();
		
	}.bind (this));
	
}

App.prototype = Sprite.prototype;
{% endcapture %}
{% include embed.md %}

## Using `Event.ENTER_FRAME` and `getTimer`

When you need to update based on animation frames, but also need to know how much time has elapsed, you could use a combination of both `Event.ENTER_FRAME` and `openfl.utils.getTimer`.

This approach is common for many games, which need to know the amount of time that has elapsed since the last frame.

{% capture typescript %}
```ts
import Event from "openfl/events/Event";
import getTimer from "openfl/utils/getTimer";

...

let speed = 60 / 1000;
let cacheTime = getTimer ();

this.addEventListener (Event.ENTER_FRAME, (e:Event) => {
	
	let currentTime = getTimer ();
	let deltaTime = currentTime - cacheTime;
	
	bitmap.x += speed * deltaTime;
	
	if (bitmap.x > 100) {
		
		speed *= -1;
		bitmap.x = 100;
		
	} else if (bitmap.x < 0) {
		
		speed *= -1;
		bitmap.x = 0;
		
	}
	
	cacheTime = currentTime;
	
});
```
{% endcapture %}
{% capture haxe %}
```js
import openfl.events.Event;
import openfl.Lib.getTimer;

...

var speed = 60 / 1000;
var cacheTime = getTimer ();

addEventListener (Event.ENTER_FRAME, function (e) {
	
	var currentTime = getTimer ();
	var deltaTime = currentTime - cacheTime;
	
	bitmap.x += speed * deltaTime;
	
	if (bitmap.x > 100) {
		
		speed *= -1;
		bitmap.x = 100;
		
	} else if (bitmap.x < 0) {
		
		speed *= -1;
		bitmap.x = 0;
		
	}
	
	cacheTime = currentTime;
	
});
```
{% endcapture %}
{% capture es6 %}
```js
import Event from "openfl/events/Event";
import getTimer from "openfl/utils/getTimer";

...

let speed = 60 / 1000;
let cacheTime = getTimer ();

this.addEventListener (Event.ENTER_FRAME, (e) => {
	
	let currentTime = getTimer ();
	let deltaTime = currentTime - cacheTime;
	
	bitmap.x += speed * deltaTime;
	
	if (bitmap.x > 100) {
		
		speed *= -1;
		bitmap.x = 100;
		
	} else if (bitmap.x < 0) {
		
		speed *= -1;
		bitmap.x = 0;
		
	}
	
	cacheTime = currentTime;
	
});
```
{% endcapture %}
{% capture es5 %}
```js
var Event = require ("openfl/events/Event").default;
var getTimer = require ("openfl/utils/getTimer").default;

...

var speed = 60 / 1000;
var cacheTime = getTimer ();

this.addEventListener (Event.ENTER_FRAME, function (e) {
	
	var currentTime = getTimer ();
	var deltaTime = currentTime - cacheTime;
	
	bitmap.x += speed * deltaTime;
	
	if (bitmap.x > 100) {
		
		speed *= -1;
		bitmap.x = 100;
		
	} else if (bitmap.x < 0) {
		
		speed *= -1;
		bitmap.x = 0;
		
	}
	
	cacheTime = currentTime;
	
});
```
{% endcapture %}
{% include code.md %}

{% capture embed %}
var Bitmap = openfl.display.Bitmap;
var BitmapData = openfl.display.BitmapData;
var Sprite = openfl.display.Sprite;
var Stage = openfl.display.Stage;
var Event = openfl.events.Event;
var getTimer = openfl.utils.getTimer;

var App = function () {
	
	Sprite.call (this);
	
	BitmapData.loadFromFile ("{{ site.baseurl }}/learn/assets/openfl.png").onComplete (function (bitmapData) {
		
		var bitmap = new Bitmap (bitmapData);
		this.addChild (bitmap);
		
		var speed = 60 / 1000;
		var cacheTime = getTimer ();
		
		this.addEventListener (Event.ENTER_FRAME, function (e) {
			
			var currentTime = getTimer ();
			var deltaTime = currentTime - cacheTime;
			
			bitmap.x += speed * deltaTime;
			
			if (bitmap.x > 100) {
				
				speed *= -1;
				bitmap.x = 100;
				
			} else if (bitmap.x < 0) {
				
				speed *= -1;
				bitmap.x = 0;
				
			}
			
			cacheTime = currentTime;
			
		});
		
	}.bind (this));
	
}

App.prototype = Sprite.prototype;
{% endcapture %}
{% include embed.md %}

## Using an Animation Library

When you move beyond a simple illustration, and begin building real interactive projects, use of an animation (or "tween") library is more productive for expressive animation. In addition to automatically animating based upon `requestAnimationFrame`, an animation library may make it simpler to combine animation of multiple properties at once, make it simple to receive callbacks when the object has been updated or completes an animation, and often includes multiple "easing" equations in order to animate properties along a quadratic, exponential or elastic animation equation rather than a simple linear equation, similar to the examples above.

The "AddingAnimation" sample (for [TypeScript](https://github.com/openfl/openfl-samples-ts/tree/master/features/display/AddingAnimation), [Haxe](https://github.com/openfl/openfl-samples/tree/master/npm/features/display/AddingAnimation), [ES6](https://github.com/openfl/openfl-samples-es6/tree/master/features/display/AddingAnimation) and [ES5](https://github.com/openfl/openfl-samples-es5/tree/master/features/display/AddingAnimation)) is configured to use the Actuate animation library, but many others are available.

Here is what some of the above animation might look like using Actuate:

{% capture typescript %}
```ts
import Actuate from "motion/Actuate";
import Elastic from "motion/easing/Elastic";
import Linear from "motion/easing/Linear";

...

Actuate.tween (bitmap, 4, { y: 200 }).repeat ().reflect ();
Actuate.tween (bitmap, 4, { x: 100 }).ease (Elastic.easeOut).repeat ().reflect ();
Actuate.tween (bitmap, 4, { alpha: 0 }).ease (Linear.easeNone).repeat ().reflect ();
```
{% endcapture %}
{% capture haxe %}
```js
import motion.Actuate;
import motion.easing.Elastic;
import motion.easing.Linear;

...

Actuate.tween (bitmap, 4, { y: 200 }).repeat ().reflect ();
Actuate.tween (bitmap, 4, { x: 100 }).ease (Elastic.easeOut).repeat ().reflect ();
Actuate.tween (bitmap, 4, { alpha: 0 }).ease (Linear.easeNone).repeat ().reflect ();
```
{% endcapture %}
{% capture es6 %}
```js
import Actuate from "motion/Actuate";
import Elastic from "motion/easing/Elastic";
import Linear from "motion/easing/Linear";

...

Actuate.tween (bitmap, 4, { y: 200 }).repeat ().reflect ();
Actuate.tween (bitmap, 4, { x: 100 }).ease (Elastic.easeOut).repeat ().reflect ();
Actuate.tween (bitmap, 4, { alpha: 0 }).ease (Linear.easeNone).repeat ().reflect ();
```
{% endcapture %}
{% capture es5 %}
```js
var Actuate = require ("motion/Actuate").default;
var Elastic = require ("motion/easing/Elastic").default;
var Linear = require ("motion/easing/Linear").default;

...

Actuate.tween (bitmap, 4, { y: 200 }).repeat ().reflect ();
Actuate.tween (bitmap, 4, { x: 100 }).ease (Elastic.easeOut).repeat ().reflect ();
Actuate.tween (bitmap, 4, { alpha: 0 }).ease (Linear.easeNone).repeat ().reflect ();
```
{% endcapture %}
{% include code.md %}

{% capture embed %}
var Bitmap = openfl.display.Bitmap;
var BitmapData = openfl.display.BitmapData;
var Sprite = openfl.display.Sprite;
var Stage = openfl.display.Stage;
var Actuate = motion.Actuate;
var Elastic = motion.easing.Elastic;
var Linear = motion.easing.Linear;

var App = function () {
	
	Sprite.call (this);
	
	BitmapData.loadFromFile ("{{ site.baseurl }}/learn/assets/openfl.png").onComplete (function (bitmapData) {
		
		var bitmap = new Bitmap (bitmapData);
		this.addChild (bitmap);
		
		Actuate.tween (bitmap, 4, { y: 200 }).repeat ().reflect ();
		Actuate.tween (bitmap, 4, { x: 100 }).ease (Elastic.easeOut).repeat ().reflect ();
		Actuate.tween (bitmap, 4, { alpha: 0 }).ease (Linear.easeNone).repeat ().reflect ();
		
	}.bind (this));
	
}

App.prototype = Sprite.prototype;
{% endcapture %}
{% include embed.md %}


## Next Steps

There are more sample projects with additional features (such as sound, animation and video) in each of the OpenFL samples repositories:

 - [https://github.com/openfl/openfl-samples-ts](https://github.com/openfl/openfl-samples-ts)
 - [https://github.com/openfl/openfl-samples](https://github.com/openfl/openfl-samples)
 - [https://github.com/openfl/openfl-samples-es6](https://github.com/openfl/openfl-samples-es6)
 - [https://github.com/openfl/openfl-samples-es5](https://github.com/openfl/openfl-samples-es5)

Each of the samples can be tested using `npm install` then `npm start`