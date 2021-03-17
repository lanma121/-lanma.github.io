


> Written with [StackEdit](https://stackedit.io/).
React 有两种常见的副作用，需要清理的和不需要清理的
Effects Without Cleanup

网络请求、手动更改DOM和日志记录等是不需要清理的副作用，因为它们执行后就立即退出了。

Effects With Cleanup
事件订阅等是需要清理的副作用，因为这可能会造成内存泄漏
