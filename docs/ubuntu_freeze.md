### Ubuntuがフリーズしたとき
#### pidを確認してkill
- Ctrl + Alt + F2でコマンドウィンドウに移動Ctrl + Alt + F7で戻る
- `top`でプロセスpidを確認。`kill -9 <pid>` でkill

#### Ubuntuで安全に強制終了、強制再起動するコマンド [link](https://wiki.ubuntulinux.jp/UbuntuTips/Others/MagicSysRq)
- **強制終了** Alt+PrintScreenを押しながらR+S+E+I+U+**O** を順に押す
- **再起動** Alt+PrintScreenを押しながらR+S+E+I+U+**B** を順に押す
