ARG VARIANT="3.13-slim-bookworm"
FROM python:${VARIANT}

# 環境変数の設定 (推奨)
ENV PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=off \
    PIP_DISABLE_PIP_VERSION_CHECK=on \
    PIP_DEFAULT_TIMEOUT=100 \
    DEBIAN_FRONTEND=noninteractive

# システム依存関係のインストール
# git, タイムゾーン設定用、ビルドツール (tiktoken, pyheif等のC/Rust拡張用),
# Playwright実行依存、pyheif実行依存 (libheif-devはlibheif1に依存し、それがランタイムに必要)
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    # 基本ツール
    git \
    tzdata \
    # ビルドツール (PythonパッケージのC/C++/Rust拡張モジュールをコンパイルするために必要)
    build-essential \
    gcc \
    rustc \
    cargo \
    # pyheif のビルド・実行依存 (libheif-dev はヘッダーを、依存するlibheif1が共有ライブラリを提供)
    libheif-dev \
    libde265-dev \
    # Playwright の実行依存関係 (Chromiumを動かすために必要)
    # Playwright公式ドキュメントや公式イメージの依存関係を参考にしています。
    # (https://playwright.dev/docs/docker や mcr.microsoft.com/playwright/python の Dockerfile など)
    ca-certificates \
    fonts-liberation \
    # 以下は一般的なブラウザ実行に必要なライブラリ群です
    libasound2 \
    libatk-bridge2.0-0 \
    libatk1.0-0 \
    libatspi2.0-0 \
    libcairo2 \
    libcups2 \
    libdbus-1-3 \
    libdrm2 \
    libexpat1 \
    libfontconfig1 \
    libgbm1 \
    libglib2.0-0 \
    libgtk-3-0 \
    libnspr4 \
    libnss3 \
    libpango-1.0-0 \
    libpangocairo-1.0-0 \
    libx11-6 \
    libx11-xcb1 \
    libxcb1 \
    libxcomposite1 \
    libxcursor1 \
    libxdamage1 \
    libxext6 \
    libxfixes3 \
    libxi6 \
    libxrandr2 \
    libxrender1 \
    libxss1 \
    libxtst6 \
    wget \
    xdg-utils \
    # -- ここまでPlaywright依存 --
    && \
    # タイムゾーン設定 (日本時間)
    ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
    dpkg-reconfigure --frontend noninteractive tzdata && \
    # 不要になったパッケージの削除とクリーンアップ
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# pip のアップグレード
RUN pip install --upgrade pip

# requirements.txt のコピーとPythonパッケージのインストール
# requirements.txt は Dockerfile と同じディレクトリに配置してください
# COPY requirements.txt /tmp/pip-tmp/
# RUN pip install --no-cache-dir -r /tmp/pip-tmp/requirements.txt && \
    # Playwrightのブラウザバイナリをダウンロード (依存するシステムライブラリは上記でインストール済み)
#    playwright install && \
#    rm -rf /tmp/pip-tmp

# --- (オプション) 非 root ユーザー vscode を作成・利用する場合 ---
# ARG USERNAME=vscode
# ARG USER_UID=1000
# ARG USER_GID=$USER_UID
# RUN groupadd --gid $USER_GID $USERNAME && \
#     useradd --uid $USER_UID --gid $USER_GID -m $USERNAME --shell /bin/bash
# # USER $USERNAME

# アプリケーションの作業ディレクトリ設定、コードのコピー、実行コマンドなどは
# この後に適宜追加してください。例：
# WORKDIR /app
# COPY . /app
# CMD ["python", "your_application.py"]