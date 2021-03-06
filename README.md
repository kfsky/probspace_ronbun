# probspace_論文コンペ

## コンペティション概要
本コンペでは、論文投稿サイト（プレ・プリントサーバー）の公開情報を用いて、被引用数を予測する
* https://prob.space/competitions/citation_prediction

## コンペの特徴  
今回のコンペでは、学習データの一部に目的変数（被引用数）が含まれておらず、代わりにDigital Object Identifier（DOI）により計算された低精度被引用数が代替変数として付与されている。 

## 順位  
Public：30位、Private：29位  
![image](https://user-images.githubusercontent.com/46860245/113080144-5dc19880-9211-11eb-8aaa-90cf5931ae47.png)


# 開発環境
google colab proで開発

# 特徴量作成
* title, abstract

　BERTを用いて特徴量を作成。titleに関してはPCAで15次元まで使用。
  abstractでは寄与率が90%を超えるまで使用
  
* journal-ref, doi

　外部データを用いて、出版先などを判定して、特徴量として学習
 
* versions

　以下の特徴量を追加（トピック参照）
 
  * 論文が投稿された時刻(unixtime)
  * 論文が最後に更新された時刻(unixtime)
  * 最後の更新と投稿された時刻の差分
  * 論文が何回更新されたか
 
* categories

　word2vecで特徴量作成
 
* CountEncoder

　"submitter", "categories", "publisher"
 
* LabelEncoder

　"submitter", "categories", "publisher"
 
* StringLength__

　'title', 'abstract','comments',
 
# model
LightGBM, CatBoost のアンサンブル（kfold=5)

# Note  
CVの結果が中々PBに反映されて行かなかったので、CVのやり方がよくなかった？  
有効な特徴量を作成できなかった。

# Late Subでのメモ  
上位層は基本的に、doi-citesとcite（今回の目的変数）との差を予測するモデルを実装していた。  
1位の方は、以下の様に目的変数を設定  
<B>np.log1p(cites) - np.log1p(doi_cites)</B>

過去のコンペでもあったとのこと（https://prob.space/competitions/youtube-view-count)  


# Author  
kfsky
