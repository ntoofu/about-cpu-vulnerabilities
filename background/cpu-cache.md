# キャッシュとサイドチャネル攻撃

* CPUはメモリ上のデータを読み書きするが, 自身も小容量で高速な **キャッシュ** を持っており, アクセスの高速化を図っている

![CPU-Cache-RAM](https://developers.redhat.com/blog/wp-content/uploads/2016/02/will-cohen-blog_graphics-02-300x300.png) [画像出典](https://developers.redhat.com/blog/2016/03/01/reducing-memory-access-times-with-caches/)

## なぜ必要か

* 現代的なCPUは動作周波数が数GHzあり, メインメモリに広く採用されるDRAMに比べて桁違いに早い
    * メインメモリからデータを取ってくるのには, 何百クロックというオーダーで時間がかかると言われている
* CPUにとって, メモリとのやり取りで待ち状態にならないようにすることは極めて重要
* そのために, x86 CPUの多くで3階層のキャッシュを持ち, 極力メモリへのアクセスを減らそうとする

![Cache Hierarchy](https://slideplayer.com/slide/4145290/13/images/32/Intel+Core+i7+Cache+Hierarchy.jpg) [画像出典](https://slideplayer.com/slide/4145290/)

## キャッシュの限界

* 高速動作が求められること, 回路の大きさなどの物理的な制約などから, キャッシュ自体の容量には限度がある
* したがってメモリ上にあるデータのうちキャッシュ上にも存在しているのはごく一部であり, キャッシュ上に無いものをロードしようとすると, アクセス高速化の恩恵が受けられない

## サイドチャネル攻撃との関連性

* サイドチャネル攻撃 = デバイスの動作を観測して本来秘匿されている情報を読み取る攻撃
  * e.g. 信号線から放射される電磁波から流れる情報を得る
  * e.g. 処理に要する時間の違いから情報を得る (タイミング攻撃)

![Tempest](https://pbs.twimg.com/media/DPa4hLaX0AE5HzC.jpg)[画像出典](https://twitter.com/daniel_bilar/status/934138325320871936)

* CPUからメモリへのアクセスについても, その速度の差で, キャッシュ上にデータがあったかどうかが推測可能となる
  * キャッシュ上のデータの存在はサイドチャネル攻撃により推測されうる
  * しかし単純に考えれば, データのキャッシュ上の存在有無がわかるだけである上, メモリアクセス時間を測定できる時点でそのデータを読み取れる状況にあるため, 一見特に問題は無い

### FLUSH+RELOAD

* 一例として, 攻撃者がvictimがメモリのどこにアクセスしたかを盗み見ることが出来る
  * 同じメモリ領域にアクセスできる必要があるが, 暗号処理に対して攻撃し, メモリアクセスのパターンから鍵を推定することが出来る
    * [paper](https://eprint.iacr.org/2013/448.pdf)

* 1: cacheには色々な情報が入っている
![FLUSH+RELOAD 1](/images/flush_reload-1.svg)
* 2: cacheを攻撃者がFLUSHする
![FLUSH+RELOAD 2](/images/flush_reload-2.svg)
* 3: victimがメモリをアクセスすると, cacheに読み込まれる
![FLUSH+RELOAD 3](/images/flush_reload-3.svg)
* 4a: 攻撃者がメモリをロードする(RELOAD)と, 大抵はcache missとなりデータを得るのに時間がかかる
![FLUSH+RELOAD 4](/images/flush_reload-4.svg)
* 4b: 攻撃者がメモリをロードする(RELOAD)と, victimがアクセスしたところだけcache hitとなり高速にデータを得る
![FLUSH+RELOAD 5](/images/flush_reload-5.svg)
