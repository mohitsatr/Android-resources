I'll assume you have basic understanding of Android Development and Kotlin. 
This guide is meant to build on top of those basics and help you build strong mental models so you can shine in your Android interview. I wish a resource guide like this one existed when I started learning Android development about 6-7 months ago. 

### Interview
https://android-interview-prep.web.app/

### Android Lifecycle
I feel the best way to learn about Android's lifecycle is to just play with it.
Here are some starting points:
1. How restart cycle works
2. How do you know your app was destroyed by Android

Checksout lifecycle cheatsheets [here](https://medium.com/androiddevelopers/the-android-lifecycle-cheat-sheet-part-i-single-activities-e49fd3d202ab) and [here](https://github.com/xxv/android-lifecycle)

Learn [how viewmodels actually works internally](https://youtu.be/HWiYYihaJ4A?si=8KP84K2oZnV1Oi78) and survive the configuration changes.

### Flows:  
The main advantage of flows is Reactive state managament. Basically, the UI (composables, for example) is constantly observing(collecting from) the flow and whenever the state gets updated somewhere, UI automatically reacts to the changes and updates itself.

1. [Live data is dead! Long live stateflow](https://medium.com/@joostklitsie/livedata-is-dead-long-live-stateflow-ecd7e03b1777)
2. [Go with the flow](https://youtu.be/kPIC8ytcXFM?si=uX1dqM379sEZNcVG) awesome talk covers the basics of flows, difference between cold and hot flows and while breaking down Kotlin's syntatical sugar. Rule of thumb: [Never send one off events through StateFlow](https://x.com/peja_1616/status/2003541150037418160)
3. While learning about flows, you will come across channels and might wonder [what's difference between them](https://medium.com/better-programming/stop-calling-kotlin-flows-hot-and-cold-48e87708d863)
### Coroutines/Structured concurrency
Like me, If you do not have first hand experience working with AsyncTasks and never had to deal with callback hell, you might not be immediately able to appreciate the genius behind coroutines.


1. [notes on structured concurrency](https://vorpus.org/blog/notes-on-structured-concurrency-or-go-statement-considered-harmful/#what-is-a-goto-statement-anyway) A great post to understand what might be called "un-structured concurrency".
2. [why Coroutines are Actually no Magic in Kotlin](https://youtu.be/Ndw64X9o9J0?si=FVWtse6vjPnt-h--) turns out coroutines(among many things) are just an abstraction over the callbacks.
3. [Kotlin Coroutines Mechanisms](https://youtu.be/rQlyUBlFy34?si=CMhO23wqzX5aeqGC) A slightly intermediate level talk about CoroutineScope & CoroutineContext. Make sure checkout Amanda's blog posts all well.
4. Coroutines are light-weight and better than threads because they work on top of threads. [Coroutines vs. Threads: The True Cost of a Context Switch](https://youtu.be/Dv9Mx__pUZA?si=Dv6QCzJdL8Z9Z4Q5).
5. [Inside Kotlin Coroutines: State Machines, Continuations, and Structured Concurrency](https://proandroiddev.com/inside-kotlin-coroutines-state-machines-continuations-and-structured-concurrency-b8d3d4e48e62) 65 Min deep dive into Finite-State-Machine (FSM) transformation that powers Kotlin Coroutines, from Compiler Mechanics to Structured Concurrency.
6. [Asynchrony and the infinite conveyor belt](https://youtu.be/kBQ3uAse5mk?si=RUO4ilPEy4k-SZi8) very useful insights into coroutines cancellation, Job vs scope and Flows. https://proandroiddev.com/seven-recipes-to-understand-flows-and-asynchrony-in-kotlin-1bd7fe041480
7. Fun [Coroutine Puzzles](https://www.youtube.com/watch?v=1l4ISq2kyYs&t=306s&pp=0gcJCQ0LAYcqIYzv) 
 
After you are done with basics, here's are pratical patterns you will come across in real codebases like UI update must happen on Main dispatcher, I/O dispatcher is for tasks which take time to finish like requesting data from file system, database, network request etc.(literally Input/Output) and default dispatcher is for CPU intensive tasks like doing a complex calculation. If you try to perform last two on Main dispatcher, you will end up blocking the UI i.e user will not be do anything on the screen, leading to ANR. Learn more about [ANRs here](https://youtu.be/GxkExvA_ruo?si=kbgnVTrHhUZ04FHq) if you are interested.
8. [Android Process Lifecycle + Coroutines == ❤](https://medium.com/proandroiddev/android-process-lifecycle-coroutines-7a8128470dd4)
9. [Tips, best Practices, and real-world insights](https://youtu.be/f0sEBqGmfI0?si=crzaqPMOQZX3-brT)
10. [Pratical Pattens for Kotlin Coroutines in Production](https://youtu.be/tOeZUN1Z-kQ?si=ICfA3bM6EUe-UqNF)
### Architecure: MVVM/MVI
Okay, Everyone's favourite. MVVM are MVI ares the one you will most likely be asked during the interview.
1. [MVWTF: Demystifying Architecture Patterns](https://youtu.be/yCJd61-AMQY?si=dmrBFaJL-VI4M_pD) A great to understand why these Architecture even exist in the first place and why we can't just keep all the code in the Activity.
2. According to [Zhuinden](https://www.reddit.com/r/android_devs/comments/1qcphb7/comment/o26wr8i/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button):

	*The difference is that in MVI, the View => ViewModel function calls are replaced with a sealed class, and that class is thrown onto a channel, and the ViewModel collects that channel, and when it has 1 function with 1 big when(event) {} statement instead of having separate functions.*
	
	*(MVI also always stores the UiState in a single MutableStateFlowUIState() regardless of the fact that that doesn't support properly, but MVI fans always ignore this part.)
	```kotlin
	savedStateHandle.getStateFlow("") + combine()
	``` 
	
	*Having a data class or a sealed class with data-subclasses (sometimes data object) doesn't change depending on whether it is MVVM or MVI because you want immutable data model for your ui state anyway; sealed class just means sometimes some states are mutually exclusive*
	
	*One could argue (and I do, regularly) that "MVI" is just "MVVM with extra steps", except none of those extra steps are necessary unless you want to support undo functionality + saving a history of actions so that you can remove/re-do operations at will. If you don't need that, then you don't need MVI. But it's good to know about it anyway because there's many "Android Lead Devs" who copy-pasted their architecture without understanding it, and questioning their authority will just get you in trouble regardless of whether they are right or wrong.*

Not required, But while you are at it, it won't a be bad idea to implement your own [State machine for the UI layer](https://proandroiddev.com/model-view-state-machine-mvs-7dc371275b60)

### Jetpack Compose: UI   
1. Kyriakos Georgiopoulos's [gists](https://gist.github.com/Kyriakos-Georgiopoulos) 
are hands down best resource I have come across for learning 
Compose UI. Simply Copy his gists, play with them, break and when you are ready 
try to implement it yourself without looking at the original code. 
Do it multiple times.  

2. or If you are into gaming, try reimplementing [instant messaging app seen in Persona 5](https://github.com/chris-horner/Persona-IM)   

#### Compose Advance: Beyond UI   
1. Compose is build on something like a State Machine, and It can be used for 
pretty interesting things like like dependency injection.

### Gradle:
1. [Gradle Key Concepts and Best Practices](https://youtu.be/41gwL4Vd7sI?si=A4YtmnJ59lVCdr6j)    
2. [Gradle without Guesswork](https://youtu.be/2Xept1q4CV8?si=0z0ZzpuPDM3aQ9eN)   

### Miscellaneous/Interesting
1. [Self-Referential Generics in Kotlin: When Type Safety Requires Talking to Yourself](https://underdroid.hashnode.dev/self-referential-generics-in-kotlin)   

2. [From Source Code to APK: A Manual Deep Dive into the Android Build Process](https://www.youtube.com/watch?v=C77WssXZEvo&t=803s)   

3. [State managment without Flow](https://youtu.be/t89nFRuH_KA?si=z-F92fop4uLig6ZX)
Interesting and mostly conceptual talk.

4. An interesting case study of how guys at Trivago slimmed down their 2000 lines
long Viewmodel [Architectural Makeover: Slimming Down ViewModels at Trivago!](https://www.youtube.com/watch?v=g2h8SoNM7MQ)

5.[Dive dive into the Android Gradle Plugin](https://youtu.be/YZE99qO7OtY?si=txEtsN48r0O9gNgm) 

I have tried to keep this list short and non-exhausive. But I feel this is enough to help you get started with any (modern)Android codebase. Note: I have only included the links I have personelly gone thr
ough and found helpful. 

Thanks for reading!
