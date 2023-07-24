---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# 将Adobe Analytics与Customer Journey Analytics集成

{{analytics-description}}

{{customer-journey-analytics-description}}

将Adobe Analytics与Customer Journey Analytics集成可提供以下主要优势：

+ **全面的见解** 顾客的行为和偏好。
+ **无缝的跨渠道跟踪** 以全面视角。
+ **统一的数据和报告** 以便进行准确分析。
+ **增强的个性化** 以及客户参与度的提高。
+ **实时数据洞察** 进行敏捷决策。

## 常见集成

<table>
    <thead>
        <tr>
            <td>Experience Cloud应用程序</td>
            <td>集成使用</td>
            <td>何时使用</td>
            <td>常见用例</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><a href="../../integrations/tutorials/analytics-customer-journey-analytics/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Customer Journey Analytics分析</a></td>
            <td>Experience Platform源连接器</td>
            <td>
                <ul>
                    <li>TODO：使用此集成可将Analytics数据从报表包摄取到Experience Platform中。</li>
                    <li>当Customer Profile的数据可用性从数据收集开始算起可以在2-30分钟之间，并且数据湖的可用性最高可达90分钟。</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>由用户界面启动的工作流简单明了。</li>
                    <li>映射用户界面以将Analytics prop和eVar复制到新的XDM字段。</li>
                    <li>从Real-Time Customer Profile和Customer Journey Analytics中获得价值的最快方式。</li>
                </ul>
            </td>
        </tr>
        <tr>
            <td><a href="https://www.adobe.com/" target="_blank" rel="noreferrer">LINK TODO： Analytics与Customer Journey Analytics</a></td>
            <td>Experience Platform边缘</td>
            <td>
                <ul>
                    <li>适用于需要实施长期策略的情况。 这会使用AEP Web SDK、AEP Mobile SDK或Edge Network Server API将数据从设备直接发送到Experience Platform。</li>
                    <li>当您是新客户或现有客户时，他们需要Analytics数据可用性到客户配置文件以支持相同和下一页个性化用例。</li>
                </ul>
            </td>
            <td>
                <ul>
                    <li>为收集用于支持用例的数据提供最高级别的控制。</li>
                    <li>客户端数据可轻松映射到XDM字段。</li>
                    <li>使Real-Time Customer Profile能够更快地提供数据。</li>
                </ul>
            </td>
        </tr>  
    </tbody>          
</table>
