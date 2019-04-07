PERM(Policy, Effect, Request, Matchers)模型很简单, 但是反映了权限的本质 – 访问控制
Policy: 定义权限的规则
Effect: 定义组合了多个 Policy 之后的结果, allow/deny
Request: 访问请求, 也就是谁想操作什么
Matcher: 判断 Request 是否满足 Policy
