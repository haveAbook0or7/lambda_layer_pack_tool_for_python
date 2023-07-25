# ランタイム python3.10, アーキテクチャ x86_64
# Amazon Linux 2
FROM amazonlinux:2

# ------------------------------------------------
# python3.10をインストール
# ------------------------------------------------
RUN echo y | yum install gcc openssl-devel bzip2-devel libffi-devel wget sudo tar make \
    & cd /opt \
    & sudo wget https://www.python.org/ftp/python/3.10.10/Python-3.10.10.tgz \
    & sudo tar xzf Python-3.10.10.tgz \
    & cd Python-3.10.10 \
    & sudo ./configure --enable-optimizations \
    & sudo make altinstall \
    & sudo rm -f /opt/Python-3.10.10.tgz
# pythonのデフォルトを3.10に変更
RUN echo 'alias python=python3.10 pip=pip3.10' >> ~/.bashrc
# ------------------------------------------------

# 設定ファイルを読み込み
RUN source ~/.bashrc

# ホスト側と同期するディレクトリ作成
RUN mkdir /home/deploy
