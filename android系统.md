- 前期历史：诺基亚时代可能几个程序员就可以做出来简单的操作系统，但是安卓时代不行了，代码量庞大；
  <img width="2076" height="1596" alt="image" src="https://github.com/user-attachments/assets/5d71cb9a-40af-41ce-8ab9-adb1929e7f96" />
  <img width="2256" height="1480" alt="image" src="https://github.com/user-attachments/assets/ce9d86be-8426-41fd-9dc4-fcd29ed5c874" />
- 诺基亚为什么死掉，因为编程模型是c++；安卓为什么火，因为编程模型是java，开发者要求相对低；安卓本身是个linux；
- 安卓做了crash consistency很多事情，才让更多的开发者可以方便开发app；
  <img width="2278" height="1666" alt="image" src="https://github.com/user-attachments/assets/9a392cf3-4f5c-4c95-b8e6-5deedbee393d" />
  <img width="1384" height="2038" alt="image" src="https://github.com/user-attachments/assets/1f35d66b-a439-417e-a761-c1753e81c517" />
- 安卓里app使用同一个uid启动进程线程，所以你要杀死一个app时，需要遍历来杀死同一个uid的进程或者线程；所以有可能app检测到进程被杀死了，由另外一个进程重新拉起；构建死锁，如果杀死其中一个则唤醒另外一个；流氓app就是这么来的；
- 文件锁，两个独立进程通过flock来同步或者构建死锁
  <img width="668" height="336" alt="image" src="https://github.com/user-attachments/assets/ecfae7ee-1993-44c3-8df0-8546327a6c5b" />

- 华为将gms从他的系统里面剥离出来已经很难了，少说多做，想在这么多代码里找到自己的位置，就是去动手写；
  <img width="1848" height="1444" alt="image" src="https://github.com/user-attachments/assets/c3e9f72f-439c-4102-9751-cf1703e35507" />

### 附件
- https://jyywiki.cn/OS/2022/slides/31.slides.html#/4
