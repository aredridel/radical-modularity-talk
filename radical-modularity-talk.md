# Radical Modularity

^ This is going to be a talk full of ideas, and not all of them are ones that can just be used out of the box.

----

## What is a module?

^ I'm actually going to spend some time on this one because while it's an everyday word in our industry, it's one we don't often hear defined.

^ I want you to think about what it means to make software modular.

----

## What is a module?

###  A bit of software that has an interface defined between it and the rest of the system.

^ This is one of the simplest definitions I could come up with.

^ There are some implications here: There's a separation between the module and the rest of the system. I'm not saying how far, but it's actually a thing.

^ We can talk about the interface and I'm going to get to that later, but there's a lot of kinds of interfaces to software.

^ The bit I think is really interesting is the word _defined_. This means we've made decisions in making a module -- just extracting something blindly into a separate file probably only counts on a technicality. Defining something is a bit intentional.

----

## What is a module?

### A piece of software with a defined interface and a name that can be shared with or exposed to others.

^ I'm a programmer working for a package manager company, and I think of code as art, and I've been making open source my entire professional life, so I've also got a particular bunch of things I also mean when I say module. 

^ I won't advocate sharing everything,  since I'm talking about radical modularity and not radical sharing here, but I want the option. The rest, though, are where things get interesting. In particular, I want to talk about names.

^ When we name something, it takes on a life of its own. It's now an object in its own right. This happens when we name a file, it happens when we name a package. A name is the handle we can grab onto something with mentally and start treating it independently.

^ A defined interface is the first step of independence. It's the boundary that gives a thing a separate internal life and external life. Things outside a module get a relationship with the boundary, and inside the module, anything not exposed by the boundary can be re-arranged and edited without changing those relationships.

----

## Small Modules and Big Modules

>  I named her. The power of a name. That's old magic. 
>  —Tenth Doctor, “The Shakespeare Code”

^ Not every module even gets published or becomes a package on a package registry like npm or crates. We usually push things to GitHub early, but source control isn't quite the same thing as publishing things for others to use. Just separating things into a separate file — there's the naming — and choosing what constitutes the interface to the rest of the system is modularizing.

^  We can commit to names more firmly by publishing and giving version numbers, and breathe life into something as a fully separate entity, but that's not required, and that alone isn't often enough to make a whole project. 


^ Self-sustaining open source projects have to be bigger than tiny modules, and so you can either enlarge modules until they become self-sustaining, or your project is a group of related modules, like Hoodie, where there are a bunch of small, named parts.

----

## Small Modules and Big Modules

> I named her. The power of a name. That's old magic. 
> —Tenth Doctor, “The Shakespeare Code”

^ There's another option, which is to make modules so small they trend toward finished, done, names for a now-static thing. Maybe they are bestowed upon someone who tidies them up, finishes a few pieces that we left ragged, maybe just left in a little library box for someone else to discover. Maybe they're published, maybe they're widely used and loved, maybe not. Maybe they end up in a scrap heap for our later selves or others to build something new from.

----

## Small Modules and Big Modules

> Art does not reproduce what is visible; it makes things visible.
> —Paul Klee

^  Something the open source movement did that isn't all that widely acknowledged is make a huge ecosystem for the performance of software as a social art.

^  Not only that, but since then, the explosion of social openness in the creation of software has created a new, orthogonal movement less concerned with copyright law and open engineering, but open sharing of knowledge and techniques, and as a side effect of that and the rise of the app, software engineering now includes the practice of software as art and craft..

----

## Small Modules and Big Modules

> Art does not reproduce what is visible; it makes things visible.
> —Paul Klee

^ I practice code as an art.

^ A good portion of that is making concepts visible and ultimately that often means making it named. With art, though, there's some tension with engineering: sometimes we do things to show instability, to test a limit, or to reveal the tensions within our culture or systems we build. We can create a module of code only to abandon it once it‘s served its cultural purpose — be it connecting two things together, mashup style, or just moving on because there's a better way to do things.

----

## Small Modules and Big Modules

> Art does not reproduce what is visible; it makes things visible.
> —Paul Klee

^ One of the interesting differences about software artifacts as a medium of art contrasted with other fine arts is that despite working in a very definite medium, though abstract, much of what we make is never finished. It exists in our culture — yes, software creation is a reflection of our culture, and a culture of its own — and as web workers, especially working as artists, a lot of what we create straddles the lines of engineering, fine art, performance art, and craft.

----

## Small Modules and Big Modules

^ And so it goes that even the biggest pieces of software are made up of smaller parts. It's the natural way to approach problems and make them solvable. A large module is nothing more a collective name for little modules that may not have their full names and final forms.

^ I really like small modules as a norm, because I think of things in terms of named objects. I'm happy to abandon a thing I think no longer suits, and it's easier to abandon a small module than a big one. They approach done, so I'm happy to use a three year old tiny module, but a big project that's three years unmaintained is likely to be bug-ridden and poorly integrated.

----

## What if we make _everything_ a module? 

^ This is what I want to ask you today. What happens when we break off pieces and name them well and when we can, publish them, share those names and let others wield that power over them? What does this do to our culture as programmers?

-----

## Naming & publishing things

^ This is where I'm going to get practical, of a sort. I talk about npm a lot, but this can be extended: open your mind and projects and think about making interfaces around new things.

----

## CSS

^ When I started at npm we had a monolith of old CSS built up, like most web projects start accreting -- a lot of styles we weren't positive were unused, a lot of pieces and parts that depended on each other. 

^ Since then, and huge shout-out to Nicole Sullivan and her huge body of work on this — seriously, go watch her talk or read "Our best practices are killing us" — we've started tearing apart the site and rebuilding it with, get this, modules of CSS, with defined interfaces between them and the rest of the system.

^ They all have names — package names — and versions. So we'll have a module like `@npmcorp/pui-css-tables` (PUI is because we forked this system from a component system used at Pivotal Labs)

------

## CSS

### Dr. Frankenstyle

^ In this case we're using a tool called `dr-frankenstyle`. It's pretty simple. It looks at all the node modules installed in our web project and then concatenates any CSS they export with a style property in the package metadata, in dependency order.

^ This means our CSS actually has dependencies annotated into it, in the package metadata, and it's in pieces and parts. Because of this, and because it's named, we can start grappling with these things individually, and start making sense of what otherwise becomes a huge mess.

----

## CSS

### atomify-css

^ There's another project called `atomify-css` that can do similar things, and both of these systems will do one set of fixups as they build a final CSS stylesheet: they identify assets that those stylesheets refer to, and copy those over and adjust the path to work in the new context.

^ This turns out to be super powerful, because now it leads us into wanting to modularize and make explicit the dependencies between all the things.

^ Now, CSS has some pitfalls: browsers still see all of a page's CSS as a single namespace, a single heap of rules to apply. This isn't a clean interface, so modularizing everything doesn't automatically solve all your problems. It might give some new tools though...

----

## SVG

```html
<svg>
  <use xlink:href="./wonky.svg#camera-lens"/>
</svg>
```

^ So what happens if we put SVG files into packages? What's the interface to an SVG? The text? Parsed XML? Just the file name?

----

SVG

```html
<svg>
  <use xlink:href="@stoppard/hound/props/chocolates.svg#chateu-neuf-de-pape"/>
  <use xlink:href="./wonky.svg#camera-lens"/>
</svg>
```

^ We've got dependencies. SVG files can load other SVG files. `xlink` attributes could be followed, and postprocessing tools could inline those, making production-ready and browser-ready SVGs from more modular ones.

^ Now that we have HTML5 support in browsers, we can embed SVG directly into HTML, too.

----

## Templates & Helpers

^ We don't just build raw HTML anymore, but we use templating systems to break those apart. What if we published those as packages for others to consume, what if we made them modules?

^ In the process of reworking npm's website, we had components of CSS whose interface is the HTML required to invoke them: A media block has a left or right bit of image, and then some text alongside. A more sophisticated component might string several of those together, add a heading banner and some icons. The HTML required was getting complex and fragile, so that any change to the component would require the consumer to update the HTML to match. Icons were inlined, so changing an icon would mean editing a large blob of SVG.

^  In semantic versioning terms, every change became a major. While integers are free, time to check what's needed to update isn't, so this wasn't going to be a scalable approach.

----

## Templates & Helpers

```html
<div class="a-component"> 
  <div class="media-block">
    <div class="media-left">
      <svg> ... icon here </svg> ...
    </div>
  </div>
</div>
```

becomes

```handlebars
{{fromPackage "@a-team/pity-the-media" .}}
```

^ We started moving the handlebars templates we use that have the HTML to invoke a component on our website into the modules. This moved the interface boundary into something more stable. Now we can change what that component needs and does as the design evolves without having to go propagate those changes to an unknown number of callers.

-----

## Templates & Helpers

^ You'll remember I mentioned SVG icons. It turns out that inlining small icons is one of the most efficient ways to use them, but it doesn't scale very well in the development process. The alternatives, icon fonts, require a lot of infrastructure and are brittle enough that it stifles the act of moving things into a module. Icons have to be in large groups with that approach, and that trends toward very large modules, and probably to less efficient ways to do things.

----

## Templates & Helpers

^ What I ended up doing was making a small handlebars helper like the `fromPackage` helper I just showed the call to, and made a couple helpers for loading SVG from packages. Called from our handlebars templates, a single helper invocation can load and parse SVG from a package, do simple optimizations, and cache the result, and inline it. SVGs, too, then, became modules we can publish separately or in small groups.

----

## React Changed Everything

^ There is a reason that React talks have been so popular for the last couple years. It really did make a radical shift in how we design frameworks, and more importantly to me, it helped give components better interfaces. Stateless components have well defined inputs and outputs. Side effects are reduced or eliminated. This means modules can more easily declare their dependencies and give simple interfaces.

^ This also means that React components fit into packages really neatly, and automatically give an interface that's like a function call.

^ If you're a react programmer, you'll probably recognize my `fromPackage` helper as very similar to node's `require`, which is how most of us use React these days, as webpacked or browerified modules.

----

## React Changed Everything

### What can we steal from React?

^ That modularity and clear boundary on interfaces changed so much. Let's re-think how we integrate things to have interfaces that simple and clean.

^ There's been a lot of experiments, too, on having react components automatically namespace CSS they require, and then emitting HTML that uses the namespaced version. By moving the module boundary from the raw CSS to something that gets called, an active process, CSS namespacing woes can be solved by separating what the humans type from what the browser interprets a little bit.

^ How radically can we change the complexity of an API by changing what kind of thing we export?

----

## What else can be a module?

^ At PayPal, I did work to make translations be separately loadable things, which leads rapidly into separating those pieces into entirely separate packages with their own maintenance lifecycle. When you have a separate team working on something, having a clean boundary can be a great way to let work progress at a more independent pace. What else can we modularize?

----

## What else can be a module?

- Language files
- Entire websites
- License texts
- JSON Schemas
- [Lists of legal terms that need special callouts](https://www.npmjs.com/package/american-legal-homonyms)

^ [ read the bullet points ]

^ We can even make modules that are nothing but known good configurations of other modules, combined and tested.

^ That last one is really interesting. Kyle Mitchell is a lawyer who uses a lot of software in his work to draft legal texts. In so doing, he's published a lot of tiny modules of interesting stuff. Mostly they're JSON files, cleanly licensed and versioned, or small tools for assembling legal language out of smaller pieces, re-using tested and tried phrasings of things. Sounds familiar, right?

^ Text itself can be a module with an interface, even if that interface is concatenation of a bit of text.

----

##  Making Modularity

^ Hands on!

^ A lot of this is going to be specific to node, but I like node not just because it's JavaScript and I think JavaScript is a lot of fun, but because its dependency model is actually pretty unique. That's actually a lot of what drove me to node in the first place.

^ The underappreciated feature of node modules is that they nest — this really bugs windows users since their tools can't deal with deep paths — and this means that we can have a module get a known version of something, defined entirely on its terms, which means that what a package depends on is either a less-important part of or not even a part of the interface to a module. We spend a lot less time wrangling which versions of things all have to be available at once, and we can start putting dependencies behind the boundary that a module defines.

----

## Making Modularity

### 	@substack's `resolve` module

```javascript
resolve.sync('@mad-science/luncheon-meats/baloney.svg')
```

give me the file `baloney.svg` from the `@mad-science/luncheon-meats` package

^ This is a really simple module that implements node's module lookup strategy for files that aren't javascript. You can say "give me the file `baloney.svg` from the `@mad-science/luncheon-meats` package", and it will find it, no matter where it got installed into the tree — remember node modules let you share implementations if two things require compatible versions of a module — and we name the file, in this case the actual interface of this hypothetical module is to just read the file once you figure out where it is.

^ That's our primitive building block. I like this one because it matches how everything else in the runtime I use most works.

----

## Making Modularity

### Resolving paths & packages

^ There's another thing that's common to do: Add a new field to package.json. Dr. Frankenstyle uses the property `style` rather than `main` to say which file is the entry point to the package. This means that modules can do dual-duty: grouping different aspects of a thing together into a single component, rather than making the caller assemble the pieces when the pieces all go together anyway.

----

## Making Modularity

### How many guarantees can we make?

* Dependencies isolated
* Deduplication
* Local paths are relative to the file
* Single entry point

^ [read the bullets]

----

## Making Modularity

### Guarantees

^ One of the guarantees I love most is that local paths in node modules are relative to the file. This is one of the ways that make it possible to break things into modules without breaking down the interface they had as a monolithic unit. It really makes me sad that most templating languages don't maintain filenames deep enough into their internals to implement this. It's good for source mapping and it's good for modularity.

^ A lot of people fought this -- they keep fighting this in node development, but I think it exposes how people think about modularity: This is a symptom of making the whole project a single module and giving the components just enough name to navigate but not enough that they can live on their own.

----

## Making Modularity

### Guarantees

^ I keep building similar models. I often make a path rewrites for resources, so that things relative to the file where it's actually stored will work when loaded into a new context where it's used. Sometimes that's inlining. Sometimes that's copying modules into a destination and making sure their assets come along for the ride.

^ This is replicating the guarantees of node's module system, because they give me some flexibility and durability in what I make. If things have their own namespaces, their own dependencies, then I can break them less often or not at all.

----

## Going Meta

^ If everything's a module, what can we do with that?

^ Have we simplified things enough to start giving our programs the vocabulary to start extending themselves? Can we start talking about constructing programs out of larger building blocks, even if they're sometimes special purpose?

^ Can modules or remotely loaded packages be first-class objects in our programs?

^ What about generating new modules in the course of using our programs, and letting our users share them?



----

## Going Meta

### Kinds of Interfaces



^ What other kinds of interfaces can we give. Web services? Data sets with guarantees about how they'll develop in the future?

^ How radically can we simplify the interface of something?

-----

## Going Meta

### Simplicity & Durability

^ One of the most influential concepts in my career was that phrase a lot of us have heard about UNIX: everything is a file.

^ Now, that's a damn lie. There's a lot of things in unix that aren't files at all. IPC. System calls. Locks. Lots of things can be file descriptors, like sockets, but if you want to see more things shoehorned into that interface, you could go install Plan 9, but there's not very much software out there for Plan 9.

----

## Going Meta

### Simplicity & Durability

^ Even so, UNIX took off in a huge way thanks to a bunch of factors, and even Plan 9 and Inferno and systems derived from it have this really outsized longevity in our minds because of one thing:

^ They simplfied their interfaces. Radically.

^ They defined their interfaces so simply that you can sum them up in a few words.

^ "Text file. Delimited by colons."

^ "Line delimited log entries."

^ "Just a plain file, a sequence of bytes"

^ These are super durable primitives. They had all their edge cases shaved off. No record sizing to write a file no special knowledge of what bytes were allowed or not. Very few things imbued with special meaning.

^ This means these systems last because they give us building blocks to build better things out of.

----

## Going Meta

### Simplicity & Durability

^ I love to pick on unix because for all its ancient cruft there's an elegant system inside. It's not the only super simple interface that really took off either.

^ Chris Neukirchen made the `rack` library for ruby, and little did they know but it suddenly got adopted by all the frameworks and all the servers because at the core of it, a web request got simplified down to a single function call: environment in, response code, headers, and body out. It's adapted from the Python WSGI but it was a great distillation of the concepts.

^ node modules also have this ridiculously simple interface. They get wrapped in a function with five parameters, and are provided a place to put their exports and a require function for their own use. It turned out to be a great thing to build even more complicated module systems out of.

^ node streams, too. By making them pretty generic, it turns out that thousands and thousands of packages all use the shared interface and all work together.

^ It's really worthwhile asking yourself if there's a radically simplified, generic interface that your module is begging for. 

----

# Thank you

### my interface is @aredridel on Twitter

