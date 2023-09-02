---
layout: post
title: "My journey with mechanical keyboards and QMK"
comments: false
description: ""
keywords: "keyboards, electronics, linux"
published: true
visible: true
---

# Intro
So this is going to be a kind of weird post where I geek out about keyboards.
I know that for most people your computer keyboard is the most boring thing that you can think about,
and if you quickly scroll this you might be thinking "who tf cares that much about keyboards ???".
And I get it, but if you consider that for a lot of people (writers, students, scientists, programmers)
you interact with it for hours a day, it might start to make sense.

All of this for me started in 2016, I was getting into college and started to note some funky keyboards
on youtube. I've had already heard about mechanical keyboards, but never gave it any attention at all,
my trusted Microsoft membrane keyboard had all I ever could need,
but one of those funky keyboard caught my attention, the WASD mechanical keyboard,
and once I understood that you could personalize it the way that you wanted, I was hooked on the idea.

![WASD Keyboard](/assets/images/qmk/wasd_keyboard.jpg)
*WASD custom keyboard*

Since then I managed to create a little collection of mechanical keyboards, and started
exploring all the options about this hobby.
In this post I'll try to explain some of the advantages, differences and customizations possibles
when using a mechanical keyboard, and latter I'll discuss how to use some of this features
on a normal keyboard.


TODO: Fotos teclados

# Keyboard form factors
The first difference that'll notice about mechanical keyboards, is that they come in all
shapes and forms. The next image you'll see some common form factors available,
including the full size (100%) that most normal keyboards have.

![Form Factors](/assets/images/qmk/keyboard_size_guide.webp)
*Some of the available form factors*

When talking about the form of the keyboard, what you should consider is pretty straight forward,
it's basically all about the size and reducing the number of keys to the minimum usable.

If you pay attention, the 80% keyboard (A.K.A. TKL) is the 100% keyboard, but without the numpad.
Unless you do a lot of data input, using the numpad is pretty inefficient,
and takes a lot of lateral space, usually where you have a mouse.

There are other form factors, in the picture, you can see the 40% keyboard,
it is what is called a *ortholinear* keyboard, meaning all the keys are arranged in the form of a grid,
and not staggered like they normally are.


![Egodox](/assets/images/qmk/ergodox.jpg)
*Ergodox EZ, split keyboard*

# Key switches
<!-- https://www.youtube.com/watch?v=HXJzmky2DaI -->


# New Keymap and Layers

## Layers
TODO: LT
TODO: MO

## Death to the Caps Lock
Ok, hear me out, the Caps Lock it's the most misplaced and annoying key on your keyboard.
When using the Caps Lock to capitalize your words, you're losing a lot of time,
the caps lock should be used only to YELL AT PEOPLE ON PUBLIC FORUMS, nothing more.
The 'right way' is to use your shift key, press the left shift to upper case a key on the right side of the keyboard,
ex.: LShift + o = O, and the opposite for the right shift (RShift + a = A).

This way, you're only going to need the Caps lock to stuff like CONSTANTS on programming.
You don't use the caps lock anymore, no more accidentally hitting it, and now you have a extra key!
Now you can rebind this key to a more useful function,
because if you think about it, the caps lock it's in a great place on the keyboard,
on the home row, right next to your left pinky, so it's really easy to reach.

### *Tap* Caps to ESC
If you're a programmer, or are involved in some areas in IT, you probably heard of VIM.
In a nutshell, VIM is a really powerful text editor that focuses eliminating your need of using a mouse,
and allow you to manipulate text files way faster than you can with a normal text editor.

The way the VIM accomplishes this, is by being a modal text editor, it basically has multiple modes,
and the mode that you're in define the behavior for the software.
The main mode of vim is "NORMAL", in this mode you can navigate the text file and manipulate it,
with stuff like copying, cutting, deleting etc.
It's in NORMAL mode also that you enter other modes like INSERT and VISUAL for example,
and the default keybind to returning to normal mode is the ESC key !

So when using VIM, using the default settings you're a hitting the ESC key A LOT.
And looking at your keyboard, the ESC key is pretty far from the home row,
making you strecht your hand every time you want to press it.
Even if you don't use vim, the ESC key is used a lot to lose focus, de-select something and exit.

**TODO: Insert esc gif**

This way, making your Caps lock key send ESC, improves a lot the ergonomics of using the keyboard,
even if you don't use vim. Most OS's allow you to swap Caps and ESC, this way you don't lose the Caps behavior.

### *Hold* Caps to Control
Now that you're using the caps lock as an ESC, you might notice that you never hold your ESC key,
this means that we can assign the hold behavior  of the caps key to another function.
If you think about, you never hit you control key a single time (except for gaming), so why not use Caps as Control ?

This in no way a new concept, if you look the keyboard layout of computer before the PC,
you will notice that the control key in the now caps lock position was pretty common.

![Apple II Keyboard](/assets/images/qmk/appleiiekeyboard.jpg)
*Apple II Keyboard*

![MSX keyboard](/assets/images/qmk/msx_keyboard.jpg)
*MSX Keyboard*

The reasoning here is pretty straight forward: ergonomics. While the ESC on Caps is pretty common with vim users,
Control on Caps is really common on emacs, another popular editor, where most keybindings use the Control key,
usually in some really weird and uncomfortable way.
Even though I don't use emacs, we use Ctrl shortcuts all the time, to copy, paste, cut, new tab, print etc.
And the terminal multiplexer tmux also use a lot of keybindings with the control key.

Setting your Caps to Control it just really comfortable, I initially thought that I would **NEVER** adapt,
I've been using the default layout since the first day that I touched a computer,
but all it took was a single week, and no looking back.

**TODO: Insert Control gif**


## Media keys and macros
TODO: Imagem do layout com parte demarcada
## Navigation and Text editing
TODO: Imagem do layout com parte demarcada

# QMK
## Native Keyboards

## USB-USB Converter
While I do love my new toys, the older keyboards are still great, and I would like to use them with the new layout.

## Managing keymaps
### Docker container
### Symbolic links
### Submodule for QMK and keychron fork

# Software Options
## Kmonad
## Powertoys
