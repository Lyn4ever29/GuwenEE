
<div align='center'>

<h1>GuwenEE</h1>
<h3>A Corpus for Event Extraction in Classical Chinese</h3>


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

> [查看中文文档](README.md)

This warehouse is a corpus of ancient Chinese event extraction, GuwenEE, and its benchmark evaluation code.


# Introduction of the Corpus
This corpus is an event extraction corpus for the field of ancient Chinese, with raw data from the "Twenty Four Histories". Some sentences are randomly selected as annotated corpus, and constructed through a combination of large-scale language models and artificial methods.
Contains 1000 ancient Chinese sentences, 7 event type (firstly classification), 72 event types (secondary classification), and 1928 events.
The event types constructed by this corpus are as follows:

|firstly classification|secondary classification|
|--|--|
|人生|出生,婚嫁,死亡,继承,高中,......|
|战争|起兵事件,进攻事件,出征事件,围攻事件,......|
|政治|推举事件,诬陷事件,叛变事件,诏谕事件,......|
|民事|迁徙事件,宗教事件,射箭事件,其他事件,......|
|日常|会见事件,交谈事件,出行事件,谴责事件,......|
|文化|历史记载,撰写事件,......|
|地理|地震,海汛,流星雨,......|

An example from the corpus is as follows:
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

# Download the Corpus 
This corpus provides annotation results and event schemas,
you can find and download in [Releases](https://github.com/Lyn4ever29/GuwenEE/releases)


# Run Evaluation Experiment
## The design of Experiment
we Experiment on the subtask of event extraction 
we used four pre-trained language models in the field of ancient Chines,
* Event Extraction
  * Event Detection,abbreviated as ED
  * EventArgument Extraction,abbreviated as EAE
* we used the follow models：
  * [guwenbert-base](https://huggingface.co/ethanyt/guwenbert-base)
  * [roberta-clas-sical-chinese-base](https://huggingface.co/KoichiYasuoka/roberta-clas-sical-chinese-base-char)
  * [sikubert](https://huggingface.co/sikubert)
  * [sikuroberta](https://huggingface.co/sikuroberta)

## Preparation environment
If you want to run this code in your machine,you need Python 3.9+,Pytorch 1.12.1+,transform 4.21.0+ 
```shell
pip install -r requirements.txt 
```

##  Download Models
### jiayan Models 
Download the file named ```jiayan.klm`` from [Here](https://github.com/jiaeyan/Jiayan) and save it to the directory named ```tools```
> The above link is from the official website. If you are unable to access it, please give me with an [issue](https://github.com/Lyn4ever29/GuwenEE/issues)

### Pre-trained Language Models
The models we used are from [huggingface](https://huggingface.co/),
you can download them,and save to the  file directory named ```model```
* [guwenbert-base](https://huggingface.co/ethanyt/guwenbert-base)
* [roberta-clas-sical-chinese-base](https://huggingface.co/KoichiYasuoka/roberta-clas-sical-chinese-base-char)
* [sikubert](https://huggingface.co/sikubert)
* [sikuroberta](https://huggingface.co/sikuroberta)


## Data Processing
```shell
python data/data_processed.py \
    --data_dir ./original \
    --save_dir ./processed
```
## Modify configuration file
The project configuration file is located in the [config](./config) directory, which contains the relevant configurations for each model. You can modify it according to your own needs. 
For more information, please refer to [OmniEvent](https://github.com/THU-KEG/OmniEvent)


## Run ED sequence annotation 
```shell
python ./scripts/ED/sequence_labeling.py ../config/ED/sikubert/guwenee.yaml
```
##  Run EAE sequence annotation 
```shell
python ./scripts/ED/sequence_labeling.py ../config/ED/sikubert/guwenee.yaml
```

## Experimental result
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

# Appendix
The open source framework used in this repository code is as follows. We would like to express our gratitude for their contributions in the relevant open source work.
*   [jiayan](https://github.com/jiaeyan/Jiayan),a professional Python NLP tool for Classical Chinese.
*   [OmniEvent](https://github.com/THU-KEG/OmniEvent),a powerful open-source toolkit for event extraction, including event detection and event argument extraction. 
