# Install Docker on Lima (M1)

## Lima & Docker のインストール

```
brew install lima docker
```

## 設定ファイルの作成

LimaのGitHubリポジトリに含まれている、[Docker用設定ファイル](https://github.com/lima-vm/lima/blob/master/examples/docker.yaml)を利用する。<br>
また、設定値を変更する場合は下記のURLを参考にする。<br>

lima-vm/lima（GitHub）: https://github.com/lima-vm/lima/blob/master/examples/default.yaml

## 仮想マシンの作成 & 初回起動

作成したyamlファイルを元に仮想マシンを作成する。
```
limactl start ./docker.yaml
```

`limactl start`コマンドを実行すると、対話形式で選択肢が表示されるので、`Proceed with the current configuration`を選択する。<br>

## 環境変数の設定

`.zshrc`に下記を追記する。
```
export DOCKER_HOST=$(limactl list docker --format 'unix://{{.Dir}}/sock/docker.sock')
```

## Docker Compose v2 のインストール

```
mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.11.2/docker-compose-darwin-aarch64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
```
curlコマンドのURL（引数）は、インストールしたいバージョンや使用のOS・CPUに応じて適宜変更する。<br>
docker/compose（GitHub）: https://github.com/docker/compose/releases<br>

下記コマンドを実行し、`Docker Compose version v2.11.2`と表示されていればインストールは成功。
```
docker compose version
```

### Homebrewでインストール

```
brew install docker-compose
```

Homebrewのdocker-composeパッケージがv2に対応しているようなので、今後はこちらを利用しても大丈夫そう。<br>
https://formulae.brew.sh/formula/docker-compose

## シェル起動時に仮想マシンを起動

PCの再起動を行うと、仮想マシンは`Stopped`の状態になる。<br>
シェル起動時に仮想マシンを起動させたい場合は、`.zshrc`に下記を追記する。
```
limactl start docker
```

## Lima コマンド

```
# VM（仮想マシン）の状態を一覧表示
limactl ls

# VM（仮想マシン）のシェルに入る
limactl shell <VM名>

# 起動
limactl start <VM名>

# 停止
limactl stop <VM名>

# 削除
limactl delete <VM名>

# キャッシュの削除
limactl prune
```

## 参考

https://github.com/lima-vm/lima<br>
https://zenn.dev/wim/articles/build_lima_environment_on_m1_mac<br>
https://zenn.dev/takasp/articles/3cf03da87d894e<br>
https://qiita.com/akinami/items/d38b9e7c7f37bd070f40<br>
