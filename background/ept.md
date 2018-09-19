# Extended Page table

## 背景

* 通常のpage walkは, x86ではCPUが実施してくれる
* 仮想環境では, guest VMが思っているphysical addressはhost physical addressとは異なるため, 直接MMUを使ってもらっては困る
* shadow paging
  * guestのpage tableの更新を色々な方法で追跡し, host側にも似たようなpage table(shadow page table)を作る
  * ただし, shadow page tableはguest physical addressではなくhost physical addressを記す
  * CPUからは, shadow pageを参照するようにする
  * 遅い
    * page tableの更新に追従するために, 都度guestの処理からhypervisorの処理に遷移しなければならない

## EPTとは

* 仮想環境でのメモリアクセスのハードウェア支援機構
  * Nehalem以降で入ったらしい
* CPUに対し, page tableだけでなく, guest physical -> host physicalの変換表(EPT)も指定
* bare metalの場合と同様に, guestのpage tableを使ってアドレス変換をした後, EPTにより更に変換をする形になる

![EPT](http://virtualization.info/images/EPT-716833.png) [画像出典](http://virtualization.info/en/news/2008/03/intel-to-introduce-new-virtualization.html)
