
<div align='center'>

<h1>GuwenEE</h1>
<h3>古汉语事件抽取语料库</h3>


------

<p align="center">  
    <a href="https://github.com/Lyn4ever29/GuwenEE/releases">
        <img alt="Download" src="https://img.shields.io/badge/Download-GuwenEE-orange">
    </a>
    <a href="https://github.com/Lyn4ever29/GuwenEE/blob/main/LICENCE">
        <img alt="License" src="https://img.shields.io/badge/Licence-CC--BY-blue">
    </a>
</p>


</div>


> If you want to read this instruction in English, please click [here](README.en.md).


本仓库是古汉语事件抽取的语料库GuwenEE及其基准测评代码库。


# 语料库介绍
本语料库是一个古汉语领域事件抽取语料库，原始数据来自《二十四史》，从中随机抽取部分句子作为标注语料，通过大规模语言模型与人工相结合的方式构建。
包含古汉语句子1000条，7个事件类别（一个分类），72个事件类型（二级分类），1928 个事件。
本语料库构建的事件类型如下：

|事件类别|事件类型|
|--|--|
|人生|出生,婚嫁,死亡,继承,高中,......|
|战争|起兵事件,进攻事件,出征事件,围攻事件,......|
|政治|推举事件,诬陷事件,叛变事件,诏谕事件,......|
|民事|迁徙事件,宗教事件,射箭事件,其他事件,......|
|日常|会见事件,交谈事件,出行事件,谴责事件,......|
|文化|历史记载,撰写事件,......|
|地理|地震,海汛,流星雨,......|

以下是本语料库示例：
```json
{
    "text": "十二月己未，突厥复攻茹茹，茹茹举国南奔。",
    "id": "c255a834c06e459d8be4b88d01c347aa",
    "event_list": [
        {
            "event_type": "战争/进攻事件",
            "trigger": "攻",
            "trigger_start_index": 9,
            "arguments": [
                {
                    "argument_start_index": 0,
                    "role": "时间",
                    "argument": "十二月己未",
                    "alias": []
                },
                {
                    "argument_start_index": 10,
                    "role": "被攻击者",
                    "argument": "茹茹",
                    "alias": []
                },
                {
                    "argument_start_index": 6,
                    "role": "进攻者",
                    "argument": "突厥",
                    "alias": []
                }
            ],
            "class": "战争"
        },
        {
            "event_type": "战争/逃逸事件",
            "trigger": "奔",
            "trigger_start_index": 18,
            "arguments": [
                {
                    "argument_start_index": 0,
                    "role": "时间",
                    "argument": "十二月己未",
                    "alias": []
                },
                {
                    "argument_start_index": 17,
                    "role": "地点",
                    "argument": "南",
                    "alias": []
                },
                {
                    "argument_start_index": 13,
                    "role": "逃逸者",
                    "argument": "茹茹举国",
                    "alias": []
                }
            ],
            "class": "战争"
        }
    ]
}
```

# 语料库下载
本语料库提供标注结果和事件Schema,您可以在本仓库中[Releases](https://github.cong/Lyn4ever29/GuwenEE/releases)中下载

# 测评实验运行
## 实验设计
使用4个在古汉语领域的预训练模型在事件抽取的子任务上进行实验。
* 事件抽取任务
  * 事件识别(Event Detection,ED)
  * 事件元素提取(EventArgument Extraction,EAE)
* 选用的模型：
  * [guwenbert-base](https://huggingface.co/ethanyt/guwenbert-base)
  * [roberta-clas-sical-chinese-base](https://huggingface.co/KoichiYasuoka/roberta-clas-sical-chinese-base-char)
  * [sikubert](https://huggingface.co/sikubert)
  * [sikuroberta](https://huggingface.co/sikuroberta)

## 安装环境
运行本仓库代码，需要Python 3.9+,Pytorch 1.12.1+,transform 4.21.0+ 
```shell
pip install -r requirements.txt 
```

## 数据处理
```shell
python data/data_processed.py \
    --data_dir ./original \
    --save_dir ./processed
```
## 修改配置文件
项目配置文件在[config](./config)目录下，其中包含各个模型的相关配置，您可以根据自己的需要修改。更多内容，请参考[OmniEvent](https://github.com/THU-KEG/OmniEvent)


## 模型下载

本文所用预训练模型来自于[huggingface](https://huggingface.co/),请将模型下载到本地加载使用,下载到本地```model```目录下
* [guwenbert-base](https://huggingface.co/ethanyt/guwenbert-base)
* [roberta-clas-sical-chinese-base](https://huggingface.co/KoichiYasuoka/roberta-clas-sical-chinese-base-char)
* [sikubert](https://huggingface.co/sikubert)
* [sikuroberta](https://huggingface.co/sikuroberta)

## 运行ED序列标注任务
```shell
python ./scripts/ED/sequence_labeling.py ../config/ED/sikubert/guwenee.yaml
```
##  运行EAE序列标注任务
```shell
python ./scripts/ED/sequence_labeling.py ../config/ED/sikubert/guwenee.yaml
```

## 实验测评结果
<table style="height: 340px;" width="707">
<tbody>
<tr>
<td style="width: 141.238px;"></td>
<td style="width: 141.238px;"></td>
<td style="width: 141.238px;">Guwen-BERT</td>
<td style="width: 141.238px;">roberta-clas-sical-chinese-base</td>
<td style="width: 141.238px;">Siku-BERT</td>
<td style="width: 141.238px;">Siku-RoBERTaED</td>
</tr>
<tr>
<td style="width: 141.238px;" rowspan="3">ED</td>
<td style="width: 141.238px;">Precision</td>
<td style="width: 141.238px;">27.10</td>
<td style="width: 141.238px;">58.72</td>
<td style="width: 141.238px;">63.09</td>
<td style="width: 141.238px;">59.30</td>
</tr>
<tr>
<td style="width: 141.238px;">Recall</td>
<td style="width: 141.238px;">27.10</td>
<td style="width: 141.238px;">28.34</td>
<td style="width: 141.238px;">48.30</td>
<td style="width: 141.238px;">45.17</td>
</tr>
<tr>
<td style="width: 141.238px;">F1</td>
<td style="width: 141.238px;">38.77</td>
<td style="width: 141.238px;">38.23</td>
<td style="width: 141.238px;">54.71</td>
<td style="width: 141.238px;">51.28</td>
</tr>
<tr>
<td style="width: 141.238px;" rowspan="3">EAE</td>
<td style="width: 141.238px;">Precision</td>
<td style="width: 141.238px;">21.42</td>
<td style="width: 141.238px;">20.05</td>
<td style="width: 141.238px;">45.56</td>
<td style="width: 141.238px;">40.40</td>
</tr>
<tr>
<td style="width: 141.238px;">Recall</td>
<td style="width: 141.238px;">11.96</td>
<td style="width: 141.238px;">8.15</td>
<td style="width: 141.238px;">43.26</td>
<td style="width: 141.238px;">39.61</td>
</tr>
<tr>
<td style="width: 141.238px;">F1</td>
<td style="width: 141.238px;">14.50</td>
<td style="width: 141.238px;">11.57</td>
<td style="width: 141.238px;">44.38</td>
<td style="width: 141.238px;">40.00</td>
</tr>
</tbody>
</table>

# 附录
本仓库代码使用的开源框架如下,在此感谢他们在相关开源工作中的贡献。
*   [古汉语分词器jiayan](https://github.com/jiaeyan/Jiayan)
*   清华事件抽取开源框架[OmniEvent](https://github.com/THU-KEG/OmniEvent)
