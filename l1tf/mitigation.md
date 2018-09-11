# 緩和策

## 影響箇所について

* もともとはSGXというIntelの提供するセキュリティ機構が影響を受けるという話だった
* OSについてもユーザープロセスから困難だとは思うが原理上はL1 cacheの内容を読み出される可能性がある
* Hypervisorについては, 悪意のあるようなguest kernelが動作しうる場合は, L1 cacheが読み出される可能性がある

## 対策方法について

* OSについては, page tableの制御を握っているため, present = 0 のエントリ作成時にadderss部分に細工をして大抵の場合で影響が出ないようにできる
  * 少なくともLinuxでは直ぐにそういうパッチが出ている
    * address部分が普通のマシンでは存在することがないレベルで大きなphysical addressを指し示すように, ビット反転等で細工する
  * パフォーマンス影響は極めて軽微
* Hypervisorについてを含め, 根本的には L1 cacheを適宜flushする必要がある
  * gusetの処理に移るたびにL1 cacheをflushしておけば, 基本的には余計なものは漏洩しないはず
  * パフォーマンス影響はあるものの, L1 cacheだけのflushであればある程度に収まると考えられている
  * Microcode updateがあり, それによってflushを効率的にできる
* ただし, Intel Hyper Threading TechnologyのようなSimultaneous Multi-thread (SMT) の場合は, 更に厄介
  * 論理スレッドがL1 cacheを共有してしまっているので, L1 cacheを適宜flushするだけではダメ
  * 盗み見られたらマズい処理と, 攻撃者のものかもしれない処理を, 同じSMTのスレッドのペアに割り当てないようにするしかない
  * (実は, Spectre variant 2でも同様に, SMTの場合はBTBやBHBを共有しているためにIBPB等で完全に対処できるとは言い切れない事情があったりする)
