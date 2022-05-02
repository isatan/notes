# Git の SSH 認証で複数アカウントを利用する方法

## このページで共有したいこと

Bitbacket の外部接続認証は SSH 鍵ファイルの利用が必須です。  
プライベートと職場、双方で Bitbacket を利用するような場合の複数アカウントの設定方法を共有します。  

ここでは Bitbacket を例に解説していますが、他の Git サービスでも同様の手順で対応できると思います。

## 手順

1. アカウント毎に SSH 鍵ファイルの生成、Bitbacket への設定を行います。手順は公式ドキュメントを参照してください。
    1. 参照）[SSH キーをセットアップする](https://support.atlassian.com/ja/bitbucket-cloud/docs/set-up-an-ssh-key/)
1. SSH config の設定
    1. 下記ファイルに後述する設定を追加します。
        1. ~/.ssh/config
        ``` bash
        Host hoge   # 設定名称その1。任意の値を設定します。
            HostName bitbucket.org
            IdentityFile ~/.ssh/id_rsa_hoge # 1つ目のアカウントに設定した SSH 秘密鍵ファイル
            User git
            AddKeysToAgent yes
        Host huga   # 設定名称その2。任意の値を設定します。
            HostName bitbucket.org
            IdentityFile ~/.ssh/id_rsa_huga  # 2つ目のアカウントに設定した SSH 秘密鍵ファイル
            User git
            AddKeysToAgent yes
        ```
1. 下記コマンドで SSH に秘密鍵を登録します。
    ``` bash
    % ssh-add -K ~/.ssh/id_rsa_hoge
    % ssh-add -K ~/.ssh/id_rsa_huga
    ```
1. 正しく設定できていれば下記コマンドで認証を確認できます。引数に指定する値は SSH に設定した設定名称です。
    ``` bash
    $ ssh -T hoge
    authenticated via ssh key.

    $ ssh -T huga
    authenticated via ssh key.
    ```

以上

## 参考

- Bitbacket サポート - SSH キーをセットアップする
    - https://support.atlassian.com/ja/bitbucket-cloud/docs/set-up-an-ssh-key/
- Multiple SSH Keys settings for different github account
    - https://gist.github.com/jexchan/2351996