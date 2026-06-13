# 「作れるのに伸びないクセ診断」画像生成用プロンプト設計書（CNPキャラクター版・グラレコ図解）

本診断サイトの結果ページにおいて、診断結果の「補助教材（NG/OK比較図解）」として挿入する画像を生成するためのビジュアル設計書である。
画像生成は**ChatGPT（DALL-E 3）**などの画像生成AIで実行することを前提とし、CNP（CryptoNinja Partners）の人気キャラクターを各バイアスタイプに配役した、コピペで使える英語プロンプトを定義している。

> [!IMPORTANT]
> **グラレコ図解スタイルへの変更点**
> - 従来: キャラクターがNG/OKの行動をしている「イラスト」
> - 変更後: **テキスト見出し・矢印・アイコン・フローチャートを含む「グラフィックレコーディング（図解）」**
> - 各パネルにNG行動・OK行動の**要約テキスト**を手書き風で大きく描画し、キャラクターはそれを補助するナビゲーター役として小さく配置する
> - 読者が「文字を読むだけでNG/OKがわかる」設計にする

> [!IMPORTANT]
> **結果ページと教材画像の分離設計**
> - **HTMLで実装するもの**: タイプ名、キャッチコピー、スコア、レーダーチャート、振り返りメモ、行動改善チェックリスト。
> - **画像で生成するもの**: タイプ解説用の「NG/OK比較図解」のみ。1タイプにつき「上下対比の1枚画像」とし、全6タイプ ＋ 複合タイプ1枚 ＝ 計7枚の画像で構成する。

---

## 1. ビジュアル共通仕様（トンマナ）

診断結果の「少し痛い指摘」をマイルドにしつつ、読者が「自分ごと」として親しみを持って受け入れられるよう、以下の仕様で統一する。

- **基本比率**: 縦長 3:4（スマートフォンでの閲覧・スクロールに最適）
- **レイアウト**: 上下分割の2パネル構成（**上半分にNGゾーン、下半分にOKゾーン**）
- **境界線**: 中央に太めの手書き風の矢印（↓）を配置し「NGからOKへの改善フロー」を示す
- **スタイル**: グラフィックレコーディング風（hand-drawn graphic recording style, whiteboard illustration, marker pen texture）
- **トーン**: 柔らかいパステル調ベース、ただし**NGゾーンは薄いピンク/赤の背景色**、**OKゾーンは薄いグリーン/ミント背景色**で明確に区別
- **背景**: 白ベース（off-white background like a whiteboard）
- **文字（最重要）**:
  - 各パネルの最上部に **「❌ NG」「✅ OK」** のラベル（大きく太い手書きフォント）
  - NG行動・OK行動の**核心フレーズを手書き風テキストで大きく描画**（画像内で読める程度のサイズ）
  - 補足テキストは小さめの手書き風で添える
- **キャラクターの役割**: 各パネルの端に小さく配置するナビゲーター。NGでは困った顔、OKでは笑顔
- **装飾**: 手書き風の矢印、丸囲み、アンダーライン、吹き出し、簡易アイコン（💬📱📝✏️🔔など）を活用

---

## 2. キャラクター配役表

| タイプ名                       | キャラクター名          | キャラクター種別 | 主な外見固定要素                                          |
| -------------------------- | ---------------- | -------- | ------------------------------------------------- |
| **🔥 ペースセッター型** (量で押し切りすぎ) | **Makami（マカミ）**  | 青い狼      | 青い毛並み、白い胸毛、ひし形メタルプレート付きダークレッドの忍者額当て               |
| **🧭 コーチ型** (待ちすぎ・反応待ち)    | **Luna（ルナ）**     | 白いうさぎ    | 白い毛並み、長い立ち耳、ひし形メタルプレート付き緑の忍者額当て、水色マフラー            |
| **⚡ 強制型** (自己正解・押し切り)      | **Yama（ヤーマ）**    | コオニ      | 紫色の肌、短いグレーの鬼角2本、黒いふわふわ腰巻き                         |
| **💛 関係重視型** (内輪やさしすぎ)     | **Tart（タルト）**    | 亀        | パステルピンクの肌、緑の甲羅、頭に2枚の双葉、ひし形メタルプレート付き紫の額当て、黄金色マフラー  |
| **🔮 先見型** (構想語りすぎ)        | **LeeLee（リーリー）** | パンダ      | パンダの黒い目周り、黒い耳、白い顔、ひし形メタルプレート付きダークネイビー黒の額当て、虎柄マフラー |
| **🗳️ 民主型** (選択肢広げすぎ)      | **Orochi（オロチ）**  | 黒蛇       | 黒い鱗、赤い大きな目、深紫のマフラー                                |

---

## 3. CNP固定キャラクタープロンプト（参照・更新用マスター）

以下はCNPキャラクター情報を更新する際の参照用マスター。各タイプの `prompt` 本文には、対応する固定プロンプトをすでに直接埋め込んでいるため、**各YAMLブロックをそのまま丸ごとコピーして使用できる**。  
額当てを着用する全キャラクターは、長方形プレートではなく、**正面中央に大きなひし形のメタルプレートを1枚**配置する。プレート上に紋章・文字・ロゴ・追加スタッズは描かない。

```yaml
Makami:
  cnp_character_prompt: |
    Makami (CNP / CryptoNinja Partners), a cute blue wolf ninja mascot. Small upright chibi body, large head, short rounded body, light sky-blue fur with slightly darker blue shadows, white fluffy chest fur, light blue and white inner ears, small blue wolf tail. Sharp golden-yellow eyes, arched expressive brows, small dark-gray nose, confident and slightly mischievous smirk. Dark-red ninja forehead protector tied at the side, with one large gray diamond-shaped metal plate centered on the forehead; no rectangular plate, no symbol, no letters, no logo, no extra studs. Empty paws, no weapon. Keep Makami clearly recognizable and consistent in every panel.

Luna:
  cnp_character_prompt: |
    Luna (CNP / CryptoNinja Partners), a cute white rabbit ninja mascot. Small chibi body, round head, short body, two long upright white ears with soft gray shading, pure-white fur, small oval dark reddish-brown eyes, small black triangular nose, simple crossed rabbit mouth, calm and slightly stoic expression. Green ninja forehead protector tied at the side, with one large gray diamond-shaped metal plate centered on the forehead; no rectangular plate, no symbol, no letters, no logo, no extra studs. Large sky-blue scarf flowing to the side. Empty paws, no weapon, no bomb, no explosive item. Keep Luna clearly recognizable and consistent in every panel.

Yama:
  cnp_character_prompt: |
    Yama (CNP / CryptoNinja Partners), a cute small oni mascot. Small compact chibi body, large rounded head, short limbs, bright-purple skin with slightly darker purple shadows, two short light-gray oni horns, small cat-like pointed ears, short angular purple eyebrows, large sharp eyes with dark pupils, gray-white outer eye areas and red-pink highlights. Serious, slightly grumpy, cool and fearless expression. Charcoal-black fluffy waist wrap around the hips. No forehead protector, no weapon, no club, no large weapon on the back. Keep Yama clearly recognizable and consistent in every panel.

Tart:
  cnp_character_prompt: |
    Tart (CNP / CryptoNinja Partners), a cute stylized pink turtle ninja mascot. Pale pastel-pink skin, green turtle shell, two light-green sprout leaves on top of the head, soft-green cheek markings, warm pink gradient eyes. Purple ninja forehead protector tied at the side, with one large dark-gray diamond-shaped metal plate centered on the forehead; no rectangular plate, no symbol, no letters, no logo, no extra studs. Large golden-yellow scarf. Keep Tart clearly recognizable and consistent in every panel.

LeeLee:
  cnp_character_prompt: |
    LeeLee (CNP / CryptoNinja Partners), a cute panda ninja mascot. Small chibi mascot body, round head, short body, soft and cute proportions. Classic panda black eye patches, black round ears, white face, small oval black eyes with dark brown highlights, tiny black nose, simple cute cat-like smiling mouth. Dark navy-black ninja forehead protector tied on the side, with one large gray diamond-shaped metal plate centered on the forehead; no rectangular plate, no symbol, no letters, no logo, no extra studs. Large golden-yellow to orange scarf with black tiger stripes. Small rounded paws, no weapon. Keep LeeLee clearly recognizable and consistent in every panel.

Orochi:
  cnp_character_prompt: |
    Orochi (CNP / CryptoNinja Partners), a cute but mysterious black snake mascot. Small upright chibi snake body, rounded head, compact proportions, dark charcoal-black smooth scales with a few subtle square scale patches, large round red eyes with bright highlights, orange-red markings around the eyes, vertical orange-red stripe on the forehead, tiny nostril dots, small soft smile with a tiny pink tongue slightly visible. Deep-purple scarf flowing to the side, bundle of arrows or dart-like rods behind the body, small red lantern hanging from a horizontal wooden stick in front. No forehead protector. Keep Orochi clearly recognizable and consistent in every panel.
```

> [!IMPORTANT]
> 各図解内で表情やポーズは変更してよいが、上記の体色・顔・額当て・マフラー・固定装備・禁止事項は変更しない。グラレコの簡略表現でも、キャラクターを単なる「青い狼」「白いうさぎ」などへ省略しないこと。

---

## 4. タイプ別 グラレコ図解プロンプト（YAML形式・アスペクト比 3:4）

---

### 🔥 No.1 ペースセッター型（量で押し切りすぎ型）

```yaml
type: "No.1 ペースセッター型（量で押し切りすぎ型）"
character: "Makami (マカミ)"
aspect_ratio: "3:4"
layout: "上下分割グラフィックレコーディング（上がNGゾーン、下がOKゾーン）"
prompt: |
  アスペクト比3:4の上下分割型グラフィックレコーディング風インフォグラフィック。ホワイトボードのようなオフホワイト背景に、手描きマーカーペンイラスト。太い手書き文字ラベルが主要なビジュアル要素で、キャラクターは小さな補助マスコット。

  画像全体の最上部 — 診断結果タイトル帯:
  - 上下のNG/OKゾーンとは独立したタイトル帯を配置
  - 大きく太い手書き文字で正確に描画: 「あなたは ペースセッター型」

  上半分 — NGゾーン（薄いピンク/サーモン色の背景）:
  - 上部に大きく太い手書きラベル: 「❌ NG」
  - メインテキスト（大きな手書き文字）: 「投稿→次→次… 反応を確認せず突き進む」
  - マーカーで描いたシンプルなフロー図: [投稿] →→→ [投稿] →→→ [投稿]　未読コメントの吹き出しアイコンに大きな×マーク
  - 小さなアイコンイラスト: 「99+」に×がついた通知ベル、回り続ける時計
  - 隅に小さなちびマカミ（青い狼の忍者マスコット、白い胸毛、正面中央に大きなひし形メタルプレートがあるダークレッドの額当て）が汗をかきながら必死に前へ走り、後ろに残された吹き出しの列を無視している
  - 小さな注釈テキスト: 「フィードバックを見る暇がない！」

  中央の区切りに大きな手描きの下矢印（↓）とテキスト: 「ここを変えよう →」

  下半分 — OKゾーン（薄いミントグリーンの背景）:
  - 上部に大きく太い手書きラベル: 「✅ OK」
  - メインテキスト（大きな手書き文字）: 「1投稿 → 30分ストップ → 反応を観察 → 次へ」
  - シンプルなタイムライン図: [投稿] → [⏱ 30分休止] → [👀 確認: いいね、リプライ、保存] → [📝 振り返り] → [次の投稿]
  - 小さなアイコンイラスト: ハートアイコンの上の虫眼鏡、チェックマーク付きのノート
  - 小さなちびマカミが穏やかに座り、満足げな笑顔で虫眼鏡を持ち、ハートアイコン付きの吹き出しを見ている
  - 小さな注釈テキスト: 「次に進む前に読者の反応を観察しよう」

  CNPキャラクター固定プロンプト（必ず反映）:
  Makami (CNP / CryptoNinja Partners), a cute blue wolf ninja mascot. Small upright chibi body, large head, short rounded body, light sky-blue fur with slightly darker blue shadows, white fluffy chest fur, light blue and white inner ears, small blue wolf tail. Sharp golden-yellow eyes, arched expressive brows, small dark-gray nose, confident and slightly mischievous smirk. Dark-red ninja forehead protector tied at the side, with one large gray diamond-shaped metal plate centered on the forehead; no rectangular plate, no symbol, no letters, no logo, no extra studs. Empty paws, no weapon. Keep Makami clearly recognizable and consistent in every panel.

  スタイル: グラフィックレコーディング/ビジュアルノート風。太いマーカーの輪郭線、手書き文字、シンプルなアイコン、パステルカラーのコーディング。テキストが主役で、キャラクターはアクセント。
```

---

### 🧭 No.2 コーチ型（待ちすぎ・反応待ち型）

```yaml
type: "No.2 コーチ型（待ちすぎ・反応待ち型）"
character: "Luna (ルナ)"
aspect_ratio: "3:4"
layout: "上下分割グラフィックレコーディング（上がNGゾーン、下がOKゾーン）"
prompt: |
  アスペクト比3:4の上下分割型グラフィックレコーディング風インフォグラフィック。ホワイトボードのようなオフホワイト背景に、手描きマーカーペンイラスト。太い手書き文字ラベルが主要なビジュアル要素で、キャラクターは小さな補助マスコット。

  画像全体の最上部 — 診断結果タイトル帯:
  - 上下のNG/OKゾーンとは独立したタイトル帯を配置
  - 大きく太い手書き文字で正確に描画: 「あなたは コーチ型」

  上半分 — NGゾーン（薄いピンク/サーモン色の背景）:
  - 上部に大きく太い手書きラベル: 「❌ NG」
  - メインテキスト（大きな手書き文字）: 「ただ見てるだけ… 反応が来るのを待っている」
  - シンプルな図解: 左に投稿メッセージ → どこにも行かない点線 → 長時間待機を示す時計アイコン → 「…」の吹き出し
  - 小さなアイコンイラスト: 砂時計、CTAのないスマホ画面、空のコメント欄
  - 小さなちびルナ（白いうさぎの忍者マスコット、長い立ち耳、正面中央に大きなひし形メタルプレートがある緑の額当て、水色のスカーフ）が受け身で座り、腕を組んで空白のスマホ画面を見つめている
  - 小さな注釈テキスト: 「読者への次のステップがない！」

  中央の区切りに大きな手描きの下矢印（↓）とテキスト: 「ここを変えよう →」

  下半分 — OKゾーン（薄いミントグリーンの背景）:
  - 上部に大きく太い手書きラベル: 「✅ OK」
  - メインテキスト（大きな手書き文字）: 「すべての投稿の最後に明確なCTAを1つ入れる」
  - シンプルなフロー図: [あなたの投稿] → [最後の一行: 「これを試してみて → [リンク]」] CTAの周りに明るいハイライト枠
  - 3つのCTA例アイコン（小さな吹き出し内）: 「💬 コメントしてね！」「🔗 もっと読む」「❓ あなたの場合は？」
  - 小さなちびルナがマーカーペンを持ち、笑顔でCTAセクションを指差して読者を案内している
  - 小さな注釈テキスト: 「読者に次のステップを示そう！」

  CNPキャラクター固定プロンプト（必ず反映）:
  Luna (CNP / CryptoNinja Partners), a cute white rabbit ninja mascot. Small chibi body, round head, short body, two long upright white ears with soft gray shading, pure-white fur, small oval dark reddish-brown eyes, small black triangular nose, simple crossed rabbit mouth, calm and slightly stoic expression. Green ninja forehead protector tied at the side, with one large gray diamond-shaped metal plate centered on the forehead; no rectangular plate, no symbol, no letters, no logo, no extra studs. Large sky-blue scarf flowing to the side. Empty paws, no weapon, no bomb, no explosive item. Keep Luna clearly recognizable and consistent in every panel.

  スタイル: グラフィックレコーディング/ビジュアルノート風。太いマーカーの輪郭線、手書き文字、シンプルなアイコン、パステルカラーのコーディング。テキストが主役で、キャラクターはアクセント。
```

---

### ⚡ No.3 強制型（自己正解・押し切り型）

```yaml
type: "No.3 強制型（自己正解・押し切り型）"
character: "Yama (ヤーマ)"
aspect_ratio: "3:4"
layout: "上下分割グラフィックレコーディング（上がNGゾーン、下がOKゾーン）"
prompt: |
  アスペクト比3:4の上下分割型グラフィックレコーディング風インフォグラフィック。ホワイトボードのようなオフホワイト背景に、手描きマーカーペンイラスト。太い手書き文字ラベルが主要なビジュアル要素で、キャラクターは小さな補助マスコット。

  画像全体の最上部 — 診断結果タイトル帯:
  - 上下のNG/OKゾーンとは独立したタイトル帯を配置
  - 大きく太い手書き文字で正確に描画: 「あなたは 強制型」

  上半分 — NGゾーン（薄いピンク/サーモン色の背景）:
  - 上部に大きく太い手書きラベル: 「❌ NG」
  - メインテキスト（大きな手書き文字）: 「自分のやり方が正しい！ → 同じ投稿パターンの繰り返し」
  - ループを示す図解: [自分の仮説] → [投稿] → [低反応] → （矢印がループして戻る）[自分の仮説: やっぱり正しい！] 大きな円形矢印と×マーク
  - 小さなアイコンイラスト: 読者フィードバックの矢印をブロックするレンガの壁、×マーク付きの耳アイコン
  - 小さなちびヤーマ（紫色の肌のコオニマスコット、短いグレーの鬼角2本、黒いふわふわ腰巻き）が腕を組み、頑固な表情でループ図の横に立っている
  - 小さな注釈テキスト: 「読者のシグナル ＝ ノイズとして無視」

  中央の区切りに大きな手描きの下矢印（↓）とテキスト: 「ここを変えよう →」

  下半分 — OKゾーン（薄いミントグリーンの背景）:
  - 上部に大きく太い手書きラベル: 「✅ OK」
  - メインテキスト（大きな手書き文字）: 「次の投稿の前に → 読者コメントを3つ読む（30秒）」
  - シンプルなチェックリスト図: ☑ DM/コメントを開く → ☑ 3つのメッセージを読む → ☑ 1つの気づきをメモ → ☑ 次の投稿を調整
  - 小さなアイコンイラスト: チェックマーク付きの開いた耳アイコン、「読者の声…」と書かれたメモ帳
  - 小さなちびヤーマが思慮深く謙虚な表情で、クリップボードを持ち読者からの吹き出しを読んでいる
  - 小さな注釈テキスト: 「30秒の傾聴がすべてを変える」

  CNPキャラクター固定プロンプト（必ず反映）:
  Yama (CNP / CryptoNinja Partners), a cute small oni mascot. Small compact chibi body, large rounded head, short limbs, bright-purple skin with slightly darker purple shadows, two short light-gray oni horns, small cat-like pointed ears, short angular purple eyebrows, large sharp eyes with dark pupils, gray-white outer eye areas and red-pink highlights. Serious, slightly grumpy, cool and fearless expression. Charcoal-black fluffy waist wrap around the hips. No forehead protector, no weapon, no club, no large weapon on the back. Keep Yama clearly recognizable and consistent in every panel.

  スタイル: グラフィックレコーディング/ビジュアルノート風。太いマーカーの輪郭線、手書き文字、シンプルなアイコン、パステルカラーのコーディング。テキストが主役で、キャラクターはアクセント。
```

---

### 💛 No.4 関係重視型（内輪やさしすぎ型）

```yaml
type: "No.4 関係重視型（内輪やさしすぎ型）"
character: "Tart (タルト)"
aspect_ratio: "3:4"
layout: "上下分割グラフィックレコーディング（上がNGゾーン、下がOKゾーン）"
prompt: |
  アスペクト比3:4の上下分割型グラフィックレコーディング風インフォグラフィック。ホワイトボードのようなオフホワイト背景に、手描きマーカーペンイラスト。太い手書き文字ラベルが主要なビジュアル要素で、キャラクターは小さな補助マスコット。

  画像全体の最上部 — 診断結果タイトル帯:
  - 上下のNG/OKゾーンとは独立したタイトル帯を配置
  - 大きく太い手書き文字で正確に描画: 「あなたは 関係重視型」

  上半分 — NGゾーン（薄いピンク/サーモン色の背景）:
  - 上部に大きく太い手書きラベル: 「❌ NG」
  - メインテキスト（大きな手書き文字）: 「内輪ネタだけ → 新規さんがついてこれない」
  - 図解: 閉じた円の中に3つの笑顔が業界用語の吹き出し（「知っての通り…」「いつもの用語」）を共有し、円の外側に「???」の吹き出しを出す困った新規読者の顔と、その間の壁
  - 小さなアイコンイラスト: 鍵付きドアのアイコン、×マーク付きの辞書、略語の吹き出し
  - 小さなちびタルト（パステルピンクの肌の亀の忍者マスコット、緑の甲羅、頭に2枚の双葉、正面中央に大きなひし形メタルプレートがある紫の額当て、黄金色のスカーフ）が円の中で楽しそうに笑っているが、外の新規読者に気づいていない
  - 小さな注釈テキスト: 「業界用語の壁が新規読者をブロック！」

  中央の区切りに大きな手描きの下矢印（↓）とテキスト: 「ここを変えよう →」

  下半分 — OKゾーン（薄いミントグリーンの背景）:
  - 上部に大きく太い手書きラベル: 「✅ OK」
  - メインテキスト（大きな手書き文字）: 「業界用語を1つ、初見の読者向けに書き換える」
  - ビフォー/アフター比較ボックス:
    左ボックス（打ち消し線）: 「いつもの◯◯アプローチ…」
    右ボックス（キラキラハイライト）: 「◯◯とは△△のこと。簡単に説明すると →」
  - シンプルな「翻訳」アイコン（吹き出し → 開いた本 → 電球）
  - 小さなちびタルトが円を開いて温かく手を振り、定義が明確に書かれたホワイトボードを指差している
  - 小さな注釈テキスト: 「フォロワー0人の読者が初めて見ることを想像しよう」

  CNPキャラクター固定プロンプト（必ず反映）:
  Tart (CNP / CryptoNinja Partners), a cute stylized pink turtle ninja mascot. Pale pastel-pink skin, green turtle shell, two light-green sprout leaves on top of the head, soft-green cheek markings, warm pink gradient eyes. Purple ninja forehead protector tied at the side, with one large dark-gray diamond-shaped metal plate centered on the forehead; no rectangular plate, no symbol, no letters, no logo, no extra studs. Large golden-yellow scarf. Keep Tart clearly recognizable and consistent in every panel.

  スタイル: グラフィックレコーディング/ビジュアルノート風。太いマーカーの輪郭線、手書き文字、シンプルなアイコン、パステルカラーのコーディング。テキストが主役で、キャラクターはアクセント。
```

---

### 🔮 No.5 先見型（構想語りすぎ型）

```yaml
type: "No.5 先見型（構想語りすぎ型）"
character: "LeeLee (リーリー)"
aspect_ratio: "3:4"
layout: "上下分割グラフィックレコーディング（上がNGゾーン、下がOKゾーン）"
prompt: |
  アスペクト比3:4の上下分割型グラフィックレコーディング風インフォグラフィック。ホワイトボードのようなオフホワイト背景に、手描きマーカーペンイラスト。太い手書き文字ラベルが主要なビジュアル要素で、キャラクターは小さな補助マスコット。

  画像全体の最上部 — 診断結果タイトル帯:
  - 上下のNG/OKゾーンとは独立したタイトル帯を配置
  - 大きく太い手書き文字で正確に描画: 「あなたは 先見型」

  上半分 — NGゾーン（薄いピンク/サーモン色の背景）:
  - 上部に大きく太い手書きラベル: 「❌ NG」
  - メインテキスト（大きな手書き文字）: 「壮大なビジョンを語る… でも今日のアクションがない」
  - 図解: 空高く浮かぶ「AIの未来！」「パラダイムシフト！」「新時代！」と書かれた思考雲、その下に「今日のアクション: ______」が空白のままのTo-Doリストがデスクに置かれている
  - 小さなアイコンイラスト: 上に飛ぶロケット（ビジョンを象徴）、下に空のチェックリスト（具体的アクションがないことを象徴）
  - 小さなちびリーリー（パンダの忍者マスコット、黒い目周り、黒い耳、白い顔、正面中央に大きなひし形メタルプレートがあるダークネイビー黒の額当て、虎柄マフラー）が空を見上げて夢想し、デスクと空白のタスクリストは無視されている
  - 小さな注釈テキスト: 「読者の声: 『かっこいいけど… 結局何をすればいいの？』」

  中央の区切りに大きな手描きの下矢印（↓）とテキスト: 「ここを変えよう →」

  下半分 — OKゾーン（薄いミントグリーンの背景）:
  - 上部に大きく太い手書きラベル: 「✅ OK」
  - メインテキスト（大きな手書き文字）: 「每回の投稿の最後に: 『今日で30秒で試せるアクション』を入れる」
  - マーカーで描いたテンプレートボックス:
    「今日の30秒アクション:
     → [ツール名]を開く
     → 1行書く
     → 完了！ ✅」
  - 小さなアイコンイラスト: 30秒を示すタイマー、1行書いている鉛筆、チェックマーク
  - 小さなちびリーリーがデスクに座り、集中して紙に具体的なアクションステップを書いている、満足げな表情
  - 小さな注釈テキスト: 「ビジョン ＋ 今日の1ステップ ＝ 読者への本当の価値」

  CNPキャラクター固定プロンプト（必ず反映）:
  LeeLee (CNP / CryptoNinja Partners), a cute panda ninja mascot. Small chibi mascot body, round head, short body, soft and cute proportions. Classic panda black eye patches, black round ears, white face, small oval black eyes with dark brown highlights, tiny black nose, simple cute cat-like smiling mouth. Dark navy-black ninja forehead protector tied on the side, with one large gray diamond-shaped metal plate centered on the forehead; no rectangular plate, no symbol, no letters, no logo, no extra studs. Large golden-yellow to orange scarf with black tiger stripes. Small rounded paws, no weapon. Keep LeeLee clearly recognizable and consistent in every panel.

  スタイル: グラフィックレコーディング/ビジュアルノート風。太いマーカーの輪郭線、手書き文字、シンプルなアイコン、パステルカラーのコーディング。テキストが主役で、キャラクターはアクセント。
```

---

### 🗳️ No.6 民主型（選択肢広げすぎ型）

```yaml
type: "No.6 民主型（選択肢広げすぎ型）"
character: "Orochi (オロチ)"
aspect_ratio: "3:4"
layout: "上下分割グラフィックレコーディング（上がNGゾーン、下がOKゾーン）"
prompt: |
  アスペクト比3:4の上下分割型グラフィックレコーディング風インフォグラフィック。ホワイトボードのようなオフホワイト背景に、手描きマーカーペンイラスト。太い手書き文字ラベルが主要なビジュアル要素で、キャラクターは小さな補助マスコット。

  画像全体の最上部 — 診断結果タイトル帯:
  - 上下のNG/OKゾーンとは独立したタイトル帯を配置
  - 大きく太い手書き文字で正確に描画: 「あなたは 民主型」

  上半分 — NGゾーン（薄いピンク/サーモン色の背景）:
  - 上部に大きく太い手書きラベル: 「❌ NG」
  - メインテキスト（大きな手書き文字）: 「選択肢A、B、C、D… お好きにどうぞ！」
  - 図解: 4つの選択肢カード（A、B、C、D）が均等に並び、ハイライトもおすすめマークもなし。中央にお手上げポーズのアイコンと「お任せします！」のテキスト
  - 小さなアイコンイラスト: 方向が定まらず回るコンパス、ハテナマークの雲
  - 小さなちびオロチ（ダークチャコールの黒い蛇のマスコット、大きな丸い赤い目、深紫のスカーフ、直立ち座り）が困った様子で、選択肢に囲まれどれもおすすめできない
  - 小さな注釈テキスト: 「読者の声: 『で、どれを選べばいいの？！』」

  中央の区切りに大きな手描きの下矢印（↓）とテキスト: 「ここを変えよう →」

  下半分 — OKゾーン（薄いミントグリーンの背景）:
  - 上部に大きく太い手書きラベル: 「✅ OK」
  - メインテキスト（大きな手書き文字）: 「選択肢を見せつつ ＋ 『私のおすすめはA』を添える」
  - 図解: 同じ4つの選択肢カード（A、B、C、D）だが、選択肢Aに明るい赤丸/ハイライトと⭐が付き、吹き出し: 「私はAをおすすめします。なぜなら…」
  - マーカーで描いたシンプルな式: 「選択肢 ＋ 自分のスタンス ＝ 信頼」
  - 小さなアイコンイラスト: 選択肢Aをハイライトする指示棒、親指を立てたアイコン
  - 小さなちびオロチが選択肢Aの横に自信を持って立ち、指示棒を持って明確で決断力のある表情
  - 小さな注釈テキスト: 「『私のおすすめは…』が読者の信頼を築く」

  CNPキャラクター固定プロンプト（必ず反映）:
  Orochi (CNP / CryptoNinja Partners), a cute but mysterious black snake mascot. Small upright chibi snake body, rounded head, compact proportions, dark charcoal-black smooth scales with a few subtle square scale patches, large round red eyes with bright highlights, orange-red markings around the eyes, vertical orange-red stripe on the forehead, tiny nostril dots, small soft smile with a tiny pink tongue slightly visible. Deep-purple scarf flowing to the side, bundle of arrows or dart-like rods behind the body, small red lantern hanging from a horizontal wooden stick in front. No forehead protector. Keep Orochi clearly recognizable and consistent in every panel.

  スタイル: グラフィックレコーディング/ビジュアルノート風。太いマーカーの輪郭線、手書き文字、シンプルなアイコン、パステルカラーのコーディング。テキストが主役で、キャラクターはアクセント。
```

---

### 🌟 複合タイプ（ペースセッター型 × 強制型 の例）

```yaml
type: "複合タイプ（ペースセッター型 × 強制型 の例）"
characters: ["Makami (マカミ)", "Yama (ヤーマ)"]
aspect_ratio: "3:4"
layout: "上下分割グラフィックレコーディング（上がNGゾーン、下がOKゾーン）"
prompt: |
  アスペクト比3:4の上下分割型グラフィックレコーディング風インフォグラフィック。ホワイトボードのようなオフホワイト背景に、手描きマーカーペンイラスト。太い手書き文字ラベルが主要なビジュアル要素で、キャラクターたちは小さな補助マスコット。

  上半分 — NGゾーン（薄いピンク/サーモン色の背景）:
  - 上部に大きく太い手書きラベル: 「❌ NG — ダブルトラップ」
  - メインテキスト（大きな手書き文字）: 「スピード × 頑固さ ＝ フィードバックブラックアウト」
  - 図解: 2つの重なる問題の円（ベン図風）:
    左の円: 「🔥 確認せずに高速投稿」
    右の円: 「⚡ 反応が低くても変えない」
    重なりゾーン: 「最悪のコンボ: 誰も読まない投稿を大量生産」
  - 小さなアイコンイラスト: 虚空に向かって叫ぶメガホン、「俺のやり方」と書かれたレンガの壁、すべて×がついた通知アイコン
  - 小さなちびマカミ（青い狼、正面中央に大きなひし形メタルプレートがあるダークレッドの額当て）が必死にタイピングし、小さなちびヤーマ（紫色のコオニ、グレーの角）が腕を組んで頑固にフィードバックをブロック、両方ともNGゾーンにいる
  - 小さな注釈テキスト: 「高速生産 ＋ シグナル無視 ＝ 読者から見えなくなる」

  中央の区切りに大きな手描きの下矢印（↓）とテキスト: 「ここを変えよう →」

  下半分 — OKゾーン（薄いミントグリーンの背景）:
  - 上部に大きく太い手書きラベル: 「✅ OK — バランスデュオ」
  - メインテキスト（大きな手書き文字）: 「スピード ＋ 傾聴 ＝ 成長エンジン」
  - 図解: フローチャート:
    [スピードで投稿] → [⏱ 30分休止] → [👂 コメントを3つ読む] → [📝 トーンを調整] → [次の投稿、改善済み]
  - マーカーで描いたシンプルな式: 「ペース ＋ 謙虚さ ＝ 信頼の複利」
  - 小さなアイコンイラスト: スムーズに回る歯車、上向き成長矢印付きのハート、チェックマーク付きの開いた耳
  - 小さなちびマカミとヤーマが並んで座り、マカミが画面上のコメントを指差し、ヤーマがうなずきながら謙虚な表情でメモを取っている
  - 小さな注釈テキスト: 「高速生産 ＋ 素直な傾聴 ＝ 止まらない成長」

  CNPキャラクター固定プロンプト（必ず反映）:
  Makami (CNP / CryptoNinja Partners), a cute blue wolf ninja mascot. Small upright chibi body, large head, short rounded body, light sky-blue fur with slightly darker blue shadows, white fluffy chest fur, light blue and white inner ears, small blue wolf tail. Sharp golden-yellow eyes, arched expressive brows, small dark-gray nose, confident and slightly mischievous smirk. Dark-red ninja forehead protector tied at the side, with one large gray diamond-shaped metal plate centered on the forehead; no rectangular plate, no symbol, no letters, no logo, no extra studs. Empty paws, no weapon. Keep Makami clearly recognizable and consistent in every panel.
  Yama (CNP / CryptoNinja Partners), a cute small oni mascot. Small compact chibi body, large rounded head, short limbs, bright-purple skin with slightly darker purple shadows, two short light-gray oni horns, small cat-like pointed ears, short angular purple eyebrows, large sharp eyes with dark pupils, gray-white outer eye areas and red-pink highlights. Serious, slightly grumpy, cool and fearless expression. Charcoal-black fluffy waist wrap around the hips. No forehead protector, no weapon, no club, no large weapon on the back. Keep Yama clearly recognizable and consistent in every panel.

  スタイル: グラフィックレコーディング/ビジュアルノート風。太いマーカーの輪郭線、手書き文字、シンプルなアイコン、パステルカラーのコーディング。テキストが主役で、キャラクターたちはアクセント。
```
