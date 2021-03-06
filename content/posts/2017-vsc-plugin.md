+++
draft = false
date = "2017-03-15"
title = "Extending Visual Studio Code"
author = "Adam Bretz"
categories = ["javascript", "vsc"]
discourseUsername = "arb"
heroShot = "https://blog.stardog.com/img/vsc1.jpg"
+++

Working with Stardog should be easy and fun, no matter what your tool chain
looks like. So I'm building extensions in Visual Studio Code for Stardog, and
I'm kinda lovin' it. <!--more-->

## Introduction

When most people think of Microsoft they think of the Microsoft of old;
monoliths, vendor lock-in, "the Microsoft way", and stellar developer tooling.
Thankfully, with the recent changes in leadership, the only thing not fading
away is stellar developer tooling. Enter Visual Studio Code (_VSC_). Same high
quality tool as it's elder brother, but open source, free, and infinitely
customizable and extensible.

{{% figure src="/img/vsc1.jpg" link="https://www.flickr.com/photos/loozrboy/3119183103/in/photolist-5KCCt2-8AA9Fq-5BBkpY-8Ax4ZZ-S6DSAS-RC7Fqz-9nj1Uk-6dGeKi-SVdJPk-ojrwjj-e1LKVj-6AKKMs-6sc4eG-rtA1BY-RYwSfN-6qxWCt-RJ4Rns-6qxTHP-SoZ3KZ-Spur1T-6sc3nw-6sc3QL-8hda9Q-6s7Xwe-qLK3CV-6s7WSe-nXPpwt-Ry85Xw-qyf2j7-SATkzN-dtGp46-7cGBV4-ojo93b-ooaY6p-7ZEobu-7GzvKP-7XpF4j-4VpoaX-6sc4ph-7XpEjJ-4VtAww-6AFBZT-7V84w5-6qd7zQ-7XpEgG-jyx7P9-4Xxi5f-2TqY4p-dfp8oV-7Xmso8" attr="Loozrboy" class="inliner" %}}

That customization comes from two architectural decisions made by the Microsoft
team. First, _VSC_ is an [Electron](https://electron.atom.io/) app. That means
it's built using web technology that is very well understood and known in the
community. It makes the barrier of entry almost non-existent. Second, the
design of Visual Studio Code embraces the concept of Extensions.

A Visual Studio Code extension is a plugin that, surprise, extends the
functionality of VSC. It can be something as simple as changing the colors of UI
elements, to something more complicated like implementing IntelliSense for a new
programming language and everything in between. Thanks to the use of existing
web technologies, these extensions can be written in JavaScript or any of the
"compiles to JavaScript" options out there.

In this blog post, I'm going to share some of my personal experiences I've had
while building my first VSC extension. This isn't going to be presented as a
"getting started" style article as Microsoft has several of
those
[already](https://code.visualstudio.com/docs/extensions/example-hello-world).
Just my own observations while working through my first extension.

## Documentation

The documentation manages to simultaneously be very good and not helpful at the
same time. There are complete references for all the available APIs to any
extension you write. There is comprehensive documentation for the different
activation lifecycle events you can tap into. There is also great documentation
on the additional options required in your `package.json` file. I really liked
how Microsoft resisted the temptation to use their own custom manifest file
format too.

However, the documentation falls flat in a few areas. First, all of it is
written assuming you will be using TypeScript style. This means that it is far
more verbose than most people are used to reading. While I realize TypeScript
isn't "that different", it's different enough to be distracting when you don't
know it already and are working in a new execution context.

{{% figure src="/img/vsc2.jpg" link="https://www.flickr.com/photos/infomatique/32861881496/in/photolist-S4TJtJ-ntbh1Z-RAEAQx-73eBCG-S3RSGn-d4mP5w-pN9eBW-iMUWNS-gmSwwe-e1HpQq-pyjWBN-r5cCop-GE5cuj-4yPwJg-7tcZTf-5gLy74-SDi6ed-nVbaCu-8Ud3Po-e1BbSx-UdJAu-9uWEmo-d6h887-SkGrkk-QPwaz9-S6XXGB-gtpvp8-Rwi7g1-UdJzS-9uTzdH-onmcQW-dnUEoq-iMUWL7-jRXdE6-64LRuf-nMc7j7-Rwi5KW-S6XZJ2-RSqNY1-UdJAY-8U9ZfF-RGW9d3-dUnZDe-9NnUjX-h8Xrj2-SDiaa9-m1QTTi-nSTk19-oW3LQq-73zALi" attr="William Murphy" class="inliner" %}}


Some documentation required to build extensions is not all organized together on
the site. As an example, extensions expose new functionality
via
[commands](https://code.visualstudio.com/docs/extensionAPI/extension-points#_contributescommands).
Commands show up in the command pallet and can be bound to UI elements as well.
There are also many built in commands available to extension authors. There is a
list of what has been dubbed "complex commands" listed on one page of the
extension authoring documentation and that was very helpful. A complex command
is a command you can execute that takes parameters and there are about thirty of
them. However, there are about a hundred other ones that don't take parameters
listed in a completely different part of the VSC webpage (it's under keyboard
customization).

On the plus side, the documentation that's where you would expect it to be is
well organized and clear. Also because everything is expressed as TypeScript,
the different types and interfaces are linked together with API calls to it's
easy to see what the input arguments should be. There are also a few sample
extensions that are helpful for seeing how you can lay out your own project. The
samples also show how some of the Visual Studio Code APIs can be composed to
perform more complicated actions and commands. As an example, if you want to
take some action and then render that result in an HTML panel,
the
[Preview HTML sample](https://github.com/Microsoft/vscode-extension-samples/tree/master/previewhtml-sample) is
a great place to start. It isn't immediately intuitive how to show HTML just by
reading the API, but that sample is a code-complete blueprint showing how to
render customized HTML.

## Configuration Centric

As I said previously, commands are essentially events that you can bind to UI
actions and list in the Visual Studio Code command pallet. Because extensions
and commands are first-class citizens in VSC, you can achieve quite a bit with
just configuring events in the `package.json` file of the extension. You can
control what these commands look like when rendered as an icon or menu item (for
both light and dark themes). You can control their ordering and positioning as
well, all via configuration options set in `package.json`. You do have to
register these commands with VSC so it knows what to do when these commands are
executed, but the bulk of the configuration can be done statically.

{{% figure src="/img/vsc3.jpg" link="https://www.flickr.com/photos/58621196@N05/7579155678/in/photolist-cxK9VU-aE796Z-9mSVjM-9URxky-r22z4C-oztCoD-aoMmRf-EJwgL3-FLHxLn-EptgHF-EVgp8W-9UX2Xh-oBkYsA-qoQ93o-acH2LR-oyVZ3A-oEetie-pau7oE-cgMZ3L-p5K9E3-ENiQPE-oeEeGn-Fttuvw-FShX7p-F1UZAq-ESssge-p4m2uo-ECS4va-EUaNWr-FFqWAV-FHuzai-oNi1Ur-oET2sw-oSfNHL-Frksyd-GxMMqJ-EsP1uH-EPADn1-EaVQyd-EXsuyJ-EYjGC8-EJCtZT-oTtj7Y-F22sSu-ao7rf8-FhfmyL-EHHKNc-E8UpZB-FfkoTb-FmW6Lv" attr="Brian Donovan" class="inliner" %}}

As with most configuration centric systems, there are times where it bites you
and you just want to write some code and move on. A place where this was
particularly frustrating was trying to control *when* my new commands were
available. The command I added reaches back to a database for information my
extension needs to work properly. In essence, I need the command to be disabled
until some code condition is true. There is a `when` option associated with
commands that can be used to control when a command is available or not. The
technical name
is
["when Clause Contexts"](https://code.visualstudio.com/docs/customization/keybindings#_when-clause-contexts).
If you look through that list, there isn't anything in there about when *my*
command logic is complete and ready for user interaction. What I needed was a
when clause context I could toggle as my extension was initializing.

Unless I completely missed it, this was not documented. After looking at source
and several different issues scattered throughout GitHub, I stumbled upon the
`setContext` command. This is yet another built in VSC command that isn't
documented anywhere but ended up being supremely useful. It essentially allows
plugin authors to create their own when clause contexts and control when it's
`true` and `false`. Ultimately, this allows developers to control when their new
commands are available, and when they're not.

## Writing Code

The actual implementation of the extension features was a joy to write. The
ecosystem and tooling is excellent in typical Microsoft fashion. All of the
built in APIs available in Visual Studio Code are all written in TypeScript so
IntelleSense works great during code editing. All of the asynchronous APIs are
promise based, which is the new defacto asynchronous pattern in JavaScript. If
you use the yoman generator (and I would highly recommend this) if even provides
scaffolding and configurations for running and debugging unit tests (provided
you are using VSC to write this extension, and you almost certainly are).

One of the requirements for my extension was dynamicly changing items in the
IntelleSense list. I was anxious about how difficult that feature was going to
be, so I pushed that feature to the end of development cycle. It ended up being
extremely easy and only took about two hours to build and write tests against.
This feels like an area the VSC core team really focused on and went out of
their ways to make tapping into the IntelleSense subsystem very easy.


{{% figure src="/img/vsc4.jpg" link="https://www.flickr.com/photos/davidwilson1949/19220174350/in/photolist-vhqqdo-627d6G-f3LBHJ-9oUJx2-7xCDtF-cK77Ab-627cXJ-622Zet-cKVGbU-dfnPfo-dSxC3k-6osLNM-6oau53-5Spgv3-5SpgdC-5Spgb3-5SpgXj-5SjX4P-5SjX8t-9pebko-ntkAMh-9rPu6u-6Qfigm-gNpsM-hACZCM-7xCDvi-pN2USE-debAJu-e1tLDE-dSDboq-9pebr9-9rPtPj-9pb7W8-64Gkq2-9mBRYu-hBYviJ-9rLvna-6osLV8-9oUJtF-5SbrzS-aiqJjc-9mSVfe-aitwAU-6GPnYb-9rLvcP-aiqJmX-622YPH-c7cAay-msWmc-9ncKas" attr="David Wilson" class="inliner" %}}

When you want to run your extension, you don't need to do any weird symlinks or
hacks to get it running. The generated configuration provides a "launch
extension" debug command. Then you run this command, a new VSC window launches
and loads your in-process extension. The "host" VSC debugger then attaches to
the new VSC window and allows you to easily debug the running extension. At the
end of the day, your Visual Studio Code extension is just a Node application.
Any tooling you're used to for Node applications can be used while writing an
extension.

## Conclusion

Overall, I've enjoyed getting into VSC plugin authoring. I've been using Visual
Studio Code for a while and was always curious what it would take to build and
publish an extension. The documentation needs some work, but the bulk of what
most people will need is well documented. It is really important to read, and
possibly re-read, everything you can do just within `package.json`. That is the
main entry point for configuration and there is lot of options and
customizations available through just configuration. 

When it came time to write the code, the type definitions of the VSC APIs
combined with the features of the IDE made it a very seamless experience.
Microsoft is done trying to force JScript on it's users and is very aware of
modern JavaScript and the available APIs prove this.

If you're a Visual Studio Code user and find it missing some features, point
your
browser
[here](https://code.visualstudio.com/docs/extensions/example-hello-world) and
get crackin'!

**[Download Stardog today](http://stardog.com/) to start your free 30-day
evaluation.**
