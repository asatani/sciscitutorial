# OpenAlex SciSci チュートリアル

📑 スライド資料: [scisci_tutorial_slides.pdf](scisci_tutorial_slides.pdf)

## これは何？

[OpenAlex](https://openalex.org/) の**無料 API** を使って、**サイエンス・オブ・サイエンス（science of science ＝ 科学そのものをデータで研究する分野）** の代表的な分析を、実データで手を動かしながら一通り体験できる Python / Jupyter チュートリアルです。論文・引用・著者のデータから、次のようなことを学べます。

- OpenAlex API の使い方（検索・絞り込み・一括取得）
- 引用の基礎統計と「偏り」の測り方
- 研究者のキャリアと将来予測
- 分野の構造をネットワークと地図で俯瞰する
- 著者のジェンダー分析
- 論文の「破壊性」を測る Disruption Index

**だれ向け？** Python の基本がわかれば、科学計量学（bibliometrics）が初めてでも大丈夫です。各ノートブックは「なぜそれを見るのか」から丁寧に説明しているので、まずは**読むだけ**でも流れがつかめます。

**題材データ:** OpenAlex のトピック **`T11048` "Bacteriophages and microbial interactions"**（バクテリオファージ＝細菌に感染するウイルスと、微生物間相互作用の研究）です。各ノートブック冒頭の `NAME` を **`'qc'`** に変えると、**`T10020` "Quantum Information and Cryptography"**（量子情報）のデータセットに丸ごと切り替わります。

## いちばん簡単な始め方（Google Colab・インストール不要）

下のバッジから開いて、**上のセルから順に実行するだけ**です。各ノートブックの**最初のセル**が、必要なパッケージのインストールと事前計算済みデータのダウンロードを自動で行います。

> **既定では Google Drive を使いません（権限プロンプトなし・完全ワンクリック）。** 必要なデータ（`works.parquet` / `paper_authors.parquet` / 埋め込み等）は GitHub Release から自動取得されるので、そのまま最後まで動きます。
>
> **自分でデータを変更して他ノートブックでも使いたい／セッションをまたいで残したい場合**は、最初のセルの `USE_DRIVE = True` に変えてください。そのときだけ Drive のマウント許可を求められ、リポジトリと `data/` が各自の `MyDrive/sciscitutorial/` に置かれます（各自の Drive なので他の人と混ざりません）。なお `drive.mount` は Drive 全体へのアクセス権限を要求します（Colab の仕様でフォルダ単位には絞れません）。保存が不要なら既定の `False` のままで構いません。

| Notebook | Colab |
|---|---|
| 01 OpenAlex API アクセス | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/asatani/sciscitutorial/blob/main/code/01_openalex_api_access.ipynb) |
| 02 基礎統計 | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/asatani/sciscitutorial/blob/main/code/02_openalex_access_basic_stats.ipynb) |
| 03 研究者の将来予測 | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/asatani/sciscitutorial/blob/main/code/03_researcher_future_prediction.ipynb) |
| 04 分野ネットワーク俯瞰 | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/asatani/sciscitutorial/blob/main/code/04_field_network_overview.ipynb) |
| 05 ジェンダー分析 | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/asatani/sciscitutorial/blob/main/code/05_gender_new.ipynb) |
| 06 Disruption Index | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/asatani/sciscitutorial/blob/main/code/06_disruptiveness.ipynb) |

## おすすめの進め方

- **まず `01` を実行してください。** ここで取得・保存するデータ（`works.parquet` / `paper_authors.parquet`）を、02 以降のノートブックが読み込みます。
- そのあとは番号順（01 → 02 → … → 06）が基本ですが、**02〜06 は互いに独立**しているので、興味のあるところから読んでも構いません。
  - 全体像をつかみたい → **02 基礎統計**
  - 研究者のキャリアに興味 → **03 研究者の将来予測**
  - 分野の地図・構造を見たい → **04 分野ネットワーク俯瞰**
  - ジェンダーと不平等 → **05 ジェンダー分析**
  - 論文の新規性・破壊性 → **06 Disruption Index**
- **Colab を使う場合は 01 を実行しなくても動きます**（各ノートブックが事前計算済みデータを自動ダウンロードするため）。ローカルでも同様に自動取得されます。

## ノートブック一覧

- **`01_openalex_api_access.ipynb` — OpenAlex API 入門＋データ取得**
  API の基本操作を学びます：論文の単体取得、キーワード／著者／機関／雑誌の検索、絞り込み（`filter`）・列選択（`select`）・ページング（`cursor`）・集計（`group_by`）・サンプリング（`sample`）・オートコンプリート。**このチュートリアル全体で使う `works.parquet` / `paper_authors.parquet` をここで保存します**（付録: ランダム著者の全業績）。→ **最初に実行**

- **`02_openalex_access_basic_stats.ipynb` — 基礎統計**
  集めた論文の全体像をつかみます：被引用数の分布、引用の偏り（ジニ係数・ローレンツ曲線）、裾分布が「べき乗則か」を確かめる統計的検定（Clauset 法）、国際共著の広がりとインパクト、テキスト埋め込みで「意味的に似た論文」を検索、近年増えた新興フレーズ（N-gram）。

- **`03_researcher_future_prediction.ipynb` — 研究者の将来予測**
  著者単位の分析：h-index のランキングと分布、日本／国別の比較、生産性とインパクトの関係、活動テーマの要約（TF-IDF／GPT）、「最大ヒットはキャリア中でランダムに現れる」という経験則（ランダムインパクトルール）の検証、初期キャリアから将来の h-index を機械学習（Random Forest）で予測。

- **`04_field_network_overview.ipynb` — 分野ネットワーク俯瞰**
  分野の構造を可視化：直接引用ネットワーク、コミュニティ検出（Leiden 法）、コミュニティの自動命名（TF-IDF／GPT）、UMAP による「俯瞰地図」、ネットワークとテキストの二つの視点の比較、特定機関（例：東京大学）の強み分析。

- **`05_gender_new.ipynb` — ジェンダー分析**
  著者名からのジェンダー推定を使い、インパクト指標の男女差を調べます：頑健な指標（log(1+FWCI)・top-1% シェア）、出版年／キャリア開始年コホートでの比較、共著・生存率・キャリアイヤー別の top-1%。※推定の限界に注意（[下記](#注意点)）。

- **`06_disruptiveness.ipynb` — Disruption Index**
  論文がどれだけ「既存研究を塗り替えたか」を測る Disruption Index (DI)。`data/supplementary/` の**事前計算済み引用エッジリスト**を使い DI を**全件**計算し、DI の分布、高／低 DI の例、チームサイズとの関係（Wu et al. 2019）を再現します。

## フォルダ構成

```
sciscitutorial/
├── README.md
├── requirements.txt
├── .gitignore
├── code/                     # ノートブック本体
│   ├── 01_openalex_api_access.ipynb
│   ├── 02_openalex_access_basic_stats.ipynb
│   ├── 03_researcher_future_prediction.ipynb
│   ├── 04_field_network_overview.ipynb
│   ├── 05_gender_new.ipynb
│   └── 06_disruptiveness.ipynb
├── data/                     # 事前計算済みデータ（.gitignore / Release から取得）
│   ├── works/{bacteria,qc}/  # works.parquet, paper_authors.parquet, embeddings.npz
│   ├── career/               # 03 §3-2 用のランダム著者の全業績
│   └── supplementary/        # 06 用の引用エッジリスト等
└── output/                   # 生成された図表・CSV（.gitignore）
```

`data/` と `output/` は再生成可能なので git 管理せず、**GitHub Release で配布**します（下記）。

## ローカルで動かす

```bash
git clone https://github.com/asatani/sciscitutorial.git
cd sciscitutorial
python -m pip install -r requirements.txt
jupyter lab   # または jupyter notebook / VS Code
```

`code/` 以下のノートブックを開いて上から実行します。各ノートブック先頭のセットアップセルが、`data/` に必要なデータが無ければ **GitHub Release から自動ダウンロード**します（ローカルに既にあればそのまま使います）。

- **OpenAI を使うセル**（02 の埋め込み KNN・03/04 の GPT 要約）は任意です。使う場合のみ `OPENAI_API_KEY` を環境変数で設定してください。未設定ならそのセルはスキップされます。

## データの入手（GitHub Release）

事前計算済みデータは容量が大きいため git には含めず、GitHub Release `data-v1` の zip として配布します。ノートブックのセットアップセルが自動取得しますが、手動で置く場合は次のように展開します。

| Release アセット | 展開先 | 使うノートブック |
|---|---|---|
| `works_bacteria.zip` | `data/works/bacteria/` | 01–05（`NAME='bacteria'`） |
| `works_qc.zip` | `data/works/qc/` | 01–05（`NAME='qc'`） |
| `career.zip` | `data/career/` | 03 §3-2 |
| `supplementary.zip` | `data/supplementary/` | 06 |

```bash
# 例（リポジトリ直下で）
mkdir -p data && cd data
curl -L -O https://github.com/asatani/sciscitutorial/releases/download/data-v1/works_bacteria.zip
unzip works_bacteria.zip     # -> data/works/bacteria/...
```

01 は `data/works/{NAME}/` にデータが無ければ **OpenAlex API から取得して自作**します（`MAX_WORKS` を小さくすれば手早く試せます）。

## データセットの切り替え（bacteria ↔ qc）

各ノートブック冒頭のセットアップ／設定セルにある `NAME` を変えるだけです。

```python
NAME = 'bacteria'   # T11048 バクテリオファージ（既定）
# NAME = 'qc'       # T10020 量子情報
```

02〜06 は `data/works/{NAME}/` を読むだけなので、`NAME` を変えて再実行すれば別分野の分析になります。01 は `DATASETS` からトピック ID を自動選択して取得します。

## 注意点

- **時間効果**: 直近年の論文は被引用が蓄積していないため、`to_publication_date` で上限年を設け、`cited_by_count:desc` でサンプリングしています。年次推移は `group_by` でコーパス全体から集計します。
- **計算量**: 04 の俯瞰地図は可読性と計算量のため被引用上位 `NET_WORKS` 件の**コア**を対象にします。02 の新興フレーズと 06 の DI は全件で計算します。
- **ジェンダー分析(05)**: `gender-guesser` の名前ベース推定は、誤分類・文化差・unknown 偏り・非二元的ジェンダーの不可視化を含みます。記述統計の例示であり、能力差や因果の主張ではありません。研究用途では倫理的・方法論的検証が必須です。
- **ランダムインパクトルール(03)**: Sinatra et al. (2016) の「最大ヒットの出現タイミングはキャリア中でランダム（一様）」という経験則を、**§3-1 トピック内版**（部分キャリア・早期に偏りやすい）と **§3-2 完全版**（ランダム著者の全業績、`data/career/` にキャッシュ）の2通りで確認します。
- **Disruption Index(06)**: DI は時系列比較に注意が必要で（Leibel & Bornmann 2024）、データ被覆・引用窓・分野により値が変わります。`data/supplementary/` を使う自己完結ノートブックで、01 が取得するコーパスとは独立です。
