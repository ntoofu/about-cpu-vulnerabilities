# Lazy FP State Restore

* 別のコンテキスト(プロセス)が利用したFPU(浮動小数点演算ユニット)用のレジスタの値をサイドチャネル攻撃にて読み取る
  * 通常, レジスタの値はコンテキストスイッチ時に退避/復元されるが, FPUを使わないプロセスも多いため, 必要になるまで退避を行わないようにすることがある (=Lazy save/restore)
  * Lazy save/restore では, コンテキストスイッチ後にFPUを利用する際に例外を上げて退避処理を行うようにする
    * おそらく, この際にMeltdownと同等のことが出来るはず
* CVE-2018-3665

## 緩和策
* Eager restore (Lazy restoreの逆で, 毎回ちゃんと退避/復元する) するようにOSやhypervisorが対応すればよい
  * Sandy Bridge以降はeager restoreをH/Wで支援する仕組みがあるらしく, 既にLinuxではeager restoreするようになっていたりする
  * パフォーマンス影響も特に話題になっていない
