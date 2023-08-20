# playground_movielens
- explicitな評価があればtestにおいてランキング指標での評価ができるのでまずはmovielensを試す
- implicitな行動データがあるTrivago RecSys Challenge Data 2019のデータも取り扱っていきたい
## バッチの場合
- pythonでユーザーごとにランキングを作成してredisとかに入れとく。Goでそれを取ってくる
## リアルタイムの場合
- ユーザーベクトルをredisから取得し、faissのANNでcandidateを生成し、rerankして返却。

## 評価方法
- モデルの評価としては予測対象をtest期間に実際にratingされたものにしてそれらをきちんとrating順に予測できるかを測ることで性能を評価する
    - nDCGとかで測れる
- ただ実際にはretrievalの時点で評価が低いであろう映画はランキングの中に含まれなくなる
    - 実験では評価が低いであろう映画にも予測値をつける必要があることに注意
    - そうするとretrievalについての評価をしてもいいが、これも結局Unlabelをどう扱うかが難しい
        - 少なくともratingが5のものはUnlabelと同等以上の評価になるのでratingが5のもののrecallを測る
        - あるいは結局ratingが付いているもののランキング指標を見ることになる

