
I'm using unison every day. When it refused to work when I upgraded to 10.10 I freaked out.
I didn't want to install homebrew or MacPorts so I tried to build it myself. 

I needed to install oCaml 3.12 from here http://ocaml.org/releases/3.12.1.html (never mind it says that >=10.4 are not supported) and Xcode

If you compile with later oCaml you will get `Fatal error: Internal error: New archives are not identical` when syncing
with a server instance compiled with older oCaml (like Ubuntu)

I ran this command because make complained it couldn't find xcodebuild

    sudo xcode-select -switch /Applications/Xcode.app/Contents/Developer
    
I had to apply [this](https://trac.macports.org/attachment/ticket/40052/remove-sdk.patch) patch
because it wanted to use the 10.4 sdk, but it is probably not needed as obviously it is only for
the GUI version

I also had to change 

    NameMap : Map.S with type key = Name.t
    
to
    
    NameMap : MyMap.S with type key = Name.t


`make` still wanted to build the gui version even after I changed the 
`UISTYLE=text` variable in the Makefile, so I changed 

    ifeq ($(OSARCH),osx)
    UISTYLE=macnew

to 

    ifeq ($(OSARCH),osx)
    UISTYLE=text
    
inside Makefile.OCaml and make finally worked.


Here's a binary if you are lazy. Copy it to /usr/bin
[http://play.qwazix.com/mac/unison](http://play.qwazix.com/mac/unison)


### references

 * http://orangepalantir.org/topicspace/index.php?idnum=79
 * https://trac.macports.org/ticket/35407
