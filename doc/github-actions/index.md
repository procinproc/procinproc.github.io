# 命名規約

このドキュメントおよび GitHub Actions 実装では、以下の名称を用います。

## ワークフローのファイル名について

リポジトリ/.github/workflow/*.yml として設置するワークフローの命名規約です。

* test.yml

	* git push や pull request を契機に、テストを実行するワークフローの名称

* rpm.RPM名称.yml

	* source RPM および binary RPM を生成するワークフローの名称

* docker.Dockerイメージ名称.yml

	* Docker イメージを生成するワークフローの名称

* 対象ワークフローファイル名.invoke.yml

引数つきの repository_dispatch を Web UI で起動する用

	* workflow_dispatch すなわち WEb UI を契機に、
	  他のワークフローを repository_dispatch で起動するワークフローは、
	  起動対象のファイル名に .invoke を付加した名称とします

## ワークフローの name について

ワークフローの name 欄は、GitHub Actions の Web UI で表示される項目です。
以下のように設定します

* 動詞で開始する命令文

	* on workflow_dispatch を含む (すなわち手動 Web UI で起動できる) ワークフローで用います

	* 例

		* create pip-glibc RPM
		* create pip-glibc Docker image

* 名詞句

	* on workflow_dispatch を含まない、内部実装用途のワークフローで用います

	* 例

		* pip-glibc RPM
		* pip-glibc Docker image

## ワークフローで利用する変数の名称

「〜s 」のように複数形の場合は、
リストないしカンマ区切りの文字列形式で複数の要素を保持します

* distro

	* centos7 ないし centos8 の値をとります

* platform

	* Docker の実行環境名です

	* linux/amd64 ないし linux/arm64 の値をとります

* arch

	* platform から「linux/」を削除した名称です

	* amd64 ないし arm64 の値をとります

* archtype

	* Docker のイメージ種別です

	* arch でとりうる値に加え、さらに「multiarch」という値をとります

# 実装したワークフロー

以下「リポジトリ名:ワークフローファイル名」という形式で記述します。

client_payload は repository_dispatch で受け取る入力引数です。

## PiP-glibc リポジトリ

* PiP-glibc:test.yml

	* PiP-glibc のテスト (実際にはビルドのみ)

	* push, pull-request, workflow_dispatch で起動します

* PiP-glibc:rpm.pip-glibc.yml

	* PiP-glibc の RPM のビルド

	* repository_dispatch で起動します。

	* client_payload は以下の通りです

		* distros: centos7 centos8
		* archs: amd64 arm64

* PiP-glibc:rpm.pip-glibc.invoke.yml

	* rpm.pip-glibc.yml の呼び出し用です

	* workflow_dispatch で起動します

* PiP-glibc:docker.pip-glibc.yml

	* repository_dispatch で起動します。

	* client_payload は以下の通りです

		* distros: centos7 centos8
		* archtypes: multiarch amd64 arm64

* PiP-glibc:docker.pip-glibc.invoke.yml

	* docker.pip-glibc.yml の呼び出し用です

	* workflow_dispatch で起動します


## PiP リポジトリ

* PiP:test.yml

	* repository_dispatch で起動します。

	* client_payload は以下の通りです

		* distros: [centos7, centos8]
		* archs: [amd64, arm64]
		* pip_versions: [2, 3] or [''] if pip_ref != ''
		* pip_ref: commit_hash or ''
		* pip_testsuite_ref: commit_hash or ''
		* dispatch: true or false

* PiP:test.invoke.yml

	* test.yml の呼び出し用です

	* push, pull-request, workflow_dispatch で起動します

* PiP:rpm.pip.yml

	* repository_dispatch で起動します。

	* client_payload は以下の通りです

		* distros: centos7 centos8
		* archs: amd64 arm64
		* pip_versions: 2 3

* PiP:docker.pip-glibc-libpip.yml

	* repository_dispatch で起動します。

	* client_payload は以下の通りです

		* distros: centos7 centos8
		* archtypes: multiarch amd64 arm64
		* pip_versions: 2 3

## PiP-gdb リポジトリ

* PiP-gdb:test.yml

	* push, pull-request, workflow_dispatch で起動します

* PiP-gdb:rpm.pip-gdb.yml

	* repository_dispatch で起動します。

	* client_payload は以下の通りです

		* distros: centos7 centos8
		* archs: amd64 arm64
		* pip_versions: 2 3

* PiP-gdb:docker.pip-glibc-libpip-gdb.yml

	* repository_dispatch で起動します。

	* client_payload は以下の通りです

		* distros: centos7 centos8
		* archtypes: multiarch amd64 arm64
		* pip_versions: 2 3

## PiP-Testsuite リポジトリ

* PiP-Testsuite:test.invoke.yml

	* push, pull-request, workflow_dispatch で起動します

## PiP-pip リポジトリ

* PiP-pip:test.yml

	* push, pull-request, workflow_dispatch で起動します

* PiP-pip:docker.pip-prep.yml

	* push および workflow_dispatch で起動します

* PiP-pip:docker.process-in-process.yml

	* repository_dispatch で起動します。

	* client_payload は以下の通りです

		* distros: centos7 centos8

		* pip_versions: 2 3

		* archtypes 引数はない。
		  これは、の限定は RPM や Docker イメージの生成では必要だが
		  エンドユーザー向け Docker イメージ提供が目的の
		  このワークフローでは multiarch のみの提供で十分なため

* PiP-pip:docker.process-in-process.invoke.yml

	* workflow_dispatch で起動します。

	* docker.process-in-process.yml の呼び出し用

# 実装したワークフロー間の呼び出し関係

repository_dispatch 機能による、ワークフロー間の呼び出し関係を示します。

括弧内はイベント名です。

## PiP-glibc リポジトリ

* PiP-glibc:test.yml
  → (pip-glibc-test-ok) → PiP-glibc:rpm.pip-glibc.yml

	* ※ pull-request の場合は dispatch しない

* PiP-glibc:rpm.pip-glibc.invoke.yml
  → (pip-glibc-rpm-invoke) → PiP-glibc:rpm.pip-glibc.yml

* PiP-glibc:rpm.pip-glibc.yml
  → (pip-glibc-rpm-built) → PiP-glibc:docker.pip-glibc.yml

* PiP-glibc:docker.pip-glibc.invoke.yml
  → (pip-glibc-docker-invoke) → PiP-glibc:docker.pip-glibc.yml

* PiP-glibc:docker.pip-glibc.yml
  → (pip-glibc-built) → PiP:test.yml

## PiP リポジトリ

* PiP:update.yml
  → (pip-update) → PiP:test.yml

	* ※ pull-request の場合は dispatch: false を渡す

* PiP:test.invoke.yml
  → (pip-test-invoke) → PiP:test.yml

* PiP:test.yml
  → (pip-test-ok) → PiP:rpm.pip.yml

* PiP:rpm.process-in-process.yml
  → (pip-rpm-built) → PiP:docker.pip-glibc-libpip

* PiP:docker.pip-glibc-libpip
  → (pip-built) → PiP-gdb:test.yml

## PiP-Testsuite リポジトリ

* PiP-Testsuite:test.invoke.yml
  → (pip-testsuite-update) → PiP:test.yml

	* ※ pull-request の場合は dispatch しない

## PiP-Testsuite リポジトリ

* PiP-Testsuite:update.yml
  → (pip-testsuite-update) → PiP:test.yml

	* ※ 常に dispatch: false を渡す

## PiP-gdb リポジトリ

* PiP-gdb:test.yml
  → (pip-gdb-test-ok) → PiP:rpm.pip.yml

	* ※ pull-request の場合は dispatch しない

* PiP-gdb:test.yml
  → (pip-gdb-test-ok) → PiP-pip:test.yml

	* ※ pull-request の場合は dispatch しない

* PiP-gdb:rpm.pip-gdb.yml
  → (pip-gdb-rpm-built) → PiP-gdb:docker.pip-glibc-libpip-gdb.yml

* PiP-gdb:docker.pip-glibc-libpip-gdb.yml

## PiP-pip リポジトリ

* PiP-pip:test.yml
  → (pip-pip-test-ok) → PiP-pip:docker.process-in-process.yml

	* ※ pull-request の場合は dispatch しない

* PiP-pip:docker.pip-prep.yml
  → (pip-prep-docker-built) → PiP-glibc:docker.pip-glibc.yml

