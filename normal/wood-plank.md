# \[循环\] 原木合成木板

在部分魔改整合包中，原木是不能直接分解成四个木板，就比如在CF比较知名E2E整合包中，所有的原木都只能分解成两个对应的木板。

或许看到这你可能会有些疑问，这个问题的解决方案不是很简单吗？

修改配方，先删除原版的配方再添加自己的配方不就可以了吗？

```typescript
//删除橡木木板配方并添加自己的
recipes.removeByRecipeName("minecraft:oak_planks");
recipes.addShapeless(<minecraft:planks> * 2, <minecraft:log>);

//删除白桦木板配方并添加自己的
recipes.removeByRecipeName("minecraft:birch_planks");
recipes.addShapeless(<minecraft:planks:2> * 2, <minecraft:log:2>);

......
```

诚然，这种方式可行，考虑到原版的木头类型也只有六个，也就只用写六遍。

但实际开发的环境上，我们制作魔改整合包中会有各种各样的模组，部分模组也会带各种的树，就比如暮色，比如集成动力……等等模组带的各种树，自然只多了一两种我们自然可以像上例那般去写，如果多了十种，百种？或者在我们好不容易写完全部树后，你突然想加入另外一个模组了，而那个模组又带了几种原木……

这时我们可能得寻求另一种思路了：

在CrT文档的 [**Crafting Table Recipes**](https://docs.blamejared.com/1.12/zh/Vanilla/Recipes/Crafting/Recipes_Crafting_Table/#other-functionality) ****一节的下面会有个不起眼的地方标注着这样一行代码：

> recipes.all;

它返回的是一个 [**ICraftingRecipe**](https://docs.blamejared.com/1.12/zh/Vanilla/Recipes/Crafting/ICraftingRecipe/) ****的数组，包含了整合包的所有工作台合成，得知了这个，我们便可以更方便的修改这些配方： 

```typescript
//循环成单个 ICraftingRecipe 方便处理
for recipe in recipes.all{
    //原木的配方是一个无序配方
    var rec1D as IIngredient[] = recipe.ingredients1D;
    //无序配方的情况下，输入只有单项，输出符合板的矿辞从而可以大致得到需要的配方
    if(rec1D.length == 1 && <ore:plankWood>.matches(recipe.output)){
        //此循环提取出配方中的IIngredient项，因为IIngredient不是IItemStack
        //所以只得再进行个循环提取出结果再用matches判断
        for i in rec1D[0].items{
            //判断输入物是否属于原木矿辞
            if(<ore:logWood>.matches(i)){
                //修改配方只能先删后增
                recipes.removeByRecipeName(recipe.fullResourceDomain);
                recipes.addShapeless(recipe.output * 2, rec1D);
            }
        }
    }
}
```

一个简单的循环后，我们就已经成功的将四个木板修改到了两个木板了，一劳永逸，哪怕是加入再多的模组，只要它的木头拥有矿辞（~~_没有矿辞的都是异类_~~_）_，也完全不需理会。



补充另外一种修改方式（~~这tm居然在文档没有写~~）

```typescript
for item in <ore:plankWood>.items{
    //从输出找配方，然后再处理
    for recipe in recipes.getRecipesFor(item){
        var rec1D as IIngredient[] = recipe.ingredients1D;
        if(rec1D.length == 1){
            for i in rec1D[0].items{
                if(<ore:logWood>.matches(i)){
                    recipes.removeByRecipeName(recipe.fullResourceDomain);
                    recipes.addShapeless(recipe.output * 2, rec1D);
                }
            }
        }
    }
}
```



事实上，友谊妈妈在HDS还写了另外一种方式合成（

{% embed url="https://github.com/ProjectHDS/Herodotus/blob/master/.minecraft/scripts/hds\_main/utils/wood.zs" %}

