# ランタイム python3.8, アーキテクチャ x86_64
# Amazon Linux 2
FROM amazonlinux:2

# ------------------------------------------------
# python3.8をインストール
# ------------------------------------------------
# python3.8の実行ファイルを有効にする
RUN amazon-linux-extras enable python3.8
# python3.8をインストール
RUN yum install python3.8 -y

# pythonのデフォルトを3.8に変更
RUN echo 'alias python=python3.8 pip=pip3.8' >> ~/.bashrc
# ------------------------------------------------

# 設定ファイルを読み込み
RUN source ~/.bashrc

# ホスト側と同期するディレクトリ作成
RUN mkdir /home/deploy
