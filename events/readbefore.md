# 阅前必看

当看这一大章之前，我希望你起码对事件具体的作用先有个大概的了解，虽不必你背下全部事件的用途，但先去把全部事件的作用简单的过一遍再来阅读显然更加合理。

指路 - &gt; [https://docs.blamejared.com/1.12/zh/Vanilla/Events/Events/](https://docs.blamejared.com/1.12/zh/Vanilla/Events/Events/)



写事件一定要安装 [**ZenUtils**](https://www.curseforge.com/minecraft/mc-mods/zenutil) ****！！！

写事件一定要安装 [**ZenUtils**](https://www.curseforge.com/minecraft/mc-mods/zenutil) ****！！！

写事件一定要安装 [**ZenUtils**](https://www.curseforge.com/minecraft/mc-mods/zenutil) ****！！！

它 **1.7.1+** 版本提供了 **/ct reloadevents** 指令，允许你在游戏内热重载去调试你的环境，不用重启客户端了！！！



因为事件涉嫌到较多的逻辑判断，所以整个事件的案例我会分解成四个部分来供给解析：

1. 案例分析：简单的描述出这个想实现一个怎么样的效果。
2. 流程解析：理清思路，通过流程图去分析事件应该作怎么样的判断。在写任何事件时，我觉得这是一个很好的方法，虽然简单的事件你大可通过边改边写的方式慢慢作判断，不会遇到问题，而当到事件复杂了起来，会陪伴你的将会是无穷无尽的重载去调整，而这部分的调整在有了一个清晰的流程后大可避免。
3. 代码实现：用代码的话如何实现。（不会讲解代码具体是作何用途，请务必配合 [WIKI](https://docs.blamejared.com/1.12/en/index/) 阅读）
4. 补充细节：补充流程解析中没有预先想好的问题。

总而言之，我希望是能够将我写事件的思路解析出来，让你会明白我写事件的思路和想法是怎么样的，而不是希望将这些案例当做是一个供给抄改的模板，当换了个需求，没有了案例的时，你又陷入了不知从何下手的苦恼。

