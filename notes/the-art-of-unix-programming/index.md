---
layout: page
title: The Art of Unix Programming - by Eric S. Raymond 
---
### My Notes 


Unix supports casual programming, scripting tasks, flexible power.
\\
\\
Design for simplicity and consistency.  Unix programs should communicate with other programs not yet thought of, by making output consistent and organized.
\\
\\
Programs shouldn’t need to know about the internal elements of other programs.
\\
\\
Programmers should be allowed to do what they want, like delete a bunch of files in one command, but only if they own those files.  Permissions should keep one users’ code from harming others’.
\\
\\
Process-spawning should be inexpensive and Process control should be flexible and easy to manage. Process spawning happens when another process or thread of control is created so two or more processes can share the CPU concurrently. 
\\
\\
**Constructing simple software is more difficult than constructing complicated software.**
\\
\\
Modularity has been a hardware approach since the late 1800s. We break complexity into smaller, more manageable pieces.
\\
\\
Modules should be encapsulated and not expose their internals one another, call in the middle of one another’s implementations or share global data “promiscuously.”  They communicate using APIS, “narrow, well-defined sets of procedure calls and data structures.”
\\
\\
API description should make sense in English first.  Many designers first write comments then do the coding.
\\
\\
**Compactness means a program can fit in the human head, be understood and experienced by user without a manual.**
\\
\\
Phone numbers are seven digits because research showed that humans can remember 7 digits plus or minus two in short-term memory.  Does your API require a programmer to remember more than 7 entry points?
\\
\\
**Orthogonality** means no side effects.  Each action changes just one thing without affecting others.
\\
\\
**SPOT** == Single Point of Truth. This means initialize once, import elsewhere. don’t duplicate knowledge.
\\
\\
**Compactness** - many of the best Unix programs are just wrappers around one algorithm that does one thing well, clearly (ex: diff, grep, yacc)
\\
\\
Top-down vs Bottom-up software design

  - Are we thinking about the user and high-level functionality?  Or the limits of hardware, specific calls to low-level machine stuff… then gluing them together later?
  - Main event loops are the “top.”
  - Network connections, protocols are the “bottom.”
  - “Which end of the stack you start with matters a lot, because the layer at the other end is quite likely to be constrained by your initial choices.”

Unix programmers tend to lean towards the bottom-up approach because low-level hardware primitives are so important.
\\
\\
Often, glue layers are needed to connect top-down and bottom-up approaches.  Impedance-matching.  Glue should be thin, minimal.
\\
\\
C Became widely accepted in the mid-80s across different architectures because it was general purpose, not extensive.  Also the libraries are well-modularized.  (ex: Gimp plugins are small, simple C programs).
\\
\\
There is some conflict between Unix traditions and Object-Oriented (OO)  approaches.  Why?

  - Unix conventions are similar to C, and it’s hard to do objects in C.
  - Unix has few layers of abstraction between hardware and top-level objects of program whereas OO-programming encourages lots of easy abstraction, meaning more thicker glue everywhere.
  - OO can lead to more side effects.

**Modularity means good design.**  Global Variables are “poison” to modularity.  They Let components leak info to one another.  Are your modules much more than seven?  Are your functions describable in a one-line comment?  Break things up into subprograms when they get too complex.
\\
\\
**Complexity of module rises to the square of # of API calls.** Try to keep API calls around seven or less. 
\\
\\
**Text streams encourage encapsulation and human understanding.  They are more portable and easily editable/readable by human eyes and fingers.**
\\
\\
**“Beauty is the ultimate defense against complexity.”**
\\
\\
**Elegance == Power + Simplicity**
\\
\\
Elegant code should be **transparent and discoverable**.  This makes it easier to read, debug and evolve forward.
\\
\\
Transparency means you can hold a mental model of all or most cases of a program.
\\
\\
Transparency vs Discoverability

  - Transparency means you can see what it does
  - Discoverability means you can get in there and start tweaking code

Transparent and discoverable code will be easier to debug, maintain and share, making it more likely to survive forward-porting, maintenance and time.  Some things to ask yourself when you’re trying to be transparent/discoverable:

  - What is max depth of procedure-call hierarchy?  How many levels will human brain have to call to understand what’s happening.  (“If it’s more than four, beware”)
  - Does code have strong, visible invariants (rules that can’t be broken)?
  - Are your API calls orthogonal (no side effects)?
  - Are there lots of prominent data structures?
  - Is there a clean one-to-one mapping between data structures or classes and the entities in the world they represent?
  - Is it easy to get to things quickly, be it a file or function?
  - Does your code proliferate special cases or avoid them?

**Simplicity + transparency == Code that will be understandable by others and live longer, keep getting developed**.  Any unix program that is more than a decade old is an example of this b/c unix coders would rather re-write a program that fix bugs again and again.
\\
\\
Multiprogramming (also known as multiprocessing) is splitting up a large program into cooperating processes.  The philosophy of keeping programs modular, doing one thing well, also applies to subroutines.
\\
\\
Make processes simple and allow them to communicate, concentrate on interfaces.  Unix allows this by:

  - making process-spawning cheap
  - allowing communication thru pipes, sockets, message-passing, etc
  - encouraging simple, transparent text formats

Protocol logic, that is communication between processes, should be easy to hold and visualize in the human head.  “Premature Optimization” occurs when you put system performance ahead of a clear logical picture, when it comes to multiprogramming.
\\
\\
Threads are defined by author as multiple concurrent processes occupying the same address space.
\\
\\
Programs often call subprograms.  The example given is the Unix mutt Mailer user, which calls a text editor (configurable) when you compose or reply.  At this point mutt does a “shellout” and creates a temporary file name, calling the program set by your EDITOR variable.  When the editor terminates, mutt assumes the temp file contains the text you want.  Most unix mail programs use this convention so we can just specify the text editor of our choice.
\\
\\
**Pipes were created by Doug McIlroy and they kick ass, encourages “do-one-thing-well” design and led to socket abstraction techniques for networking.**
\\
\\
Pipes need at least two I/O data streams, standard input and standard output (0 and 1 are numeric descriptors, respectively).
\\
\\
Normally, stdin and stdout direct to keyboard and monitor.  Redirection operators can be used to reroute like this:

  - ls > foo
  - wc < foo

The latter takes its word count from the file “foo.”  The programs do not communicate farther than the second may take input args from first.
\\
\\
Pipes, on the other hand, connect stdout of one program to stdin of another, which is called a pipeline.  For example *ls | wc*
\\
\\
**Piping is used from one process to another, whereas redirection is used for multiple shell commands.**  Pipes’ weakness is that they are unidirectional.  You can’t send info back up the pipeline.  The opposite of a shellout is a wrapper.
\\
\\
**A Wrapper is a new interface for a called program**.  As with shellouts, there is no protocol for interprocess communication.  But the wrapper may define args that will modify the callee’s behavior.
\\
\\
Security Wrappers and Bernstein Chaining are used for security purposes.  First a password must be verified, for example, before control is passed on to the next program and so forth.
\\
\\
Peer-to-peer communication happens when data flows freely in both directions.  This is contrary to the above examples, where there is a master or driver program calling a subordinate.
\\
\\
Race conditions occur when two signals need to happen in a certain order, but there is no mechanism to assure that they do.  Causes intermittent issues, very hard to debug!
\\
\\
Sockets are used to encapsulate data networks.  Usually it’s a bidirectional byte stream between two programs.  Error detection is included by default.  Sockets have different protocol families.
\\
\\
**“To use sockets gracefully, in the unix tradition, start by designing an application protocol for use between them - a set of requests and responses which expresses the semantics of what your programs will be communicating about in a succinct way.”**
\\
\\
PostgreSQL is a case study here.  It’s not a big monolith where multiple instances of the program are trying to access same disk.  Instead, there is a postmaster which has exclusive access to db disk/files.  Different clients communicate with postmaster through TCP/IP sockets and data is passed around as text streams.  This is simpler b/c server only needs to take SQL requests from clients and serve.  The clients don’t need to know underlying details of db.  They can be specialized for different interfaces, uses.
\\
\\
Shared memory is the fastest way to pass information between two process, but programs must be on same hardware.  Sockets can span oceans.
\\
\\
IPC == Interprocess Communication
\\
\\
RPC == Remote Procedure Call.  This happens when one program calls another program that is in a different address space.  It may be on a different machine, or elsewhere in the network.  RPCS are not accepted in unix b/c they lead to messy implementations, and violate the rule of simplicity (says the author).  Although, SOAP is an example of RPC and Text Streams combining.
\\
\\
Remember, threads share the same address space.  Unix programmers do not like them.  Tis better to have lightweight processes with their own address spaces.  Races and deadlocks start to occur with multi-threading.  Also, threads aren’t encapsulated well, they share too much, know too much about one another.
\\
\\
**Avoid threads if possible b/c they are a “performance hack.”**
\\
\\
**“Machine resources get cheaper over time, but space in programmer’s heads only gets more expensive.”**
\\
\\
People are better at understanding data than control flows.  We should strive to put as much of our code as possible into data representation.  It’s easier to manipulate later than control flow.
\\
\\
**“It’s good practice to move as much of the complexity in your design as possible away from procedural code into data, and good practice to pick data representations that are convenient for humans to maintain and manipulate.”**
\\
\\
Let machines translate the code into forms that are good for machines to deal with.
\\
\\
By “lifting” our code up to a higher level, problems can become more manageable.  Again, fewer lines of code means fewer bugs.
\\
\\
**Data-driven programming clearly distinguishes code from data structures on which it acts.**  The idea is that one can make changes to the logic of the program by editing not the code but the data structure.
\\
\\
In DD programming, data is not in state of an object, but defines the control flow of a program.  OO wants encapsulation the most, whereas DD wants as little fixed code as possible.
\\
\\
A good Stack Overflow explanation of DD Programming here (here)[http://stackoverflow.com/questions/1065584/what-is-data-driven-programming].  Says that If you have a program that has four states - UP, DOWN, STOP, START - you can control the program by offering different input sets like

  - UP, DOWN, STOP, START, DOWN
  - UP, DOWN, UP, DOWN

Program code stays the same but data controls the flow!  This is very unixy when you stop and think about it.  We pipe all these streams around through the same programs and get different outputs.
\\
\\
ascii is the first case study.  A little program written by the author that can convert between decimal, hex, octal, binary representations of bytes.  It’s data-driven because character name strings live in table structure.  The code is short not much and it navigates the table.  We can easily change the tables, add entries, delete, etc.
\\
\\
**“Reuse, simplification, generalization, orthogonality:  this is the Zen of Unix in action.”**
\\
\\
Ad hoc - means it’s done for a particular purpose.
\\
\\
**“Constructive laziness is one of the cardinal virtues of the master programmer.”**
\\
\\
Learn to separate policy from mechanism.  See this pattern quickly and automatically.
\\
\\
Unix programs generally communicate with their environment in two ways -

  - Start-up environment queries
  - Interactive channels

“What should be configurable?” - An important question to ask oneself before beginning.  The author says unix programmers answer “everything,” which can result in a lot of options and confusion for the average user.  So maybe its better to ask, “What should not be configurable?”

  - Anything that can be detected automatically
  - Optimization switches;  it’s the programmer’s job to set these
  - Anything that can be piped to another program

A good rule of thumb:  Be adaptive unless doing so causes 0.7 seconds of latency or more.  Human beings don’t generally notice startup latency less than that.
\\
\\
Config items typically live in 5 Places

  - Run-control files in in /etc (or fixed location in system land)
  - System-set environment vars
  - Run-control files (dot-files) in user’s home dir
  - User-set environment variables
  - Switches and args passed to program from command line

And they are usually queried in that order so more local (listed later) settings override global ones.  Think about which option is best.  For example, vars that change often should be passed in as args.  If they seldom change, put in individual user control or dot file.  If it’s site-wide, use run-control file in system space.
\\
\\
Run-control files may be in /etc if they’re for everybody.  Per user, they may be a dot-file in user’s home dir like .vimrc or .bashprofile.  rc stands for run-control!
\\
\\
Dot-files are generally read once on startup so speed isn’t an issue.  We leave them in text-format so they can be read/edited by humans.
\\
\\
Some run-control patterns-

  - Support comments on the same line
  - Don’t make distinction between tabs and one or more whitespaces.  Ignore trailing tabs, spaces.  Human eye should see it all!
  - Treat multiple blank lines same as single blank line
  - Treat the file as a series of simple tokens and proved escape char options.
  - Support string syntax with quotes.  Be wary of treating single/double quotes differently
  - Follow convention by default

The books lists interpreters as an exception to the above.
\\
\\
Environment variables are key-value pairs.  System Environment Vars, such as HOME, USER, and PATH, are usually set when a terminal is started up.  Notice that PATH uses the colon to separate multiple fields, which are often file paths in this convention.
\\
\\
User Environment Vars are less common but used for application independent settings that are shared by lots of programs, for example, EDITOR, PAGER, MAILER, BROWSER.
\\
\\
When to use them?  When the value varies too often for dotfiles but doesn’t change on every startup.
\\
\\
Command-Line Options are ‘switches’ used to control programs.  Lowercase letters are default and uppercase letters are usually variants of their lowercase counterparts.  The GNU full-word switches came along later when we ran out of letters.  They tend to use to dashes.  Most common switches should also have lowercase unix implementation too.
\\
\\
There are some common command line flags, often with an accompanying number to set the level (for example, -d1 sets debugging on to level 1)

  - a all
  - b buffer or batch
  - d debug
  - f force or file
  - h headers or help
  - n number
  - o output
  - r recurse or reverse
  - u user
  - v verbose or version
  - x enable debugging or extract
  - y yes, for programs that require confirmation
  - z enable compression

We have investigated in the order of least easily changed to most easily changed - run control files, environment variables and command line flags.  Things that are easier to change will usually override those that are harder to change.
\\
\\
Case study - fetchmail only uses two environment variables, USER and HOME.  USER points to .fetchmailrc config file b/c configuration rarely changes.  Fetchmail gets host/login/password values from .netrc file.
\\
\\
Many command line options can complicate the code base, bulk up the manual and lead to unforeseen interactions between different flags - all bad things!  One workaround is to include a “-o” option which allows you to override config file values from command line (in the normal key-value config way).
\\
\\
The /etc/ dir typically holds config values for all users, which can be overridden by each users local rc files.
\\
\\
Recurring themes -

  - Design programs to communicate with other programs
  - Rule of Least Surprise

After startup, programs generally get their input from stdin, ipc (interprocess communication such as events, network messages) and files.
\\
\\
Different programs call for different interface design.
\\
\\
Mimic programs that are already known by the user.  Familiarity is a good thing for use.  Don’t be too clever for the user’s good.
\\
\\
Delegate when possible (shellout to existing text editor or pager).  This allows simpler code and more familiarity for user.  When you can’t delegate, emulate.
\\
\\
**Why write a frontend at all when you can use the universal browser?**  It’s widely used, accessible over geographic distances, has html + javascript for functionality, separates client logic from server code.
\\
\\
Disadvantages of browser as frontend -

  - it has a batch style of interaction.  for example, fine-grained mouse interactivity doesn’t travel well.  it’s more typical to fill out a form, click submit and have all data sent in one go.  server runs on backend and sends back more html in one go.  
  - managing stateless protocol can be tricky.  server doesn’t keep up with client state.  cookies can workaround this.  they’re like environment variables

**Silence is golden.  Don’t make your program say things unless it’s absolutely necessary.  There is enough noise in the world for humans and finite screen space.  Also, it can confuse other programs that are trying to interact.  Use verbose flag if necessary.  Don’t badger user with questions on startup that need “yes” every time.**
\\
\\
**“The most powerful optimization technique in any programmers’ toolbox is to do nothing.”**
\\
\\
Often times, if you just wait a few months your hardware will get faster on its own (Moore’s Law).  Programmer time is more expensive than hardware.  The book claims that it is hardly ever worth the time to optimize for constant performance increase such as O(n squared) to O(n).   “Linear performance gains tend to be rapidly swamped by Moore’s Law.”
\\
\\
Before you do any optimizing, measure your code.  We are often wrong about where the bottlenecks are, even when we know the code well.
\\
\\
Profilers are tools that help you measure where bottlenecks are.  Reading them can be challenging b/c of -

  - Instrumentation Noise - The profiler inserts more statements which results in more execution time.
  - Imposed External Latencies - This includes disk and network accesses, cache fills, process-context switches - things that can be random or different each time the program is run.  Adding together the results from lots of profile runs can help with this.
  - Overweighting of upper nodes in Call Graph - This happens when your functions call other subroutines and the profiler overweights the function in question.  Try to make your functions call only low-level routines and not other code that you’ve written.

Keep your programs working in the cache!  Another reason to keep them small - your data structures and loops stay in memory while they execute.
\\
\\
**“The average instruction spends more time being loaded than it does executing.”**
\\
\\
Rules for optimization are changing as processors get faster, but cache distance isn’t optimizing at the same rate.
\\
\\
Beware of round-trip network calls!  They are very expensive.  If you can get the client to handle something on its own, without talking to server you have a big win.  Avoid latency whenever possible.
\\
\\
It is generally better to go for short startup times and quick response over pre-computation of big things.
\\
\\
Three general strategies for reducing latency are -

   - Batching Operations - Graphical APIs, for example, use a lot of processing power to update the screen.  For this reason, operations are written to an internal buffer and the programmer decides how often to update.  Persistent service daemons use batching too b/c they modify shared resources.
  - Overlapping Operations - In this case, we don’t block or wait on intermediate steps before moving to others.  Often used in networking protocols.
  - Caching Operations - Compute expensive results as needed and cache for reuse.  For example, we can make a binary cache of our passwords so users can login quickly on very large sites.  We’ll need to check the timestamp in db before assuming cache is current.  Or put a wrapper around the db insert so cache is automatically updated each time a password changes.  There are downfalls including duplication and loss of storage space + race conditions can happen too.  This is mostly an issue in complex large systems.  Python does this with .pyc files - it runs compiled code unless source is newer.

“When you think you are in a situation that demands caching, it is wise to look one level deeper and ask yourself why the caching is necessary.  It may well be more more difficult to solve that problem than it would be to get all the edge cases in the caching software right.”
\\
\\
**“Everything should be made as simple as possible but no simpler.” - Einstein**
\\
\\
What is complexity?

  - Implementation Complexity is how hard it is for a programmer to understand a program so he or she can mentally model and debug it.
  - Interface Complexity is what the user, customer sees.  How complex is it to use and understand the program?
  - Codebase size is the number of lines of code.  This tends to measure the life-cycle cost of a program.  More lines of code means more debugging and debugging is the most expensive and time-consuming part of development.

Beware Feature Creep, which can cause all three types of complexity to rise.  A blivet trap is when you try to stuff tend pounds of manure into a five-pound bag.
\\
\\
The chapter talks about Worst is Better designs, which keep implementation complexity down while giving a lesser interface to the user.  This ‘worse’ design is a forerunner for open source and allows for more iterations to keep being built on a simple implementation until it outdoes the pretty interface (which has uglier details underneath the hood, thus harder to maintain).
\\
\\
There is not a one-size-fits-all answer, says Raymond.  “Complexity is a cost you must budget very carefully.”
\\
\\
Sources of Complexity -

  - Essential Complexity is evident in airplanes.  The shit has to work.  You can’t trade away features for simplicity.
  - Accidental Complexity happens when someone didn’t find the simplest way to implement “a specified set of features.”  Can usually be eliminated by good design or redesign.
Optional Complexity has to do with desired features.  You have to change project’s objectives to eliminate it.

So we often have disagreements about what a project’s objectives are.  There can be confusion about was is accidental and what is optional.
\\
\\
Accidental code complexity often results from premature optimization.
\\
\\
Accidental codebase bloat often results from duplicating code or not re-organizing it so it can be reused.
\\
\\
Optional complexity is usually the most subjective.  Can we agree on what features need to be included?
\\
\\
“The sources of complexity have to be grappled with in different ways.  Codebase size can be attacked with better tools.  Implementation complexity can be addressed with better choice of algorithms.  Interface complexity has to be addressed with better interaction design, a skill involving considerations of ergonomics and user psychology.  This skill is less common (and possibly more difficult) than writing code.”
\\
\\
“You cut accidental complexity by noticing that there is a simpler way to do things.  You cut optional complexity by making context-dependent judgements about what features are worthwhile.  You can only cut essential complexity by having an epiphany, fundamentally redefining the problem you are addressing.”
\\
\\
Sometimes complexity is necessary.  It’s worth questioning it before beginning something.  The book discusses this topic by examining Five Text Editors with the following benchmarks in mind -

  - Plain-text editing (all the following examples can do this, but many programs like word processors cannot)
  - Rich-text editing
  - Syntax awareness
  - Output Parsing
  - Interaction

“In old-school unix, the only framework was pipelines, redirection and the shell; the integration was done with scripts, and the shared context was (essentially) the file system itself.”
\\
\\
Unix gives you lots of development tools, but you have to put them together yourself.  IDE’s may seem easier, giving you editor, compiler, etc but they lack the flexibility.  In unix, you may use lots of different languages and different tools for a project.
\\
\\
“Unix encourages a more flexible style, one less exclusively centered on the edit/compile/debug loop.”
\\
\\
“The one-time cost of climbing the learning curve should be more than paid off by the ability to write programs more efficiently, and spend less attention on low-level details and more on design.”
\\
\\
The book claims that vi and emacs are about 50/50 with Unix users and all other text editors barely register at all.
\\
\\
Vi stands for visual editor.
\\
\\
Emacs stands for Editing MACros.  Emacs key bindings can be found in many places, including web browsers.
\\
\\
The book says that vi is better for small jobs and Emacs for larger jobs, like editing lots of files and scripting, etc.  Many folks use both, for different jobs.
\\
\\
Make - 

  - Manages your linkage and compilation into binaries
  - Human readable text file that manages dependencies
  - Can figure out things on its own, for example if mysource.c has changed it will re-compile to mysource.o for a target that depends on it
  - Make contains a bad design flaw, where it depends on tabs instead of whitespace!  It was baked in early on, and still remains.
  - Can be used to generate documentation.  Wow.
  
Utility Productions have certain conventions.  For example, following these conventions with your makefile will make it easy to understand -
all makes every executable in your project.  Should be at the top of your makefile and happen when developer types make with no arg

  - "test" should run all tests
  - "clean" should remove all files made by make all, for example binaries and objects
  - "dist" make a source archive, like a tar, for sending to other developers
  - "distclean" undoes dist, analogous to clean
  - "install" installs the executables and docs in a place where other users can use them.
  - "uninstall" undoes install.  It’s a courtesy to your user, especially if project builds large files, databases, etc.

Makefiles are text files and can thus be generated by programs.  A few makefile generators of note are makedepend, Imake, autoconf, automake
\\
\\
Version Control is discussed next.  Great for recovering from mistakes, remembering what you did, saving space, sharing code, collaborating, etc.  Common conventions include following files, checking them out, trunks, branches.  Here are the unix tools surveyed in this chapter -

  - SCCS (Source Code Control System) was the first one developed by Bell Labs.  It’s now obsolete.
  - RCS (Revision Control System) was next, most used for unix when book was published
  - CVS (Concurrent Version System) doesn’t lock files when checked out.  Subversion and I suppose Git are examples of CVS.

Runtime Debugging - Oh, the errors we get while a program runs.  Transparency helps - Design things so we can see the flow of data with the human eye and hold the mental models in our brains.
\\
\\
Debuggers should allow us to examine state of program at run time, examine breakpoints, execute single statements.  gdb is a well-known unix debugger.
\\
\\
“Remember the unix philosophy.  Spend your time on design quality, not the low level details, and automate away everything you can -- including the detail work of runtime debugging.”
\\
\\
Generally, 90% of your program’s execution time happens in 10% of the code.  Profilers help you id this 10%.  Don’t optimize the other 90%!  Tis a waste of time.
\\
\\
Emacs has built-in functionality for driving nearly all the tools discussed in this chapter - make, debugging w/ breakpoints, version control and more.  Emacs should be able to do all that any IDE does, only better, WAY MORE FLEXIBLE/CUSTOMIZABLE.
\\
\\
**“When the superior man refrains from acting, his force is felt for a thousand miles.” - Tao Te Ching**
\\
\\
“Remember the rule of economy.  Reinventing fire and the wheel for every new project is terribly wasteful.  Thinking time is precious and very valuable relative to all other inputs that go into software development; Accordingly, it should be spent solving new problems rather than rehashing old ones for which known solutions already exist.”
\\
\\
“The most effective way to avoid reinventing the wheel is to borrow someone else’s design and implementation of it.  In other words, to  reuse code.”
\\
\\
The author says transparency is key here and well-commented source code is its own documentation.  Source code lasts and object code doesn’t b/c hardware is always changing.
\\
\\
Open source also feeds programmers need to be artists, to spread their vision, inspire, be useful, be understood and appreciated, admired even.  Most open source unix code is installed via ./configure; make; make install;
\\
\\
In many ways, open source encourages the best work.  gcc is an example of this and hardly any proprietary C compilers are used any more.  When evaluating open source code, read the docs, skim the code and check the readme for more contributors.  The latter is a good sign.
\\
\\
Open source requirements -

  - Unlimited right to copy
  - Unlimited right to redistribute in unmodified form
  - Unlimited right to modify for personal use

If you give credit where credit’s due, and point to source code you borrowed, you can use other open-source code legally.
\\
\\
Unix was the first production operating system ported to different processor architectures.  
\\
\\
Hardware-independent code is more future proof b/c future hardwares are unknown.
\\
\\
C has been around for decades; it’s amazing how long it’s survived, this thin-layer-on-op-of hardware language.
\\
\\
In 1973 Unix was re-written in C.
\\
\\
A nice slapdown on WYSISWYG editors kicks off this chapter.  They violate the law of transparency because they insert codes that you can’t see or manipulate.  Markup-centered editors that are transparent are suited for power-users.
\\
\\
Unix documentation is written by knowledgeable unix hackers for their peers.  Also, unix documentation include bugs and is unembarrassed by admitting this.  It’s a sign of quality work.
\\
\\
Unix docs look cryptic, but be careful!  When reading Unix documentation,  “Read every word carefully, because whatever you want to know will probably be there, or deducible, from what’s there.  Read every word carefully, because you will seldom be told anything twice.”
\\
\\
troff is an original documentation tool that adds transparent encoding for new paragraphs, bold, etc - similar to markdown.  It’s referenced time and time again in this chapter as the original.
\\
\\
nroff is a troff variant used for man pages now.
\\
\\
Downsides to the unix approach include image display and non-English translation.  Also web hyperlinks can be an issue too.
\\
\\
The author says unix documentation is “presently” a mess, as of 2003.  Should look into this some time and see which theory pans out - a unified documentation format that works across distros and allows web links.  Docbook is a solution that looks promising (in 2003).  It’s XML-based.
\\
\\
“Development teams that are loosely networked and very large can do astoundingly good work.”

Rules of Open Source -

  - Let it be open w/ no secrets.  Encourage re-distribution and peer review.  Grow the community.
  - Release early, release often.  Just make sure you release builds, runs and demonstrates promise.  It must do some portion of its final job.  Releases should be off main line, not separate branches.  Make sure your first release compiles or it may die.
  - Reward contribution with praise.  Help people feel important.

“Most people become involved in open-source software by writing patches for other people’s software before releasing projects of their own.”

Take heed: patches that are presented in a careless way are usually not quality.  The opposite is also true.  Present yourself professionally, with knowledge of conventions.  Convey smarts and experience.

Some tips on how to get your patch accepted -

  - Send a diff, not the whole file (unless it’s a new file).The diff command and its dual, patch, are the most basic tools of open-source dev.
  - Make sure your patch is against current version of code
  - Don’t include patches for generated files
  - Don’t use default -e format of diff.Use -c or -u.
  - Include good documentation with the patch
  - Include an explanation with the patch
  - Include useful, comments
  - Be confident but unassuming.  Brief is good.
  - Don’t take it personally if patch isn’t accepted
  - Follow existing naming conventions carefully.  Lowercase!  like foobar-1.2.3.tar.gz  - There are three numbers here.  The first means the new release has incompatible features.  The second means compatible new features.  The last is a minor patch, bug fix, tiny feature.

People building from source expect to type configure; make; make install and get a clean build.
\\
\\
Test your code and include a test suite!  make test should do it.  Use as many compilers as possible with all the warning flags on.  This is sanity testing.
\\
\\
Recommended Python Source Code Checker (in 2003) -
http://sourceforge.net/projects/pychecker/
\\
\\
Spell-check your documentation, readme and error messages.
\\
\\
With C/C++ code, it’s often cool to have a portability layer that abstracts away OS API separated out.  This way, an OS specialist can port your software.
\\
\\
A platform is at least the compiler and library/operating system release.
\\
\\
README file is meant to be read first, then INSTALL, HISTORY, etc
\\
\\
RPM (Red Hat Package Manager) is de facto standard for installable binary packages under Linux.  It’s a good idea for your software to provide installable RPMS as well as source tarballs.
\\
\\
Including checksums will ensure that files aren’t corrupted and don’t have Trojans inserted.
\\
\\
**Many of the changes in unix approach are due to the fact the computing power is becoming exponentially cheaper while programmer brain time is not.**
\\
\\
Unix adapts like nature, discarding things that aren’t efficient or necessary.
\\
\\
“The learned response of unix programmers, reinforced over thirty years, was to go back to the first principles - to try to get more leverage out of Unix’s basic abstractions of streams, namespaces, and processes in preference to layering on new ones.”
\\
\\
There is a discussion about Plan 9, which was a re-thinking of Unix.  Many of its principles were better even “more Unix than Unix” in the everything-is-a-file approach.  So why didn’t it replace Unix? -  “Unix creaks and clanks and has obviou rust spots, but it gets the job done well enough to hold its position.  There is a lesson here for ambitious system architects:  The most dangerous enemy of a better solution is an existing codebase that is just good enough.”
\\
\\
Still as of 2003, many of Plan 9’s conventions are working their way into Linux.
\\
\\
“A Unix file is just a big bag of bytes, with no other attributes.”  Nothing about the file type or the application that uses it is stored elsewhere.  More generally, everything is a byte stream, even hardware devices.  Pipes and shell programming sprang from this.  This byte stream approach poses problems with GUI interfaces that uses software objects that don’t conform to read, write, update, delete file operations.  Unix wants all the data about a file IN the file so we can just cat it to a new name and have the same thing.  GUI OS’s may hold file info elsewhere, as a bunch of name-value pairs of meta info.
\\
\\
The idea of double-clicking an icon of a file, which passes it to a handler program - this is different from Unix, but it’s become very popular.  Unix doesn’t handle this mode gracefully.  There are too many layers needed to pull it off in a Unixy way.
\\
\\
File deletion is forever.  This is a pain for some.  Unix does what you tell it, even if you say “Shoot me in the foot.”  Many feel that protecting oneself should happen at the GUI or Application level but not at the OS level.
\\
\\
Unix assumes a static file system.  While program is running, the files it uses are not changing.  Linux has file-and-dir-notification features as of 2003.
\\
\\
Unix API doesn’t have exceptions.  A workaround is often achieved by using a binding of Unix API in Java or Python that does handle them.
\\
\\
“[Mac Programmers] design from the outside in, first asking ‘What kind of interaction do we want to support?’ and then building app logic behind it…  By contrast, Unix people are all about infrastructure.  We are plumbers and stonemasons.  We design from the inside out…  ‘How do we get reliable packet stream delivery from point A to point B...’”
\\
\\
Unix programmers are elitists and can be dicks about it, even though their origins lie in being rebels and giving power to the people.  They fought against mainframes, but now they run them.
\\
\\
Even when unix tries to be UI friendly, it usually fails.
\\
\\
Unix shouldn’t let their be Microsoft getting all the market, says the author.  We should be more sympathetic and take on the UI challenge and “embrace the user-centered virtues of Macintosh.”  The lack of empathy for the average user gave Microsoft their shot at the grail.
\\
\\
Gives a nod to agile programming and the unix-like way of testing and re-building/scrapping design often.
\\
\\
“MacOS X has Unix underneath, and in 2003 Mac developers are making the mental adjustment to learn the infrastructure-focused virtues of Unix.  Our challenge will be, reciprocally, to embrace the user-centered virtues of Macintosh.”
\\
\\
*Note: Below this line are more in-depth notes on various subjects.  No read to read on unless you're hungry for more.*

-------

#### More on Textuality

Retaining data in file formats and passing data around between programs, networks, etc - These are important issues.  File formats and pointers may not make sense across different machines (32-bit vs 64-bit memory, for example).
\\
\\
For transmission and storage, data structures must be flattened or serialized into byte-stream representations that can be later recovered.  Serialization is often called marshalling and its inverse is called unmarshalling.
\\
\\
According to the author, it’s better to compress a text stream than design a complex binary file format.  Textual protocols are more future-proof.  Large images and multimedia often require a binary protocol to get the most bit density.
\\
\\
Some networking protocols need to be binary to execute in better time or have lots of instructions.  SMTP and HTTP are text-based protocols.  As a result, they require more bandwidth and take more time to parse.
\\
\\
Unix Password File Format is a text file with records one per line and colon-separated fields
\\
\\
PNG is given as an example thoughtfully designed binary data format.  If pixels were stored textually, the size and download times would go up significantly.  Transaction economy was chosen over transparency.  
\\
Traditional unix tools include:

  - **grep** (Globally search for Reg Ex and Print)
  - **find** lines that match string or regex and print
  - **sed** (Stream EDitor) streams and transforms text.  Ex: *sed ‘s/regex/replacement/g inputfile > outputfile*
  - **awk** - An interpreted programming language that can be used to format output, reads input line by line
  - **tr** translates text, replaces like this - *tr ‘abcd’ ‘efgh’*
  - **cut** extracts specific sections from input

Conventional metafile formats are best because they work with common tools and other developers will be familiar with your code quicker.  Some examples of unix meta-formats:

  - DSV - Delimiter-Separated Values (colon, or tab separates values)
  - XML is accepted solution to nested data, but it can be hard to parse and difficult for humans to read.

Textual file Format Conventions

  - Generally, make one record per line is best for text-stream tools.  Trailing white spaces should be allowed too.
  - Less than 80 chars per line (fits terminal)
  - "#" for comments
  - Backslash escape convention
  - One-record-per-line formats should use colon or whitespaces.  Don’t allow distinction between tab and white spaces to be significant!  Should work the same with any whitespace or local tab settings
  - Favor hex over octal

SMTP (Secure Mail Transfer Protocol) and POP3 (Post Office Protocol) protocols both use plain text, line-oriented format for mail.  They have stood the test of time - simple, effective, human readable.
\\
\\
In internet transmission, the payload is the data that we actually want (minus meta info, headers and other overhead).
\\
\\
Many internet protocols are built on top of HTTP.  HTTP requests are messages in RFC-822/MIME-like format.  The header contains identification/authentication info and the first line is a method call on some resource specified by URI.  The most important methods are:

  - GET - fetch a resource
  - PUT - modify a resource
  - POST - send data to a backend process

Most firewalls leave port 80 open.
\\
\\
Building web servers on top of http makes them work easier to build and more likely to work everywhere.
\\
\\
MIME is MultIpuropose Mail internet extension.  It allows mail to send text in different formats, add files, meta info, etc.
\\
\\
Ex: fetchmail's -v flag spits out everything that’s happening when you run it - querying server, running scripts, communicating, etc - it basically shows everything that’s happening.  This makes debugging very easy.  “Don’t let your debugging tools be mere afterthoughts or treat them as throwaways.”
\\
\\
gcc stages: preprocessor=>parser=>code generator=>assembler=>linker
\\
The first three stages take in and emit text, the assembler emits binary code for linker to take in.  Like fetchmail, debugging is built into the program.  Command line flags show you what’s happening at each stage.
\\
\\
terminfo uses the unix file system as a database.  Look in usr/share/terminfo/ and you’ll see a bunch of subdirectories that are named 0-9, a-z.  Rather than scanning a long text file on startup for the right data, it takes less time to index the file system and open the desired file.  Also, you can use unix’s built-in permissions instead of writing your own.  This is an example of the everything-is-a-file approach.  The tradeoff, in this example, is space b/c file systems normally allocate 4k for every non-empty disk file.  However, space is cheap these days and quicker startup times are awesome.
\\
\\
If you need to edit binary data (like in audacity), write a tool that does lossless mapping to editable textual format, or some other format (like waveform), that makes sense to human mind/eye.  If you can’t do a textualizer, try doing a browser.

#### More on Minilanguages

Minilanguage, in this context, means a language that is created for some specific application domain.  The idea is that the programmer will write less lines of code with a minilanguage and the program will therefore be less error-prone.
\\
\\
This chapter discusses two right ways for creating a minilang:

  - realizing up front that pushing up a level will make notation more compact, expressive and less bug-prone
  - notice that your specs imply recurring control flow and data layouts

And one wrong way

  - Wading into it without thinking in advance, adding ad hoc patch after patch

**Imperative programming** is the most common kind where you write for loops, add numbers, etc - Java, C++, Python.
\\
\\
**Declarative programming is more functional.  Each function handles a specific task and you pass things around.  It’s more of a mindbender but can result in orders of magnitude of less code which makes it good for big projects.**
\\
\\
Makefiles are a declarative minilanguage.  They set up simple constraints that apply actions.
\\
\\
SNG is used as an example of in this minilanguages chapter.  SNG translates bits of a PNG into editable all-text  form.  You can “parse by eyeball” and edit with general-purpose tools.  Those are good things of course.
\\
\\
Regular Expressions (aka regexp) can be considered declarative minilanguages.  That’s because they can perform operations that would otherwise take tons more code.  grep is the simplest regexp tool.  Takes input and filters output of every line through regexp.
\\
\\
Regexp examples

  - x.y matches x followed by any character followed by y
  - x\\.y matches x followed by literal period followed by y
  - ^ is beginning of line
  - $ is end of line
  - [...] is any char between brackets
  - [^...] is any char except those between brackets
  - the book shows other examples on page 224...

Shell uses variations on regexp called “Glob expressions”:

  - \* matches any sequence of chars
  - ? matches any single char

egrep is more powerful than regular grep and perl takes it a step farther.
\\
\\
Glade is another case study.  It’s used for building GUI interfaces for X.  You modify widgets on an interface panel and glade produces an XML file describing the interface.  This file can be fed to code generator that grinds out C, C++, Python or Perl code for your interface.  This is an example of a domain-specific minilanguage.
\\
\\
m4 is a case study that does macro expansion, takes simple input strings and expands them.  This is an example of text string transformation.  Can be used to generate config files and much more.
\\
\\
**awk is an old-school unix tool.**  When awk runs, it steps through each line of input file.  Each line is checked against every pattern/action pair in order.  If the pattern matches the line, the associated action is performed.  awk has been on the decline since 1990, largely replaced by Perl.  awk was more efficient but computing power increases so rapidly that it became a moot point.
\\
\\
Postscript is another case study.  It was built to control printers and other imaging devices.  Eventually bitmaps are rendered for printing.  However, Postscript uses text or simple vector graphics to pass info around b/c this sort of info travels more quickly and is device-resolution independent.
\\
\\
dc is the oldest language on unix.  It was used for very precise arithmetic.
\\
\\
There is a lot to consider when designing a minilanguage.  If you can get by with a data file format do that.  If not, think very carefully about what the recurring pieces are and what the programmer needs to keep flexible.  If it’s possible for somebody to do “Turing-Complete” things like loop and recursion they will.
\\
\\
You can also create a minilanguage by extending/embedding an existing language.  This is more often appropriate for imperative minilangs.  The host language acts like a framework in this case.

#### Common Design Patterns

**The Filter Pattern** takes data on stdin, performs some transformation and sends data to stdout.  They are not interactive, though they may take some configuration options and switches.  Examples include grep, tr and sort.  Some rules-

  - Be loose in what you take in, strict in what you emit
  - Never throw away info unless you have to (might be useful later)
  - Don’t add noise.

The **Cantrip Pattern** is very simple.  No input, no output.  Just invocation and exit.  Examples include clear, rm and touch.  Separating interface from design is good.  Many programs are an interactive wrapper that calls a cantrip.
\\
\\
The **Source Pattern** takes no input, but it does output.  Examples include ls, ps and who.
\\
\\
The Sink Pattern takes stin but emits no output.
\\
\\
The Compiler Pattern uses neither stdin or stdout but may write to error logs.  Think about a compiler or transforming images.  Other examples include gzip, gunzip.  “Compiler-like interface” is well understood in the Unix community.
\\
\\
Contrary to the patterns above, the Ed Pattern requires continuous input.  Think of browserlike and editorlike programs.  Less scriptable b/c it needs constant interaction and what it says back may be unpredictable.
\\
\\
The Roguelike Pattern takes continuous input in the form of one key at a time (like vim or emacs).  Some menus are like this - you hit a letter but don’t need to press enter.  Very difficult to script.  Hard to learn but they remain used.  Why?  Good performance, quick start-up, works over remote connections - all these situations would required more juice to start a full-fledged text editor with GUI, for example.  And of course, you can often get your ideas across quicker like with vim.
\\
\\
The Separated Engine and Interface Pattern is very common in unix.  Keep your algorithms and core logic separate from code that interacts with the user.  Analogous to MVC with Engine as Model and Interface as View + Controller.  Model is separate whereas View + Controller tend to be closer to one another.  There are sub-patterns of this design -
Configurator/Actor Pair - Interface controls startup environment of a filter or daemon-like program which runs without user commands.
\\
\\
Spooler/Daemon Pair - Similar to configurator/actor, this is used when program needs no user interaction but there are shared resources.  Typically, there is a dir where we put jobs and spooler continuously polls looking for work (this is queue listener).  If job succeeds, request and data are deleted from spool area.  Examples include print spooler systems and old-school email over dial-ups that would try once then place job in queue for later if it failed.
Driver/Engine Pair - Has continuous communication between driver and engine.  also, engine can run on its own.
\\
\\
Client/Server - Like a driver/engine pair but engine part is daemon running in background (not interactive).  Examples include web browser and databases.  TCP/IP sockets are used from client-server pairs over the web.
\\
\\
CLI Server Pattern - Program which has simple CLI interface for reading stdin and writing stdout.  “When backgrounded, the server detects this and connects its standard input and output to specified TCP/IP port.”
\\
\\
Language-Based Interface Patterns - It’s like a minilanguage with an interface or GUI thrown on top for interaction.
\\
\\
**“To facilitate scripting and pipelining it is wise to choose the simplest interface pattern possible.”**
\\
\\
Polyvalent-Program Pattern has thin API over its library.  Other modes include cantrip, GUI, scripting interface, and maybe even roguelike interface.  In short, there are many ways to access the underlying service library.

#### Interface Design Considerations

Interface designs, are measured (in this chapter) in five ways -

  - concision
  - expressiveness
  - ease
  - transparency
  - scriptability

Good interfaces pack a lot of power into a few ‘state changes’ (key strokes, mouse clicks, etc).  The best designs allow combinations of actions unforeseen by designer but expressive to the user.
\\
\\
**Ease of use is inversely proportional to burden on user.**
\\
\\
Transparency in UI design relates to how easily user can figure it out on their own, without documentation.  Transparency (in this context) increases as user has to hold less things in their head while doing a task.  Immediate results, feedback, error messages - these help transparency.
\\
\\
Scriptability means how easily the program can interact with other programs, be automated.
\\
\\
mnemonic - aiding in remembering things.
\\
\\
CLI (command line interface) vs Visual interfaces 

  - cli scriptable, concise but harder to learn
  - some tasks are just more visual in nature though, like drawing a picture, dragging and organizing objects around
  - cli’s really shine as the task scales up.  if you need to tie together multiple documents or copy 50 links from a webpage, you can script it in less time
  - GUIs are highly unscriptable - they need human interaction

Consumer software is usually designed with ease of use in mind, a minimum learning curve.  Unix software emphasizes transparency and expressiveness, even if it’s more difficult to learn. Unix tools make less assumptions about end use cases.  Policy belongs to the user.

#### Thoughts on Different Languages

C and C++ have been the heavyweight, badass languages for decades.  Fortran and COBOL are mentioned as 2nd, 3rd niches.  However, while C and C++ optimize for performance, they do so at the expense of implementation and debugging time.  As processors and disks get faster, this becomes less and less of an issue.  Now we want to write programs quicker with less bugs, and have them be more maintainable/readable over time.  This is the same revolution that made C/C++ more favorable than assembly programming years ago!
\\
\\
In particular, memory management, is a huge burden in C and C++.  Memory leaks, buffer overflows, dangling pointers - they cause all kinds of bugs, crashes, security breaches and most of all, programmer time.
\\
\\
Languages that don’t make you do memory management, have a memory manager built into their runtime executable somewhere.
\\
\\
The shell is the unix interpreter.
\\
\\
Mixing languages is knowledge-intensive instead of coding-intensive.
\\
\\
C is good for maximum speed, real-time requirements (?), and apps that are tightly coupled to the OS kernel.  Dig down low enough in the following languages, and you’ll find C.\\
C can be thought of as a “high-level assembler for the unix virtual machine.”\\
C is as close as you can get to the bare metal, while remaining portable.\\
Learning C can help you understand hardware-architecture levels.\\
\\
\\
C++ hit the scene in the 1980s.  It was built to be backwards compatible with C and that caused problems, writes the author.  In particular C++ doesn’t solve C’s memory management problems.  The author hates C++’s OO nature and “thick glue” resulting from it.
Often found in GUIs, toolkits and games where OO makes more sense.
\\
\\
C++ Tries to do too much, says the author, so nobody can really hold the language in their head and understand it.  It’s messy, big and un-Unixy.
\\
\\
Shell is great for prototyping, writing early versions of programs quick, on the fly, working out the algorithm.  Eventually speed may become an issue.  There can be portability problems b/c shell scripts make use of other programs that may be different or not present on other hosts.
\\
\\
Recommended reading is [The Unix Programming Environment](http://bin.sc/Teaching/2014/JavaScript/resources/The%20Unix%20Programming%20Environment.pdf)
\\
\\
“Perl is Shell on Steroids.”  Great for pattern-matching, line-reading and has data structures likes hashes and dictionaries.  It’s more difficult to learn than shell and gets messy, hard to maintain when it’s big.
\\
\\
TCL (Tool Command Language) is a scripting language interpreter designed to work with C libraries.  Essentially, you can script C wth it.  Includes a popular toolkit.  Flexible and simple.  Doesn’t scale well, is tricky to debug, has syntax that takes some getting used.
\\
\\
Python is a scripting language designed to be closely integrated with C.  Can import and export data to dynamically loaded C libraries.  Can be called as a script from C too.
\\
\\
Jython allows similar mixins with Java.
Allows for OO or procedural programming.
Ships with tk toolkit for building interfaces.
Slower than C/C++ but that’s usually not an issue and you can combine with C code for the bottlenecks, if necessary
\\
\\
Java is similar to C++ but smaller, perhaps easier to understand.  Also it has automatic memory management aka garbage collection.  Slower than C and C++ of course.  More complex than Python.  Was created to write-once-run-anywhere anywhere and be Java is very portable.  It also has good networking capabilities built in.
\\
\\
Emacs Lisp is a scripting language used to program the behavior of Emacs.  It’s kind of an interactive Perl-Pattern-Matching to Batch-Editing tool.
\\
\\

