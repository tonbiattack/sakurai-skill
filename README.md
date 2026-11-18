# sakurai-skill

YouTube「[桜井政博のゲーム作るには](https://www.youtube.com/@sora_sakurai_jp)」の**全本編257本の字幕**から作った、ゲーム制作の原則をまとめた Agent Skill（Claude Code・DeepSeek Harness などで使える `SKILL.md`）です。

ゲームの企画・ゲーム性・難易度・ご褒美・導入・操作感（ヒットストップ、ジャンプ、カメラ）・モーション・エフェクト・グラフィック・UI・サウンド・パラメーター調整・デバッグ・チーム運営・企画書・広報を考えるとき、レビューするとき、「手応えがない」「もっさり」と言われて直すときに使います。

> 非公式のファンメイドです。桜井政博氏・有限会社ソラとは関係ありません。中身は動画の内容を AI が要約したもので、桜井氏の発言そのものではありません。正確な内容は元の動画で確かめてください。

## まずは動画で

**[X の紹介ポスト](https://x.com/MvKWt5XpPV36233/status/2120977715864252887)** に、使い方の動画（約100秒）を付けています。このスキルで何ができるか（入れ方・頼み方・手ごたえの足し方・別のAIによるレビュー・レベルデザインの見直しなど）が、作例のゲームと一緒に見られるので、先に見ておくと分かりやすいです。

## 入れ方

### Claude Code

```bash
git clone https://github.com/tonbiattack/sakurai-skill.git
cp -r sakurai-skill/skills/sakurai ~/.claude/skills/
```

Claude Code では「桜井さんならどうする？」「このゲームの手応えをレビューして」などと頼むと読み込まれます。

### DeepSeek Harness（DSH）

DSH は `SKILL.md` をそのまま読めるので、同じディレクトリを DSH のスキルルートに置くだけで使えます。

```powershell
git clone https://github.com/tonbiattack/sakurai-skill.git
# ユーザー共通（全プロジェクトで使う）
Copy-Item -Recurse -Force sakurai-skill\skills\sakurai $env:USERPROFILE\.agents\skills\
# プロジェクト単位で使う場合は <repo>\.agents\skills\ か <repo>\.dsh\skills\ に置く
```

`-Force` を付けると、更新時に上書きできます（付けないと2回目は「既に存在します」で失敗します）。

DSH のスキルルートは優先度順に `<repo>/.dsh/skills` → `<repo>/.agents/skills` → `~/.dsh/skills` → `~/.agents/skills` です。置いたあとはセッションを開き直すか、スキル一覧が更新されるのを待つと `sakurai` が候補に出ます。`references/` はスキル本体から相対パスで読まれるので、ディレクトリごとコピーしてください。

## このフォークについて

このリポジトリは [kociden/sakurai-skill](https://github.com/kociden/sakurai-skill) のフォークです。本編の原則はそのままに、**フォーク独自の拡張**を足しています。

| 拡張 | 中身 |
|---|---|
| 14章「説明は任意、プレイ開始は最短」 | 強制チュートリアルや開始前の長文を標準解にしない。深く知りたい人向けの説明は別導線で用意する |
| `references/fork-onboarding.md` | 二層導線（すぐ遊ぶ人／詳しく知る人）の設計パターン、説明の段階構成、作成時・レビュー時のチェックリスト |

**桜井氏が提唱した原則ではありません。** 自作ゲームの運用から得た指針で、原典の根拠が確認できる部分にだけ動画番号を添え、それ以外は【独自】として区別しています。

### 適用例

- ブラウザの一画面ゲーム（ui-rush / type-rush / query-rush など）で、初回起動から説明を読まずに遊べる導線を維持したまま、「遊び方」ページを任意で追加する。
- レビュー時に「チュートリアルを足す」提案が出たとき、初回プレイまでの手数が増えていないかをセットで確認する。
- ルールが複雑なゲームで、基本ルール → 操作 → 具体例 → 攻略のコツの順に説明を段階分けし、読む量をプレイヤーに選ばせる。

## 作例：ハジキ

<img src="examples/hajiki/hajiki_icon.gif" width="160" align="right">

このスキルを読み込んだ AI エージェントが、企画から磨き込み・タイトル・アイコンまで作った Godot 4.6 の一画面アクションです。敵の弾を引きつけるほど強く打ち返せます。

- [unityroom で遊ぶ](https://unityroom.com/games/hajiki)
- [このゲームでスキルのどこを参照したか（動画番号つき）](examples/hajiki/README.md)

<br clear="right">

## 中身

```
skills/sakurai/
  SKILL.md            原則の要約（15章）と、レビュー・企画・手応えの直しの手順
  references/         動画ごとの要点をカテゴリ別に並べたもの（必要なときだけ読む）
    gameplay.md  planning.md  concepts.md  specs.md  motion.md  effects.md  graphics.md
    ui.md  sound.md  tech.md  team.md  mindset.md  pr.md  misc.md
    fork-onboarding.md  このフォークの拡張（説明は任意・プレイ開始は最短）
tools/
  to_text.py          字幕（srv1）を時刻なしの本文にする
  build_refs.py       要点メモをカテゴリ別の references/ に並べ替える
```

- `SKILL.md` は約3.2万字（UTF-8 で約32KB）。読み込みを軽くするため、具体的な数値・手順・作品例は `references/` に分け、必要なカテゴリだけを読む作りです。
- 各要点には動画の通し番号（古い順の 001〜259）とタイトルを付けているので、元の動画をたどれます。
- `fork-onboarding.md` だけは動画由来ではなく、このフォーク独自の拡張です（原典由来の部分には動画番号を付けています）。

## 既存の Sakurai-Skills との違い

同じチャンネルを元にしたスキルに [kokuren333/Sakurai-Skills](https://github.com/kokuren333/Sakurai-Skills) があります（このリポジトリより先に公開されています）。目的は近いですが、作り方と向きが違います。

| | kokuren333/Sakurai-Skills | このリポジトリ |
|---|---|---|
| 元にした物 | まとめ動画（雑談を除く）を Gemini が要約した中間メモ。README によると動画との照合はしていない | 全本編257本の自動字幕を直接取得して要約 |
| 範囲 | 原則177件＋横断原則17件 | 動画257本すべて（雑談回も `misc.md` に収録） |
| 言語 | 英語が中心（日本語 README あり） | 日本語 |
| 形 | 24個のタスク別スキル、ルーター、チェックリスト、原則グラフ、lint ルール、テストまで揃った体系的な作り | スキル1つ（SKILL.md＋参照15ファイル）の軽い作り |
| 出典のたどり方 | SOURCE / GENERALIZED / AGENT の3層で、元メモの記述と解釈を分けて記録 | 要点ごとに動画番号とタイトル |
| 向いている使い方 | 大きなプロジェクトで、工程ごとのレビューや点検を体系的に回す | 普段の会話で手軽に読み込み、具体的な数値・手順・作品例まで引く |

どちらも AI による要約で、人の目で全部を照合したものではありません。このリポジトリは元が自動字幕なので、聞き取りの誤りが要約に残っている可能性があります。

## 注意

- 原則は桜井氏の大作・アクション寄りの経験に基づくので、ジャンルや規模に合わない所はそのまま当てはめず、狙い（遊ぶ人を楽しませること）から読み替える前提で書いています。
- 14章と `references/fork-onboarding.md` は**このフォーク独自の拡張**で、桜井氏の見解ではありません。引用するときは「桜井氏の動画では」ではなく「このフォークの指針では」と書いてください。
- 誤りや抜けを見つけたら issue で教えてください。
