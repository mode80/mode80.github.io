# I learned 7 different programming languages so you don't have to

I began last year with a goal to *really* understand machine learning. I feel utterly lucky to be alive at a time when software is taking its first tentative steps into the realm of intelligence, and I want front row seats to this historic event. Once this has occurred, computers will speak my language. But to see it unfold, I must learn to speak theirs...

By way of background, I haven't programmed professionally since the late 90s. My language of choice at that time is too embarrassing to admit (hint, initials  VB). Since then, I've been mostly busy helping early stage start-ups get funded. So what follows is an "outsider's" inside perspective on the *very best* programming language. 

Yes, fighting words! Let's begin. 


## Python 

Of course, my first stop was Python. This is the language I would teach my kids first. I love that it reads like pseudo-code and it allows you to think big picture about the problem you're trying to solve. It's the lingua franca of data science, so I expected my journey to stop here. 

But Python is slow. And being slow, it requires libraries written in C to do anything as performance-critical as machine learning. And with that , you lose Python's best parts. It no longer reads like English. Code that would naturally be written as a loop must instead become a "tensor manipulation". Get ready to write a lot of comments.

Also, I wasn't *truly* understanding machine learning with Python. It was a string of incantations...import this module, paste in some torchured API calls, and behold... digits deciphered! I needed to see the guts.

I did take Jeremy Howard's [excellent course](https://www.fast.ai/posts/part2-2022.html) which aims to teach building Stable Diffusion from scratch with Python. That seemed perfect. But I found, while I could readily nod my head watching Jeremy walk through the code, that wasn't enough for me to *really* understand. I decided I would need to re-implement it from scratch and "without peeking". But to see what to implement, I had to peek. Hm. I know, I'll re-write it in a different language!

## Rust

Enter Rust. Rust is what all the cool kids are learning. It's been voted the "most loved" language by programmers for many years. Surely, I should learn this one. But it would be a lot to learn a new language together with machine learning at the same time. So I took a step back and decided to learn just Rust, by re-implementing something I'd already written once in Python. 

### Sidebar: The best way to learn a language

I really like this approach and I highly recommend it. Take something moderately complex that you've already written, then re-write it in the language you're learning. Having done this seven times, I wouldn't try it again any other way. Just spend a half-day browsing some "quick start" intros for general familiarity. Then dive in and make something. You can look up the rest as you go. 

My Rosetta Stone project of choice for this is a Yahtzee bot. In the dice game of solo Yahtzee, there is a "statistically correct" choice for every possible situation -- about a billion of them. A program that prints the expected value of a player's initial choice, must first calculate the same for all billion+ possibilities that follow. Spoiler: it's 254. But a naive Python program to calculate that runs for about 50 days.

I found this challenge to be great for test-driving the capabilities of a new language. At about 1000 lines of code, it's just complex enough to require some custom data types and some care with performance. There's a correct result that you only see once it's done right. And with some extra cleverness, it can be made to finish in minutes instead of days.

Doing this, I've found that I can learn a new language to a level of basic proficiency in 2 or 3 weeks (not including time spent on my "day job"). I would compare that to 2 or 3 months it takes to fully read some programming tome from Amazon. Plus reading only gives you a surface awareness -- like recognizing a human language but not speaking it yourself. Stop reading and start coding. Make something.


## Back to Rust 

So what's up with Rust? Well, I can't say it's my "most loved". 

It taught me appreciation for low level implementation details I'd never had to think about before. But I didn't really want to think about those things. For example, Rust made me responsible for managing "lifetimes" of my variables by way of noisy annotations. I was used to languages that take care of this logistical plumbing for me. It'd be nice to "opt-in" to that level of control when it's needed, but Rust always needs it. 

I will say there's a certain satisfaction every time I got new code to compile in Rust. It feels like solving a puzzle. As a bonus, the strict rules of Rust allow my IDE to highlight errors as I go. When your Python program runs, you don't really know it's correct until you've tested every code path. 

But Rust is pedantic in other ways which takes that a bit too far for my taste. There is so much defensive error checking and/or value "unwrapping". It's hard to tease all that apart from the core of a programmer's intent. My brain has a "complexity limit", and if I'm using some of that up to parse through the plumbing, it shrinks the limit of "problem complexity" I can tackle. 

Rust does have some nice abstractions that other low-level programming languages lack. You can write a Python-like list comprehension with no speed penalty. It has conveniences for multithreading via some good libraries and macros. 

But with Rust, I'd say most programmer convenience is sacrificed on the altar of "never crash". This makes it a good choice for sending a rocket to space, but not so much for exploratory data science. It also isn't really a great choice for a website backend, or a game, or a GUI desktop app. It wouldn't shine in those cases, and (consequently?) there are no really mature libraries in those areas. 

Lastly, I had hoped to write GPU code in Rust, but sadly it doesn't do this.

By the end of my journey with Rust, I'd made my Yahtzeebot run in 10 minutes with the help of some multithreading and SIMD magic. Much of that speed is due to the low level nature of Rust, but also from making me use a different approach. The Python code felt like an elegant fit to the shape of the problem as it lived in my mind. The Rust code felt like a translation that catered more to the needs of the machine, but with pretty great performance as the payoff.  


## Julia

The tradeoff I experienced with Rust left me wondering if there was a way to have the best of both worlds. Maybe Julia? 

Julia is beautiful. For anyone who finds curly braces to be noisy, Julia compares well to Python for pure aesthetics. Julia's loops are fast, so you're not forced to turn everything into a tensor. But you can also natively do math on arrays with broadcasting whenever that approach is cleaner.

With Julia, I managed to get the Yahtzeebot down to just 3.38 minutes. I mean, wow. 

This is an example of where tooling makes a difference. Julia and Rust both compile via LLVM, so I could probably go back and make the Rust version run just as fast. But I couldn't readily iterate on performance improvements with Rust because I had to keep switching over to a Windows machine where I could use a good line-by-line profiler which didn't exist for Rust on a Mac. With Julia's tooling, each line of code had a visual indicator of its performance hit right there in my editor with VSCode. I could also launch a REPL there and test out freshly written functions interactively. That was a great workflow and worked well for most changes.  

In machine learning, the GPU is an essential ingredient, and Julia appealingly lets me write GPU code in the *same language* as the high level "glue" code. 

Things that require a lot of fiddling and ceremony in other languages -- like multithreading and SIMD -- become a matter of dropping a single-word macro in front of my loop. Similarly, I can just prefix array access with `@inbounds` when I need speed over safety. 

So, Julia for the win? 

Sadly, no. 

But, so close! 

I'll start with the minor drawbacks. The 'end' keyword for terminating blocks is prettier than a bracket, but fairly annoying for making one-liners. 

I like named parameters for self-documenting code. Julia allows named parameters but naming them doesn't provide any flexibility with regard to their position when calling the function. 

Julia's has 1-based array indexing by default. This fits my worldview, and you can even define some of your arrays to start at 0, but this flexibility does tend to cause bugs working with other people's code. This is compounded by Julia's pursuit of composability. If you cram your custom data value into someone else's function, and if it seems to be the right shape, it will probably work! Unless it doesn't. In that case you just get silently wrong answers. This is a deal-breaker for a lot of scientific computing people where Julia would otherwise shine.  

I also find Julia's errors to be fairly obtuse. And I see a lot of them because dynamic typing means the tools can't catch most errors before run-time. 

But the big dealbreaker with Julia is it *only* compiles on the fly. That means you can't just hand a compiled executable to someone. You must give them instructions to install Julia and all your dependencies first. That's no good for anything you want to ship like a desktop app or a game. You also don't want Julia on your server recompiling with every web request. 

Like all compiled languages, the development cycle involves a lot of recompiling as you go. For small programs it doesn't matter, but this creeps up as the program grows -- or when you add a heavy dependency, like a plotting library. Julia suffers from this compiled-language drawback, but without the normal advantage of getting a compiled executable you could distribute. 

Basically, Julia is the sexy sports car you can only drive at the track. 

I wanted a "daily driver" that I could love for its beauty and speed, but that could also take me anywhere.


## C# 

C# maybe? It has the full backing of Microsoft, but also mature open source options. You can make everything from desktop GUI apps to command line utilities. There's a decent enough data science ecosystem. You can develop for backend web. You can even develop front end web, via wasm. My past has involved some game programming, where C# is in heavy use. There's even a reinforcement learning environment for the Unity game engine that I was drooling to play with. C# seems like the go anywhere, do anything, language. 

But, oh the semicolons. The curly braces. The syntax verbosity. The LongAssDictionaryWordFunctionNames(). For better or worse, this is a language born for enterprise IT departments. It's hard to feel happy grinding out a contrived LINQ expression for the same result as a "practically English" list comprehension in Python. The core language limits the kind of expressiveness that might confuse the next programmer who might inherit your code. But at the same time, C# has grown to become a kitchen sink of language features. The learning curve to grok any and all C# is steep. And the object-oriented roots run deep and date back to a time before composition was preferred to inheritance. 

Plus, if machine learning is an important use, then performance matters a lot. According to online benchmarks, C# is "only" 4x slower than the fastest compiled languages. But that's the difference between a 1-day and a 4-day iteration when tuning hyperparameters for machine learning. 

My Yahtzeebot, after a "line-by-line" translation of the Julia version to C#, clocked in at 3 hours and 17 minutes. That's better than Python's 50 days, and I think I could have driven it lower. But it wasn't striking distance to Julia's ~3 minute run time. By now I was developing a rhythm for learning new languages and became determined to find "the perfect" one. 


## Swift

There was a big push a few years ago to adopt Swift in machine learning circles. For whatever reason, that effort stalled, but I wanted to see if it could fit my needs regardless. I knew Swift was a heavily funded and well documented language, given that it's backed by Apple. Like C#, it had a lot of possible applications for diverse uses. I could make mobile apps! At least for iOS. 

As a compiled language, I assumed performance would be on par with Julia and Rust. 

After using it, I found Swift to be more like C# than I expected, with many of its drawbacks. Verbosity. Legacy API baggage. And slow. 

Slow? I thought Swift was an LLVM-compiled flavor-of-C kindof thing?

All I know is my "line-by-line" translation of Yahtzeebot for Swift ran for over 4 hours. I got that down to 1.5 hours after compiling with aggressive compiler flags that I scrounged off the internet. I didn't like that I couldn't selectively apply those optimizations *in code* like I could with Julia's `@inbounds` prefix for example. To the extent you can do this at all in Swift, you're really punished with parameter names like `unsafeUninitializedCapacity:...`. Yikes. I'd need a wider monitor.  

API naming aside, Swift syntax is "almost" pretty. You do need curly braces but get to ditch semicolons. Variable types are mosty inferred. The syntax is less verbose than C# where Swift has less punctuation and constructor ceremony. The `x...y` syntax is handy for ranges. Loops in the style of `for object in collection` are typical versus looping indexes. It doesn't hurt my eyes too badly.

I was prepared to write some of my own machine learning "library" code. But I was surprised to find so little support for basic numerics in the standard library. If you want to `sum()` over an array without reduce, you write it yourself. Want `unique()` items from a list? Write it yourself, or reach for a package. It's cool that I can define my own operators, but do I really want to need my own `**` operator for ints so that `Int(pow(Float(x),Float(y))` can instead be `x**y`?

Some brave language design choices were made with Swift. Some of them were... not to my liking. You _must_ name every parameter when calling a function (unless you shove underscores all around your function definitions). This feels unnecessary in world where you can have your IDE display the parameter names at the call site for you. 

You can pass a closure body to the last parameter of a function with the `in` keyword. Cool, but why "in"? No idea. Mutable parameters were once designated with the var keyword (as in "variable" not fixed). But now it's been changed to `&`. To me that smacks of just giving up and matching the obscure choices of legacy languages. 


## C

Speaking of legacy languages... :) 

By this point, I was feeling brave. I was polishing off new languages in weeks and had developed the opinion that, outside of stylistic choices, they're mostly the same. They all have some concept of a "list", and a dictionary-like collection, probably some kind of JSON library if the need ever arose. Just slightly different vocabulary for the same stuff, right? I could probably learn C. Why not?

Holy wow. Where are my lists? Why must I pass length as a parameter to every single function? Pointers are arrays? Semicolons everywhere. Does no one make use of other people's code for anything? 

My professional programming career of decades past had consisted of slinging GUI buttons on a form designer with some database code sprinkled in. Back then, C was for the neckbeards in my department who hacked on printer drivers for fun. C was too close to the metal and too far from the user for my taste. C definitely wasn't something I was going to enjoy.  

But while re-making Yahtzeebot in C, it kinda grew on me? It's so fundamental and pure. There are only 32 keywords. You're intimately aware at every step that everything is just bytes of data somewhere in memory. How to interpret that is up to you. And you must. It's utterly painful to do "simple things", but I learned to appreciate all the sugar in other languages. 

The bugs in C are brutal. You feel lucky when your program crashes. The alternative is to puzzle over some inscrutable value your variable has adopted because you overwrote memory in some other location entirely. Finding these kinds of bugs is measured in days. 

But C is the gold standard for fast. So fast. Yahtzeebot in 2.5 minutes. Raw power and papercuts at every step. It's empowering to realize I can dive in and see how Python itself was implemented. Battling with C shone a bright light on the unknowable mystery of the layers below me that I'd been oblivious to for so long. 

It was the hardest language to learn and I'm glad I did it. But C won't be "my" language. I want to be productive and benefit from the many conveniences that modern languages enable. 

If only there was a language with the speed of C and the elegance of Python. Surely, that's too much to hope for. Or is it?

## Nim

Yes, this is where the journey ends. You've probably never heard of Nim. There's no giant corporation behind it. Mostly it's been one dude hacking on this thing since 2008, supported by a handful of others. The community is tight. If you ask a question on the forum, the language creator may well reply. There's no way such a niche project could check every box on the "perfect language" list right? 

Well, let me try to convince you.

Nim compiles to C, and it's just as fast. Yahtzeebot runs in *under* 2.5 minutes -- 6 seconds faster than my fastest hand-crafted C version. 

That blows my mind, because Nim code reads like Python. It's a language I could teach my kids. There is no noisy extra crap all over everywhere -- just the intent of your code, plainly described. You look at it, and it does what you expect. 

At the same time, it gives you great power. For example, you can write a section of Nim code that transforms other Nim code at compile time. You can do this with some simple substitution using a Nim template, or carve right into the abstract syntax tree with a Nim macro. (This is best used sparingly, but hints at the flexibility.) 

Because Nim compiles to C on its way to a fully distributable binary, it leverages the highly tuned compiler ecosystem for C and it runs everywhere. You can make slim binaries that run on tiny embedded devices. That's crazy for a language that reads like Python. 

You can also "drop down" and write a block of C code right inside of a Nim file and it all compiles together. I thought this would be super useful for performance-critical code, but I never needed it. Nim is just that fast already. If you don't need portability, you can even drop down to Assembly.

Nim gives you all this raw speed, but it doesn't make you reinvent basic data structures like C . You get all the conveniences of a modern programming language and then some: Yes to lists (whew). Also, dictionaries, exception handling, type inference, first class functions, custom operators, custom iterators, named parameters, variadic function arguments, hot code reloading support, and a modern garbage collector (which is strictly optional in case you want to manage memory by hand). Nim comes with a detachable batteries-included standard library, and a good package manager. There's a pretty spry package ecosystem that covers machine learning foundations, scientific computing, game development, desktop apps, and web development for both the server and the frontend. 

Wait, Nim on the frontend? Did I mention Nim can also compile to JavaScript? In this capacity, it acts like an alternative to TypeScript. You can drive other web frameworks written in JavaScript, or choose one of several in pure Nim.

Nim speaks easily to other languages. You can import and use any C library. It's also shockingly easy to drive Python code, which gives access to all its great packages. Matplotlib is at your disposal. There's a similar bridge to Julia that I haven't tried. In addition to C and JavaScript, Nim supports compiling to C++ or Objective-C. As expected, it's easy to interop with libraries written in those languages too. 

There are also many little language niceties... too many to list them all here. But just to give you a taste...

- You can declare your arrays to start at 1 if that's your thing. But they're typed as such, so you won't accidently hand a 1-based array to a function that expects 0-based, as in Julia. 

- Inside every function you can return a value by assigning to the special variable "result". In practice this makes many 3-line functions into 1-liners. 

- Nim guides you toward making value-objects by default. These live on the stack and benefit from better performance versus the more typical "box all the things" approach of Java and other languages.  

- Like Swift, you can pass the body of a procedure to the last parameter of function which takes a closure there. This enables some nice API options. 

- You don't have to decide between snake_case or camelCase. You can define your variable either way and both references will work. Ditto for most cases of capitalization. I thought this might be problematic, but in practice it's brilliant. I think that sentiment applies to many of Nim's unexpected design choices. 

- Another in that category: You can choose to call your function like `add(a,b)`, but also like `a.add(b)` or even `a.add b`. While not immediately apparent, this helps improve readability in different scenarios. 

There's even a little-known REPL called INim that supports the kind of REPL-driven workflow that I learned to like in Julia. Because of this, I haven't much used the Nim kernel for Jupyter notebooks, but that option exists. 

Now I would be somewhat remiss not to mention Nim's drawbacks, as I see them. 

You might wish for a package that hasn't been written yet, or find that the best candidate hasn't been updated in a while. The excellent language interop helps here. There's probably a C library or a Python package that you can easily use.

Similarly, you won't for sure find a Stack Overflow entry that solves the exact problem you're facing. I imagine this is what the Python ecosystem might have been like back when Guido could have answered your question over IRC. 

The Nim tooling is a little lacking in some areas. There's a plugin for VSCode that gives you autocomplete and refactoring support. But sometimes the refactoring just doesn't work and you have to fall back on search/replace. You do get reliable break-point debugging for stepping through your Nim code. But behind the scenes it's really debugging the intermediate C code. So the variable names you're shown for inspection are mangled versions of the originals. On Mac, you'll need to discover and configure "lldbnim.py" in your IDE or the mangling is worse. Lastly, the debugger sometimes insists on stepping into library code when you really want to focus on just your own.  I anticipate things here will improve.

The order of your function declarations do matter, so you can't make reference to a function defined below the one you're working on.  There's an experimental feature to fix this that doesn't always work, but you can always write a forward declaration as a last resort. 

I haven't found a way to program the GPU in Nim, but I think this might be possible for a package to enable this through some combination of macros and a possible CUDA integration. 

Like Python, indentation matters, so I can't indent a bunch of code under a comment heading then collapse that in my editor. The style guide says to indent with two space. Sorry, but I'm sticking to four. 

Nim is definitely a language with a lot of features, so it takes a while to learn them all. There are many pragmas. Everything isn't critical to learn. But then if you're diving into other people's code you may find stuff you don't recognize.


## Punchline

So why aren't more people using Nim? I don't know! 

It's the closest thing to a perfect language that I've used by far.

I hope you'll try it. Maybe you'll write a package that helps to define Nim in the way Rails did for Ruby. Those opportunities still exist, and we'd love to have you.  



ps. My [Yahtzeebot](https://github.com/mode80/nimzbot/blob/master/yahtzbot.nim) Nim code is on Github. I dare you to make it faster. ;)  