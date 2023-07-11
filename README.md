# Yikwid Browser Source Repository

This repository contains all the patches and theming that make up Yikwid Browser, as well as scripts and a Makefile to build it. There also is the [Settings repository](https://github.com/yikwid/yikwid-browser-settings), which contains the browser preferences.

## Yikwid Browser build instructions

There are two ways to build the Yikwid Browser. You can either use the source tarball or compile directly with this repository.

### Building from the Tarball

First, **[download the latest tarball](https://github/yikwid/yikwid-browser/releases)**.
```bash
tar xf <tarball>
cd <folder>
```

Then you have to bootstrap your system to be able to build it. You only have to do this one time. It is done by running the following commands:

```bash
./mach --no-interactive bootstrap --application-choice=browser
./lw/setup-wasi-linux.sh
```

Finally you can build the browser and then package or run it with the following commands:

```bash
./mach build
./mach package
# OR
./mach run
```

### Building with this Repository

First, clone this repository with Git:

```bash
git clone --recursive https://github.com/yikwid/yikwid-browser.git yikwid-browser
cd yikwid-browser
```

Next, build the source code with the following command:

```bash
make dir
```

After that, you have to bootstrap your system to be able to build Yikwid Browser. You only have to do this one time. It is done by running the following command:

```bash
make bootstrap
```

Finally you can build the browser and then package or run it with the following commands:

```bash
make build
make package
# OR
make run
```

## Development Notes

### How to make a patch

The easiest way to make patches is to go to the Yikwid Browser source folder:
```bash
cd yikwid-$(cat version)
git init
git add <path_to_file_you_changed>
git commit -am initial-commit
git diff > ../mypatch.patch
```
### How to work on an existing patch

The easiest way to make patches is to go to the Yikwid Browser source folder:
```bash
make fetch # get the firefox tarball
./scripts/git-patchtree.sh patches/sed-patches/disable-pocket.patch
```
Now change the source tree the way you want, keeping in mind to `git add` new files. When done, you can create the new patch with:
```bash
cd firefox-<version>
git diff 4b825dc642cb6eb9a060e54bf8d69288fbee4904 HEAD > ../my-patch-name.patch
```
This ID is the hash value of the first commit, which is called `initial`. Dont forget to commit changes before doing this diff, or the patch will be incomplete.


### How to create a patch for problems in Mozilla's [Bugzilla](https://bugzilla.mozilla.org/).

Well, first of all:

* [Create an account](https://bugzilla.mozilla.org/createaccount.cgi).
* Handy link: [Bugs Filed Today](https://bugzilla.mozilla.org/buglist.cgi?cmdtype=dorem&remaction=run&namedcmd=Bugs%20Filed%20Today&sharer_id=1&list_id=15939480).
* The essential: [Firefox Source Tree Documentation](https://firefox-source-docs.mozilla.org/).

Now that you have a patch in Yikwid Browser, that's not enough to upload to Mozilla. See, Mozilla only accepts patches against Nightly. So here is how to do that:

If you have not done already, create the `mozilla-unified` folder and build Firefox with it:
```bash
hg clone https://hg.mozilla.org/mozilla-unified
cd mozilla-unified
hg update
MOZBUILD_STATE_PATH=$HOME/.mozbuild ./mach --no-interactive bootstrap --application-choice=browser
./mach build
./mach run
```
If you skipped the previous step, you could ensure that you're up to date with:
```bash
cd mozilla-unified
hg pull
hg update
```
Now you can apply your patch to Nightly:
```bash
patch -p1 -i ../mypatch.patch
```
Now you let Mercurial create the patch:
```bash
hg diff > ../my-nightly-patch.patch
```
And it can be uploaded to Bugzilla.

##### *(excerpt from the Mozilla readme)* Now the fun starts

Time to start hacking! You should join us on [Matrix](https://chat.mozilla.org/), say hello in the [Introduction channel](https://chat.mozilla.org/#/room/#introduction:mozilla.org), and [find a bug to start working on](https://codetribute.mozilla.org/). See the [Firefox Contributors’ Quick Reference](https://firefox-source-docs.mozilla.org/contributing/contribution_quickref.html#firefox-contributors-quick-reference) to learn how to test your changes, send patches to Mozilla, update your source code locally, and more.

## Hey, I'm using MacOS or Windows..
We understand, life isn't always fair 😺. The same steps as above do apply, you'll just have to walk through the beginning part of the guides for:
* [MacOS](https://firefox-source-docs.mozilla.org/setup/macos_build.html): The cross-compiled Mac .dmg files are somewhat new. They should work, perhaps with the exception of the `make setup-wasi` step.
* [Windows](https://firefox-source-docs.mozilla.org/setup/windows_build.html): Building on Windows is not very well tested.

Help with testing these targets is always welcome.
