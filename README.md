[![CC BY-SA 4.0][cc-by-sa-shield]][cc-by-sa-link]
[![Multi-Relational Multi-Party Chat Corpusのバージョン](https://img.shields.io/github/v/release/nu-dialogue/multi-relational-multi-party-chat-corpus)](https://github.com/nu-dialogue/multi-relational-multi-party-chat-corpus)
[![Hugging Face Datasets Hub](https://img.shields.io/badge/Hugging%20Face_🤗-Datasets-ffcc66)](https://huggingface.co/datasets/nu-dialogue/multi-relational-multi-party-chat-corpus)


# Multi-Relational Multi-Party Chat Corpus

Multi-Relational Multi-Party Chat Corpus [^1] は，初対面や家族といった話者間の関係性に着目した，約1,000件の日本語雑談対話からなるコーパスです．

![Multi-Relational Multi-Party Chat Corpusのロゴ](./Multi-Relational_Multi-Party_Chat_Corpus_Logo.png?raw=true)

> [!NOTE]
> 本レポジトリは，収集した全1,000対話から，個人に関する情報をマスクし，倫理的配慮に基づいて適切と考えられる対話を選定したものです．
> [論文](#文献)では1,000対話に対する分析結果が報告されており，本レポジトリの統計情報とは若干異なることにご注意ください．


## 統計情報

|  | Multi-Relational Multi-Party Chat Corpus[^1][^2]<br>初対面対話 | Multi-Relational Multi-Party Chat Corpus[^1][^2]<br>家族入り対話 | CEJC<br>3人対話のみ[^3] |
| --- | --- | --- | --- |
| 対話数 | 477対話 | 483対話 | 297対話 |
| 話者数 | 40人 | 26人（家族6組12人，初対面14人） | 677人 |
| 対話あたりの発話数 | 104.8発話 | 105.0発話 | 1221.8発話 |
| 総発話数 | 49,986発話 | 50,694発話 | 362,888発話 |
| 発話あたりの文字数 | 10.9文字 | 12.2文字 | 6.7文字 |
| 語彙数 | 8,553単語 | 9,702単語 | 44,069単語 |
| 総単語数 | 669,600単語 | 758,356単語 | 1,264,683単語 |
| タイプ・トークン比 | 0.013 | 0.013 | 0.034 |

語彙数および単語数を求めるにあたって，形態素解析器には [MeCab](https://taku910.github.io/mecab/)，辞書には [NEologd](https://github.com/neologd/mecab-ipadic-neologd) を用いました．


## データフォーマット

[`multi_relational_multi_party_chat_corpus/`](./multi_relational_multi_party_chat_corpus) 内に，対話データ `dialogues/*.json` と，話者データ `interlocutors.json` があります．
```
multi_relational_multi_party_chat_corpus/
├── VERSION              // 本コーパスのバージョン
├── dialogues/           // 対話データ（1ファイルにつき1対話）
│   ├── A_first_time/    // 初対面対話
│   │   ├── A00101.json
│   │   ├── A00102.json
│   │   ├── ...
│   │   └── A09905.json
│   └── B_family/        // 家族入り対話
│       ├── B10001.json
│       ├── B10002.json
│       ├── ...
│       └── B15505.json
└── interlocutors.json   // 話者データ（1ファイルに全ての話者分）
```


### 対話データ

対話データには，対話ID，対話の種類，話者ID，発話，および，話者ごとの評価スコアが含まれます．評価スコアは，1が低いことを，5が高いことを示します．

| キー | 型 | 説明 |
| --- | --- | --- |
| dialogue_id | str | 対話ID．アルファベットは対話の種類（例：`B10008`の`B`）．数字の最初3桁は話者の組み合わせの通し番号（例：`B10008`の`100`）．数字の最後2桁はその話者の組み合わせが実施した対話の通し番号（例：`B10008`の`08`）． |
| dialogue_type | str | 対話の種類．`First time` (A), `Family` (B) のいずれか． |
| interlocutors | list (str) | 話者IDのリスト． |
| relationship | list (str) | 関係性のある話者IDのリスト． |
| utterances | list (dict) | 発話のリスト． |
| utterances.utterance_id | int | 発話ID．対話内で固有．0始まりのインデックス． |
| utterances.interlocutor_id | str | 話者ID． |
| utterances.text | str | 発話テキスト． |
| utterances.mention_to | list (str) | メンション先の話者IDのリスト． |
| evaluations | list (dict) | 話者ごとの評価スコアのリスト． |
| evaluations.interlocutor_id | str | 話者ID． |
| evaluations.informativeness | int | 情報量の評価スコア (1~5)． |
| evaluations.comprehension | int | 理解度の評価スコア (1~5)． |
| evaluations.familiarity | int | 親しみやすさの評価スコア (1~5)． |
| evaluations.interest | int | 興味の評価スコア (1~5)． |
| evaluations.proactiveness | int | 積極性の評価スコア (1~5)． |
| evaluations.satisfaction | int | 満足度の評価スコア (1~5)． |

```jsonc
{
    "dialogue_id": "B10008",
    "dialogue_type": "Family",
    "interlocutors": [
        "うさぎ",
        "えのき",
        "てばさき"
    ],
    "relationship": [
        "えのき",
        "てばさき"
    ],
    "utterances": [
        {
            "utterance_id": 0,
            "interlocutor_id": "うさぎ",
            "text": "こんにちは",
            "mention_to": []
        },
        {
            "utterance_id": 1,
            "interlocutor_id": "えのき",
            "text": "こんにちは",
            "mention_to": []
        },
        {
            "utterance_id": 2,
            "interlocutor_id": "てばさき",
            "text": "こんにちは",
            "mention_to": []
        },
        {
            "utterance_id": 3,
            "interlocutor_id": "うさぎ",
            "text": "何かスポーツとかやられてますか？",
            "mention_to": []
        },
        {
            "utterance_id": 4,
            "interlocutor_id": "えのき",
            "text": "特にやってないです",
            "mention_to": []
        },
        {
            "utterance_id": 5,
            "interlocutor_id": "てばさき",
            "text": "@うさぎ 運動は苦手ですね…",
            "mention_to": [
                "うさぎ"
            ]
        },
        // ...
    ],
    "evaluations": [
        {
            "interlocutor_id": "うさぎ",
            "informativeness": 4,
            "comprehension": 4,
            "familiarity": 4,
            "interest": 4,
            "proactiveness": 4,
            "satisfaction": 4
        },
        // ...
    ]
}
```

個人に関する情報を含む発話は該当部分がマスクされています．また，本レポジトリでは，倫理的配慮に基づいて適切と考えられる対話を選定しています．

|  | 公開方針 | 例 |
| --- | --- | --- |
| 個人に関する情報 | `＊＊` でマスク | <ul><li>目の前に＊＊があります</li><li>＊＊で働いてます</li></ul> |


### 話者データ

話者データには，話者IDをキーとして，話者ID，ペルソナ，性格特性，属性，テキストチャットの経験，関係性のある話者の情報が含まれます．性格特性スコアが高いほど，その性格的な傾向が強いことを示します．

| キー | 型 | 説明 |
| --- | --- | --- |
| interlocutor_id | str | 話者ID． |
| persona | list (str) | ペルソナ．10文からなる． |
| personality | dict | 性格特性． |
| personality.BigFive_* | float | Big Five[^4] のスコア (1~7)． |
| personality.KiSS18_* | float | Kikuchi's Scale of Social Skills[^5] のスコア (1~5)． |
| personality.IOS | int | Inclusion of Others in the Self[^6] のスコア (1~7)． |
| personality.ATQ_* | float | Adult Temperament Questionnaire[^7] のスコア (1~7)． |
| personality.SMS_* | float | Self-Monitoring Scale[^8] のスコア (1~5)． |
| demographic_information | dict | 属性． |
| demographic_information.gender | str | 性別．`Male`, `Female`, `Other` のいずれか． |
| demographic_information.age | str | 年齢．`-19`, `20-29`, `30-39`, `40-49`, `50-59`, `60-69` のいずれか． |
| demographic_information.education | str | 教育歴．`High school graduate`, `Two-year college`, `Four-year college`, `Postgraduate`, `Other` のいずれか． |
| demographic_information.employment_status | str | 就労状況．`Employed`, `Homemaker`, `Student`, `Retired`, `Unable to work`, `None` のいずれか． |
| demographic_information.region_of_residence | str | 居住地域．日本の都道府県名． |
| text_chat_experience | dict | テキストチャットの経験． |
| text_chat_experience.age_of_first_chat | str | 初めてテキストチャットをした時の年齢．`-9`, `10-19`, `20-29`, `30-39`, `40-49`, `50-59` のいずれか． |
| text_chat_experience.frequency | str | 普段のテキストチャットの頻度．`Every day`, `Once every few days`, `Once a week`, `Less frequent than these` のいずれか． |
| text_chat_experience.chatting_partners | list (str) | 普段のテキストチャットの相手．`Family`, `Friend`, `Colleague`, `Other` のいずれか． |
| text_chat_experience.typical_chat_content | str | 普段のテキストチャットの内容． |
| pair_information | dict | 関係性のある話者の情報． |
| pair_information.pair_flag | bool | 関係性のある話者がいるかどうか． |
| pair_information.relationship | list (str) | 話者との関係．`Frequent acquaintance`, `Someone known for years`, `Someone engaged in joint activities` のいずれか． |
| pair_information.relationship_detail | str | 話者との関係の詳細． |
| pair_information.pair_interlocutor_id | str | 関係性のある話者の話者ID． |

```jsonc
{
    "てばさき": {
        "interlocutor_id": "てばさき",
        "persona": [
            "現在文系の大学生である。",
            "好きな食べ物はジャーキー、嫌いな食べ物は生野菜だ。",
            "ゲームや漫画、小説が好きで、最近は原神というゲームにはまっている。",
            "地元のボランティアに申し込んでみたい。",
            "休日はずっと寝て過ごしたいけど課題に追われている。",
            "外出の用事があるけど寒い日はお湯を飲むことにしている。",
            "猫派だけど小動物は大体好き。",
            "ロールキャベツはなんとか作れる。",
            "埼玉生まれ埼玉育ちの埼玉県民である。",
            "最近はボカロや谷山浩子さん、Sound Horizonの曲をよく聴く。"
        ],
        "personality": {
            "BigFive_Openness": 4.083333333333333,                 // 開放性
            "BigFive_Conscientiousness": 2.3333333333333335,       // 誠実性
            "BigFive_Extraversion": 3.9166666666666665,            // 外向性
            "BigFive_Agreeableness": 4.916666666666667,            // 協調性
            "BigFive_Neuroticism": 4.333333333333333,              // 神経症傾向
            "KiSS18_BasicSkill": 2.3333333333333335,               // 初歩的な社会的スキル
            "KiSS18_AdvancedSkill": 3.0,                           // より高度の社会的スキル
            "KiSS18_EmotionalManagementSkill": 2.3333333333333335, // 感情処理の社会的スキル
            "KiSS18_OffenceManagementSkill": 3.0,                  // 攻撃に代わる社会的スキル
            "KiSS18_StressManagementSkill": 3.3333333333333335,    // ストレスを処理する社会的スキル
            "KiSS18_PlanningSkill": 2.0,                           // 計画の社会的スキル
            "IOS": 3,                                              // 他人との関係
            "ATQ_Fear": 4.142857142857143,                         // 恐れ
            "ATQ_Frustration": 4.5,                                // 欲求不満
            "ATQ_Sadness": 5.285714285714286,                      // 悲しさ
            "ATQ_Discomfort": 3.1666666666666665,                  // 不快
            "ATQ_ActivationControl": 4.142857142857143,            // 賦活的制御
            "ATQ_AttentionalControl": 1.8,                         // 注意
            "ATQ_InhibitoryControl": 3.857142857142857,            // 抑制的制御
            "ATQ_Sociability": 4.6,                                // 社交性
            "ATQ_HighIntensityPleasure": 5.571428571428571,        // 強い刺激への快
            "ATQ_PositiveAffect": 5.6,                             // 肯定的感情
            "ATQ_NeutralPerceptualSensitivity": 4.8,               // 知覚敏感性
            "ATQ_AffectivePerceptualSensitivity": 4.2,             // 感情的知覚敏感性
            "ATQ_AssociativeSensitivity": 6.8,                     // 連想的敏感性
            "SMS_Extraversion": 2.5,                               // 外向性
            "SMS_OtherDirectedness": 2.75,                         // 他者指向性
            "SMS_Acting": 3.0                                      // 演技性
        },
        "demographic_information": {
            "gender": "Female",
            "age": "-19",
            "education": "High school graduate",
            "employment_status": "Student",
            "region_of_residence": "Saitama"
        },
        "text_chat_experience": {
            "age_of_first_chat": "10-19",
            "frequency": "Every day",
            "chatting_partners": [
                "Family",
                "Friend",
                "Colleague"
            ],
            "typical_chat_content": "業務連絡、雑談、助言、相談、感動の共有"
        },
        "pair_information": {
            "pair_flag": true,
            "relationship": [
                "Someone known for years"
            ],
            "relationship_detail": "産みと育ての母",
            "pair_interlocutor_id": "えのき"
        }
    },
    // ...
}
```


## 本コーパスの使用例

> [!CAUTION]
> **本コーパスの使用にあたっては，次のことに十分注意してください．**
> * 本コーパスのデータから個人を特定しようとしないこと．
> * 本コーパスを，特定の話者へのなりすましに用いないこと．
> * 本コーパスを話者の属性や性格特性の推定などに用いる際は，自身の情報を推定されたくない話者の権利についても留意すること[^9]．


## 文献

```
@inproceedings{tsuda-etal-2025-multi-relational-multi-party-chat-corpus,
    title = "Constructing a Multi-Party Conversational Corpus Focusing on Interlocutor Relationships",
    author = "Tsuda, Taro and
      Yamashita, Sanae and
      Inoue, Koji and
      Kawahara, Tatsuya
      and Higashinaka, Ryuichiro",
    booktitle="Proceedings of the 29th Workshop on the Semantics and Pragmatics of Dialogue",
    year = "2025",
    pages = "193--202"
}


@inproceedings{tsuda-etal-2025-multi-relational-multi-party-chat-corpus-ja,
    title = "{M}ulti-{R}elational {M}ulti-{P}arty {C}hat {C}orpus: 話者間の関係性に着目したマルチパーティ雑談対話コーパス",
    author = "津田 太郎 and 山下 紗苗 and 井上 昂治 and 河原 達也 and 東中 竜一郎",
    booktitle = "言語処理学会第31回年次大会発表論文集",
    year = "2025",
    pages = "4011--4016"
}
```


## 謝辞

本コーパスは，[JSTムーンショット型研究開発事業，JPMJMS2011](https://www.avatar-ss.org/) の支援を受けて構築しました．

<img src="./Moonshot_Logo.png?raw=true" alt="ムーンショットのロゴ" style="max-width: 100%; width: 300px;">


## ライセンス

本コーパスは [CC BY-SA 4.0][cc-by-sa-link] の下で提供されます．

[![CC BY-SA 4.0][cc-by-sa-image]][cc-by-sa-link]


[^1]: 津田 太郎，山下 紗苗，井上 昂治，河原 達也，東中 竜一郎．[**Multi-Relational Multi-Party Chat Corpus: 話者間の関係性に着目したマルチパーティ雑談対話コーパス**](https://www.anlp.jp/proceedings/annual_meeting/2025/pdf_dir/D10-6.pdf)．言語処理学会第31回年次大会発表論文集, pp. 4011-4016, 2025.
[^2]: Taro Tsuda, Sanae Yamashita, Koji Inoue, Tatsuya Kawahara, and Ryuichiro Higashinaka. **Constructing a Multi-Party Conversational Corpus Focusing on Interlocutor Relationships**. In Proceedings of the 29th Workshop on the Semantics and Pragmatics of Dialogue, pp. 193-202, 2025.
[^3]: Hanae Koiso, Haruka Amatani, Yasuharu Den, Yuriko Iseki, Yuichi Ishimoto, Wakako Kashino, Yoshiko Kawabata, Ken’ya Nishikawa, Yayoi Tanaka, Yasuyuki Usuda, and Yuka Watanabe. **Design and evaluation of the corpus of everyday Japanese conversation**. In Proceedings of the Thirteenth Language Resources and Evaluation Conference, pp. 5587–5594. 2022.
[^4]: 和田 さゆり．**性格特性用語を用いたBig Five尺度の作成**．心理学研究, pp.61–67, 1996.
[^5]: 菊池 章夫．**KiSS-18研究ノート**．岩手県立大学社会福祉学部紀要, vol. 6, no. 2, pp.41-51, 2004.
[^6]: Arthur Aron, Elaine N Aron, and Danny Smollan. **Inclusion of other in the self scale and the structure of interpersonal closeness**. Journal of Personality and Social Psychology, vol. 63, no. 4, p. 596, 1992.
[^7]: David E Evans and Mary K Rothbart. **Developing a model for adult temperament**. Journal of Research in Personality, vol. 41, no. 4, pp. 868–888, 2007.
[^8]: 岩淵 千明，田中 国夫，中里 浩明．**セルフ・モニタリング尺度に関する研究**．心理学研究, vol. 53, no. 1, pp. 54–57, 1982.
[^9]: Rachael Tatman. **What I won’t build**. Keynote at the Fourth Widening Natural Language Processing Workshop at Annual Meeting of the Association for Computational Linguistics. 2020. https://www.rctatman.com/files/Tatman_2020_WiNLP_Keynote.pdf


[cc-by-sa-link]: https://creativecommons.org/licenses/by-sa/4.0/deed.ja
[cc-by-sa-image]: https://i.creativecommons.org/l/by-sa/4.0/88x31.png
[cc-by-sa-shield]: https://img.shields.io/badge/License-CC%20BY%20SA%204.0-green.svg
