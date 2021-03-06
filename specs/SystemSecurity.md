# :traffic_light:システム負荷対策
システム負荷対策

![システム負荷対策](https://github.com/commerble/ecspec/blob/master/specs/media/systemload.jpg)

# システム負荷対策

## 基本指針
- ECの基本、最も根幹となるオリジンサーバーへの負荷を減らす。
- オリジンサーバーへの負荷を減らすために、キャッシュを適切に多用していくこと。
- キャッシュは多層で行うことが出来るため、
- 負荷対策は比較的日進月歩領域なので、注意してトレンドを追いたい。
- スケールアップとスケールアウトの方式がある。

## CDN(Contents Delivery Network)
- CDNとはContents Delivery Network で外部のサーバーからコンテンツ配信を行うため、オリジンサーバーへの負荷を減らすことが出来る。
- 具体的には商品の画像を中心としたメディアを配信することが出来る。リクエストに対するレスポンスを単一のサーバーから配信すると当然負荷が高まる。
- HTML自体もCDNにキャッシュすることが出来る仕様にすれば、よりオリジンサーバーへの負荷を減らすことが出来る。
- Google bot やその他botのクロールがあっても問題がなくなる。
- CDNはCloudfrare、Cloudfront、Akamai、KeyCDN、Stackpath, Verizonなど複数社が提供している。
- WAFやその他有用なネットワーク機能を提供するCDNサービスがあるので、価格など含めて検討する。
- キャッシュするメディアの容量によっては高額になるので、商品画像などの軽量化や最適化をきちんと行ったうえで利用する。

## 



## ECのフロントエンドとEC基幹システムを別にする
- 管理側で受注情報や商品情報の大量ダウンロードや更新を行うと、フロントサイトのパフォーマンス自体に影響を及ぼすことがある。
- ECのフロントエンドとEC基幹システムを別にすると、バックエンドの業務がフロントエンドに負荷を与えるようなことがなくなる。


## 検索
- 検索自体を別サーバーや別サービス(以降検索サービスと呼びます)で行い、EC機能(購入のトランザクション)とは別にする。
- 商品情報を検索するのは負荷が非常に高い。商品点数が増えるほどに高くなる。
- 注意点として、検索サービスとECを切り離すと、商品情報や在庫情報の連携(商品追加や価格をはじめとした商品情報の変更、在庫の変更)にタイムラグが生まれる。タイムラグを考慮した仕様、画面仕様にすること。
- 

## 非同期
- 外部API(決済、検索等)を呼び出した際に同期処理だと先方の速度によってはこちらの処理が待ち状態となり、サーバー負荷が不要に上がることがある。
- 非同期にすることによって、待ち状態や待ち行列を減らすことが出来る。



## 目次に戻る
- [目次に戻る](https://github.com/commerble/ecspec/blob/master/Readme.md)

