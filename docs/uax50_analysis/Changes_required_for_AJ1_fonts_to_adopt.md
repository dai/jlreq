# Analysis of changes required for Adobe-Japan1 (AJ1) Japanese fonts to adopt UAX 50

### Why is UAX 50 relevant?
The stability of glyph orientation between different applications and fonts was not an issue when vertical text was primarily created on word processor applications, and distributed in their proprietary formats or in print on paper or PDF. With web technologies, especially with EPUB, however, the stability of the glyph orientation becomes critical.

## Changes required for AJ1 fonts to adopt UAX 50
There are four orientation mismatches between AJ1 and UAX 50.

#### Remove vert
The following characters rotate (Tr) with AJ1 fonts while UAX 50 expects upright (U). The vert feature for these characters need to be removed to make them upright
- ‖ U+2016 DOUBLE VERTICAL LINE (双柱) ¹
- ✂ U+2702	SCISSORS ²

#### Add rotated glyph
The following character appears upright (U) while UAX 50 expects rotation with a special glyph (Tr). Add a rotated glyph, a vert feature
- 〰 U+3030	WAVY DASH ³
- ； U+FF1B FULLWIDTH SEMICOLON ⁴

#### Note
1. As DOUBLE VERTICAL LINE is a separator, "R" would be the natural expectation. c.f. 句読点、記号・符号活用辞典 (punctuation dictionary) by Shogakkan.
2. As SCISSORS is emoji-like, "U" would be the natural expectation.
3. As WAVY DASH is a dash, "R" would be the natural expectation.
4. U+FF1B FULLWIDTH SEMICOLON has UAX 50 value "Tr", and AJ1 is "U". However it is not clear from the UAX documentation, according to Koji Ishii, the "U" orientation is allowed for U+FF1B. In fact, the "Table 2. Glyph Changes for Vertical Orientation" in UAX 50 shows both upright and rotated glyphs. If the glyph orientation shown with the Table 2 is a normative part of the document, this change is not necessary.

## Issues that are blocking the adoption

2015 年に UAX 50 ができたが、momentum がほぼない。

* 一言で言うと社会的コストが大きすぎる
  * フォントの縦書き文字の方向ほ変更は互換性のために不可能であるので、別のフォント名を持った新しい商品を作る必要がある。開発および商品ラインアップを増やすコスト
  * UAX#50 の目標達成のためには、ユーザーが所有するフォントを全て入れ替える必要がある。そのコスト及び移行にかかる期間の問題
  * 移行期間中、一時的に不統一が拡大する。対応／非対応アプリケーション、対応／非対応フォント、の四つの組み合わせができる

## A proposal to make all AJ1 fonts UAX 50 compliant as-is

対応に必須な変更が4文字なので、逆に言うと、 これら4文字の UAX#50 の定義を変更することで AJ1 フォントを as-is で UAX#50 対応とすることができる。
つまり日本の主なフォントが一挙に UAX#50 対応になる。

#### U+2016	双柱, DOUBLE VERTICAL LINE
- AJ1: R もしくは Tr 相当。vert が横書き字形を機械的に回転した字形を指している
- アプリケーション：InDesign は vert を使わず回転（R 動作）。ブラウザはブラウザとフォントの組み合わせで複雑にバラバラ
- フォント（mac）：和文書体以外に Times や Helvetica を含む多くの欧文フォントがこの文字を持っている
- 現状のUAX#50: U

提案：UAX#50 を R に変更する

#### U+2702	SCISSORS
- AJ1: R もしくは Tr 相当。vert が横書き字形を機械的に回転した字形を指している
- アプリケーション：InDesign は回転？　ブラウザとフォントの組み合わせで複雑にバラバラ。 U+2016 の方向と同一
- フォント（mac）：Apple Emoji が下向きである以外は、和文書体、Arial Unicode、Zapf Dingbats がこの文字を持っていて全て右向き。絵文字的なのでUが本来ではある
- 現状のUAX#50: U

提案 A：UAX#50 を R に変更する
提案 B：もしくは、U+FF1A/B 全角コロンとセミコロンが Tr だが U/Tu 実装を例外的に許容しているのと同じ扱いにし、本来は U だが、Tu / Tr 動作を許容する

#### U+3030	WAVY DASH
- AJ1: U。vert なし
- アプリケーション：InDesign 正立。ブラウザはほぼ回転。Chrome はフォントに関わらず正立
- フォント（mac）：Apple Emoji、他は全て和文書体
- 現状のUAX#50: Tr。フォントが vert を持っていて回転されることを期待

提案：UAX#50 を R にしてアプリケーションに回転させる、もしくは U/Tu に変更して正立させる、ことで互換性を取ることができる。R がより広く実質的な互換性を確保できる

>参考情報
>文字の性質：石井さんによると、一部のフォントは回転かつ反転している。その動作を保存する必要のある場合のために Tr になっているとのこと。
>村上さんによると、そのようなフォントはMS明朝などに限られ例外的とのこと

#### U+FF1B	FULLWIDTH SEMICOLON
- AJ1: U。vert なし
- アプリケーション：InDesign 正立
- フォント（mac）：Apple Emoji、他は全て和文書体
- 現状のUAX#50: Tr。フォントが vert を持っていて回転されることを期待

提案：UAX#50 を U/Tu に変更して正立させることで互換性を取ることができる


## Remaining issues
UAX 50 orientation is achieved by the coordination of applications and fonts, i.e. both of them need to adopt to achieve the UAX 50 orientation. Generally speaking regardless of UAX 50 adoption status applications rotate glyphs when their expected orientation is R, and otherwise leave them to the font. Depending on the application's current behavior a significant number of characters can change their orientation when they adopt UAX 50. Applications for publishing market are affected the most. It is mainly because UAX 50 applications rotate characters that have been traditionally upright in Japanese layout. They include Greek, Cyrillic, some math operators and other symbols. While most changes look reasonable especially in an internationalized environment, an issue is that the document layout based on UAX 50 is not compatible with the existing one due to the change. Also, the change of the glyph orientation happens regardless of the original design intent of the glyphs in the fonts.

I can think of two possible impacts:
- Applications / CSS might need to add a switch that allows users to choose between UAX 50 layout vs traditional layout.
- While changing the glyph design is not necessary for fonts to adopt UAX 50, adoption by applications might affect them long-term.

## Analysis
Here are the complete analysis.

↓Current behavior|UAX R|UAX Tr|UAX Tu|UAX U
--|--|--|--|--
R |OK|add Tr glyph|check glyph|check glyph
Tr|check glyph (′″ Appendix x)|confirmed OK (「」： Appendix x)|remove vert|remove vert (‖✂)
Tu|check glyph|add Tr glyph|confirmed OK (ぁ、Appendix x)|OK
U |check glyph|add Tr glyph (〰；)|OK (!?)|OK


Current|UAX 50|影響|文字の例
----|-----|----|------
U/Tu|R|全角文字が回転。影響の大きいカテゴリ|ギリシャ文字、キリル文字、数学記号など。Appendix 1
Tr|R|回転された縦書き字形の調整が不可能になる|問題となる文字は知られていない。上記「使われなくなるvert」参照
U/Tu|Tr||(U+3030	WAVY DASH)
Tr|U/Tu|この例は知られていない。
R|U/Tu|アプリケーションが回転 → vertがないので正立|若干の約物、分数、数学記号など。Appendix 2
R|Tr|回転 → UAX#50の期待に反する正立|(全角セミコロン、WAVY DASH。上記提案のUAX#50の変更で解決)


#### Characters that have been traditionally upright but rotated with UAX 50
- Greek
- Cyrillic
- Math symbols (these are already proportional in some Japanese fonts)
- Other symbols
See Appendix 1 for the list of characters.

#### Characters that have been traditionally rotated but upright with UAX 50


#### vert entries that are no longer be used
There will be no impacts as long as applications rotates glyphs correctly at the center. See Appendix 3 for the list of characters.

AJ1 で U+2032, U+2033 は Tr 動作に見える


上記の提案によって AJ1 フォントは変更なしで UAX#50 対応となる。
ただしアプリケーションが UAX#50 に対応することで、文字によって縦書き字形が変わる場合のあることに注意する必要がある。
これは字形の選択がアプリケーションとフォントの協調によって決まるからである。
簡単に言うと、アプリケーションは、文字が UAX#50 用語で言うところの R である場合には自ら回転させ、その他の場合にはフォントに従う。
ゆえ、アプリケーションが UAX#50 に対応することで R への変更または R からの変更のある文字の縦書き字形に影響がある。以下にまとめる。

現状|UAX#50|影響|文字の例
----|-----|----|------
U/Tu|R|全角文字が回転。影響の大きいカテゴリ|ギリシャ文字、キリル文字、数学記号など。Appendix 1
Tr|R|回転された縦書き字形の調整が不可能になる|問題となる文字は知られていない。上記「使われなくなるvert」参照
R|U/Tu|アプリケーションが回転 → vertがないので正立|若干の約物、分数、数学記号など。Appendix 2
R|Tr|回転 → UAX#50の期待に反する正立|全角セミコロン、WAVY DASH。上記提案のUAX#50の変更で解決

これによりフォントへの長期的影響がある。現状全角で実装されているギリシャ文字や数学記号などはプロポーショナルに移行することが期待されるであろう。
国際化システムにおいては、日本語文脈においても、これらの文字には他のフォントが用いられることも多い。
日本語フォントにあるギリシア文字のセットはギリシア語を表すには不十分であるし、また数式にはもっぱら専用の数学記号フォントが用いられる。
日本語フォントがこれらの文字を持ち続ける意味を再考する必要があろう。

アプリケーションへの影響は文書互換性。レイアウトを保つことが重要なアプリケーションでは、UAX#50 に対応する際に UAX#50 モードを導入するなどの対応が必要になるかもしれない。
CSS にも同様なスイッチが必要になる可能性がある。この場合、UAX#50 とJIS互換モードの切り替えになるが、このJIS互換モード、の定義が必要となろう。


## Appendix

注意：下記のリストは JIS X 0213 の範囲内での調査。AJ1の文字全てをカバーするわけではない

### Appendix 1: 現在正立だが、アプリケーションのUAX#50対応によって回転となる文字
#### 外国語文字（最初の一文字のみ）
Unicode|文字|文字名|Unicode character name|UAX 50|InD F|InD P|GC|UAX 11|Unicode Block
-------|----|-----|----------------------|-|-|-|-|-|--
U+0391|Α|ギリシア大文字ALPHA|GREEK CAPITAL LETTER ALPHA|R|U|R|L&|A|Greek and Coptic
U+0410|А|キリール大文字A|CYRILLIC CAPITAL LETTER A|R|U|R|L&
#### 数学記号
Unicode|文字|文字名|Unicode character name|UAX 50|InD F|InD P|GC|UAX 11|Unicode Block
-------|----|-----|----------------------|-|-|-|-|-|--
U+FF0D|－|全角負符号、減算記号|FULLWIDTH MINUS SIGN|R|U|U|Pd|F|Halfwidth and Fullwidth Forms
U+FF1C|＜|全角不等号（より小）|FULLWIDTH LESS-THAN SIGN|R|U|U|Sm|F|Halfwidth and Fullwidth Forms
U+FF1E|＞|全角不等号（より大）|FULLWIDTH GREATER-THAN SIGN|R|U|U|Sm|F|Halfwidth and Fullwidth Forms
U+2208|∈|属する|ELEMENT OF|R|U|R|Sm|A|Mathematical Operators
U+2209|∉|要素の否定、元の否定|NOT AN ELEMENT OF|R|U|R|Sm|N|Mathematical Operators
U+220B|∋|元として含む|CONTAINS AS MEMBER|R|U|R|Sm|A|Mathematical Operators
U+221D|∝|比例|PROPORTIONAL TO|R|U|R|Sm|A|Mathematical Operators
U+2225|∥|平行|PARALLEL TO|R|U|R|Sm|A|Mathematical Operators
U+2226|∦|平行の否定|NOT PARALLEL TO|R|U|R|Sm|N|Mathematical Operators
U+2227|∧|及び（合接）|LOGICAL AND|R|U|R|Sm|A|Mathematical Operators
U+2228|∨|又は（離接）|LOGICAL OR|R|U|R|Sm|A|Mathematical Operators
U+2229|∩|共通集合|INTERSECTION|R|U|R|Sm|A|Mathematical Operators
U+222A|∪|合併集合|UNION|R|U|R|Sm|A|Mathematical Operators
U+223D|∽|相似|REVERSED TILDE|R|U|R|Sm|A|Mathematical Operators
U+2243|≃|漸進的に等しい、ホモトープ|ASYMPTOTICALLY EQUAL TO|R|U|R|Sm|N|Mathematical Operators
U+2245|≅|同形|APPROXIMATELY EQUAL TO|R|U|R|Sm|N|Mathematical Operators
U+2248|≈|近似的に等しい、同相|ALMOST EQUAL TO|R|U|R|Sm|A|Mathematical Operators
U+2252|≒|ほとんど等しい|APPROXIMATELY EQUAL TO OR THE IMAGE OF|R|U|R|Sm|A|Mathematical Operators
U+2260|≠|等号否定|NOT EQUAL TO|R|U|R|Sm|A|Mathematical Operators
U+2261|≡|常に等しい、合同|IDENTICAL TO|R|U|R|Sm|A|Mathematical Operators
U+2262|≢|合同否定|NOT IDENTICAL TO|R|U|R|Sm|N|Mathematical Operators
U+2266|≦|より小さいか又は等しい|LESS-THAN OVER EQUAL TO|R|U|R|Sm|A|Mathematical Operators
U+2267|≧|より大きいか又は等しい|GREATER-THAN OVER EQUAL TO|R|U|R|Sm|A|Mathematical Operators
U+226A|≪|非常に小さい|MUCH LESS-THAN|R|U|R|Sm|A|Mathematical Operators
U+226B|≫|非常に大きい|MUCH GREATER-THAN|R|U|R|Sm|A|Mathematical Operators
U+2276|≶|小さいか大きい|LESS-THAN OR GREATER-THAN|R|U|R|Sm|N|Mathematical Operators
U+2277|≷|大きいか小さい|GREATER-THAN OR LESS-THAN|R|U|R|Sm|N|Mathematical Operators
U+2282|⊂|真部分集合|SUBSET OF|R|U|R|Sm|A|Mathematical Operators
U+2283|⊃|真部分集合（逆方向）|SUPERSET OF|R|U|R|Sm|A|Mathematical Operators
U+2286|⊆|部分集合|SUBSET OF OR EQUAL TO|R|U|R|Sm|A|Mathematical Operators
U+2287|⊇|部分集合（逆方向）|SUPERSET OF OR EQUAL TO|R|U|R|Sm|A|Mathematical Operators
U+22A5|⊥|垂直|UP TACK|R|U|R|Sm|A|Mathematical Operators
U+2213|∓|負又は正符号|MINUS-OR-PLUS SIGN|R|U|R|Sm|N|Mathematical Operators
U+22BF|⊿|直角三角|RIGHT TRIANGLE|R|U|R|Sm|A|Mathematical Operators
U+29BF|⦿|丸中黒|CIRCLED BULLET|R|U|R|Sm|N|Miscellaneous Mathematical Symbols-B
U+221A|√|根号|SQUARE ROOT|R|U|R|Sm|A|Mathematical Operators
U+221F|∟|ファクトリアル、直角|RIGHT ANGLE|R|U|R|Sm|A|Mathematical Operators
U+222B|∫|積分記号|INTEGRAL|R|U|R|Sm|A|Mathematical Operators
U+222C|∬|2重積分記号|DOUBLE INTEGRAL|R|U|R|Sm|A|Mathematical Operators
U+29FA|⧺|2プラス|DOUBLE PLUS|R|U|R|Sm|N|Miscellaneous Mathematical Symbols-B
U+29FB|⧻|3プラス|TRIPLE PLUS|R|U|R|Sm|N|Miscellaneous Mathematical Symbols-B
U+2200|∀|すべての（普通限定子）|FOR ALL|R|U|R|Sm|A|Mathematical Operators
U+2202|∂|デル、ラウンドディー|PARTIAL DIFFERENTIAL|R|U|R|Sm|A|Mathematical Operators
U+2203|∃|存在する（存在限定子）|THERE EXISTS|R|U|R|Sm|A|Mathematical Operators
U+2205|∅|空集合|EMPTY SET|R|U|R|Sm|N|Mathematical Operators
U+2207|∇|ナブラ|NABLA|R|U|R|Sm|A|Mathematical Operators
U+2212|－|負符号、減算記号|MINUS SIGN|R|U|R|Sm|N|Mathematical Operators
U+2220|∠|角|ANGLE|R|U|R|Sm|A|Mathematical Operators
U+222E|∮|経路積分記号|CONTOUR INTEGRAL|R|U|R|Sm|A|Mathematical Operators
#### その他
Unicode|文字|文字名|Unicode character name|UAX 50|InD F|InD P|GC|UAX 11|Unicode Block
-------|----|-----|----------------------|-|-|-|-|-|--
U+212B|Å|オングストローム|ANGSTROM SIGN|R|U|R|Lu|A|Letterlike Symbols
U+00B6|¶|段落記号|PILCROW SIGN|R|U|R|Po|A|Latin-1 Supplement
U+203E|‾|オーバーライン、論理否定記号|OVERLINE|R|U|R|Po|A|General Punctuation
U+00A8|¨|ウムラウト、ダイエレシス|DIAERESIS|R|U|R|Sk|A|Latin-1 Supplement
U+2194|↔|同等|LEFT RIGHT ARROW|R|U|U|Sm|A|Arrows
U+21D2|⇒|ならば（含意）|RIGHTWARDS DOUBLE ARROW|R|U|U|Sm|A|Arrows
U+21D4|⇔|同値|LEFT RIGHT DOUBLE ARROW|R|U|U|Sm|A|Arrows
U+2934|⤴|曲がり矢印上がる|ARROW POINTING RIGHTWARDS THEN CURVING UPWARDS|R|U|R|Sm|N|Supplemental Arrows-B
U+2935|⤵|曲がり矢印下がる|ARROW POINTING RIGHTWARDS THEN CURVING DOWNWARDS|R|U|R|Sm|N|Supplemental Arrows-B
U+2196|↖|左上向矢印|NORTH WEST ARROW|R|U|U|So|A|Arrows
U+2197|↗|右上向矢印|NORTH EAST ARROW|R|U|U|So|A|Arrows
U+2198|↘|右下向矢印|SOUTH EAST ARROW|R|U|U|So|A|Arrows
U+2199|↙|左下向矢印|SOUTH WEST ARROW|R|U|U|So|A|Arrows


### Appendix 2: 現在回転だが、アプリケーションのUAX#50対応によって正立となる文字
Unicode|文字|文字名|Unicode character name|UAX 50|InD F|InD P|GC|UAX 11|Unicode Block
-------|----|-----|----------------------|-|-|-|-|-|--
U+203C|‼|感嘆符二つ|DOUBLE EXCLAMATION MARK|U|R|R|Po|N|General Punctuation
U+2047|⁇|疑問符二つ|DOUBLE QUESTION MARK|U|R|R|Po|N|General Punctuation
U+2048|⁈|疑問符感嘆符|QUESTION EXCLAMATION MARK|U|R|R|Po|N|General Punctuation
U+2049|⁉|感嘆符疑問符|EXCLAMATION QUESTION MARK|U|R|R|Po|N|General Punctuation
U+2042|⁂|アステリズム|ASTERISM|U|R|R|Po|N|General Punctuation
U+2051|⁑|ダブルアステ|TWO ASTERISKS ALIGNED VERTICALLY|U|R|R|Po|N|General Punctuation
U+00BC|¼|4分の1|VULGAR FRACTION ONE QUARTER|U|R|R|No|A|Latin-1 Supplement
U+00BD|½|2分の1|VULGAR FRACTION ONE HALF|U|R|R|No|A|Latin-1 Supplement
U+00BE|¾|4分の3|VULGAR FRACTION THREE QUARTERS|U|R|R|No|A|Latin-1 Supplement
U+00A9|©|著作権表示記号|COPYRIGHT SIGN|U|R|R|So|N|Latin-1 Supplement
U+00AE|®|登録商標記号|REGISTERED SIGN|U|R|R|So|A|Latin-1 Supplement
U+210F|ℏ|エイチバー|PLANCK CONSTANT OVER TWO PI|U|R|R|Ll|N|Letterlike Symbols
U+2135|ℵ|アレフ|ALEF SYMBOL|U|R|R|Lo|N|Letterlike Symbols
U+2127|℧|モー|INVERTED OHM SIGN|U|R|R|So|N|Letterlike Symbols
U+2305|⌅|射影的関係|PROJECTIVE|U|R|R|So|N|Miscellaneous Technical
U+2306|⌆|背景的関係|PERSPECTIVE|U|R|R|So|N|Miscellaneous Technical
U+2318|⌘|コマンド記号|PLACE OF INTEREST SIGN|U|R|R|So|N|Miscellaneous Technical


### Appendix 3: UAX#50アプリケーションによって vert が無視される文字
（JIS X 0213 外 の AJ1 文字を含むがデータが完全ではない）

Unicode|文字|文字名|Unicode character name|UAX 50|InD F|InD P|GC|UAX 11|Unicode Block
-------|----|-----|----------------------|-|-|-|-|-|--
U+00B0|°|度|DEGREE SIGN|R|Tr|R|So|A|Latin-1 Supplement
U+2010|‐|ハイフン（四分）/ハイフン|HYPHEN|R|Tr|R|Pd|A|General Punctuation
U+2015|||HORIZONTAL BAR|R|Tr|Tr|Pd|A|
U+2025|‥|二点リーダ|TWO DOT LEADER|R|Tr|R|Po|A|General Punctuation
U+2026|…|三点リーダ|HORIZONTAL ELLIPSIS|R|Tr|R|Po|A|General Punctuation
U+2032|′|分|PRIME|R|Tr|R|Po|A|General Punctuation
U+2033|″|秒|DOUBLE PRIME|R|Tr|R|Po|A|General Punctuation
U+2190|←|左向矢印|LEFTWARDS ARROW|R|Tr|Tr|Sm|A|Arrows
U+2191|↑|上向矢印|UPWARDS ARROW|R|Tr|Tr|Sm|A|Arrows
U+2192|→|右向矢印|RIGHTWARDS ARROW|R|Tr|Tr|Sm|A|Arrows
U+2193|↓|下向矢印|DOWNWARDS ARROW|R|Tr|Tr|Sm|A|Arrows
U+21C4|⇄|右矢印左矢印|RIGHTWARDS ARROW OVER LEFTWARDS ARROW|R|Tr|Tr|So|N|Arrows
U+21C5|⇅||UPWARDS ARROW LEFTWARDS OF DOWNWARDS ARROW|R|||So||Arrows
U+21C6|⇆||LEFTWARDS ARROW OVER RIGHTWARDS ARROW|R|||So||Arrows
U+21E6|⇦|左向白矢印|LEFTWARDS WHITE ARROW|R|Tr|Tr|So|N|Arrows
U+21E7|⇧|上向白矢印|UPWARDS WHITE ARROW|R|Tr|Tr|So|A|Arrows
U+21E8|⇨|右向白矢印|RIGHTWARDS WHITE ARROW|R|Tr|Tr|So|N|Arrows
U+21E9|⇩|下向白矢印|DOWNWARDS WHITE ARROW|R|Tr|Tr|So|N|Arrows
U+239B|⎛||LEFT PARENTHESIS UPPER HOOK|R|||Sm||Miscellaneous Technical
U+239C|⎜||LEFT PARENTHESIS EXTENSION|R|||Sm||Miscellaneous Technical
U+239D|⎝||LEFT PARENTHESIS LOWER HOOK|R|||Sm||Miscellaneous Technical
U+239E|⎞||RIGHT PARENTHESIS UPPER HOOK|R|||Sm||Miscellaneous Technical
U+239F|⎟||RIGHT PARENTHESIS EXTENSION|R|||Sm||Miscellaneous Technical
U+23A0|⎠||RIGHT PARENTHESIS LOWER HOOK|R|||Sm||Miscellaneous Technical
U+23A1|⎡||LEFT SQUARE BRACKET UPPER CORNER|R|||Sm||Miscellaneous Technical
U+23A2|⎢||LEFT SQUARE BRACKET EXTENSION|R|||Sm||Miscellaneous Technical
U+23A3|⎣||LEFT SQUARE BRACKET LOWER CORNER|R|||Sm||Miscellaneous Technical
U+23A4|⎤||RIGHT SQUARE BRACKET UPPER CORNER|R|||Sm||Miscellaneous Technical
U+23A5|⎥||RIGHT SQUARE BRACKET EXTENSION|R|||Sm||Miscellaneous Technical
U+23A6|⎦||RIGHT SQUARE BRACKET LOWER CORNER|R|||Sm||Miscellaneous Technical
U+23A7|⎧||LEFT CURLY BRACKET UPPER HOOK|R|||Sm||Miscellaneous Technical
U+23A8|⎨||LEFT CURLY BRACKET MIDDLE PIECE|R|||Sm||Miscellaneous Technical
U+23A9|⎩||LEFT CURLY BRACKET LOWER HOOK|R|||Sm||Miscellaneous Technical
U+23AA|⎪||CURLY BRACKET EXTENSION|R|||Sm||Miscellaneous Technical
U+23AB|⎫||RIGHT CURLY BRACKET UPPER HOOK|R|||Sm||Miscellaneous Technical
U+23AC|⎬||RIGHT CURLY BRACKET MIDDLE PIECE|R|||Sm||Miscellaneous Technical
U+23AD|⎭||RIGHT CURLY BRACKET LOWER HOOK|R|||Sm||Miscellaneous Technical
U+23B0|⎰||UPPER LEFT OR LOWER RIGHT CURLY BRACKET SECTION|R|||Sm||Miscellaneous Technical
U+23B1|⎱||UPPER RIGHT OR LOWER LEFT CURLY BRACKET SECTION|R|||Sm||Miscellaneous Technical
U+2500|||BOX DRAWINGS LIGHT HORIZONTAL|R|Tr|Tr|So|A|Box Drawing
U+2501|||BOX DRAWINGS HEAVY HORIZONTAL|R|Tr|Tr|So|A|Box Drawing
U+2502|||BOX DRAWINGS LIGHT VERTICAL|R|Tr|Tr|So|A|Box Drawing
U+2503|||BOX DRAWINGS HEAVY VERTICAL|R|Tr|Tr|So|A|Box Drawing
U+2504|┄||BOX DRAWINGS LIGHT TRIPLE DASH HORIZONTAL|R|||So||Box Drawing
U+2505|┅||BOX DRAWINGS HEAVY TRIPLE DASH HORIZONTAL|R|||So||Box Drawing
U+2506|┆||BOX DRAWINGS LIGHT TRIPLE DASH VERTICAL|R|||So||Box Drawing
U+2507|┇||BOX DRAWINGS HEAVY TRIPLE DASH VERTICAL|R|||So||Box Drawing
U+2508|┈||BOX DRAWINGS LIGHT QUADRUPLE DASH HORIZONTAL|R|||So||Box Drawing
U+2509|┉||BOX DRAWINGS HEAVY QUADRUPLE DASH HORIZONTAL|R|||So||Box Drawing
U+250A|┊||BOX DRAWINGS LIGHT QUADRUPLE DASH VERTICAL|R|||So||Box Drawing
U+250B|┋||BOX DRAWINGS HEAVY QUADRUPLE DASH VERTICAL|R|||So||Box Drawing
U+250C|||BOX DRAWINGS LIGHT DOWN AND RIGHT|R|Tr|Tr|So|A|Box Drawing
U+250D|┍||BOX DRAWINGS DOWN LIGHT AND RIGHT HEAVY|R|||So||Box Drawing
U+250E|┎||BOX DRAWINGS DOWN HEAVY AND RIGHT LIGHT|R|||So||Box Drawing
U+250F|||BOX DRAWINGS HEAVY DOWN AND RIGHT|R|Tr|Tr|So|A|Box Drawing
U+2510|||BOX DRAWINGS LIGHT DOWN AND LEFT|R|Tr|Tr|So|A|Box Drawing
U+2511|┑||BOX DRAWINGS DOWN LIGHT AND LEFT HEAVY|R|||So||Box Drawing
U+2512|┒||BOX DRAWINGS DOWN HEAVY AND LEFT LIGHT|R|||So||Box Drawing
U+2513|||BOX DRAWINGS HEAVY DOWN AND LEFT|R|Tr|Tr|So|A|Box Drawing
U+2514|||BOX DRAWINGS LIGHT UP AND RIGHT|R|Tr|Tr|So|A|Box Drawing
U+2515|┕||BOX DRAWINGS UP LIGHT AND RIGHT HEAVY|R|||So||Box Drawing
U+2516|┖||BOX DRAWINGS UP HEAVY AND RIGHT LIGHT|R|||So||Box Drawing
U+2517|||BOX DRAWINGS HEAVY UP AND RIGHT|R|Tr|Tr|So|A|Box Drawing
U+2518|||BOX DRAWINGS LIGHT UP AND LEFT|R|Tr|Tr|So|A|Box Drawing
U+2519|┙||BOX DRAWINGS UP LIGHT AND LEFT HEAVY|R|||So||Box Drawing
U+251A|┚||BOX DRAWINGS UP HEAVY AND LEFT LIGHT|R|||So||Box Drawing
U+251B|||BOX DRAWINGS HEAVY UP AND LEFT|R|Tr|Tr|So|A|Box Drawing
U+251C|||BOX DRAWINGS LIGHT VERTICAL AND RIGHT|R|Tr|Tr|So|A|Box Drawing
U+251D|||BOX DRAWINGS VERTICAL LIGHT AND RIGHT HEAVY|R|Tr|Tr|So|A|Box Drawing
U+251E|┞||BOX DRAWINGS UP HEAVY AND RIGHT DOWN LIGHT|R|||So||Box Drawing
U+251F|┟||BOX DRAWINGS DOWN HEAVY AND RIGHT UP LIGHT|R|||So||Box Drawing
U+2520|||BOX DRAWINGS VERTICAL HEAVY AND RIGHT LIGHT|R|Tr|Tr|So|A|Box Drawing
U+2521|┡||BOX DRAWINGS DOWN LIGHT AND RIGHT UP HEAVY|R|||So||Box Drawing
U+2522|┢||BOX DRAWINGS UP LIGHT AND RIGHT DOWN HEAVY|R|||So||Box Drawing
U+2523|||BOX DRAWINGS HEAVY VERTICAL AND RIGHT|R|Tr|Tr|So|A|Box Drawing
U+2524|||BOX DRAWINGS LIGHT VERTICAL AND LEFT|R|Tr|Tr|So|A|Box Drawing
U+2525|||BOX DRAWINGS VERTICAL LIGHT AND LEFT HEAVY|R|Tr|Tr|So|A|Box Drawing
U+2526|┦||BOX DRAWINGS UP HEAVY AND LEFT DOWN LIGHT|R|||So||Box Drawing
U+2527|┧||BOX DRAWINGS DOWN HEAVY AND LEFT UP LIGHT|R|||So||Box Drawing
U+2528|||BOX DRAWINGS VERTICAL HEAVY AND LEFT LIGHT|R|Tr|Tr|So|A|Box Drawing
U+2529|┩||BOX DRAWINGS DOWN LIGHT AND LEFT UP HEAVY|R|||So||Box Drawing
U+252A|┪||BOX DRAWINGS UP LIGHT AND LEFT DOWN HEAVY|R|||So||Box Drawing
U+252B|||BOX DRAWINGS HEAVY VERTICAL AND LEFT|R|Tr|Tr|So|A|Box Drawing
U+252C|||BOX DRAWINGS LIGHT DOWN AND HORIZONTAL|R|Tr|Tr|So|A|Box Drawing
U+252D|┭||BOX DRAWINGS LEFT HEAVY AND RIGHT DOWN LIGHT|R|||So||Box Drawing
U+252E|┮||BOX DRAWINGS RIGHT HEAVY AND LEFT DOWN LIGHT|R|||So||Box Drawing
U+252F|||BOX DRAWINGS DOWN LIGHT AND HORIZONTAL HEAVY|R|Tr|Tr|So|A|Box Drawing
U+2530|||BOX DRAWINGS DOWN HEAVY AND HORIZONTAL LIGHT|R|Tr|Tr|So|A|Box Drawing
U+2531|┱||BOX DRAWINGS RIGHT LIGHT AND LEFT DOWN HEAVY|R|||So||Box Drawing
U+2532|┲||BOX DRAWINGS LEFT LIGHT AND RIGHT DOWN HEAVY|R|||So||Box Drawing
U+2533|||BOX DRAWINGS HEAVY DOWN AND HORIZONTAL|R|Tr|Tr|So|A|Box Drawing
U+2534|||BOX DRAWINGS LIGHT UP AND HORIZONTAL|R|Tr|Tr|So|A|Box Drawing
U+2535|┵||BOX DRAWINGS LEFT HEAVY AND RIGHT UP LIGHT|R|||So||Box Drawing
U+2536|┶||BOX DRAWINGS RIGHT HEAVY AND LEFT UP LIGHT|R|||So||Box Drawing
U+2537|||BOX DRAWINGS UP LIGHT AND HORIZONTAL HEAVY|R|Tr|Tr|So|A|Box Drawing
U+2538|||BOX DRAWINGS UP HEAVY AND HORIZONTAL LIGHT|R|Tr|Tr|So|A|Box Drawing
U+2539|┹||BOX DRAWINGS RIGHT LIGHT AND LEFT UP HEAVY|R|||So||Box Drawing
U+253A|┺||BOX DRAWINGS LEFT LIGHT AND RIGHT UP HEAVY|R|||So||Box Drawing
U+253B|||BOX DRAWINGS HEAVY UP AND HORIZONTAL|R|Tr|Tr|So|A|Box Drawing
U+253D|┽||BOX DRAWINGS LEFT HEAVY AND RIGHT VERTICAL LIGHT|R|||So||Box Drawing
U+253E|┾||BOX DRAWINGS RIGHT HEAVY AND LEFT VERTICAL LIGHT|R|||So||Box Drawing
U+253F|||BOX DRAWINGS VERTICAL LIGHT AND HORIZONTAL HEAVY|R|Tr|Tr|So|A|Box Drawing
U+2540|╀||BOX DRAWINGS UP HEAVY AND DOWN HORIZONTAL LIGHT|R|||So||Box Drawing
U+2541|╁||BOX DRAWINGS DOWN HEAVY AND UP HORIZONTAL LIGHT|R|||So||Box Drawing
U+2542|||BOX DRAWINGS VERTICAL HEAVY AND HORIZONTAL LIGHT|R|Tr|Tr|So|A|Box Drawing
U+2543|╃||BOX DRAWINGS LEFT UP HEAVY AND RIGHT DOWN LIGHT|R|||So||Box Drawing
U+2544|╄||BOX DRAWINGS RIGHT UP HEAVY AND LEFT DOWN LIGHT|R|||So||Box Drawing
U+2545|╅||BOX DRAWINGS LEFT DOWN HEAVY AND RIGHT UP LIGHT|R|||So||Box Drawing
U+2546|╆||BOX DRAWINGS RIGHT DOWN HEAVY AND LEFT UP LIGHT|R|||So||Box Drawing
U+2547|╇||BOX DRAWINGS DOWN LIGHT AND UP HORIZONTAL HEAVY|R|||So||Box Drawing
U+2548|╈||BOX DRAWINGS UP LIGHT AND DOWN HORIZONTAL HEAVY|R|||So||Box Drawing
U+2549|╉||BOX DRAWINGS RIGHT LIGHT AND LEFT VERTICAL HEAVY|R|||So||Box Drawing
U+254A|╊||BOX DRAWINGS LEFT LIGHT AND RIGHT VERTICAL HEAVY|R|||So||Box Drawing
U+261C|☜||WHITE LEFT POINTING INDEX|R|||So||Miscellaneous Symbols
U+261D|☝||WHITE UP POINTING INDEX|R|||So||Miscellaneous Symbols
U+261E|☞|指示マーク|WHITE RIGHT POINTING INDEX|R|Tr|Tr|So|A|Miscellaneous Symbols
U+261F|☟||WHITE DOWN POINTING INDEX|R|||So||Miscellaneous Symbols
U+27A1|➡||BLACK RIGHTWARDS ARROW|R|||So||Dingbats
U+2B05|⬅||LEFTWARDS BLACK ARROW|R|||So||Miscellaneous Symbols and Arrows
U+2B06|⬆||UPWARDS BLACK ARROW|R|||So||Miscellaneous Symbols and Arrows
U+2B07|⬇||DOWNWARDS BLACK ARROW|R|||So||Miscellaneous Symbols and Arrows
U+FF1D|＝|全角等号|FULLWIDTH EQUALS SIGN|R|Tr|Tr|Sm|F|Halfwidth and Fullwidth Forms
