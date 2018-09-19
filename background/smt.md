# Simultaneous Multi-threading

* SMT (Simultaneous Multi-threading) は, 1つのCPUコアで複数の論理的なスレッド(一連の処理のコンテキスト)を動作させるテクニック
* CPUに対し回路の追加はわずかながら, ある程度の性能向上が見込める

## 背景

* Out-of-Order 実行などを行っても, 結局cache miss等が起きたりすることでストールが起こることは避け得ない
* さらなる性能向上のためには, cache miss等で発生した長い待ち時間もCPUを遊ばせないことが必要

## SMTの内容

* プログラムカウンタや各種レジスタを追加して, 複数の命令列を同じCPUの命令実行パイプラインに流すようにする
* 複数のスレッドを同時に実行できるようになり, 1つのコアで見た目上は(論理的には)コアが複数あるかのように振る舞うことになる
* こうすることで, 片方のスレッドがcache miss等で長い待ち時間に陥っても, その間もう片方のスレッドを集中的に実行するようになり, 全体ではCPUの稼働率を高められる
* スレッド間では基本的にデータの依存性が無いため, cache miss以外でも依存の解決待ち等が起こりにくくなる

![SMT](https://qph.fs.quoracdn.net/main-qimg-f499708300143b01862533bf33f30a9e) [画像出典](https://www.quora.com/How-does-multithreading-differ-from-hyper-threading)
