# Debian Testing

From a WSL perspective.

## Origins

So this all began with my growing obsession with Markdown.  Then I was prompted to take action by a course at Drexel, MATH 410.  This is a statistics course that requirese the use of a stats-focused programming language, R.

The problem was that my programming environement is entirely WSL+VSCode based, so the proposition of installing RStudio made me scoff.  "Why would I install a whole IDE for this when I already have a great workflow?"  If only I knew how deep this rabbit hole would go...

Part of the class was submitting documents that required bits of math, answers to questions, sections of R code, and code output all in one place.  Lucky for me, there's an epic project focused on producing sweet, reproducable documents out of R and Markdown in the form of an R extension called `rmarkdown`.  (So creative I know.)  You should check out their [site](https://rmarkdown.rstudio.com/), [cheat sheet](https://rstudio.com/wp-content/uploads/2016/03/rmarkdown-cheatsheet-2.0.pdf), and ["bookdown"](https://bookdown.org/yihui/rmarkdown/).  (I might write more about RMarkdown, but the Bookdown book is actually really good.)

But looking deeper inside of how `rmarkdown` works, I found the real goldmine: **Pandoc**.

### Pandoc

Check out Pandoc [here](https://pandoc.org/).  From their site:

> If you need to convert files from one markup format into another, pandoc is your swiss-army knife.

And it is _f r e a k i n g_ amazing.

I immediately started using Pandoc for everything.  I swore I would never write things in a word processor ever again.  (That didn't happen but we're getting close.)

One of the most amazing things about it is that it's still constantly being updated with new and useful features.  The only problem, is that the version that's in Ubuntu's 18.04 LTS release is ANCIENT now!

So I wanted the newest version **bad**.  [Installing Pandoc for linux.](https://pandoc.org/installing.html#linux)

My plan (at the time), install a different WSL distro that gets better package updates. Duh?


> Ok present-day Max here,
> 
> Another reason for this decision was that I KNEW rmarkdown used pandoc, and didn't want to bork that setup on my Ubuntu instance because I was still needing to churn out math homework weekly.

### Debian

Lets be clear here.  I am NOT a Linux expert (yet :eyes:).  But I have been using WSL Ubuntu for a while, I'm familiar with `apt` (or so I thought)... This should be cake right.

WRONG.

Debian Stable is not that far ahead of Ubuntu, and there are TONS of warnings about [NOT BREAKING DEBIAN](https://wiki.debian.org/DontBreakDebian).  But there were not a lot of instructions on how to get onto Debian Testing/Unstable/Sid/Experimental WITHOUT breaking things.

So here's what I did:

## My Guide

### 1. Get your system up to date

### 2. Make a backup of those crusty old apt sources 

```console
user@system:~$ cp /etc/apt/sources.list /etc/apt/sources.list.backup
```

Now just in case anything awful happens we have the original sources.

### 3. Replace with new shiny apt sources

Edit your `/etc/apt/sources.list` file to look like the following:

```apt sources
deb http://deb.debian.org/debian/ testing main contrib non-free
deb-src http://deb.debian.org/debian/ testing main contrib non-free

deb http://security.debian.org/debian-security testing-security main contrib non-free
deb-src http://security.debian.org/debian-security testing-security main contrib non-free

deb http://http.us.debian.org/debian unstable main non-free contrib
deb-src http://http.us.debian.org/debian unstable main non-free contrib

deb http://http.us.debian.org/debian experimental main non-free contrib
deb-src http://http.us.debian.org/debian experimental main non-free contrib
```

Note: this includes the testing-security (which will contain nothing) and experimental.

### IMPORTANT: Apt Pinning

Because we're including several repos, we need to give them priorities.

Edit (or creat if it doesn't exist) EITHER `/etc/apt/preferences` OR `/etc/apt/preferences.d/my_preferences`

Make it this:

```apt preferences
Package: *
Pin: release a=testing
Pin-Priority: 650

Package: *
Pin: release a=unstable
Pin-Priority: 600

Package: *
Pin: release a=experimental
Pin-Priority: 500
```

### 4. Finally! Update time!

Run:

```console
user@system:~$ sudo apt update
```

And as long as nothing goes wrong, also run:

```console
user@system:~$ sudo apt full-upgrade
```

Congrats! You're now running Debian Testing.

### 5. Optional (But good ideas)

Install and learn how to use `apt-listbugs` and `debsecan`.  Apparantly they can save your arse if you're about to upgrade to somehthing unstable, AND let you know when there's a security patch out for a package that you need to get from another repo (like experimental).

# My Research

## Official Advice:

There's LOTS of confusing and at times conflicting information in here.

### [Don't Break Debian]()

> #### Take notes
>
> It's easy to forget the steps you took to do something on your computer, especially several months later when you're trying to upgrade. Sometimes when you try several different ways of solving a problem, it's easy to forget which method was successful the next day!
>
> It's a very good idea to take notes about the software you've installed and configuration changes you've made. When editing configuration files, it's also a very good idea to include comments in the file explaining the reason for the changes and the date they were made.

### [Debian Testing Page](https://wiki.debian.org/DebianTesting)

> #### How to upgrade to Debian (next-stable) Testing
>
> 1. Edit your apt sources, changing 'stable' (or buster, the current codename for stable) to 'testing' (or bullseye, the current code name for the next stable release).
>
> 2. **Remove, disable or comment out your stable security updates apt sources (anything with security.debian.org in it).**
>
> 3. Remove, disable or comment out any other stable-specific apt sources, like *-backports or *-updates.
>
> 4. Verify that your installation is not fixed to a specific release in /etc/apt/apt.conf.d/00default-release

> #### Best practices for Testing users
>
> * **It is a good idea to include unstable and experimental in your apt sources** so that you have access to newer packages when needed. With the `APT::Default-Release` apt config setting or with apt pinning you can have packages from testing by default but if you manually upgrade some packages to unstable or experimental, then you will get upgrades within that suite until those packages migrate down to unstable or testing. The apt pinning needs priorities lower than 990 and equal to or higher than 500 for this to work nicely. You can also pin some packages to unstable/experimental that you always want the latest versions of.
>
> * **It is a good idea to install security updates from unstable** since they take extra time to reach testing and the security team only releases updates to unstable. If you have unstable in your apt sources but pinned lower than testing, you can automatically add temporary pinning for packages with security issues fixed in unstable using the output of [debsecan]().

### [Debian Unstable Page](https://wiki.debian.org/DebianUnstable)

> Use the current "stable" installer to install a minimal stable system (recommended).
> 
> Change your apt sources to point to "unstable".
> 
> Run `apt update` and `apt full-upgrade`.


### [Debian Apt Preferences Page](https://wiki.debian.org/AptConfiguration?action=show&redirect=AptPreferences)



### [Debian Manual FAQs: Apt](https://www.debian.org/doc/manuals/debian-faq/pkgtools.en.html#apt-get)

> Starting with Debian Jessie, some frequently used apt-get and apt-cache commands have an equivalent via the new apt binary. This means some popular commands like apt-get update, apt-get install, apt-get remove, apt-cache search, or apt-cache show now can also be called simply via apt, say apt update, apt install, apt remove, apt search, or apt show. The following is an overview of the old and new commands:
>
> ```
>  apt-get update             ->  apt update
>  apt-get upgrade            ->  apt upgrade
>  apt-get dist-upgrade       ->  apt full-upgrade
>  apt-get install package    ->  apt install package
>  apt-get remove package     ->  apt remove package
>  apt-get autoremove         ->  apt autoremove
>  apt-cache search string    ->  apt search string
>  apt-cache policy package   ->  apt list -a package
>  apt-cache show package     ->  apt show package
>  apt-cache showpkg package  ->  apt show -a package
> ```

## Unofficial Advice:

### [Silver Moon | August 6, 2016 | BinaryTides.com](https://www.binarytides.com/enable-testing-repo-debian/)

Too long to go over the whole thing, but has the idea of pinning included.  Hopefully this is a best practice?  (Comment if you have a better way!)

> Edit or create one of the following:
> 
> `/etc/apt/preferences`
> OR
> `/etc/apt/preferences.d/my_preferences`
> 
> ```apt
> Package: *
> Pin: release a=stable
> Pin-Priority: 700
> 
> Package: *
> Pin: release a=testing
> Pin-Priority: 650
> 
> Package: *
> Pin: release a=unstable
> Pin-Priority: 600
> ```

### [robschi | June 22, 2020 | reddit.com/r/debian](https://www.reddit.com/r/debian/comments/hed8bg/how_debian_sid_are_stable/fvr7k65?utm_source=share&utm_medium=web2x)

I have no idea if this embed will work.

<a class="embedly-card" href="https://www.reddit.com/r/debian/comments/hed8bg/how_debian_sid_are_stable/fvr7k65">Card</a>
<script async src="//embed.redditmedia.com/widgets/platform.js" charset="UTF-8"></script>

