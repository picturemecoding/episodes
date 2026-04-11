# Why Should I Use Rust?

**Episode Type:** Topic Episode
**Hosts:** Mike + co-host
**Date:** 2025-01-01

---

## Episode Summary

---

Topic 1: High on Fire “Cometh the Storm”

First question: Mike, do I need to know a compiled language?

- Why would I need a compiled language tool in my toolbox?
- Does it depend on the kind of work I’m doing or not (Web? Data? Embedded? Games?)
- Let’s focus on web + database + data-munging: do I still need to know a compiled language in this case?
- What candidates would you consider for others (like if you’re reviewing resumes) in this category: is it *any* compiled language?

## Why Rust?

- Compiled (unlike Python), straight to machine code (no VM, like Java)
  + Talk about LLVM (rustc is a frontend which compiles to LLVM IR)
- Fast, approaching C
  + “Speed and safety” from the [rerun blog](https://www.rerun.io/blog/why-rust):
    - “We've had fast languages like C and C++, and then we've had safe languages like Lisp, Java, and Python. The safe languages were all slower. Common wisdom said that a programming language could either be fast or safe, but not both. Rust has thoroughly disproved this, with speeds rivaling C even when writing safe Rust.”
- Good (static) type system
  + Sum types (ADTs)
  + Pattern matching
  + Generics
  + Traits
  + Variables immutable by default
  + Hindley-Milner inference
  + Correct by design programs (leveraging the type system and the borrow checker). Making illegal states unrepresentable.
- Memory safety w/o GC
  + Carol Nichols (core contributor, author of rust book):
    - “Instead of having a janitor walk around a party and ask ‘are you done with that? Are you done with that?’ Whereas if you bring your own dish to the potluck, you have to wash it and bring it home at the end of the night. The compromise is that you are responsible… And the compiler is responsible for enforcing this.”
  + Crazy talk but I find it *beneficial* to think about the memory my programs are using, to be economical in my usage of memory.
    - Sometimes it takes a while to get a feel for this. Just yesterday I was reviewing a PR and there was a clone followed by a reference to an attribute on the cloned object.
  + I found that I vastly preferred “owned” data and references: I actually *like* thinking about the data in my program in this way.
    - Contrast with Python (interview question I got many years ago about call-by-reference and call-by-value)
  + Yehuda Katz: “Rust does something everyone said was impossible when I started programming; being able to write tens of thousands of lines of safe code without the performance costs of garbage collection.”
  + What we would say to a programmer who says, “Well, I don’t care about garbage collection. Why should I care?”
  + Carol Nichols, gave an [ACM tech talk](https://learning.acm.org/techtalks/rust) where she talks about the history of trains. It was super dangerous to work on trains in the 19th century and she talks about the invention of air brakes, a safety improvement for trains which she compares to C. She says that the software industry can be arrogant in thinking it’s better than other industries, so we should learn from the history of trains. Nichols calls it “a systems language designed for the next 40 years.”
- Tooling is incredible
  + Rustup
  + Cargo
  + Clippy & Rustfmt
  + Rust-analyzer (with VSCode, the “Quick fix” thing is beautiful)
- High quality libraries and applications:
  + Ripgrep
  + Btop
  + Rayon
  + Tokio
- Concurrency features
  + Assisted by memory safety features
  + Assisted by the type system (Sync and Send)
  + Message passing
  + Fearless multithreading is nuts (example: one of my first multithreaded python programs). Deadlocks still possible. Resource starvation. Holding locks too long. Memory is safe.
- Full-featured, general purpose
  + It’s got a lot of what we would call “high level” (iter().map(...).filter(...).flatten(...))
  + It doesn’t feel like you’re sacrificing any contemporary “modern”-feeling paradigms to write code in Rust. It feels like the language of the future, in fact, potentially ironically.
- “Zero cost” abstraction:
  + Where? How do you mean this?
    - I infer it means abstraction where you only pay a penalty at compile-time (generics, maybe?)
  + Carol Nichols: I would like to hold less information in my head. I am incapable of holding all the things. Compiler helps me.

## Why \_not\_ Rust?

- Hard to learn (depends a bit on where you start)
- Not good for exploratory or prototypingwork
- Compiles slowly compared to, say, C or Go
- The complexity tradeoff isn’t worth it.
- The memory model can make things like double-linked lists, trees, or graphs harder data structures to write.
- Not enough libraries
- Learning how to *use* libraries can be tricky: you often get the most mileage of reading their code!
- I often feel like manual memory management is “a lot to justify” for simple applications.
- All the emphasis on “systems programming” makes me think I’m in the wrong room.
  + If it’s the obvious choice for systems programming, then maybe it’s too much effort when *not* doing that?

## How would you prioritize the benefits of Rust?

One thing that happens a lot when Rust experts talk about the language is they talk about “systems language” and memory safety, and lots of people say, “I don’t care about these things” or “I don’t have these problems”, so they don’t see any argument to using Rust.

For my part, I appreciate four things about the language, but these are not necessarily things:

1. Type system & Fast (Tied for first)
2. Tooling
3. Features/paradigms in the language (it’s nice to write)

Second question: we like rust. People give reasons for using it and I don’t often prioritize *those* reasons. Am I using it for the wrong reasons?

## Other Benefits

Probably not convincing or worthwhile arguments for other people, but potentially interesting:

- You combine all of the above benefits and I can spend more time up front building a thing, then leave it alone for a year, and come back and make major changes or fearlessly bump dependencies.
  + The other day I was coaching someone on their first contribution to a rust project.
  + We spent about 30 minutes getting the project going and then modifying some existing data definitions to represent the new logic we wanted to support.
  + After that, I said, “Okay, now you get to fix all the compiler errors from our changes to these data definitions.”
  + And that person went off and made the changes required. I had no concerns, and they didn’t come back to me asking for any help until they needed a PR review. They were guided by the compiler and able to contribute. Afterwards that person wanted to do a lot more rust.
- It’s probably already baked into your favorite language!
  + Python (orjson, pydantic, cryptography, others)
  + Javascript (parcel, deno, and others)
- Large companies adopting it en masse:
  + Mozilla with Firefox
  + Microsoft
  + AWS
  + Cloudflare
  + Dropbox

## Languages that Require “Effort” to Learn and to Use

- Contrast with Haskell
  + Type astronauting
- Contrast with Go
  + Pretty dumb all the time
- Contrast with Java
  + Null and void
  + From the StackOverflow piece:
    - “This isn't to say that all static type systems are equivalent. Many statically-typed languages have a large asterisk next to them: they allow for the concept of NULL. This means any value may be what it says or nothing, effectively creating a second possible type for every type. Like Haskell and some other modern programming languages, Rust encodes this possibility using an optional type, and the compiler requires you to handle the None case”
- How do we justify the effort?

Coolness factor:

I think an interesting point of discussion is that Rust seems to have a sort of “coolness” factor that makes it appeal to certain nerds, irrespective of the language features. Is it because of the connection to functional programming? Is it *because* it’s kind of hard to learn? Maybe because it came out of Mozilla? It reminds me a bit of the buzz around Java in the mid-90s, although obviously the languages are quite different.

Backlash to that?

One of GitHub’s staff software engineers, Jason Orendorff, who co-authored a book on programming with Rust, said about the language:

“To me, what’s great about Rust is that it’s both fast AND reliable,” according to Orendorff. “It lets me write multi-headed programs that run on 16 cores and keep them readable, maintainable, and crash-free. It also lets me write very low-level algorithms requiring control over memory layout and pull in a crate that makes HTTPS requests super simple. It’s the combination of these features that makes Rust so unique.”

John Gjengset, author of Rust for Rustaceans and Crust of Rust video series, from an [interview](https://nostarch.com/blog/software-engineer-jon-gjengset-gets-nitty-gritty-rust):

If you’re writing code in Python there are a whole host of problems the language lets you get away with not thinking about – that is, until they come back to bite you later. Whether that comes in the form of bugs due to dynamic typing, concurrency issues that only crop up during heavy load, or performance issues due to lack of careful memory management, you’re doing reactivedevelopment. You build something that kind of works first, and then go round and round fixing issues as you discover them.

Rust is different because it forces you to be more proactive. An apt quote from RustConf this year was that Rust “gives you the hangover first” – as a developer you’re forced to make explicit decisions about your program’s runtime behavior, and you’re forced to ensure that fairly large classes of bugs do not exist in your program, all before the compiler will accept your source code as valid. And that’s something developers need to learn, along with the associated skill of debugging at compile time as opposed to at runtime, as they do in other languages.

It’s that change to the development process that causes much of (though not all of) Rust’s steeper learning curve. And it’s a very real and non-trivial lesson to learn. I also suspect it’ll be a hugely valuable lesson going forward, with the industry’s increased focus on guaranteed correctness through things like formal verification, which only pushes the developer experience further in this direction. Not to mention that the lessons you pick up often translate back into other languages. When I now write code in Java, for instance, I am much more cognizant of the correctness and performance implications of that code because Rust has, in a sense, taught me how to reason better about those aspects of code.

## Articles and Resources

- [Why Rust is the most admired language among developers - The GitHub Blog](https://github.blog/2023-08-30-why-rust-is-the-most-admired-language-among-developers/)
- [Why Rust? — Rerun](https://www.rerun.io/blog/why-rust)
- [Speed of Rust vs C](https://kornel.ski/rust-c-speed)
- [What is Rust and Why Is it So Popular? – Stack Overflow](https://stackoverflow.blog/2020/01/20/what-is-rust-and-why-is-it-so-popular/)

## Signal Discussion

Mike: There are some cases where i think it's a fairly obvious choice: basically anything where you would have used C before is a good candidate for Rust. Eg, networking tools, some Linux kernel stuff, maybe embedded software. We've used it for some web services where latency is a significant concern.

Erik: I like the idea of making illegal states unrepresentable too, of using the type system to make it more obvious that the code is correct

Mike: I have a hard time imagining using it for, say, data science. While i think it has a lot of virtues as a language (eg the type system), i'd have a hard time giving up the ability to "experiment" like i can in Python.

My current take is that as a professional software engineer, i need to know one compiled language well, and i have no motivation to do C. So i want to get reasonably strong in Rust as my compiled option

Erik: I agree with that. I need to have the \*ability\* to deploy something significantly fast, close to the limits of what's possible speedwise. Rust is a good candidate for python interop, I think, but it's also a good language

Mike: There are types of software where you really \_need\_ to get close to the metal. For a long time the choice there was C, but it has the well-known issues with memory (buffer overflows, stack overflows)

Rust hopes to provide memory safety with C-like performance, along with other correctness guardrails.

So, for instance, if i were going to write a device driver for my new IoT device, I'd want a compiled language, and now i'd choose Rust.

I think there are other scenarios, like if i were to write my own queuing system i might use Rust (although RabbitMQ is in Erlang so, who knows)

Erik: I think similarly, if I'm writing a distributed system where speed and safety are important, I'd pick rust. If I were writing bank account software, I'd pick rust. There are examples where I feel less confident shipping python based on what's going to be asked of the app.

There's often some old software engineering elitism in here. Some engineers I've met think you can't do serious software in Python. It has to be Java or C. But if I'm writing some network service which doesn't see massive volume and it just needs to talk to some networked resources, then Python is probably fine