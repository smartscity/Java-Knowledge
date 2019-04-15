---
description: 分布式系统一致性算法
---

# 消息传递的一致性算法

## Paxos & Vector Clock

* **e.g.**
  * **PaxosStore for wechat**
  * **zookeeper**

## Raft & Log Sync

* **e.g.**
  * **Redis Sentinel**
  * **Mongodb 复制**

## Paxos VS Raft

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">&#x9009;&#x4E3E;&#x7B97;&#x6CD5;</th>
      <th style="text-align:left">&#x6700;&#x65B0;Node</th>
      <th style="text-align:left">&#x65F6;&#x95F4;</th>
      <th style="text-align:left">&#x590D;&#x6742;&#x5EA6;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Paxos</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left">&#x4E0D;&#x9650;&#x4E8E;</td>
      <td style="text-align:left">O(N)</td>
      <td style="text-align:left">&#x9AD8;</td>
    </tr>
    <tr>
      <td style="text-align:left">Raft</td>
      <td style="text-align:left">&#x662F;</td>
      <td style="text-align:left">
        <p>&#x4EC5;&#x9650;&#x4E8E;</p>
        <p>Max Term&amp;Index</p>
      </td>
      <td style="text-align:left">O(N)</td>
      <td style="text-align:left">&#x4E2D;</td>
    </tr>
  </tbody>
</table>## 比较

| 算法 | 相同点 | 不同点 |
| :--- | :--- | :--- |
| **Paxos** |  |  |
| **Raft** |  |  |
| **ZAB** |  |  |

