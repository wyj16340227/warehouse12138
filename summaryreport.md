# <center>**_Summary Report_**</center>
## **_Stage1_**
> 第一阶段感觉是为了让我们快速适应`java`的编程环境。其实在上个学期的时候，我选修了`java`课程，但是由于我们在`windows`下使用了`eclipse`，集成度很高，所有的东西都不需要我们做，不用考虑环境变量，引入文件，依赖文件，`Ant`等等，且由于学习的比较简单，也没有使用到`JUnit` `Sonar`这些工具。<br>
> 经过第一阶段的学习，收获最多的应该是`Sonar`，使用了`ant`，很像`build`，不光是功能像，”以后打死也不会再用了“的感觉也很像。而`JUnit`，目前学习到的水平，也通过简单的`Debug`实现，而且说实话，一般不涉及到需要反复测试很多数据来`Debug`的话，我还是很乐意一次次输入的。
> 反观`Sonar`，可以说是又爱又恨了...
> 在某些方面，`Sonar`很好用，可以帮助我查看代码中的潜藏bug，（可能编程能力比较强，所以还没有遇到过`Critical` `Blocker`这样的障碍），尤其是一些在测试的数据中显示不出来，肉眼又看不到的深层bug。在不能透视内存的情况下，可以说是一个神器了。
> 但是在另一方面，`Sonar`可以让人抓狂，比如说魔术数，比如不让我用`ArrayList<>`，既然不让我用`ArrayList<>`，你为啥不把这种结构从`java`里面拆掉呢？[smile] [smile] [smile]一般因为`Sonar`所造成的问题都需要很久去解决。可以说`Sonar`是真的让人又爱又恨了。
> 但是比较令人振奋的是过了实训，`Sonar`改不改就是我的事情了。

---------
## **_Stage2_**
> 这个阶段应该是编程能力以及码字能力提升最强的阶段了。问答题很痛苦，暂且不论，让我的白头发来说，单纯的谈一下收获。
> 最先提升的是代码阅读能力。
> 幸运的是这一阶段的代码可读性很强，比较难懂的是各个类的继承，以及整个`GridWorld`的架构，个人感觉最难读懂的应该是`GUI`交互以及和整个地图的接口。这一部分也阅读了一些，很多难以理解的代码，调用了很多库，很强，叹为观止，何时我才能像大佬一样优秀。
> 其次，提升最大的是编程能力，实话说，这个阶段的很多函数的实现都很简单，每一个类都只是在原有的父类基础上稍作修改，难度比较大的应该是地图的部分。
> 地图的部分实现难度主要体现在逻辑上，即使从编程难度来讲不难，最主要的就是搞清楚每一个函数的接口要实现的是什么功能，才能编写正确的函数。
> 感觉第二阶段最大的难度就是题太多了（不只是问答题），颇有稻草压死骆驼的意味，这个阶段也是脱发最严重的阶段，迫不得已，理了个平头，这样就看不出来了。蛮机智的。
> 跪拜大佬。【大佬】orz【我】

-----
## **_Stage3_**
> 这个阶段怎么说呢，感觉是来送福利的？
> 第一题的图像处理，乍一看很复杂，实际上运用了很多前人留下的函数和类，尤其是在文件存储（API）以及提取色彩通道（`RGBFliter`）的部分。就使得实现起来很简单。最主要的还是对于bmp的文件结构了解加深了一些，还有就是位操作符，感觉从微观的角度对于存储有了一些了解。（为了了解左移右移以及位与，查阅了很多帖子）
> `JUnit`遇到了不少问题，最主要的还是写比较函数，最后还是通过老方法，拿到`BufferImage`，提取出色彩数组，再一个一个的进行比较。开始的时候其实设置了一些误差限制，让平均色彩差值为1（每个点），但是后来发现其实可以做到零误差，于是就取消了误差。
> 第二题的`MazaBug`最大的难度还是理解为什么要使用`Stack<ArrayList>`，按照我自己的理解，使用`Stack<Location>`更加的简单，高效，并且逻辑也相对简单，不容易出现bug，经过了很长时间的理解才发现了一些好处（可能没有发现到点子上，大佬不要介意），第一点，可以保存每一个点的路径，即`Stack`中每一个元素的第一个元素；第二点，可以快速的返回。
> `MazeBug`的预测函数也有点意思，大部分情况下，确实能够起到一些作用，当然，我在原先的基础上还增加了初始时候的预判断，先根据终点的位置对初始的方向的权值进行了第一次加减。
> `MazeBug`还有一个取巧的地方就是不用判断前进方向的路程是否走过，因为走过的地方会留下一朵花，只要在得到可前进位置的时候将其剔除即可。
> 第三题可以说是完全送福利了...
> 第一问的`BFS`算法真的是来搞笑的吗，为什么还给了`A*`算法的实现？
> 第二问就很开放了，在写第二问的时候遇到了很大的问题，就是到底使用哪些参数以及比例。
> 看了群里的信息，最后用了曼哈顿距离、欧拉距离（说白了就是直线距离），以及错误节点数（直接使用了原先实现），几个距离很好算，但是比例就...
> 经过我的（5）次实验，最后找出的比例是1:2:1。这个比例可以说是很强了，跑出来的次数也基本在2000~12000之间。

## **_Summary_**
> 说实话，想不到实训这么快就过去了，在我的印象中（初级实训留下的阴影印象），实训是一件很痛苦，很绝望的事情（TA大佬请将这句话视为：陶冶情操，享受生活的事情）。但是这次的实训有点简单，差点让我误以为自己的编程能力提高了很多。可能主要还是学习了eclipse以及java环境的配置的一些东西，且相对来说题目开放一些，而且大佬们已经留下了很多很强的库，直接调用即可。要是让我自己打一个`GridWorld`，估计也会猝死的吧。
> 对于`java`，只想说，确实好用，但是感觉之前在课程上学习到的多线程、网络通讯等等都没用到，毕竟这个才是精髓（噩梦）。
