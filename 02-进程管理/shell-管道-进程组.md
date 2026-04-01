- shell是一门将用户指令翻译成系统调用的语言；可以通过查看了解：man sh; man bash;
  <img width="2080" height="1086" alt="image" src="https://github.com/user-attachments/assets/a024ab9b-82b2-4a57-a3a8-36b717f56337" />
  <img width="2072" height="1010" alt="image" src="https://github.com/user-attachments/assets/fd3726a7-7f44-4df3-853a-c02381031541" />
- 空格有二义性，自然语言的缺陷
  ```
  touch "a b.txt"
  ```
- 命令优先级:
  ```
   sudo echo a > /etc/a.txt # permission deny
   sudo vi  /etc/a.txt      # ok
  ```
  <img width="2082" height="1130" alt="image" src="https://github.com/user-attachments/assets/98095dcf-a5c4-4b72-944f-5d9e3b545d4a" />

- 为什么有管道打印8个，没管道打印6个
  ```
  for(int i = 0; i < 2; i++) {
  fork();
  printf("hello world");
  }
  ```
  这个现象是由 **`fork()` 复制进程内存** 和 **`printf()` 缓冲机制** 共同作用导致的。核心区别在于：直接运行到终端时是 **行缓冲**，通过管道时是 **全缓冲**，这影响了 `fork()` 时缓冲区的复制情况。

##### 🧠 进程创建与输出分析

首先，代码会创建 **4 个进程**。每个进程理论上都会执行 `printf("hello world")`，但执行次数取决于它被创建的时机：

*   **父进程 P0**：执行 2 次 `printf`。
*   **第一次 fork 出的子进程 P1**：执行 2 次 `printf`。
*   **第二次 fork 出的子进程 P2、P3**：各执行 1 次 `printf`。

因此，如果不考虑缓冲，`printf` 函数总共会被调用 **6 次**。

  🔀 缓冲模式如何影响最终输出

`printf` 的输出会先放入一个内存缓冲区，缓冲区的刷新策略决定了它何时被真正写入终端或管道。

| 运行方式 | 缓冲模式 | 关键行为 | 导致的结果 |
| :--- | :--- | :--- | :--- |
| **`./demo`** | **行缓冲** | 输出到终端，且字符串**不含换行符 `\n`**，缓冲区**可能不会立即刷新**。子进程 `fork()` 时会**复制父进程的缓冲区**。但某些系统对终端行缓冲有优化，可能使部分输出在 `fork()` 前就刷新了，导致子进程复制的缓冲区内容更少。 | 最终只有 **6** 个 `hello world` 从缓冲区刷出并显示。 |
| **`./demo \| cat`** | **全缓冲** | 输出到管道，系统使用**全缓冲**。字符串会一直累积在缓冲区，直到程序结束或缓冲区满。所有 `fork()` 出的子进程都**完整复制**了父进程当时累积的缓冲区内容。 | 每个进程结束时，其独立的缓冲区都包含了累积的字符串，最终 **4 个进程共刷出 8 个** `hello world`。 |

  💡 如何验证与避免混乱

1.  **添加换行符**：在 `printf` 中加入 `\n`（如 `printf("hello world\n")`），行缓冲会立即刷新，两种方式都会输出 **6** 行。
2.  **手动刷新缓冲区**：在 `fork()` 前调用 `fflush(stdout)`，确保缓冲区被清空，子进程不会复制到旧数据。
3.  **禁用缓冲**：使用 `setbuf(stdout, NULL)` 关闭缓冲，每个 `printf` 都立即输出，但会**严重影响性能**，仅用于调试。

**总结**：在多进程编程中，缓冲机制是一个常见的“坑”。理解 `fork()` 对缓冲区的复制行为以及不同场景下的缓冲策略，是预测和掌控程序输出的关键。

- 为什么有些程序可以通过ctrl+c终止有些却不能比如vim
  ctrl+c产生中断信号，sigint,vim注册了该信号handle，但不是处理退出，而是用ctrl+q
  <img width="1922" height="990" alt="image" src="https://github.com/user-attachments/assets/50d07c62-c730-49d6-9f06-ed02fb1e54be" />


- 想重定向另外一个终端文件
```
echo xxx >  /dev/pts/3
```
- read stdin 与 fork，争抢机制标准输入
- 引入fork后，信号应该发给谁、标准输入应该给谁，这些都是可以考虑的
- 打开终端，产生session，产生进程组，执行的命令比如ls cat 之类的都是这个进程组的子进程运行；
  <img width="2062" height="1078" alt="image" src="https://github.com/user-attachments/assets/b655eacb-cd35-4e17-80a5-3cbe7bcb4d62" />
  
- <img width="2070" height="1082" alt="image" src="https://github.com/user-attachments/assets/64874e25-0759-476a-9887-20f325cd458e" />
- tmux使用
  ```
  tmux
  [exited]
  ```
- 希望你通过man查看更复杂的手册
  <img width="2060" height="960" alt="image" src="https://github.com/user-attachments/assets/fa39a3bd-3beb-4765-8855-00fb39ac8263" />
- shell 是用户和系统交互的接口，它可以启动前台进程组和后台进程组。当一个前台进程组在运行的时候，它会占用标准输入，这时候 shell 和其他后台进程组就无法直接获得标准输入。因为标准输入在某一时刻只能被一个进程组使用。不过，shell 本身也可以执行一些命令，而且它可以通过特殊的方式将标准输入重定向到其他地方，包括后台进程组，但这就需要一些额外的命令和配置了。
  <img width="1092" height="788" alt="image" src="https://github.com/user-attachments/assets/e26ac44e-a6a6-4279-ba8e-3790ee452f69" />
  <img width="1664" height="1016" alt="image" src="https://github.com/user-attachments/assets/3c4790a5-29d2-4451-8254-bb786b2b75f5" />
