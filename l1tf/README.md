# L1 Terminal Fault (Foreshadow-NG)

* CVE-2018-3620 (OS)
* CVE-2018-3646 (VMM)
* メモリのswapを扱うための仕組みの中で, 無効なアドレスにCPUがアクセスを投機的に行ってしまう不具合を利用し, L1 cache上に存在する任意のアドレスのデータを読み取る
  * 仕組み的にはMeltdownに近い
* 色々な事情が重なり, 第三者がコントロールできるkernelが動く仮想化ホストが一番影響を受ける
* 2018/8 に公開された
  * 2018/1 とは違い, 公開前に各OSベンダ等にきちんと連絡がなされ, 一斉公開する動きが上手く出来ていたようで, あまり混乱はなかった印象
* [論文](https://foreshadowattack.eu/foreshadow-NG.pdf)

![Foreshadow](https://foreshadowattack.eu/foreshadow.svg) [画像出典](https://foreshadowattack.eu/)
