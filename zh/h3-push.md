# HTTP/3服务器推送

HTTP/3的服务器推送与HTTP/2（[RFC 7540](https://httpwg.org/specs/rfc7540.html)）类似，但机制上有所不同。

服务器推送实际上就是对一个未曾发出的客户端请求做出响应！

服务器推送仅在客户端同意的前提下才允许发出。在HTTP/3中，客户端甚至能通过通告给服务器的最大推送流ID来设置所接受推送的次数限制。超出限制将导致连接错误。

如果服务器端认为客户端可能需要某个并未要求但应该有的额外资源，服务器可以通过请求流发送一个 `PUSH_PROMISE` 帧，使该推送请求看上去像是一个响应，然后通过新的流发送实际响应。

虽然客户端之前已经表示过推送可接受，但如果客户端认为适合，每个推送流仍可以随时取消，然后发送一个 `CANCEL_PUSH` 帧到服务器。

## 问题重重

自从推送这一特性在HTTP/2中讨论、开发、部署以来，它就备受争议、讨论和抨击。为了让它有用，人们付出了许多努力。

推送从来不是没有代价的，尽管它省了半个往返的延迟，它还是会消耗带宽。服务器也很难或不可能从高层面确定一个资源是否应该被推送过去。
