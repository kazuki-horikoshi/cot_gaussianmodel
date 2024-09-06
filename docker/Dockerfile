# ベースイメージ
FROM python:3.10-slim

# 作業ディレクトリを設定
WORKDIR /app

# システム依存関係をインストール
RUN apt-get update && apt-get install -y \
    build-essential \
    libopenblas-dev \
    liblapack-dev \
    && rm -rf /var/lib/apt/lists/*

# PythonライブラリとJupyterをインストール
RUN pip install --no-cache-dir \
    numpy \
    pandas \
    matplotlib \
    gpflow \
    scikit-learn \
    jupyter

# hashlibを使ってパスワードを生成
RUN echo "import os; import hashlib; password = os.environ.get('JUPYTER_PASSWORD', 'default_password').encode('utf-8'); hash = hashlib.sha1(password).hexdigest(); salt = os.urandom(4).hex(); print(f'sha1:{salt}:{hash}')" | python3 > .jupyter_pw

# Jupyter設定ファイルを作成し、パスワードを反映
RUN jupyter notebook --generate-config && \
    echo "c.NotebookApp.password = open('/app/.jupyter_pw').read().strip()" >> /root/.jupyter/jupyter_notebook_config.py

# ポートを開放
EXPOSE 8888

# Jupyterを起動
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]
