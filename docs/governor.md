---
label: Governor(Contract)
title: Governor(Contract)
sidebar_position: 2
---

## Governor

# Functions

### constructor

```
constructor(
        uint256 profileId_,
        address hub_,
        uint256 timelockDelay
    )
```

构造函数

#### Parameters

| Name          | Type    | Description                  |
| ------------- | ------- | ---------------------------- |
| profileId\_   | uint256 | profileId                    |
| hub\_         | address | lensHub 合约地址             |
| timelockDelay | uint256 | timelock 中 queue 的等待时间 |

### propose

```
function propose(
        address[] memory targets,
        uint256[] memory values,
        string[] memory signatures,
        bytes[] memory calldatas,
        string memory description,
        uint256 votingPeriod,
        uint256 votingDelay,
        uint256 quorumVotes
    ) public returns (uint256)
```

发起提案

#### Parameters

| Name         | Type      | Description                                                                                    |
| ------------ | --------- | ---------------------------------------------------------------------------------------------- |
| targets      | address[] | 提案要调用的合约地址                                                                           |
| values       | uint256[] | 提案要调用合约时要附带的 msg.value                                                             |
| signatures   | string[]  | 提案要调用合约时用的 methodId，可以为空，为空时请将 methodId 打包进 calldatas                  |
| calldatas    | bytes[]   | 提案要调用合约时用的 calldata，signatures 不为空时将拼接成 signatures+calldatas 的格式调用合约 |
| description  | string    | 提案描述                                                                                       |
| votingPeriod | uint256   | 提案持续时间（区块数）                                                                         |
| votingDelay  | uint256   | 提案公示时间（区块数）                                                                         |
| quorumVotes  | uint256   | 提案通过所需票数                                                                               |

#### Return Values

| Index/Name | Type    | Description |
| ---------- | ------- | ----------- |
| 0          | uint256 | 提案 ID     |

### queue

```
function queue(uint256 proposalId) external
```

将通过的提案加入 Timelock 队列中等待执行

#### Parameters

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | 提案 ID     |

### execute

```
function execute(uint256 proposalId) external payable
```

执行在 timelock 中可执行的提案

#### Parameters

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | 提案 ID     |

### cancel

```
function cancel(uint256 proposalId) external
```

取消提案，可由发起人主动取消，或发起提案人在 Timelock 的 delay 周期内权重小于发起提案阈值时，可由第三方取消提案（提案人属于白名单时，需要由 whitelistGuardian 进行取消）

#### Parameters

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | 提案 ID     |

### castVote

```
function castVote(uint256 proposalId, uint8 support) external
```

对指定提案进行投票

#### Parameters

| Name       | Type    | Description                       |
| ---------- | ------- | --------------------------------- |
| proposalId | uint256 | 提案 ID                           |
| support    | uint8   | 投票方向： 0=反对，1=同意，2=弃权 |

### state

```
function state(uint256 proposalId) public view returns (ProposalState)
```

获取提案状态

#### Parameters

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | 提案 ID     |

#### Return Values

| Index/Name | Type          | Description  |
| ---------- | ------------- | ------------ |
| 0          | ProposalState | 对应提案状态 |

```
    enum ProposalState {
        Pending,//待投票
        Active,//投票中
        Canceled,//已取消
        Defeated,//失败
        Succeeded,//成功
        Queued,//在timelock队列中待执行
        Expired,//已超时
        Executed//已执行
    }
```

# Struct

### Proposal

```
    struct Proposal {
        uint256 id;//提案ID
        address proposer;//提案人
        uint256 eta;//提案通过后预计可执行时间
        uint256 startBlock;//提案开始区块
        uint256 endBlock;//提案结束区块
        uint256 forVotes;//提案同意票数
        uint256 againstVotes;//提案反对票数
        uint256 abstainVotes;//提案弃权票数
        bool canceled;//提案是否被取消
        bool executed;//提案是否被执行
        string description;//提案描述,一般为ipfs的cid
        uint256 quorumVotes;//提案通过所需票数
    }
```
