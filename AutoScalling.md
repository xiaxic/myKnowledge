# なぜWebサーバをAuto Scalingにしなかった？

## 理由

- BYOLライセンス管理が複雑
- OSログをローカル保存していた

## 将来的にAuto Scalingを採用する条件

- ログをCloudWatch Logs等へ集約する
- ライセンス管理方法を整理する


# Auto Scaling環境におけるライセンス管理の検討事項

## 対応案

- Marketplace版やサブスクリプション版へ移行する
- Auto Scalingの上限台数を保有ライセンス数以内に制限する


# Auto Scaling環境におけるOSログ管理

## 背景

ローカルディスクにOSログを保存している場合、インスタンス削除時にログが消失する。

## 対応案

- CloudWatch Logsへログを集約する
- rsyslogでSyslogサーバへ転送する
- ログローテーション設定を見直す
- ログ取得手順をサーバログイン前提から変更する

## 対象ログ例（RHEL）

- /var/log/messages
- /var/log/secure
- /var/log/cron
- /var/log/maillog
- /var/log/dmesg
- systemd-journaldログ

## 実装例

### CloudWatch Agent

収集対象に以下を設定する。

- /var/log/messages
- /var/log/secure
- /var/log/cron

### rsyslog

rsyslog.confで外部Syslogサーバへ転送する。

### systemd-journald

永続保存設定(Storage=persistent)を見直し、必要に応じて集約先へ転送する。
