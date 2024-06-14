> 日本語
# 最終目標：全世界の言語に対応する
1. 世界中の「言語」を記述する。
2. 世界中の「言語」をリストアップし、ネイティブによる容認制判断を行う。
3. ネイティブによる容認性が低い言語(less-documented languageと呼ぶ)に、系統が近いと思われる言語を探す。
4. 系統が近い言語で事前学習済みモデルを作成する。
5. その言語でファインチューニングを行う。
6. もう一度容認性判断を行う。

# 1. False-friendsの意味の差を数値化する
## 1.1. False-friendsとは何か？
**語源は同じだが、意味が異なる単語のペア**のことです。
例えば、英語**to ignore**とフランス語**ignorer**はどちらも、ラテン語の**ignōrāre**から派生しています。しかし英語の場合は「**無視する**」という意味なのに対し フランス語では「**を知らない**」という意味になります。
このto ignoreとignorerのような組み合わせのことを、**False-friends**と呼びます。
## 1.2. なぜFalse-friendsの意味の差を数値化するのか？
最終的に言語間の距離を算出するためです。これまでの[言語統計学](https://en.wikipedia.org/wiki/Linguistic_distance)の分野では、「語源が同じ単語を共有する割合」によって言語間の距離が算出されて来ました。しかし、それらの対象は英語とドイツ語など、「分化したのは比較的最近だが今ではほぼ全く違う言語」などでしか有用ではありません。なぜなら「方言」と言えるほど近い言語(ポルトガル語・カスティーリャ語(一般的にはスペイン語と呼ばれる)・カタルーニャ語など)のように、同語源の単語を多く共有する言語同士の差を図る指標としてあまり参考にならないからです。実際は、そのような方言集合体でこそ言語と方言の境目をはっきりさせたいのに、です。
## 1.3. どのように数値化するのか？
以下の2ステップで行います。
1. 個々の単語の**意味をベクトル(実数の集合)に変換**する
2. 単語同士のベクトルの**コサイン類似度を算出**する
### 1.3.1. 意味をベクトルに変換する
この作業のアルゴリズムはここで説明するにはあまりにも複雑であるため割愛します。ただ、誤解されそうな点を中心に完結に説明します。
#### 文脈に基づいてベクトル化されています
ベクトル化の作業は、決してでたらめに行なっているわけではありません。一緒に出てくる単語などを元に統計的に調整された数値です。
#### 元となる文章の種類やサイズによって異なります
前項を踏まえると、想像に難くないと思います。大量の文章データであるほどより多くの単語データが確保でき、質・量共に性能が向上します。
### 1.3.2. コサイン類似度を算出する
ベクトルは実数の集合であるため、通常の四則演算ではなく、コサイン類似度を用いて差を算出します。
