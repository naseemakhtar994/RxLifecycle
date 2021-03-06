# RxLifecycle

Rx binding of stock Android Activities & Fragment Lifecycle, avoiding memory leak

This library allows one to automatically finish sequences based on Android lifecycle state,
This capability is useful in Android, where incomplete subscriptions can cause memory leaks.

**You don't need to extends Activity or Fragment**

<a href='https://ko-fi.com/A160LCC' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://az743702.vo.msecnd.net/cdn/kofi1.png?v=0' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>

[ ![Download](https://api.bintray.com/packages/florent37/maven/rxlifecycle/images/download.svg) ](https://bintray.com/florent37/maven/rxlifecycle/_latestVersion)

```java
dependencies {
    compile 'com.github.florent37:rxcomponent-lifecycle:(lastversion)'

    compile "com.android.support:appcompat-v7:26.0.0"
    
    compile 'io.reactivex.rxjava2:rxjava:2.1.0'
}
```

# Listen to activity / fragment lifecycle events

```java
RxLifecycle.with((Fragment / Activity)this)
           .onDestroy()
           .subscribe(event -> 
                /*do what you had to do on view destroy*/
            );
            
RxLifecycle.with(getLifeCycle())
           .onResume()
           .subscribe(event -> 
                /*do what you had to do on view resume*/
            );
````

Available events :
- `.onCreate()`
- `.onStart()`
- `.onResume()`
- `.onPause()`
- `.onStop()`
- `.onDestroy()`

# Automatically Dispose Rx Observables

You can dispose an Rx operation when the activity state changes, for example

Using `doOnSubscribe`

```
Single.timer(10, TimeUnit.MINUTES) //simulates long operation
          
          .doOnSubscribe(disposable -> RxLifecycle.with(this).disposeOnDestroyed<Long>(disposable)
          
          .subscribe(l -> 
               ...
          });
```

Or `compose`

```
Observable.timer(10, TimeUnit.MINUTES) //simulates long operation
          
          .compose(RxLifecycle.with(this).disposeOnDestroyed<Long>())
          
          .subscribe(l -> 
               ...
          });
```

Availables : `disposeOnStop`, `disposeOnPause`, etc...

# Wait until an Activity state

You can **pause** an Rx chain until it's not on an event, for example wait for activity to be resumed to perform an animation

```
Observable.timer(10, TimeUnit.MINUTES) //simulates long operation
          
          //will pause
          .flatMap(l -> RxLifecycle.with(this).onlyIfResumedOrStarted(l))
          
          //only if resumed
          .subscribe(l -> 
               ...
          });
```

# Credits

Author: Florent Champigny [http://www.florentchampigny.com/](http://www.florentchampigny.com/)

Blog : [http://www.tutos-android-france.com/](http://www.www.tutos-android-france.com/)

<a href="https://plus.google.com/+florentchampigny">
  <img alt="Follow me on Google+"
       src="https://raw.githubusercontent.com/florent37/DaVinci/master/mobile/src/main/res/drawable-hdpi/gplus.png" />
</a>
<a href="https://twitter.com/florent_champ">
  <img alt="Follow me on Twitter"
       src="https://raw.githubusercontent.com/florent37/DaVinci/master/mobile/src/main/res/drawable-hdpi/twitter.png" />
</a>
<a href="https://www.linkedin.com/in/florentchampigny">
  <img alt="Follow me on LinkedIn"
       src="https://raw.githubusercontent.com/florent37/DaVinci/master/mobile/src/main/res/drawable-hdpi/linkedin.png" />
</a>


License
--------

    Copyright 2017 Florent37, Inc.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
