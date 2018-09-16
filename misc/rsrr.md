# Rogue System Register Read

* 投機的な実行の結果をサイドチャネル攻撃で読み出すことで, System Registerの情報をユーザー権限から得る
  * System Register = 演算の引数や結果を格納するレジスタではなく, CPUの挙動自体を制御する設定を格納するレジスタ
* 同時に公開されたSpeculative Store Buffer Bypassに比べ注目度も低く詳細な情報は分からず
* CVE-2018-3640
* Variant 3a とも
  * 位置づけとしては Variant 3 (Meltdown) の亜種となるらしい

## 緩和策
* CPUのmicrocode修正で完結すると考えられているらしい
