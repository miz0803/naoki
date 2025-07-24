# summary

|#|job template | playbook | role |
|-|-|-|-|
|1|deployfiles_to_develop|deploy_develop.yml|transfer_files|
|2|deployfiles_to_preproduciton|deploy_pre_production.yml|transfer_files|

# inventory
- セグメントの関係で1つのファイルに2環境分のグループを定義しました。
- グループ変数には、開発・プレ本それぞれ識別できる変数`env_dir`を定義しました。これによって本処理の方で読み込む変数ファイルを変更します。
- `unyo_kanri_xxx`以外はダミーグループですので用意しても用意しなくてもよいです。事前にすり合わせさせていただいたシナリオではいずれも使いません。

# role
## tasks
- 環境用の変数を読み込み、ファイルを送付する処理を書きました。
- 環境用の変数は、`vars/`配下に`環境名.yml`という形で作成しており、環境名はグループ変数から取得します。

## vars
- 環境毎に用意しています。
- `host_keys`は、配布先となるサーバのバリエーション分記載が必要です。面倒ですが。
- `file_map`配布先のサーバ（パス）とfrom・toをマッピングした変数リストです
- 例えばhost_aは、Git上の`files/host_a/xxx.sh`を運用管理サーバの`/srv/host_a/program/main/infra/batch/bin/xxx.sh`に配備します。この時Ansibleの仕様で変更が加えられていない場合、処理は行われません（`ok`となります）
- 面倒ですが、サーバ分`filemap.サーバ名`のリストを用意する必要があります。

## Playbook
- 実行するroleと実行先のグループ（inventoryに定義済のもの）を紐づけます。