title: knowledge-map 
date: 2018-08-05 09:37:09 
tags: [JavaScript] 
categories: Summary
---

&emsp;&emsp;今年三月底介入新业务线进行 hybird 开发至今，总算告一段落，期间经历了
许多，也成长了许多，其中经历最深刻的一个功能点就是个性化作业的知识图谱的模块，下
面就简单的记录一下知识图谱相关的知识。 &emsp;&emsp;知识图谱顾名思义就是将知识点
之间的关系通过关系图的方式进行展示，包括前置后置节点，以及学生掌握程度等信息。如
下图所示
：![知识图谱](//ww1.sinaimg.cn/large/8c55dc23gy1ftyl6thdn4j21ay10onkk.jpg)

## 图谱的特点

&emsp;&emsp;图谱的实现是需要经过多方同事的共同努力 ，其主要特点如下：

1.  构建
    - 多学科教研老师梳理得到
    - 知识点颗粒细、数量多、覆盖广，配备习题，支持多版本教材
    - 知识点间有着前驱后继关系，表示学习的先后顺序
2.  定量描述
    - 可从图谱展示平台上直接展示结构
    - 图谱连接边上有预先设计的权重，为知识点跳转概率
    - 根据图谱结构，通过 pageRank 计算出每个知识点的重要程度
3.  更新优化
    - 教研可在图谱展示平台动态观看图谱并改动
    - 使用学生答题数据生成动态模型，反映了知识点间的隐含关系。可以提供教 研进行
      参考并直接用于推荐

## 图谱的组成

1.  锚点关系图谱

- 有向无环图
- 锚点重要程度定量化描述(PageRank)

2.  锚点关系

- 根据学习先后顺序分为前驱、后继
- 关系边上有设定转移权重

3.  锚点

- 教研以试题分类角度设定
- 表示一类题型相似、难度相近题目

## 图谱数据分析

&emsp;&emsp;具体分析如下图：

```json
{
  "code": 200,
  "info": "操作成功",
  "result": {
    // 所有学生数量
    "studentTotalCount": 16,
    // 已覆盖知识点的掌握程度
    "coverdAnchorPointMastery": 0.3051658455,
    // 已掌握知识点数量
    "masterAnchorPoint": 40,
    // 知识点总数
    "totalAnchorPoint": 50,
    // 知识点数组
    "anchorMappingData": [
      {
        "anchorPointId": "02_05_347",
        "name": "已知解析式求函数定义域",
        // 所属知识点类型 必会 拔高 高阶
        "topicLayer": "definite",
        "anchorTopicId": "6d72e77d-f865-403f-906f-cf95eac6f637",
        // 前置知识点数组
        "precursors": [
          {
            "anchorPointId": "02_05_346",
            "weight": 0.0,
            "name": null
          }
        ],
        // 后置知识点数组
        "postcursors": null,
        "catalogs": [
          {
            "code":
              "233_12020300128-968_12020300128-968-36327_12020300128-968-36328",
            "name": null
          },
          {
            "code": "09_19020501-003_003_001",
            "name": null
          },
          {
            "code": "08_19020501-002_02_002",
            "name": null
          }
        ],
        "knowledges": null,
        "phase": null,
        "subject": null,
        "level": null,
        "cards": null,
        "videos": null,
        "anchorMastery": -1.0,
        "anchorUserNum": 0,
        "x": 5,
        "y": 5
      }
    ],
    // 包含的知识点类型数组 必会、拔高、高阶
    "examinationTypeList": [
      {
        "totalAnchorPoint": 22,
        "masterAnchorPoint": 22,
        "coverdAnchorPointMastery": 0.3479859241,
        "examinationTypeCode": "definite",
        "examinationTypeName": "必会题",
        "sort": 1,
        // 包含知识点数组
        "anchorPoints": [
          {
            "anchorTopicId": "6d72e77d-f865-403f-906f-cf95eac6f637",
            "name": "已知解析式求函数定义域"
          }
        ]
      },
      {
        "totalAnchorPoint": 15,
        "masterAnchorPoint": 15,
        "coverdAnchorPointMastery": 0.2627470402,
        "examinationTypeCode": "raise",
        "examinationTypeName": "拔高题",
        "sort": 2,
        "anchorPoints": [
          {
            "anchorTopicId": "3bea88ba-14d9-4060-a7df-635508adf1bd",
            "name": "函数创新类问题—特殊点问题"
          }
        ]
      },
      {
        "totalAnchorPoint": 3,
        "masterAnchorPoint": 3,
        "coverdAnchorPointMastery": 0.2032459627,
        "examinationTypeCode": "higher",
        "examinationTypeName": "高阶题",
        "sort": 3,
        "anchorPoints": [
          {
            "anchorTopicId": "b5b43871-038e-40ca-bc8e-84df51264f2f",
            "name": "数形结合的应用"
          }
        ]
      }
    ]
  }
}
```

## 图谱的最终效果
&emsp;&emsp;代码中其实还是有很多业务逻辑，并没有很完全的去抽离，以后有机会再来做的。    
&emsp;&emsp;最终效果如下图所示：
![图谱](//ww1.sinaimg.cn/large/8c55dc23gy1ftz5d2vyhyj21fi0yotk9.jpg)   
&emsp;&emsp;详细代码请移步codepen上去看。

<p data-height="500" data-theme-id="0" data-slug-hash="EppgGG" data-default-tab="js,result" data-user="illuSioN4ng" data-pen-title="knowledge-map" class="codepen">See the Pen <a href="https://codepen.io/illuSioN4ng/pen/EppgGG/">knowledge-map</a> by illuSioN4ng (<a href="https://codepen.io/illuSioN4ng">@illuSioN4ng</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>
