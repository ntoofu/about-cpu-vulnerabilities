# 緩和策

## 影響箇所について

* ユーザープロセスとkernelがpage tableを共用している場合に起こりうるため, そのような仕組みのOS上で発生しうる
* spectreなどとは違い, page tableが共用されていなければ問題は起きないため, 基本的にOS内で影響範囲は閉じる

## 対策方法について

* LinuxはKPTI (Kernel Page Table Isolation) という機構を導入
  * ユーザープロセス実行中のpage tableからkernelの利用するメモリ領域を極力排除するもの
    * ユーザープロセスからkernelのメモリ領域を見せなくすることが大事
    * pageごとのフラグによるアクセス制御に頼っているとMeltdownの餌食になるため
    * ユーザー用とkernel用で, 1つのプロセスに対し2つのページテーブルを用意することになる
  * パフォーマンス影響があり, 特に性質上頻繁にkernelの処理に移る (=system callを良く呼ぶ) ものへの影響が大きい (I/O重視のプログラムなど)
    * ページテーブルを頻繁に切り替えるようになる = TLBがよくflushされ, TLB missが増大 = パフォーマンス低下
    * `PCID` という仕組みがあると, TLBのflushを一定度抑止できるため, パフォーマンス低下を軽減できる
      * TLBのエントリにプロセス等を識別するためのタグを仕込めるようになるもので, 都度flushする必要がなくなる
      * flushする際も `INVPCID` により PCIDごとにTLBをflushできるようになるため, 部分的にflushする制御がしやすくなる

![KPTI](https://upload.wikimedia.org/wikipedia/commons/3/33/Kernel_page-table_isolation.svg) [画像出典](https://en.wikipedia.org/wiki/Kernel_page-table_isolation)
