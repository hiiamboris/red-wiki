### Introduction

Some anti-virus products are known to falsely flag Red toolchain and applications built with it. This issue is by no means unique and affects other languages, both large and small: [Go](https://forum.kaspersky.com/index.php?/topic/353855-kaspersky-deletes-go-programming-language-executables/), [Rust](https://users.rust-lang.org/t/malware-in-rust-installer/8058/8), [Swift](https://mjtsai.com/blog/2018/02/22/avast-anti-virus-false-positives-for-apps-that-use-swift/), [Nim](https://github.com/nim-lang/Nim/issues/1) (the very first issue!), [D](https://news.ycombinator.com/item?id=18347138), [Visual Basic](https://social.msdn.microsoft.com/Forums/vstudio/en-US/e725ffad-2477-46d8-8112-71202f330f5d/unknown-publisher-anti-virus-thinks-my-application-is-a-virus-vb-2010-how-to-solve-this?forum=vbgeneral), [Delphi](https://community.norton.com/en/forums/delphi-and-false-positive-problem-again-it-seems) and [AutoHotkey](http://www.donationcoder.com/forum/index.php?topic=15210.0).

The aim of this article is to explain why this likely happens, to raise general awareness of the issue, and to provide ways of mitigating it on the user's side.


#### Common abbreviations

* AV — Anti-virus;
* PE — [Portable Executable](https://en.wikipedia.org/wiki/Portable_Executable).

### Problematic vendors

AV vendors that most commonly flag Red toolchain or binaries compiled with it, in no specific order:

* Norton;
* Avast;
* Avira;
* AVG;
* McAfee;
* Windows Defender.

### Contributing factors

Multiple factors are at play in this issue, the common denominator being the usage of extremely poor heuristics by AVs mixed with one or many Red language design aspects.

| Design aspect | Factor |
|-|-|
| Highly dynamic, homoiconic. | Heavy reliance on [runtime library](https://github.com/red/red/blob/414cdb325f90b3a8eec3731549f3aa05dfee2d72/runtime/red.reds), which needs to be included in every PE compiled with the toolchain. |
| Small. | Runtime library and dynamic user code **`*`** contained in every PE (see above) are serialized and stored in [RedBin](https://doc.red-lang.org/en/redbin.html) format, which is then compressed with [custom CRUSH algorithm](https://github.com/red/red/blob/414cdb325f90b3a8eec3731549f3aa05dfee2d72/runtime/crush.reds) **`†`**; at boot-time, the runtime library is uncompressed into memory. Compressed data has high entropy, which, coupled with the uncompression process, is a common feature of a "generic malware" (that's how AVs usually mark Red binaries). |
| Self-contained. | Toolchain is built from scratch and features custom linker and compiler; PE layout differs from common formats used by Visual Studio and GCC linkers, big part `DATA` segment is compressed (see above); compiler output is currently unoptimized. Some AVs view custom PE layout as suspicious and likely treat unoptimized CPU usage as a part of malware's profile. |
| Comes with batteries included. | Runtime library is dense and feature-packed; in order to stay compact, it heavily relies on underlying OS API, some of which are considered to be "blacklisted" by AV vendors: file access and networking (for ports and general I/O), keyboard handling (required for View GUI system), cryptography (mainly hashing functions), screenshot capture (`to image!` on View face) &c. |

All the above are pure speculations, since the Red team never received any feedback or analysis details from vendors which it contacted with a whitelist request (e.g. [Avira](https://twitter.com/red_lang/status/887970289618829312) and [Norton](https://github.com/red/red/issues/500#issuecomment-24805674)).

As of `0.6.4`, on the first launch toolchain acts as an installer by doing the following:

1. Compilation of dynamic compression library which is then used to pack RedBin data; this happens because Red compiler depends on Rebol2 SDK during the bootstrapping phase, which can interact with native code only via `load/library` FFI.
1. Compilation of Red console, which serves as a gateway to Red interpreter; this happens because Red strives to provide a go-to solution and cover all programming needs, from high-level scripting to low-level coding.

The above is done transparently to the user, with a dynamic library and console executable being placed in the `%AppData%` folder. Some AVs consider it as a threat and sign of malware trying to hide in the system. Because of this, Red will likely offer console and toolchain separately.

Last but not least, Red is in constant and active development, which means that the binary profile of toolchain executable changes with every significant commit in the codebase. Coupled with periodic updates of whitelist/blacklist databases and usage of machine learning algorithms (e.g. [Windows Defender ATP](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-defender-antivirus/utilize-microsoft-cloud-protection-windows-defender-antivirus)), this renders heuristics used by AVs unreliable — binary whitelisted today will be different tomorrow, and neural network is fed with ever-changing training data which makes it jump around sub-optimal local minimas. 

### What you can do

In general:
* DYOR. Red is an open-source project with code freely available for review.
* In case of a doubt, don't trust only your AV and scan suspicious binary on [VirusTotal](https://www.virustotal.com).
* Report any false-positives to your AV vendor; ideally, request a manual review by AV analysis team and whitelisting.
* Switch to a more reliable AV, or don't use AV at all and follow safe practices. 

For application developers:
* Whitelist all Red-related local folders.
* Use `--no-compress` option which will omit RedBin (de)compression; obviously, this comes at the cost of increased binary size. Use [UPX](https://upx.github.io/) if the binary size is of paramount concern to you.
* Compile console from sources; the process is painless and straightforward (see README for details).
* Use code signing, being aware of possible [CA issues](https://it.slashdot.org/story/19/06/02/0042209/ask-slashdot-what-to-do-when-your-certificate-authority-suddenly-revokes-your-cert) or rely on self-signed certificates.

---

**`*`** The one processed by the interpreter (i.e. wrapped in `do` call), or, in case of Encap mode, everything;

**`†`** Chosen for a good balance between compression ratio and (de)compression speed.