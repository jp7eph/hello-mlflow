# hello-mlflow

## How to Use

### フォアジョブの場合

```shell
docker-compose up
# 起動するので Ctrl+C で停止&削除
```

### バッグジョブの場合

```shell
docker-compose up -d
# バッググラウンドで起動

# 停止処理
docker-compose down
```

## 各種画面

| 概要 | URL |
| --- | --- |
| MLflow UI | {サーバのIP}:5001 |
| MinIO API | {サーバのIP}:9000 |
| MinIO Console | {サーバのIP}:9001 |
| Postgres Server | {サーバのIP}:5432 |
| pgAdmin | {サーバのIP}:81 | 
