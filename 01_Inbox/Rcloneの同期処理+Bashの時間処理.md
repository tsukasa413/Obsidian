---
tags:
  - Ubuntu
---
### Rcloneの同期処理+Bashの時間処理
1. 現在のマウントを停止
```
systemctl --user stop rclone-gdrive.service
systemctl --user disable rclone-gdrive.service
```
2. ローカルにVault用のフォルダを準備
	中身をあらかじめコピーしておく必要がある
3. 同期用のスクリプトを作成する
```
	mkdir -p ~/.local/bin
	mkdir -p ~/.logs
	nano ~/.local/bin/sync-obsidian.sh
```
	nano
```
#!/bin/bash