---
sidebar_position: 3
title: GraphQL
---

治理相关的 graph 字段解释

### Governor_Project

治理数据

| Field           | Type                                            | Description                  |
| --------------- | ----------------------------------------------- | ---------------------------- |
| id              | String                                          | 主键，为对应治理合约地址     |
| currentStrategy | [Governor_Strategy](#governor_strategy)         | 当前 Governor 使用的投票策略 |
| timelock        | [Governor_Timelock](#governor_timelock)         | timelock 地址                |
| profileId       | BigInt                                          | profileId                    |
| proposals       | [Governor_Proposal](#governor_proposal)[]       | Governor 发起过的提案        |
| users           | [Governor_UserProject](#governor_userproject)[] | Governor 参与过投票的用户    |

### Governor_User

用户与项目绑定关系

| Field           | Type                                            | Description      |
| --------------- | ----------------------------------------------- | ---------------- |
| id              | String                                          | 用户地址         |
| voteHistory     | [Governor_Vote](#governor_vote)                 | 用户的投票历史   |
| proposalHistory | [Governor_Proposal](#governor_proposal)[]       | 用户的提案历史   |
| projects        | [Governor_UserProject](#governor_userproject)[] | 用户参与过的项目 |

### Governor_UserProject

| Field | Type                                  | Description               |
| ----- | ------------------------------------- | ------------------------- |
| id    | String                                | userId + "\|" + projectId |
| gov   | [Governor_Project](#governor_project) | 对应项目                  |
| user  | [Governor_User](#governor_user)       | 对应用户                  |

### Governor_Proposal

提案

| Field        | Type                                  | Description                                                                                            |
| ------------ | ------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| id           | String                                | proposalId+"\|"+治理合约地址                                                                           |
| gov          | [Governor_Project](#governor_project) |
| proposer     | [Governor_User](#governor_user)       | 提案人                                                                                                 |
| eta          | BigInt                                | timelock 的 ETA，为 0 时代表提案已通过，但是还没有 queue 到 timelock 里                                |
| startBlock   | BigInt                                | 提案开始区块                                                                                           |
| endBlock     | BigInt                                | 提案结束区块                                                                                           |
| agreeVotes   | BigInt                                | 同意票数                                                                                               |
| againstVotes | BigInt                                | 反对票数                                                                                               |
| abstainVotes | BigInt                                | 弃权票数                                                                                               |
| status       | String                                | 提案状态                                                                                               |
| description  | String                                | 提案描述，如果有多个字段，建议打包成 ipfs                                                              |
| quorumVotes  | BigInt                                | 法定人数                                                                                               |
| targets      | Bytes[]                               | 调用地址                                                                                               |
| values       | BigInt[]                              | 调用 value                                                                                             |
| signatures   | String[]                              | 提案执行函数签名（encode 前），如 balanceOf(address)，若函数签名已经 encode 在 calldata 里面，可以不传 |
| calldatas    | Bytes[]                               | 调用参数（encode 后）                                                                                  |
| votes        | [Governor_Vote](#governor_vote)[]     | 投票记录                                                                                               |

### Governor_Vote

提案投票记录

| Field    | Type                                    | Description                                          |
| -------- | --------------------------------------- | ---------------------------------------------------- |
| id       | String                                  | proposalId + "\|"+治理合约地址+"\|" + voter 钱包地址 |
| proposal | [Governor_Proposal](#governor_proposal) | 投票对应的提案                                       |
| voter    | [Governor_User](#governor_user)         | 投票人                                               |
| power    | BigInt                                  | 权重                                                 |
| support  | String                                  | 投票结果：against，agree 或 abstain                  |
| reason   | String                                  | 原因                                                 |

### Governor_Timelock

| Field        | Type                                            | Description                     |
| ------------ | ----------------------------------------------- | ------------------------------- |
| id           | String                                          | 对应 project 中的 timelock 地址 |
| gov          | [Governor_Project](#governor_project)           | timelock 对应的项目             |
| delay        | BigInt                                          |
| transactions | [Governor_Transaction](#governor_transaction)[] | timelock 中所有的交易           |

### Governor_Transaction

Timelock 中的交易

| Field           | Type                                    | Description                                                                                        |
| --------------- | --------------------------------------- | -------------------------------------------------------------------------------------------------- |
| id              | String                                  | timelock 中的交易 hash，keccak256(abi.encode(target, value, signature, data, eta));                |
| timelock        | [Governor_Timelock](#governor_timelock) | 交易对应的 timelock                                                                                |
| target          | Bytes                                   | 调用地址                                                                                           |
| value           | BigInt                                  | 调用 value                                                                                         |
| signature       | String                                  | 调用函数签名（encode 前），如 balanceOf(address)，若函数签名已经 encode 在 calldata 里面，可以不传 |
| calldata        | Bytes                                   | 调用参数（encode 后）                                                                              |
| eta             | BigInt                                  | 预计可执行时间                                                                                     |
| status          | String                                  | 交易状态：Canceled，Executed 或 Queued                                                             |
| transactionHash | Bytes                                   | 交易执行哈希                                                                                       |
