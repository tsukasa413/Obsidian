---
tags:
  - Ubuntu
---
### Rcloneを用いたマウント
この方法ではかなりの頻度で同期処理をしているせいかめちゃくちゃObsidianの動作がとっても重かった。やり方は以下。
1. 同期用のフォルダを作る
2. Google Driveの設定を行う
	rcloneの設定を開始
```
rclone config
```
	New Remoteを選択
	name> gdrive(これは接続設定のニックネーム)
	Storage> 数字(これはリストの中からGoogleDriveを探し出してそれに対応した番号)
	client_id>(何も入力せずにEnterでよし)
	client_secret>(何も入力せずにEnterでよし)
	scope>1(Full access to all files, excluding Application Data Folder.)
	- `root_folder_id>` → **Enter**
    service_account_file>` → **Enter**
    Edit advanced config?` → `n` と答えて **Enter**
    Use auto config? ->y
    Configure this as a team drive? ->n

3. Rcloneでマウント
```
rclone mount gdrive: ~/GoogleDrive --vfs-cache-mode full --daemon
```
4. PC起動時に自動で接続する
```
nano ~/.config/systemd/user/rclone-gdrive.service
```
	nano
```
[Unit]
Description=Rclone Mount for Google Drive
After=network-online.target

[Service]
Type=simple
ExecStart=/usr/bin/rclone mount gdrive: %h/GoogleDrive --vfs-cache-mode full --allow-other --poll-interval 1m
ExecStop=/bin/fusermount -u %h/GoogleDrive
Restart=on-failure

[Install]
WantedBy=default.target
```
	設定を有効にする
```
systemctl --user enable --now rclone-gdrive.service
```
5. マウント設定をより強固にする
```
sudo nano /etc/fuse.conf
```
	ファイルの中に、`#user_allow_other` という行がある。 `#` を削除。