# 验证 Nova 的 secgroup 部分

|内容编号|内容名称|
|--------|--------|
|01|secgroup|


|测试编号|测试目的|操作|预期结果|实际结果|备注|Rally/Tempest/None|
|--------|--------|----|--------|--------|----|------------------|
|01080101|创建并列出 secgroup||||rules_per_security_group 参数只能指定 rules 的数量||
|01080102|创建 secgroup 后，将其删除||||||