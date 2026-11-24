# Window Programming
Pre-Mid Semester Exam Mockup – Answers
Part 1: Software Development, Methodologies & Principles (Chapter 1)
1. Choosing the Right SDLC Model
Question: Which SDLC model(s) would you recommend for (A) flight-control software for a commercial aircraft and (B) a food-delivery mobile app for a startup in a rapidly changing market? How do requirements stability, risk tolerance, and the timing of testing influence the decision?
•	(A) Flight control: Waterfall or Spiral. Requirements are fixed and well understood, risk tolerance is near zero, and failure is catastrophic. Waterfall provides formal specifications and documentation. Spiral adds risk analysis in every loop.
•	Testing timing: Waterfall tests late, so safety-critical projects need heavy verification and validation. Agile tests continuously within each iteration.
2. The Hidden Trade-offs of Rapid Application Development
Question: Why does skipping extensive upfront planning and relying on rapid prototyping create scalability or reliability problems later on?
•	Skipping upfront planning means there is no overall architecture.
•	Prototypes often evolve into the real product, which builds up architectural debt: tight coupling and shortcuts never designed to scale.
•	Time-boxed delivery cuts testing and deep risk analysis, so reliability gaps appear under heavy load.
•	Large or mission-critical systems need high availability and a solid design. Fixing the architecture later is very costly.
3. Balancing Software Development Principles
Question: Can trying too hard to follow DRY or Separation of Concerns violate KISS or YAGNI? Give an example of how a developer might “over-engineer” a simple program.
Yes. These principles can conflict, and over-applying DRY or Separation of Concerns produces complexity the problem does not need.
•	Example: a console calculator built with an abstract factory, a service layer, a repository, interfaces with a single implementation each, and a plugin system for operations nobody asked for.
•	Another example: merging two similar-looking but unrelated code blocks into one abstraction full of flags, just to avoid repetition.
The skill is judging when a principle stops paying off. Build what is needed now, keep it simple, and refactor when a real need appears.
4. The Economics of Testing and Code Quality
Question: Why is the cost of correcting an error lowest in the early stages of the SDLC and highest after deployment? How do TDD and code reviews help keep that cost low?
•	Defects compound. A requirements error found early costs a sentence to fix. In design it costs a redesign, in code a rewrite, and after deployment downtime, patches, data damage, retesting, redeployment and lost customers.
•	TDD: tests are written first, so each unit is verified immediately, and the tests act as a regression safety net.
•	Continuous Integration: every commit is built and tested automatically, so integration bugs surface within minutes.
•	Code reviews: a second person catches logic and design flaws before they spread, and knowledge is shared across the team.
Part 2: .NET Architecture, C#, and Window Programming (Chapter 2)
5. MSIL, JIT, and the Platform Independence Paradox
Question: If MSIL is CPU-independent, why are traditional .NET Framework applications platform-dependent on Windows? How did technologies like Mono change that?
•	MSIL itself is CPU-independent, but running it needs the CLR and the class libraries (WinForms, Win32 calls, registry). In the traditional .NET Framework these existed only for Windows.
•	The JIT compiler turns MSIL into native code for the local CPU, but only inside that Windows-specific runtime.
•	Mono is a third-party, open-source implementation of the CLR and libraries for Linux, macOS and other systems, which made .NET code cross-platform.
6. Managed vs. Unmanaged Code & Language Interoperability
Question: How does Managed Code running inside the CLR protect a program from the reliability and safety problems of Unmanaged (Win32) Code? How do MSIL and the CTS make it possible to write one part of an application in C# and another in VB.NET without data conflicts?
•	Managed code runs under the CLR, which provides garbage collection (no leaks or dangling pointers), type safety, array bounds checking (prevents buffer overflows) and metadata verification.
•	Unmanaged Win32 code relies on manual memory management and raw pointers, so it is exposed to leaks, overflows and crashes.
•	Interoperability: all .NET languages compile to MSIL with metadata. The CTS defines the shared types, so C# string and VB.NET String both map to System.String, which derives from System.Object. Data passes between languages without conflicts.
7. Solving “DLL Hell”: Assemblies, Manifests, and Namespaces
Question: How do .NET Assemblies, Manifests/Metadata, Strong Names, and the GAC solve DLL Hell? Why is it incorrect to use the terms Namespace and Assembly interchangeably?
•	The problem: installing one application could overwrite a shared DLL in System32 with an incompatible version and break another application.
•	Assemblies (.exe/.dll) are self-describing. The manifest/metadata lists name, version, culture and dependencies, so no registry entries are needed.
•	Private assemblies sit in the application's own folder, so applications are isolated from each other.
•	Shared assemblies go in the GAC and need a strong name (name, version, culture, public key token and digital signature). This makes them unique and tamper-proof, and several versions can run side by side. Each application binds to the version it was built with.
•	Namespace vs. assembly: a namespace is a logical grouping of classes, and an assembly is a physical unit of deployment. One assembly can hold many namespaces, and one namespace can span several assemblies.
8. Abstraction in Data Access: ADO.NET vs. Entity Framework & LINQ
Question: Why was working with raw DataSets and manual database connections considered error-prone, and what do developers gain (or trade off) by working at the higher level of abstraction provided by Entity Framework?
•	ADO.NET problems: you open and close connections manually, write SQL as strings, and convert untyped DataSet rows into objects with casts and column-name strings. Typos and SQL injection show up at runtime, not compile time.
•	Entity Framework / LINQ gains: tables map to classes and rows to objects automatically. Queries are strongly typed with compile-time checking and IntelliSense. LINQ gives one syntax for querying SQL, XML and in-memory collections.
•	Trade-offs: less control, sometimes inefficient generated SQL, some performance overhead, a learning curve, and complex queries that are harder to tune.
9. Under the Hood of Event-Driven Programming
Question: Trace what happens from the moment a user clicks a Button on a form to the moment the code runs, connecting the main loop, events, event listeners, delegates/lambda expressions, and event handlers.
A console app runs sequentially from Main() until it exits. A Windows Forms app instead waits for the user:
1.	Main() calls Application.Run(new Form1()), which starts the message loop (main loop).
2.	InitializeComponent() creates the controls and subscribes the handler, for example button1.Click += button1_Click;. This adds a delegate to the event's invocation list. A lambda expression such as (s, e) => ... can be used instead.
3.	The application now sits idle, waiting for messages.
4.	The user clicks, and the operating system places a mouse message in the application's message queue.
5.	The loop retrieves the message and dispatches it to the Button, which raises its Click event. The Button is the event source and the handler is the event listener.
6.	The event invokes every delegate registered for it, so the event handler button1_Click(object sender, EventArgs e) runs on the UI thread.
7.	The handler returns and the loop goes back to waiting for the next message.

