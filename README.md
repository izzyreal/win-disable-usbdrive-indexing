# win-disable-usbdrive-indexing
Windows 10 registry patches to disable and enable USB drive indexing to prevent `System Volume Information` creation.

## Usage

Download the registry patches from [the release page](https://github.com/izzyreal/win-disable-usbdrive-indexing/releases/tag/v1.0) or from here:

[DisableRemovableDriveIndexing patch](https://raw.githubusercontent.com/izzyreal/win-disable-usbdrive-indexing/main/DisableRemovableDriveIndexing.reg)

[EnableRemovableDriveIndexing patch](https://raw.githubusercontent.com/izzyreal/win-disable-usbdrive-indexing/main/EnableRemovableDriveIndexing.reg)

Double-click a `.reg` file of your choice and follow the on-screen instructions to patch your registry.

`DisableRemovableDriveIndexing.reg` is for disabling indexing, meaning that Windows will not pollute your USB drives with metadata in a `System Volume Information` directory that it creates on your USB drive. The patch adds a key to `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Windows Search` called `DisableRemovableDriveIndexing` and sets its value to `1`.

`EnableRemovableDriveIndexing.reg` is for enabling back indexing. This patch removes the key mentioned earlier. Any other keys in `Windows Search` will remain untouched.

## What is the worst that could happen?

As far as I know, applying the `DisableRemovableDriveIndexing.reg` patch can only slow down searches if you use Windows Search to find files on your removable drives. Undo the patch by double-clicking `EnableRemovableDriveIndexing.reg` to get your normal search speed back.

### Background

The Akai MPC2000XL integrated sequencer/sampler uses a [tweaked version of the FAT16 filesystem](https://vmpcdocs.izmar.nl/vmpc_specific_settings.html#background). The tweak consists of allowing file names of up to 16 characters, as opposed to 8 characters, which is standard for FAT16. This is why MPC2000XL sound names can be 16 characters long.

Now, what happens when you insert your MPC2000XL CF card or other media in a Windows machine, is that Windows will write new files to it as part of its removable drive indexing (a process to speed up searching for files with Windows Search). Part of writing new files to a volume includes _rewriting_ what was already there in the file allocation table -- something that Windows does not know how to do in the Akai-tweaked way.

What was means in practice is that your 2KXL CF and other media will be corrupted the moment you put them in a Windows machine, if they have files with names longer than 8 characters. The `SND` files in question will still be there, but their names have become mangled, so any `PGM` files that depend on these `SND` files having long file names will no longer find its sounds.

By disabling indexing, and by preventing the creation of `System Volume Information`, this registry patch protects your MPC2000XL's CF and other media from this kind of corruption.

**Note that any write operation to your 2KXL media performed via Windows will corrupt any 8 - 16 letter file names, even when you have applied this registry patch.** To be safe, never copy files to your 2KXL media, never create new files and never remove files from your 2KXL media, unless you're using special software like [VMPC2000XL](https://www.izmar.nl).
