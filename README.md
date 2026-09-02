# CrapCheckThis

im lazy. apparently not lazy enough to stop writing windows tools, but definitely too lazy to keep connecting the dots between them by hand

quite a few of you already know my two heavy hitters. if not, quick detour:

- **[FluentCleaner Classic](https://github.com/builtbybel/FluentCleaner)** - the cleaner. signature-driven checks and cleanup for the stuff windows and your apps leave behind.
- **[CrapFixer](https://github.com/builtbybel/CrapFixer)** - the tweaker. windows settings, privacy tweaks and an app remover, also fed by signatures.

i didn't want to merge them. that would turn two focused projects into one oversized thing i'd then have to untangle again. been there.

but getting them to work together? that made sense

## meet the little middleman

**CrapCheckThis is their companion app: a shared, read-only starting point.**

it brings the signature-driven checks from FluentCleaner Classic and CrapFixer into one windows audit. look at the findings, keep the report, or pass a whole category to the right app for the next step.

it works on its own. the other two are optional, and neither needs CrapCheckThis to keep doing its job.

**the cleaner integration is for FluentCleaner Classic only. not the winui 3 version.**

## one scan, three kinds of crap

- **w · windows settings** - compares registry values with the recommendations in `wintweak2.ini`.
- **b · potential bloatware** - matches installed app packages against `winappx.ini`.
- **c · cleanable data** - checks files and registry entries described by `winapp2.ini`.

the audit uses the entries marked as recommended/default in your loaded packs, with the relevant windows-version and application-detection checks. this isn't every feature of both apps crammed into another window.

a match isnt a verdict, either. an app isnt evil just because an ini knows its name. read the details before deciding what should stay.

the scan changes no registry values, uninstalls no apps and deletes no files. **export** lets you save the full log or an image of the current window. check for personal info before sharing either.

## found something? send it next door

next to **scan**, **review** gives you three separate routes:

- **w → CrapFixer:** review windows settings that need attention.
- **b → CrapFixer:** open the app remover with matching installed packages selected.
- **c → FluentCleaner Classic:** review the cleaner findings.

each route needs matching results and its own companion app. missing apps and empty categories stay grey. you don't need both apps installed just to use one route.

the receiving app scans again with its own database and prepares the selection. then **you** decide whether to apply tweaks, remove apps or clean up. nothing gets fixed just because you clicked review, and your saved tweak/cleaner selections aren't replaced.

## the mildly nerdy bit

the rules live in loose ini files. small parsers turn them into entries; the scanners check the registry, installed packages and filesystem. compatible packs can be imported through **check packs** and live in `packs\`. adding supported rules doesn't need a recompile.

the handoff is deliberately boring: a temporary json list plus a command-line argument. original signature names or package names go across, not scripts, registry commands or a list of files to blindly delete. the target resolves those names against its own signatures. unknown entries get skipped and reported.

no background service or local server needed. just two processes agreeing on a few names.

the packs themselves are data, not executable plugins. the app does use a fixed, read-only powershell query to list installed packages; imported signatures can't supply commands.

cleaner totals are estimates, not promises. locked or inaccessible files are skipped, and files, permissions or database versions can change before the second scan.

## plug them in

use handoff-enabled builds and keep each app's complete set of files together:

- CrapFixer goes in `apps\CrapFixer\` next to CrapCheckThis.
- FluentCleaner Classic goes in `apps\FluentCleaner.Classic\`.

don't copy just the exe. old builds without handoff support won't understand the selection.

CrapCheckThis itself is c# / winforms on .net framework 4.8. a small window with a list still gets the job done

### updating the bundled apps

1. close CrapCheckThis and the companion app. back up the companion's folder first.
2. get the latest **handoff-compatible** portable release from [CrapFixer releases](https://github.com/builtbybel/CrapFixer/releases) or [FluentCleaner releases](https://github.com/builtbybel/FluentCleaner/releases). for FluentCleaner, pick **classic**, not winui 3.
3. unpack the complete app into its folder above, replacing program files. keep your personal settings, custom signatures and extensions. don't copy only the exe or accidentally add another nested app folder.
4. reopen CrapCheckThis, scan and use **review**. no installation or reconnecting required.

the executable should end up at `apps\CrapFixer\CrapFixer.exe` or `apps\FluentCleaner.Classic\FluentCleaner.Classic.exe`. avoid leaving an older companion exe directly next to CrapCheckThis.exe: that location is checked before the apps folder.

**the wiring depends on the handoff interface, not the version number.** newer builds must keep the command-line switches and json selection format. an update that removes or changes that interface can break the handoff; the same exe name alone isn't enough. if a release doesn't confirm CrapCheckThis support, stick with the bundled build for now.


## my tiny windows maintenance universe

CrapCheckThis spots it. CrapFixer handles the settings and apps. FluentCleaner Classic takes out the trash

three separate tools, a shared signature habit, and now a little wiring between them. the audit is read-only. everything after that is optional.

---

full source is coming soon. i still need to tidy things up and refactor a few corners before throwing the whole pile on github.
