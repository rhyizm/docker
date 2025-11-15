ARG VARIANT="3.13-alpine"
FROM python:${VARIANT}

# 基本パッケージのインストール
RUN apk update && apk add --no-cache git

# ビルド依存関係 (後で削除)
RUN apk add --no-cache --virtual .build-deps gcc musl-dev libheif-dev libde265-dev build-base rust cargo

# pip のアップグレードを最初に
RUN pip install --upgrade pip

# タイムゾーン設定
RUN apk add --no-cache tzdata && \
    ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
    echo "Asia/Tokyo" > /etc/timezone

# 依存関係インストール
COPY requirements.txt /tmp/pip-tmp/
RUN pip install --no-cache-dir -r /tmp/pip-tmp/requirements.txt && \
    rm -rf /tmp/pip-tmp

# --- (オプション) 非 root ユーザー vscode を作成・利用する場合 ---
# ARG USERNAME=vscode
# ARG USER_UID=1000
# ARG USER_GID=$USER_UID
# RUN addgroup --gid $USER_GID $USERNAME && \
#     adduser --disabled-password --gecos '' --uid $USER_UID --gid $USER_GID $USERNAME
# USER $USERNAME