# syntax=docker/dockerfile:1

# Python 3.14.2-slim-trixie
ARG PYTHON_VERSION_SHA256=sha256:2751cbe93751f0147bc1584be957c6dd4c5f977c3d4e0396b56456a9fd4ed137
# uv:0.9.18
ARG UV_VERSION_SHA256=sha256:5713fa8217f92b80223bc83aac7db36ec80a84437dbc0d04bbc659cae030d8c9

####################################################
# uvを利用するためのステージ
# バージョンを参照するために別ステージで定義
# COPY --formで変数が利用できないため
####################################################
FROM ghcr.io/astral-sh/uv@${UV_VERSION_SHA256} AS uv

####################################################
# ビルドステージ
####################################################
FROM python@${PYTHON_VERSION_SHA256} AS builder
COPY --from=uv /uv /uvx /usr/local/bin/

# 作業ディレクトリを/appに変更
WORKDIR /app

# 依存関係のインストール
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --locked --no-install-project --no-editable

# ソース一式を中間イメージにコピー
COPY . /app

# Syncを実行
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync --locked --no-editable --no-dev

####################################################
# 最終ステージ、実行用のビルド
####################################################
FROM python@${PYTHON_VERSION_SHA256} AS runner
WORKDIR /app

RUN groupadd -r -g 10001 app && useradd -r -u 10001 -g app app

COPY --from=builder --chown=app:app /app/.venv /app/.venv

ENV PATH="/app/.venv/bin:${PATH}"
USER app

CMD ["app"]
