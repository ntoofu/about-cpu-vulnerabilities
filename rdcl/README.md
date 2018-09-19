# Rogue Data Cache Load

* CVE-2017-5754
* Out-of-Order実行を利用してユーザープログラムからkernelのメモリ領域にアクセスし, サイドチャネル攻撃によりその内容を読み出す
* Meltdown として 2018年1月に騒いでいたやつの1つ
  * 一応, 確認した範囲では[2017年には既に指摘されている](https://cyber.wtf/2017/07/28/negative-result-reading-kernel-memory-from-user-mode/)
  * 実行が比較的簡単であり, かつ緩和策のパフォーマンス影響も大きかったため, インパクトは大きい
* Variant 3 とも

![Meltdown](https://meltdownattack.com/meltdown.svg) [画像出典](https://meltdownattack.com/)
