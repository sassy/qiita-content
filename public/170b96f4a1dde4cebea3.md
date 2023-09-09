---
title: Glacierと Glacier Deep Archive
tags:
  - AWS
private: false
updated_at: '2020-11-08T16:10:45+09:00'
id: 170b96f4a1dde4cebea3
organization_url_name: null
slide: false
---
Glacierと Glacier Deep Archiveのメモです。
主な目的は取り出し期間をまとめたかっただけです。

# Glacierとは
- バックアップなど中長期保存用の**S3よりも安価**なストレージ(大切)
- 99.999999999％％ の耐久性
- 毎月わずか 1 USD/テラバイトでデータを保存
- 1 つのアーカイブの最大サイズは 40 TB
- 保存可能なアーカイブ数とデータ量に制限なし

## Glacierの取り出しタイプ
### 迅速
- 通常1〜5分以内(早い)

### 標準
- 3〜5時間

### 大容量
- 大量のデータを1日以内に低コストで取得
- 大体5〜12時間で完了

### プロビジョニングキャパシティ 
- 迅速取り出しの取得容量を必要なと きに利用できることを保証する仕組み

# Glacier Deep Archiveとは
- 中長期保存用ストレージタイプ
- Glacierよりも値段が安い
- データ取得はGlacierより遅い
- 最小保存期間は180日
- 基本的なデータモデル・管理はGlacierと同じ

## データの取り出し
- 標準取り出しで、データは12時間以内 (Glacierの場合は標準で3〜5時間)
- 大量取り出しで、48 時間以内にデータを取り出す大容量取り出しをすることで取得 コストを低減

# ポイント
- 1-5分以内ならGlacier(迅速)
- 3-5時間以内ならGlacier(標準)
- 12時間以内でOKならGlacier Deep Archive
