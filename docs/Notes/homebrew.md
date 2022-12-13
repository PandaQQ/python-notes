# 1. Error: Cannot install in Homebrew on ARM processor in Intel default prefix (/usr/local)

> The solution on stack overflow as below:

For what it's worth, before installing Homebrew you will need to install Rosetta2 emulator for the new ARM silicon (M1 chip). I just installed Rosetta2 via terminal using:

```
/usr/sbin/softwareupdate --install-rosetta --agree-to-license
```

This will install rosetta2 with no extra button clicks.

After installing Rosetta2 above, you can then use the Homebrew cmd and install Homebrew for ARM M1 chip: ` arch -x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"`

Once Homebrew for M1 ARM is installed use this Homebrew command to install packages: `arch -x86_64 brew install <package>`



#### And the install package is as below:

```
arch -x86_64 brew install <package>
```