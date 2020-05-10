**（一）jquery选择器**

**01、层次选择器**

如果想通过 DOM 元素之间的层次关系来获取特定元素, 例如后代元素, 子元素, 相邻元素, 兄弟元素等, 则需要使用层次选择器。

![img](G:\22、有道云笔记\weixinobU7VjrWOLcnLRz6p4X2INvFDlr8\6a671b1c19524611bda6129d561951ef\clipboard.png)

**注意**:  (“prev ~ div”) 选择器只能选择 “# prev ” 元素后面的同辈元素; 而 jQuery 中的方法 siblings() 与前后位置无关, 只要是同辈节点就可以选取

**02、基本过滤选择器**

![img](G:\22、有道云笔记\weixinobU7VjrWOLcnLRz6p4X2INvFDlr8\c27755705a354948902353ea8cf95532\clipboard.png)

**03、内容过滤选择器**

![img](G:\22、有道云笔记\weixinobU7VjrWOLcnLRz6p4X2INvFDlr8\5544406a8c444578af2fac0fe6a9e074\clipboard.png)

**04、可见性过滤选择器**

可见性过滤选择器是根据元素的可见和不可见状态来选择相应的元素

![img](G:\22、有道云笔记\weixinobU7VjrWOLcnLRz6p4X2INvFDlr8\779622bc91ae4123845a52da6e0180aa\clipboard.png)

可见选择器 :hidden 不仅包含样式属性 display 为 none 的元素, 也包含文本隐藏域 (<input  type=“hidden”>)和 visible:hidden 之类的元素。

**05、属性过滤选择器**

![img](G:\22、有道云笔记\weixinobU7VjrWOLcnLRz6p4X2INvFDlr8\c3d157436a2d4d6c8c4100447c03334f\clipboard.png)

**06、子元素过滤选择器**

![img](G:\22、有道云笔记\weixinobU7VjrWOLcnLRz6p4X2INvFDlr8\268578c7e1fd4eee920ae7c6687e7f98\clipboard.png)

nth-child() 选择器详解如下：

(1) :nth-child(even/odd): 能选取每个父元素下的索引值为偶(奇)数的元素

(2) :nth-child(2): 能选取每个父元素下的索引值为 2 的元素

(3) :nth-child(3n): 能选取每个父元素下的索引值是 3 的倍数 的元素

(4) :nth-child(3n + 1): 能选取每个父元素下的索引值是 3n + 1的元素

**07、表单对象属性过滤选择器**

![img](G:\22、有道云笔记\weixinobU7VjrWOLcnLRz6p4X2INvFDlr8\1126a7addea94dad84ab62c8a57c40b9\clipboard.png)

**08、表单选择器**

![img](G:\22、有道云笔记\weixinobU7VjrWOLcnLRz6p4X2INvFDlr8\9d8a2d197f0f4c319ade27f7ff765d62\clipboard.png)

**（二）节点**

**01、创建节点**

**创建节点:** 使用 jQuery 的工厂函数 $(): $(html); 会根据传入的 html 标记字符串创建一个 DOM 对象, 并把这个 DOM 对象包装成一个 jQuery 对象返回.

**注意:** 动态创建的新元素节点不会被自动添加到文档中, 而是需要使用其他方法将其插入到文档中; 

当创建单个元素时, 需注意闭合标签和使用标准的 XHTML 格式. 例如创建一个<p>元素, 可以使用 $(“<p/>”) 或 $(“<p></p>”), 但不能使用 $(“<p>”) 或 $(“<P>”)创建文本节点就是在创建元素节点时直接把文本内容写出来; 创建属性节点也是在创建元素节点时一起创建。

**02、插入节点**

动态创建 HTML 元素并没有实际用处, 还需要将新创建的节点插入到文档中, 即成为文档中某个节点的子节点

![img](G:\22、有道云笔记\weixinobU7VjrWOLcnLRz6p4X2INvFDlr8\55ff749f3859409dbce3069e49d55be9\clipboard.png)



![img](G:\22、有道云笔记\weixinobU7VjrWOLcnLRz6p4X2INvFDlr8\0c21715126fe45138686b7bc61c3b1af\clipboard.png)

**03、删除节点**

**remove()**: 从 DOM 中删除所有匹配的元素, 传入的参数用于根据 jQuery 表达式来筛选元素. 当某个节点用 remove() 方法删除后, 该节点所包含的所有后代节点将被同时删除. 这个方法的返回值是一个指向已被删除的节点的引用.

**empty():** 清空节点 – 清空元素中的所有后代节点(不包含属性节点).

**04、复制节点**

**clone()**: 克隆匹配的 DOM 元素, 返回值为克隆后的副本. 但此时复制的新节点不具有任何行为.

**clone(true)**: 复制元素的同时也复制元素中的的事件

**05、替换节点**

**replaceWith()**: 将所有匹配的元素都替换为指定的 HTML 或 DOM 元素

**replaceAll()**: 颠倒了的 replaceWith() 方法.

注意: 若在替换之前, 已经在元素上绑定了事件, 替换后原先绑定的事件会与原先的元素一起消失

**06、包裹节点**

**wrap():** 将指定节点用其他标记包裹起来. 该方法对于需要在文档中插入额外的结构化标记非常有用, 而且不会破坏原始文档的语义.

**wrapAll():** 将所有匹配的元素用一个元素来包裹. 而 wrap() 方法是将所有的元素进行单独包裹.

**wrapInner():** 将每一个匹配的元素的子内容(包括文本节点)用其他结构化标记包裹起来.

**（三）属性操作**

**attr():** 获取属性和设置属性

---当为该方法传递一个参数时, 即为某元素的获取指定属性

---当为该方法传递两个参数时, 即为某元素设置指定属性的值

jQuery 中有很多方法都是一个函数实现获取和设置. 如: attr(), html(), text(), val(), height(), width(), css() 等.

**removeAttr():** 删除指定元素的指定属性

**（四）事件**

01、**is()** 方法判断元素是否可见

02、**hover():** 模拟光标悬停事件. 当光标移动到元素上时, 会触发指定的第一个函数, 当光标移出这个元素时, 会触发指定的第二个函数.

03、**toggle()**: 用于模拟鼠标连续单击事件. 第一次单击元素, 触发指定的第一个函数, 当再一次单击同一个元素时, 则触发指定的第二个函数, 如果有更多个函数, 则依次触发, 直到最后一个.

04、**toggle()** 的另一个作用: 切换元素的可见状态.

05、移除某按钮上的所有  click 事件: $(“btn”).**unbind**(“click”)

06、移除某按钮上的所有事件: $(“btn”).**unbind**();

07、**one():** 该方法可以为元素绑定处理函数. 当处理函数触发一次后, 立即被删除. 即在每个对象上, 事件处理函数只会被执行一次.

08、**fadeIn()**, fadeOut(): 只改变元素的透明度. fadeOut() 会在指定的一段时间内降低元素的不透明度, 直到元素完全消失. fadeIn() 则相反.

09、**slideDown(),** slideUp(): 只会改变元素的高度. 如果一个元素的 display 属性为 none, 当调用 10、**slideDown()** 方法时, 这个元素将由上至下延伸显示. slideUp() 方法正好相反, 元素由下至上缩短隐藏. 

**toggle():** 切换元素的可见状态: 如果元素时可见的, 则切换为隐藏; 如果元素时隐藏的, 则切换为可见的. 

**slideToggle()**: 通过高度变化来切换匹配元素的可见性. 

**fadeTo():** 把不透明度以渐近的方式调整到指定的值  (0 – 1 之间). 

JQuery 可以通过 $.get() 或 $.post() 方法来加载 xml.