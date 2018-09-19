# 仕組み

* 何らかの形で, page tableに次のようなエントリを仕込む
  * アドレス: 読み取りたいメモリの物理アドレス(攻撃対象)
  * present bit: 0
* その状態で対象のアドレスをロードしようとすると, page faultが発生する
  * そのため通常は, present=0のPTEの保持するアドレスが示すpageの中身をロードできることはない
* IntelのCPUに見つかった不具合で, 一定の条件を満たすと無効なはずのアドレスを読みに行ってしまう
  * TLBにPTEが乗っていて, L1 cacheにデータが乗っている場合が該当
  * present=0である頻度が少ないため, アクセスの高速化の目的で**present bitの確認よりわずかに先にL1 cacheを読みに行く**らしい
    * キャッシュは高速であることが望まれるため, そのアクセスにはその他色々な工夫がある
      * e.g.) VIPT: キャッシュは物理アドレスに対して保持するが, アドレス変換の時間を惜しんで, 変換と並行してページ内アドレスでキャッシュを検索し始める
* 最終的にはpage faultになるが, 例によってSpectre/Meltdownと同様にしてキャッシュを介してデータを読み出せてしまう

## page tableの誘導
* page tableの操作はkernelの仕事であるため, ユーザー権限のプログラムが自由に書き換えられるわけではない
* Linuxでは, 退避したデータの場所を示すためのデータなどを PTE の残りの箇所に格納している
  * Intelもpresent=0のPTEのアドレスのフィールドは無視されるとManualに書いているので本来問題ない
  * この時に入っている値に基づいて物理メモリにアクセスされる
    * が, 容易にコントロールな値とは思えない
  * 他のOSでも同じようなことはよくしているらしい
* 仮想マシンの場合, kernel自体に手を入れられるのであればpage tableの操作自体は可能だが, hostのpage tableを直接弄ることができるわけではない
  * guest virtual address -> guest physical address -> host physical address という関係
  * この状況での処理を高速化するために Extended Page Table (EPT) という仕組みがあり, 広く使われている
* present bit = 0 の場合の処理に関して, Intel の EPT は更に不具合を抱えていた
  * guest virtual -> guest physical のアドレス変換において present bit = 0 を扱うとき, guest physical -> host physical のアドレス変換は行われないが, **このとき guest physical address が host physical address として扱われてしまう問題がある**
  * つまり, guest VMに対してOSレベルまでコントロールできるなら, guestのpage tableは好きにでき, 不具合のおかげでL1 cache上にある好きなhost physical addressの内容をサイドチャネル攻撃を用いて読み出せてしまう
