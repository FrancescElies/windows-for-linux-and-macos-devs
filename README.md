# Windows for Linux and MacOS devs

A hitchhiker's guide for Linux or MacOS devs who need to use windows

<!--toc:start-->
- [Windows for Linux and MacOS devs](#windows-for-linux-and-macos-devs)
  - [Throw away tools that don't work on windows and learn different tools for each OS](#throw-away-tools-that-dont-work-on-windows-and-learn-different-tools-for-each-os)
  - [Throw away tools that are not cross-platform and learn tools that you can use on every OS](#throw-away-tools-that-are-not-cross-platform-and-learn-tools-that-you-can-use-on-every-os)
  - [Windows specific stuff](#windows-specific-stuff)
<!--toc:end-->

Going from MacOS or Linux to windows, why would you even do that? You might
ask. Maybe you have to share a laptop with family members who have Windows
installed? Would you really want to spend your energy convincing them that
Linux is a much better option? Or convince them to buy a expensive Mac? Making
the sweet promise that all their IT problems will be gone later on; they just
needed a bit of help.

Would you turn down an interesting job offer just because they use Windows?
They do some cool stuff, but their clients are using Windows. What would you do?
Convince the client mega-corporation to switch to Linux? Dump the offer and
continue at your non-fulfilling job because they gave you a Mac?

Unless you are in a privileged position where you can choose the OS you develop
on, chances are that you will have to touch other Operating Systems in the
future too.

We have several options, never touch windows, one could throw away tools that
don't work on windows and learn different tools for each OS or you could throw
away tools that are not cross-platform and learn tools that you can use on
every OS.

I prefer the second one, this way I don't need to keep forgetting and
relearning different tools that solve the same problem after I spend a while
without using a certain OS.

Finding good resources is not easy, I did not come across many resources
covering this topic, but the ones I found where mainly saying, use WSL and run
the tools you are used to over there, e.g. tmux.
I think WSL is amazing, but in my experience, it's works great until you need
to do native things and then you are again left naked in the middle of the blue
ocean.

Luckily Chris Patti & Ned Batchelder have some good advice on their pages.

- [windows - _Ned Batchelder_](https://nedbatchelder.com/blog/tag/windows.html)
- [windows papercuts for nix developers - _Chris Patti_](https://www.feoh.org/posts/windows-papercuts-for-nix-developers)

## Throw away tools that don't work on windows and learn different tools for each OS

If you want to go that path, the following works good on Windows.

- Code editors: [VS Code](https://github.com/microsoft/vscode),
[nvim](https://github.com/neovim/neovim) +
[kickstart](https://github.com/nvim-lua/kickstart.nvim),
[helix](https://helix-editor.com)
- Terminal: [Windows Terminal](https://github.com/microsoft/terminal)
- Shell: PowerShell
- Git UI: [Github Desktop](https://github.com/desktop/desktop),
[Lazygit](https://github.com/jesseduffield/lazygit)
- Screen sharing: [ShareX](https://getsharex.com),
[OBS Studio](https://obsproject.com), [Gimp](https://www.gimp.org)

From this list except Windows Terminal and ShareX everything else works pretty
much everywhere.

## Throw away tools that are not cross-platform and learn tools that you can use on every OS

- Code editors: [VS Code](https://github.com/microsoft/vscode),
[nvim](https://github.com/neovim/neovim) +
[kickstart](https://github.com/nvim-lua/kickstart.nvim),
[helix](https://helix-editor.com)
- Terminal: [Wezterm](https://github.com/wez/wezterm/)
- Shell: Nushell
- Git UI: [Github Desktop](https://github.com/desktop/desktop),
[Lazygit](https://github.com/jesseduffield/lazygit)
- Screen sharing: [Flameshot](https://flameshot.org),
[OBS Studio](https://obsproject.com), [Gimp](https://www.gimp.org),

Windows and ShareX flew away, instead we got Wezterm, Nushell and Flameshot to
the mix.

Wezterm as of today is the only terminal with tmux like capabilities that works
nicely across all major three OS.

Flameshot is an awesome easy to use tool to share screenshots explaining what's
going on, great to make great bug reports, explaining the issue.

Nushell is a cross platform shell with clearer error messages with cues from
bash and powershell and uses [coreutils](https://github.com/uutils/coreutils)
internally, so many unix things you know they are already available without
having to install anything else.

## Windows specific stuff

Some stuff will be very windows specific but that's ok, at the end of the day
you are here using windows right?

Here some good software for devs.

Troubleshooting some issue? Check [sysinternals](https://learn.microsoft.com/en-us/sysinternals).
Some examples below:

- Generate process dumps when a process has a hung or crashes, see
[procdump](https://learn.microsoft.com/en-us/sysinternals/downloads/procdump),
it works on [linux](https://github.com/Sysinternals/ProcDump-for-Linux) too
- See what files are open by which processes? See
[handle](https://learn.microsoft.com/en-us/sysinternals/downloads/handle)
- Programs configured to startup automatically? See
[autoruns](https://learn.microsoft.com/en-us/sysinternals/downloads/autoruns)

But if you install one thing only, install,
[system-informer](https://github.com/winsiderss/systeminformer).
A better task manager, impressive, play around with it to see what it can do
for you. Many things Sysinternals do, you can do it directly from
system-informer.
