# 参考文献

## コンピュータアーキテクチャについて
* [コンピュータアーキテクチャの話](https://news.mynavi.jp/series/architecture)
  * WEB上の連載記事
  * 詳しく書かれていて, 1から読めばわかりやすい
* [コンピュータの構成と設計](https://www.amazon.co.jp/dp/B00UJ42A0K/)
  * 所謂 "パタヘネ" 本
* [The microarchitecture of Intel, AMD, and VIA CPUs](https://www.agner.org/optimize/microarchitecture.pdf)
  * 多くの世代のx86 CPUのマイクロアーキテクチャについてまとめて記している
* [ハイパーバイザの作り方～ちゃんと理解する仮想化技術～](https://syuu1228.github.io/howto_implement_hypervisor/)
  * 仮想化関係について

## CPU脆弱性に関して
* [Project Zero](https://googleprojectzero.blogspot.com/2018/01/reading-privileged-memory-with-side.html)
  * 大きな騒ぎの引き金になった, Spectre/MeltdownのPoCに成功したという内容と解説の記事
* [SpectreとeBPF](http://mmi.hatenablog.com/entry/2018/02/02/003325)
  * 上記Project ZeroのSpectre v1, v2に対する日本語解説
* [SpectreBustersあるいはLinuxにおけるSpectre対策](https://www.slideshare.net/mhiramat/spectrebusterslinuxspectre)
  * Variant 2 についての説明が詳しい
* [Negative Result: Reading Kernel Memory From User Mode](https://cyber.wtf/2017/07/28/negative-result-reading-kernel-memory-from-user-mode/)
  * Variant 3 についての細かい部分が書いてある
  * Project Zeroでもリンクが貼ってあるところ
* [Understanding L1 Terminal Fault aka Foreshadow: What you need to know](https://www.redhat.com/en/blog/understanding-l1-terminal-fault-aka-foreshadow-what-you-need-know)
  * L1TFについてよくまとまっている
* [Speculative Store Bypass explained: what it is, how it works](https://www.redhat.com/en/blog/speculative-store-bypass-explained-what-it-how-it-works)
  * Variant 4
* [CVE-2018-3640](https://access.redhat.com/security/cve/cve-2018-3640)
  * Variant 3a
  * 詳細はあまり書かれていないが, 他に良いページも見当たらず
* [Lazy FPU Save/Restore (CVE-2018-3665)](https://access.redhat.com/solutions/3485131)
  * Lazy FP State Restore
* [TLBleed : Trasnlation Leak-aside Buffer の論文を読む](http://msyksphinz.hatenablog.com/entry/2018/07/13/040000)
  * TLBleedについての概説

## Linux kernelについて
* [詳解 Linuxカーネル 第3版](https://www.oreilly.co.jp/books/9784873113135/)
  * 定番本で参考になる部分は多い
  * とはいえだいぶ古くなってきてはいるので, 最新ではないことに気を払ったほうが良いかもしれない
* [LWN.net](https://lwn.net/)
  * Linux kernelに関わるニュース系色々…
