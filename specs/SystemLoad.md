# :traffic_light:システム負荷対策
システム負荷対策

![システム負荷対策](https://github.com/commerble/ecspec/blob/master/specs/media/systemload.jpg)

# システム負荷対策


## 前提
- 前提としてアプリケーション側でパフォーマンスチューニングや適切なコーディングが出来ていることが重要。
- 単に台数を増やすということは、チューニング出来ていない不完全なインフラを拡張することになり不利益が大きい。
- システムアーキテクチャで対応する事と、プログラミングで対応する事があり、それぞれ説明する。


# システムアーキテクチャで対応する場合の基本指針
- システム負荷対策にはスケールアップとスケールアウトの方式がある。
- スケールアップとは当該システムのCPUやメモリなどのスペックを上げることで、スケールアウトは水平にサーバーを足していくこと。
- 一般的にWEBサーバーは水平にスケールアウトがしやすく、データベースなどはスケールアップして対応するケースが多い。
- 上記が一般的なシステム負荷対処であるが、ECシステム負荷対策の基本方針として、オリジンサーバーへの負荷を減らすことが重要。
- オリジンサーバーへの負荷を減らすために、キャッシュの多用やサービス分割を行っていく。
- キャッシュはシステム上の複数の層で行うことが出来るので、後述する。
- サービス分割とは、検索やレコメンドのサービスを別に切り出したり、通販基幹システムをECフロントとは別にしたりすることである。


## キャッシュ
- CDN(Contents Delivery Network)、


### CDN(Contents Delivery Network)
- リクエストに対するレスポンスを単一のサーバーから配信すると当然負荷が高まる。
- CDNとはContents Delivery Network で外部のサーバーからコンテンツ配信を行うため、オリジンサーバーへの負荷を減らすことが出来る。
- 具体的には商品の画像を中心としたメディアをCDNから配信することが出来る。
- HTML自体もCDNにキャッシュすることが出来る仕様にすれば、よりオリジンサーバーへの負荷を減らすことが出来る。
- Google bot やその他botのクロールによる大量アクセスがあっても問題がなくなる。
- CDNはCloudfrare、Cloudfront、Akamai、KeyCDN、Stackpath, Verizonなど複数社が提供している。
- 付加機能としてWAF(Web Application Firewall)やその他有用なネットワーク機能を提供するCDNサービスがあるので、価格など含めて検討する。
- キャッシュするメディアの容量によっては高額になるので、商品画像などの軽量化や最適化をきちんと行ったうえで利用する。


### DBでのキャッシュ


### ページでのキャッシュ



## サービス分割


### 検索
- 商品情報を検索するのは負荷が非常に高い。商品点数が増えるほどに高くなる。
- よって、検索自体を別サーバーや別サービス(以降検索サービスと呼びます)で行い、EC機能(購入のトランザクション)とは別にする。
- よくある例としては、商品情報を特定のタイミング(数時間単位か、数分単位)か、検索サービス(ASP)に全件送るケースが多い。
- 商品の全件取得は、件数やデータ構造によっては負荷が高いので、注意する。
- 検索サービスとECを切り離すと、商品情報や在庫情報の連携(商品追加や価格をはじめとした商品情報の変更、在庫の変更)にタイムラグが生まれる。タイムラグを考慮した仕様、画面仕様にすること。


### ECのフロントエンドとEC基幹システムを別にする
- 管理側で受注情報や商品情報の大量ダウンロードや更新を行うと、フロントサイトのパフォーマンス自体に影響を及ぼすことがある。
- ECのフロントエンドとEC基幹システムを別にする(データベースを別にする)と、バックエンドの業務がフロントエンドに負荷を与えるようなことがなくなる。



## プログラミングで対応する事


### 非同期
- 外部API(決済、検索等)を呼び出した際に同期処理だと先方の速度によってはこちらの処理が待ち状態となり、サーバー負荷が不要に上がることがある。
- 非同期にすることによって、待ち状態や待ち行列を減らすことが出来る。


### データの大量出力



## 終わりに
- システム負荷対策は比較的日進月歩領域なので、注意してトレンドを追いたい。


## 目次に戻る
- [目次に戻る](https://github.com/commerble/ecspec/blob/master/Readme.md)

