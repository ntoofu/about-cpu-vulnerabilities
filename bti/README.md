# Branch Target Injection

* CVE-2017-5715
* **分岐予測の仕組みを悪用** し, 自分の実行したいコードを投機的に実行させる
* Project ZeroではKVMにおいてGuest VMからHostのメモリを読み出すことに成功しているすごいやつ
  * ただし実践する難易度も理解する難易度もすごいやつ(私見)
* Spectre として 2018年1月に騒いでいたやつの1つ
* Variant 2 とも
