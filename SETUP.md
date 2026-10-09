# Windows と Mac で GitHub を使う手順

このリポジトリ（`iao3350/horyu-musicschool`）を Windows と Mac の両方で編集するための手順です。

## 1. Git をインストール

| | 方法 |
|---|---|
| **Mac** | ターミナルで `git --version` を実行。入っていなければ自動でインストール案内が出る |
| **Windows** | https://git-scm.com/download/win から「Git for Windows」をインストール（設定はすべて標準のままで OK） |

GUI で操作したい場合は両方とも [GitHub Desktop](https://desktop.github.com/) が便利です。

## 2. 最初に一度だけやる設定（両方のパソコンで）

ターミナル（Windows は「Git Bash」）で実行します。

```bash
git config --global user.name  "あなたの名前"
git config --global user.email "iao3350@gmail.com"

# 日本語ファイル名（練習用/ など）を文字化けさせない
git config --global core.quotepath false
```

改行コードの設定：

```bash
# Mac
git config --global core.autocrlf input

# Windows
git config --global core.autocrlf false
```

> 改行コードはリポジトリの `.gitattributes` で LF に統一しているので、
> Windows でも Mac でも同じ内容になります。

Mac だけ、日本語ファイル名の扱いを揃えるためにこれも実行：

```bash
git config --global core.precomposeunicode true
```

## 3. GitHub にログインできるようにする

一番かんたんなのは **GitHub Desktop** か **GitHub CLI** でログインする方法です。

```bash
# GitHub CLI（https://cli.github.com/）を入れた場合
gh auth login
```

- Windows の Git for Windows には「Git Credential Manager」が入っているので、
  初回の `git push` 時にブラウザでログインすれば以後は自動です。
- Mac は初回ログイン情報が「キーチェーン」に保存されます。

## 4. リポジトリを取ってくる（clone）

```bash
git clone https://github.com/iao3350/horyu-musicschool.git
cd horyu-musicschool
```

## 5. 毎日の作業の流れ（ここが一番大事）

**作業を始める前に必ず pull、終わったら必ず push** すると、Windows と Mac で食い違いが起きません。

```bash
# ① 作業前：もう一方のパソコンでの変更を取り込む
git pull

# ② ファイルを編集する

# ③ 作業後：変更を保存して GitHub に送る
git add .
git commit -m "何を変えたかを書く"
git push
```

GitHub Desktop なら「Fetch origin / Pull」→ 編集 → 「Commit」→「Push origin」の順です。

## よくあるトラブル

- **`git push` が拒否された（rejected）**
  → もう一方のパソコンの変更をまだ取り込んでいません。`git pull` してから `git push`。
- **コンフリクト（conflict）が出た**
  → 両方のパソコンで同じ箇所を編集した状態です。ファイル内の `<<<<<<<` 〜 `>>>>>>>`
  の部分を正しい内容に直して保存し、`git add .` → `git commit` → `git push`。
- **`.DS_Store` や `Thumbs.db` が変更として出てくる**
  → `.gitignore` で無視するようにしてあるので、通常は出ません。
