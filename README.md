# USPTO - Explainable AI for Patent Professionals（銅メダル）

## コンペ結果
- **順位**: 571チーム中 67位  
- **メダル**: 銅メダル  
- **スコア**: 0.35629  
- **チーム**: Nao9999  
- **認定証**: [Kaggle Certificate](https://www.kaggle.com/certification/competitions/nao9999/uspto-explainable-ai)

## 概要
本リポジトリは、Kaggleコンペティション「USPTO - Explainable AI for Patent Professionals」における銅メダル獲得時のコードです。

コンペの目的：特許検索クエリを自動生成し、評価指標 AP@50 を最大化すること

## アプローチ概要
このソリューションでは、焼きなまし法（Simulated Annealing）を用いて特許検索クエリを最適化しています。

1. **TF-IDF による特徴抽出**  
   特許タイトル（`title`）と CPC コード（`cpc`）から TF-IDF を計算し、重要度の高い単語・コードを上位から抽出します。
2. **初期クエリの生成**  
   抽出した単語を `"word1 OR word2 OR ..."` の形で組み合わせ、タイトル用 `ti:(...)`、CPC用 `cpc:(...)` のクエリを作成します。
3. **焼きなまし法による最適化**  
   各単語を「使う／使わない」の二値で表現し、その組み合わせを焼きなまし法で探索して AP@50 を最大化します。
4. **Whoosh による検索**  
   Whoosh を用いてインデックスを作成し、生成したクエリで近傍特許を検索します。

### 主要コンポーネント
- **単語選択**:  
  タイトルから上位15語、CPCコードから上位15コードを TF-IDF の値に基づいて選択。
- **最適化**:  
  エネルギー関数として「マイナス AP@50」を用いたカスタム焼きなましアルゴリズム。
- **クエリ形式**:  
  - タイトル: `ti:(word1 OR word2 OR ...)`  
  - CPC: `cpc:(code1 OR code2 OR ...)`  
  必要に応じて両者を組み合わせて検索クエリを構成。
