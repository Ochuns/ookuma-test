# 🛠 ハンズオン手順書

この手順に沿って、「ブランチを作って Pull Request を出してマージする」までの一連の流れを体験します。
コマンドの `taro` の部分は、自分の名前(ローマ字)に置き換えてください。

---

## 0. 事前準備

- Git がインストールされていること(`git --version` で確認)
- GitHub アカウントを持っていること
- 名前とメールアドレスを設定しておく(初回のみ)

```bash
git config --global user.name "自分の名前"
git config --global user.email "GitHubに登録したメールアドレス"
```

---

## 1. リポジトリをクローンする

> **clone** = GitHub 上のリポジトリを、変更履歴ごと自分の PC にコピーすること。

```bash
git clone git@github.com:Ochuns/okaka-test.git
cd okaka-test
```

HTTPS を使う場合はこちら:

```bash
git clone https://github.com/Ochuns/okaka-test.git
cd okaka-test
```

---

## 2. 今の状態を確認する

```bash
git status      # 今いるブランチと、変更の有無がわかる
git branch -a   # ブランチの一覧(remotes/ から始まるのは GitHub 側のブランチ)
```

---

## 3. develop ブランチに移動して、最新の状態にする

> 作業ブランチは、必ず **最新の develop** から作るのがルールです。

```bash
git switch develop
git pull origin develop
```

---

## 4. 作業ブランチを作る

```bash
git switch -c feature/add-taro
```

> `-c` は create の意味。「develop から枝分かれした、自分専用の作業場所」ができました。
> ここで何をしても、develop や main には一切影響しません。

---

## 5. ファイルを追加する

`members/` フォルダに、自分の名前のファイルを作ります。
サンプル(`members/_sample.md`)をコピーして書き換えるのが簡単です。

```bash
cp members/_sample.md members/taro.md
```

エディタで `members/taro.md` を開いて、自己紹介を書いてください。

---

## 6. 変更を記録する(add & commit)

```bash
git status                                # 変更されたファイルを確認
git add members/taro.md                   # コミットする対象を選ぶ(ステージング)
git commit -m "add: taro の自己紹介を追加"  # 変更をひとまとまりとして記録
```

> **commit** = 「変更のセーブポイント」。メッセージには「何をしたか」を書きます。

---

## 7. GitHub にプッシュする

```bash
git push origin feature/add-taro
```

> **push** = 自分の PC のコミットを GitHub にアップロードすること。
> これで自分のブランチが GitHub 上にも作られます。

---

## 8. Pull Request(PR)を作る

1. ブラウザで GitHub のリポジトリページを開く
2. 「**Compare & pull request**」という緑のボタンが出ているので押す
   (出ていなければ「Pull requests」タブ →「New pull request」)
3. **base: `develop`** ← **compare: `feature/add-taro`** になっていることを確認
   ⚠️ base が `main` になっていたら `develop` に変える!
4. タイトルと説明を書いて「**Create pull request**」

> **Pull Request** = 「私の変更を develop に取り込んでください」というお願い。
> チーム開発では、ここで他のメンバーがレビュー(確認)をします。

---

## 9. レビューしてマージする

1. PR ページの「**Files changed**」タブで変更内容(差分)を確認
2. 問題なければ「**Merge pull request**」→「**Confirm merge**」

これで自分の変更が develop に取り込まれました 🎉

---

## 10. 後片付け

マージが終わった作業ブランチは、もう役目を終えたので削除します。

```bash
git switch develop
git pull origin develop        # マージ結果を手元に取り込む
git branch -d feature/add-taro # ローカルの作業ブランチを削除
```

---

## 🔥 発展編: コンフリクト(競合)を体験する

2人以上でやると体験できます。

1. 2人がそれぞれ develop から作業ブランチを作る
2. **2人とも** `greeting.md` の同じ行(「今日のあいさつ」)を書き換えてコミット & プッシュ
3. それぞれ PR を作り、1人目が先にマージする
4. 2人目の PR に「**This branch has conflicts**」と表示される ← これがコンフリクト!

### 解消のしかた(2人目の操作)

```bash
git switch develop
git pull origin develop            # 1人目の変更を取り込む
git switch feature/自分のブランチ
git merge develop                  # ここでコンフリクト発生
```

ファイルを開くと、こんなマークが入っています:

```
<<<<<<< HEAD
自分の変更
=======
相手の変更
>>>>>>> develop
```

マークを消して「正しい最終形」に手で直したら:

```bash
git add greeting.md
git commit -m "fix: コンフリクトを解消"
git push origin feature/自分のブランチ
```

PR のページに戻ると、マージできるようになっています。
