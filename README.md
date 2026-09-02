# CrapCheckThis

> **Windows has enough tools promising to “fix everything.” CrapCheckThis fixes absolutely nothing 😄 Just a  read-only Windows audit powered by signatures**

i had this slightly odd idea: the engines behind two of my more popular apps are already fed almost completely by signature files. so why keep the check part locked inside a fixer and a cleaner?

i pulled the read-only parts together and made them public as a tiny Windows crap check report. CrapCheckThis looks at the machine, tells you what stands out and then simply stops. so no fix button here and no magic optimizer stuff

## what it checks

- recommended Windows settings from `Wintweak2.ini`
- potential bloatware packages from `Winappx.ini`
- cleanable files and registry entries from `Winapp2.ini`

## how the signature thing works

its pretty simple. an `.ini` goes into `Packs\`, a small detector figures out if it contains tweak, appx or cleaner signatures, then the matching parser turns the sections into normal entries for the scanner.

extra packs are merged at runtime. if an imported pack contains the same named signature, it can override the built-in one. nothing is hardcoded into the ui and updating a pack needs no recompile.

the packs are data, not code. they cannot launch PowerShell, run EXEs, change the registry, uninstall apps or delete files.

**CrapCheckThis can look, but it cannot touch!**


---

found something? [CrapFixer](https://github.com/builtbybel/CrapFixer) can fix the Windows settings and apps, [FluentCleaner](https://github.com/builtbybel/FluentCleaner) can clean the leftovers. thats basically the little family.
