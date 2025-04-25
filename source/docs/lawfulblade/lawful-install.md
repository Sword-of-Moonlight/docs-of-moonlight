# Lawful Blade - Installation
So you've made the wise choice to install Lawful Blade eh? This bit of documentation is going to guide you through the process of getting it all set up and working on your system, so you can SoM freely and without constraint!

## Prerequisites
1. [The latest release of Lawful Blade](https://github.com/FromSoft-Modding-Committee-FSMC/LawfulBlade/releases)
2. An archiving program (7-Zip, WinRAR, whatever is default on windows.)
3. Administrator rights on your machine.
4. 5 minutes of time.

## Forewarning !!
Antivirus software is highly likely to flag Lawful Blade as malicious software - you can be assured that despite the amount of false positives, there is nothing malicious about Lawful Blade.

The reasons for these false positives being present will be listed here:

- Sword of Moonlight has lots of unsigned executables.
- Lawful Blade executables themselves are unsigned.
- Lawful Blade makes registry edits for you - these edits are specific to SoM itself, and only operates on the following key: 'HKEY_CURRENT_USER\Software\FROMSOFTWARE\SOM\INSTALL'.
- Lawful Blade includes code which can modify an executable with the large address aware flag when creating a package - this is to enable 4GB support on any pre-existing tools which are supplied inside a package.
- Lawful Blade includes RCEdit. It is used to modify the final runtime executable when you publish your game, and add an icon + metadata to your game.
- Lawful Blade includes PECHKSUM. This is used to calculate the checksum of the runtime executable after it has been modified by RCEdit, since that program doesn't do it - this stops your games from being flagged as a virus accidently.
- Lawful Blade Updater will copy itself to your temporary files directory, since the updater itself can be updated if required. It is run from the temporary directory so the LawfulBladeUpdater.exe can be replaced, as otherwise Windows will not let it be replaced since it is running.

The source is public and avaliable for those who want to investigate further. If you would like to ask for further varification, reach out to the community on discord.

## Setting up Lawful Blade
Assuming you've followed the pre-requisits, you should have a zip file with a name in the following format: 'LawfulBladeVxxx.zip', where 'xxx' is equal to a version number. Your first step will be creating a directory which will your Lawful Blade installation (I'd suggest using something in the root of your drive, like C:\Sword Of Moonlight Stuff\Lawful Blade, but anything will do so long as it isn't a [Windows Protected Folder](https://learn.microsoft.com/en-us/defender-endpoint/controlled-folders)).

After you've created your installation directory, simply copy everything from inside the previously mentioned zip file into the directory, and you're good to go.

## Running Lawful Blade
In order to execute the program, simply double click the executable after extracting it as described in the _'Setting up Lawful Blade'_ section. If it doesn't launch, you may want to check up the _'Troubleshooting'_ section at the bottom of this page.

## Troubleshooting
1. _I launch the program, but nothing happens?_
   There's a good chance you need to install the [.NET 9 Runtime](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-9.0.3-windows-x64-installer)