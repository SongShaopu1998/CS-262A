## FFS

大部分笔记记录在paper上，这里写几个ppt上讲到的之前没注意的点。

### Elevator Algorithms

在现代的磁盘调度任务中，原本很多OS执行的任务被分配到了disk controller来执行，因为磁盘可以更好的规划自身的复杂任务，其中一个例子是`elevator algorithms`:

<img src="https://shaopu-blog.oss-cn-beijing.aliyuncs.com/img/2023-09-01-192747.png" alt="image-20230901122747578" style="zoom:50%;" />

> When a new request arrives while the drive is idle, the initial arm/head movement will be in the direction of the cylinder where the data is stored, either *in* or *out*. As additional requests arrive, requests are serviced only in the current direction of arm movement until the arm reaches the edge of the disk. When this happens, the direction of the arm reverses, and the requests that were remaining in the opposite direction are serviced, and so on.[[1\]](https://en.wikipedia.org/wiki/Elevator_algorithm#cite_note-1)

### Track buffers

<img src="https://shaopu-blog.oss-cn-beijing.aliyuncs.com/img/2023-09-01-192905.png" alt="image-20230901122904377" style="zoom:50%;" />

### Block Groups Allocation

<img src="https://shaopu-blog.oss-cn-beijing.aliyuncs.com/img/2023-09-01-193417.png" alt="image-20230901123417526" style="zoom:50%;" />

使用`First Fit`的分配方法：

> Few little holes at start, then sequential runs at end of group

<img src="https://shaopu-blog.oss-cn-beijing.aliyuncs.com/img/2023-09-01-193541.png" alt="image-20230901123540450" style="zoom:50%;" />

