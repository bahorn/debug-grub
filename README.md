# debug-grub

This is a enviroment to make it easier to hack on GRUB2.
Uses a justfile to handle common actions.

## Usage

### General

GRUB is initalized as submodule, so git pull / whatever to update that.

Initial setup (getting uefi firmware and running bootstrap.sh / configure):
```
just get-ovmf && just setup-grub
```

You can copy a useful clangd into the grub directory with:
```
just install-clangd
```

Initially, and when you make a change you want to test out:
```
just build-grub && just install-grub
```

Then you run grub with:
```
just run
```

and debug by running:
```
just debug <address>
```

You get the address from the line:
```
dynamic_load_symbols 0xbde23000
```
when you run `just run`, so in this case you can debug with
`just debug 0xbde23000`.

### More Specific

You can modify `./configs/early-grub.cfg` to change the initial commands grub
runs (just rerun `just install-grub` after).
This is useful if say you want to debug a filesystem issue or something similar,
so you can setup the script to automatically create a loopback with your test
filesystem.

Copy files to `./artifacts/hda/` to use them.

If you need a module I didn't enable, just modify `grub_mod.lst` and add it in.
`just install-grub` afterwards to enable it.
(you could also copy it into hda and insmod it if you really want).

## License

Same as GRUB2, GPL3.
