---
slug: /
label: Factory(Contract)
title: Factory(Contract)
sidebar_position: 1
---

## Factory

# Functions

### createGovernor

```
function createGovernor(uint256 profileId, uint256 timelockDelay) public
```

为 profile 创建治理

#### Parameters

| Name          | Type    | Description                  |
| ------------- | ------- | ---------------------------- |
| profileId     | uint256 |                              |
| timelockDelay | uint256 | timelock 中 queue 的等待时间 |

### createGovernorForToken

```
function getGovAddr(uint256 profileId) public view returns (address)
```

获取 profile 的治理地址

#### Parameters

| Name      | Type    | Description |
| --------- | ------- | ----------- |
| profileId | uint256 |             |

#### Returns Vaues

| Name | Type    | Description    |
| ---- | ------- | -------------- |
| addr | address | 治理合约的地址 |
