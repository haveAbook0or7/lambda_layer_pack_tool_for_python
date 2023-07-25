# モジュールをLambdaレイヤー化するツール
ものによってはWindows環境下でインストールしてレイヤー化したモジュールをLambda関数で使おうとするとうまく動作しないことがあるようです。[参考資料](https://designare.jp/blog/noda/lambda%E3%81%AE%E5%A4%96%E9%83%A8%E3%83%A2%E3%82%B8%E3%83%A5%E3%83%BC%E3%83%AB%E3%81%AEimport%E3%81%A7%E3%82%A8%E3%83%A9%E3%83%BC%E3%81%8C%E5%87%BA%E3%81%9F%E5%A0%B4%E5%90%88%E3%81%AE%E8%A7%A3.html)  
本PJで使用するものが該当するかは詳しく調べていませんが、動作環境と同じ環境でインストールした方が間違いはないので、このツールを作成しました。

Lambda関数のランタイムごとの動作環境については以下を参照しています。  
[Lambda ランタイム](https://docs.aws.amazon.com/ja_jp/lambda/latest/dg/lambda-runtimes.html)

## 前提条件
* docker-composeコマンドが使える環境である。

## 対応ランタイム
* python3.10
* python3.9 [デフォルト]
* python3.8
* python3.7

**python3.10環境への切り替え方法**  
* `./.dockerFiles/python3.10-x86_64/Dockerfile`を`./Dockerfile`と差し替える。

**python3.8環境への切り替え方法**  
* `./.dockerFiles/python3.8-x86_64/Dockerfile`を`./Dockerfile`と差し替える。

**python3.7環境への切り替え方法**  
* `./.dockerFiles/python3.7-x86_64/Dockerfile`を`./Dockerfile`と差し替える。

## 使い方
1. `amazonlinux/deploy/requirements.txt`にインストールしたいモジュール名を記述する。  
   ([IoTBase(顧客/Sysmex)の共通処理リポジトリ](https://gitlab.sysmex-nss.jp/iotbase/lambdalayer/-/blob/develop/open_source_library/requirements.txt)や[管理Appの共通処理リポジトリ](https://gitlab.sysmex-nss.jp/iotbase/manageapp/lambdalayer/-/blob/develop/open_source_library/requirements.txt)からコピペしてくる。)

2. `amazonlinux`フォルダ直下で以下のコマンドを入力し、コンテナを立ち上げる。  
   ```
   # ビルド
   docker-compose build
   # コンテナを立ち上げる
   docker-compose up -d
   ```

3. 以下のコマンドを入力し、コンテナ内に入る。  
   ```
   docker-compose exec app bash
   ```

4. 以下のコマンドでモジュールをインストール。  
   ```
   pip install -r /home/deploy/requirements.txt -t /home/deploy/python
   ```

5. 4.で`amazonlinux/deploy`直下に生成された`python`フォルダを丸ごとzipファイルにする。  
   ※モジュールが`python`という名前のフォルダに入っていることが重要。他の名前だと動作しない。

6. 5.で作ったzipファイルをインフラチームに渡してレイヤーを作成あるいはバージョンを作成してもらう。  
   または、コンソールからLambdaレイヤーを作成。([こちらを参照](https://gitlab.sysmex-nss.jp/iotbase/lambdalayer/-/tree/develop#aws%E3%82%B3%E3%83%B3%E3%82%BD%E3%83%BC%E3%83%AB%E4%B8%8A%E3%81%8B%E3%82%89lambda%E3%83%AC%E3%82%A4%E3%83%A4%E3%81%AE%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E3%82%92%E4%BD%9C%E6%88%90%E3%81%97%E4%B8%80%E3%81%A4%E5%89%8D%E3%81%AE%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E3%82%92%E7%B4%90%E3%81%A5%E3%81%91%E3%81%A6%E3%81%84%E3%82%8Blambda%E9%96%A2%E6%95%B0%E3%81%99%E3%81%B9%E3%81%A6%E3%81%AE%E3%83%AC%E3%82%A4%E3%83%A4%E3%82%92%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E3%82%A2%E3%83%83%E3%83%97%E3%81%99%E3%82%8B))
