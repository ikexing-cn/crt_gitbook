# \[循环\] 神秘花瓣合成

这次依旧拿E2E整合包举例，在它的合成表中有一个有趣的合成，即一个任意的神秘花瓣搭配四个骨粉可以接得到四个对应的神秘花瓣，让我们看看作者是怎么实现这个合成的吧：

```typescript
// E2E ZS指路 -> scripts/Botania.zs
	recipes.addShapeless("Petal Duplication0", <botania:petal> * 4, [<botania:petal>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication1", <botania:petal:1> * 4, [<botania:petal:1>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication2", <botania:petal:2> * 4, [<botania:petal:2>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication3", <botania:petal:3> * 4, [<botania:petal:3>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication4", <botania:petal:4> * 4, [<botania:petal:4>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication5", <botania:petal:5> * 4, [<botania:petal:5>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication6", <botania:petal:6> * 4, [<botania:petal:6>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication7", <botania:petal:7> * 4, [<botania:petal:7>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication8", <botania:petal:8> * 4, [<botania:petal:8>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication9", <botania:petal:9> * 4, [<botania:petal:9>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication10", <botania:petal:10> * 4, [<botania:petal:10>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication11", <botania:petal:11> * 4, [<botania:petal:11>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication12", <botania:petal:12> * 4, [<botania:petal:12>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication13", <botania:petal:13> * 4, [<botania:petal:13>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication14", <botania:petal:14> * 4, [<botania:petal:14>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
	recipes.addShapeless("Petal Duplication15", <botania:petal:15> * 4, [<botania:petal:15>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
```

挺硬核的……

但围绕源代码寻找规律发现，其实整行中的变动只有 **&lt;botania:petal:?&gt;** 后面的meta值，而且是一个固定的规律，即0-15，得知了这个我们便可以用更方便的方式处理：

```typescript
//循环十六次，下标从0开始
for i in 0 to 16{
    var mysticalPetal as IItemStack = <botania:petal>.definition.makeStack(i);
    recipes.addShapeless("Petal Duplication" + i, mysticalPetal * 4, [mysticalFlower, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
}
```

这样我们仅仅用了四行代码便写出了这个合成表，省去了时间在复制粘贴 __~~_\(bushi_~~

另，利用 [ItemUtils](https://docs.blamejared.com/1.12/zh/Vanilla/Utils/IItemUtils/) 我们还可以直接以字符串得到神秘花瓣：

```typescript
//循环十六次，下标从0开始
for i in 0 to 16{
    var mysticalPetal as IItemStack = itemUtils.getItem("botania:petal", i);
    recipes.addShapeless("Petal Duplication" + i, mysticalPetal * 4, [mysticalFlower, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
}
```



但其实，我们甚至都不需要得知它的meta值顺序。

[IItemDefinition](https://docs.blamejared.com/1.12/zh/Vanilla/Items/IItemDefinition) 中提供了名为subItems的Getter，它的结果是一个 [IItemStack](https://docs.blamejared.com/1.12/zh/Vanilla/Items/IItemStack/) 数组，里面包含着的是原 [IItemStack](https://docs.blamejared.com/1.12/zh/Vanilla/Items/IItemStack/) 的全部子类，因此我们可以使用foreach的方式来添加合成：

```typescript
for item in <botania:petal>.definition.subItems{
    recipes.addShapeless(item * 4, [item, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
}

//同理，ItemUtils也是可行
for item in itemUtils.getItem("botania:petal").definition.subItems{
    recipes.addShapeless(item * 4, [item, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>, <ore:fertilizer>]);
}
```

