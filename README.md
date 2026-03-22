# macOS 26 Tahoe can't boot on unsupported T2 Macs even via OpenCore Legacy Patcher - that's why
macOS 26 Tahoe got released by Apple on 15th September 2025. The reason why unsupported T2 Macs can't boot macOS 26 Tahoe at all is that the T2 chip holds the Board ID, so the OCLP developers as of 2nd March 2026 hasn't managed to spoof the Board ID without revealing the real Board ID - which leads to the KeyStore always to panic and crash or hang the installer's framework as macOS 26 Tahoe's installer has nearly loaded - the installer has been almost loaded but the KeyStore panics and the installer displays nothing but a gray screen with a mouse that can actually move. At the best case, when trying to spoof the Board ID at the end your unsupported T2 Mac will present 2 Board IDs and the first one is the real one while the second one is the spoofed one - a red flag for macOS 26 Tahoe's installer. Here's an example:
Last login: Sun Mar  1 04:45:44 on console
boyan@Mac-mini-von-Boyan ~ % ioreg -l | grep board-id

    |   "board-id" = <"Mac-7BA5B2DFE22DDD8C">

    |         "board-id" = <"Mac-CFF7D910A743CAAF">
Disabling Secure Enclave Processor via csrutil disable in macOS Recovery and disabling Secure Boot - neither of this will help for now. The T2 chip cannot be disabled - if you were able to disable it, it wouldn't be able to communicate with the controller and as such not being able to find the SSD to boot. So, on unsupported T2 Macs, stay on macOS 15 Sequoia or macOS 14 Sonoma as macOS 13 Ventura and older versions are having more and more unpatched vulnerbilities over the time since Apple does no longer support these versions and check here OpenCore Legacy Patcher's support status for macOS 26: https://github.com/dortania/OpenCore-Legacy-Patcher/issues/1167 and wait until OpenCore Legacy Patcher 3.0.0 becomes general availability. For the OpenCore developers, it will take a lot of time - the T2 chip uses a specialized operating system that users don't interact with it, called BridgeOS and it also uses an A10 Fusion chip - like the iPhone 7. Macs with T1 chip or without any T security chip aren't affected by this issue. I tried myself to boot macOS 26 Tahoe on my 2018 Mac mini. The result was this:

Last login: Sun Mar  1 04:45:44 on console
boyan@Mac-mini-von-Boyan ~ % ioreg -l | grep board-id

    |   "board-id" = <"Mac-7BA5B2DFE22DDD8C">

    |         "board-id" = <"Mac-CFF7D910A743CAAF">
The same issue for not being able to boot macOS 26 Tahoe on unsupported T2 Macs isn't limited to the official OpenCore Legacy Patcher repository - it applies to OCLP-Mod too. OCLP-Mod fixes some macOS 26 compatability issues on unsupported Macs, but I recommend to stick with the official OpenCore Legacy Patcher repository instead as OCLP-Mod's UI is a buggy mess at best, full with experimental features that aren't quite stable. This is not the sole issue - it's also missing drivers, which are the tip of the iceberg.
I'm looking for contributors for this project to be able to add T2 support for OpenCore Legacy Patcher: https://github.com/p8bpg9zrw7-collab/OpenCore-Legacy-Patcher-T2
