Bufer，高效的数据容器，除了布尔类型，所有原始数据类型都有相应的Bufer实现。
 Channel，类似在Linux之类操作系统上看到的文件描述符，是NIO中被用来支持批量式IO操作的一种抽象。 File 或者 Socket ，通常被认为是比较高层次的抽象，而 Channel 则是更加操作系统底层的一种抽象，这也使得 NIO 得以充分利用现代操作系统底层机制，获得特定场景的性能优 化，例如， DMA （ Direct Memory Access ）等。不同层次的抽象是相互关联的，我们可以通过 Socket 获取 Channel ，反之亦然。
 Selector，是NIO实现多路复用的基础，它提供了一种高效的机制，可以检测到注册在Selector上的多个Channel中，是否有Channel处于就绪状态，进而实现了单线程对 多 Channel 的高效管理。
