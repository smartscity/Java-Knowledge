# Distributed System Algorithm

#### Paxos & Vector Clock

* **e.g.**
  * **PaxosStore for wechat**
  * **zookeeper**

#### Raft & Log Sync

* **e.g.**
  * **Redis Sentinel**
  * **Mongodb 复制**



#### Paxos VS Raft

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">选举算法</th>
      <th style="text-align:left">最新Node</th>
      <th style="text-align:left">时间</th>
      <th style="text-align:left">复杂度</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Paxos</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left">不限于</td>
      <td style="text-align:left">O(N)</td>
      <td style="text-align:left">高</td>
    </tr>
    <tr>
      <td style="text-align:left">Raft</td>
      <td style="text-align:left">是</td>
      <td style="text-align:left">
        <p>仅限于</p>
        <p>Max Term&Index</p>
      </td>
      <td style="text-align:left">O(N)</td>
      <td style="text-align:left">中</td>
    </tr>
  </tbody>
</table>