# UI编辑 - 动画脚本
<p>请确保你有一定的脚本基础，不需要特别了解，会使用游戏自带的api即可</p>
<p>在了解制作UI动画的方法后你也许会认为：使用触发器制作动画实在是太麻烦了</p>
<p>那么为什么不使用脚本制作动画呢？</p>
<p>提到脚本，你或许会认为根本学不懂，不会用。不过不用担心，在预设的脚本的基础上，使用脚本制作动画将会变得轻而易举！</p>
<p>在定义了动画函数之后，你只需要在脚本中输入几行极其简短的代码即可实现一些基本的动画效果</p>
<p>话不多说，进入正题（含API）</p>

## 基础动画
<p>这里先普及一个函数：Trigger:wait(value)</p>
<p>该函数的具体作用就是等待时间，value填需要等待的时间，以秒为单位，但是精度只能达到0.05s</p>
<p>等待时间的原理：通过卡住线程实现暂停时间，即必须等待该函数运行完成后才能继续运行脚本（这里后面要考）</p>
<p>注意：该函数只能通过监听器调用才能实现功能，否则无法实现等待时间的效果</p>
<p>这是一个例子： （为了看起来更加简洁所以去除了注释）</p>
<pre class="hljs"><code class="hljs">function linear_animation(player,uiid,elementid,from_x,from_y,to_x,to_y,time)
    local step_x = (to_x-from_x)/(time/0.05)
    local step_y = (to_y-from_y)/(time/0.05)
    local x = from_x
    local y = from_y
    for i = 1,time/0.05 do
        x = x+step_x
        y = y+step_y
        Customui:setPosition(playerid,uiid,elementid,x,y)
        Trigger:wait(0.05)
    end
end
--调用示例
function play(event)
    linear_animation(event.eventobjid,"7082053565458168217","7082053565458168217_3",200,200,600,200,3)
end
ScriptSupportEvent:registerEvent("UI.Button.Click",play)</code></pre>
<p>脚本效果：将元件由(200,200)平移至(600,200)</p>
<p>脚本看似复杂实则非常简单</p>
<p>脚本分为2个部分，定义函数部分和调用函数部分</p>
<p>函数部分即为初始化一个函数，让你能够很方便地调用，相当于脚本API接口里的API，该部分不需要特别了解，如果感兴趣的话可以研究研究</p>
<p>调用函数就相当于直接使用API，例如 Player:addItem(158087577,101,64) 即为玩家158087577添加64个土块</p>
<p><strong>接下来是脚本分析：</strong></p>
<p>定义一个名为&rdquo;linear_animation&rdquo;的函数,相当于整个脚本的核心，方便调用平移动画函数</p>
<pre class="language-lua"><code>function linear_animation(...)
    ...
end</code></pre>
<p>定义一个名为&rdquo;play&rdquo;的函数，函数内容为：将触发事件玩家的XX界面的XX元件的位置由(200,200)平移到(600,200)</p>
<pre class="language-lua"><code>function play(event)
    linear_animation(...)
end</code></pre>
<p>注册监听器，监听玩家点击按钮，当玩家点击按钮时执行play函数</p>
<p>效果如下图所示：</p>
<p><img style="max-width: 100%;" src="https://kfz-static.mini1.cn/college/dev_college_oa/1649331666819" alt="平移动画" width="700" /></p>
<p>看不懂？没关系！会用就行了！（多看几个例子说不定就能掌握）</p>
<p>以下内容为4种基础动画脚本</p>

### 平移动画
<pre class="language-lua"><code>--[[
定义一个名为"linear_animation"的函数用于调用平移动画
playerid：播放动画的目标玩家
uiid：界面id
elementid：元件id
from_x：起点X坐标
from_y：起点Y坐标
to_x：终点X坐标
to_y：终点Y坐标
time：动画播放时间，以秒为单位，精度为0.05s
]]
function linear_animation(player,uiid,elementid,from_x,from_y,to_x,to_y,time) --平移动画
    local step_x = (to_x-from_x)/(time/0.05) --设置步长，即单位时间内移动的距离
    local step_y = (to_y-from_y)/(time/0.05)
    local x = from_x --设置初始坐标
    local y = from_y
    for i = 1,time/0.05 do --播放每一帧动画
        x = x+step_x --设置下一帧坐标
        y = y+step_y
        Customui:setPosition(playerid,uiid,elementid,x,y) --修改元件坐标
        Trigger:wait(0.05) --等待0.05s后继续运行脚本
    end
end
--调用示例
function play(event)
    --调用"linear_animation"平移动画函数，由(200,200)移动到(800,200),播放2s
    linear_animation(event.eventobjid,"7082053565458168217","7082053565458168217_3",200,200,800,200,2)
end
ScriptSupportEvent:registerEvent("UI.Button.Click",play) --注册一个监听器，当按钮被点击时运行play函数</code></pre>
<p><img style="max-width: 100%;" src="https://kfz-static.mini1.cn/college/dev_college_oa/1649331666819" alt="平移动画" width="700" /></p>

### 旋转动画
<pre class="language-lua"><code>--[[
定义一个名为"rotate_animation"的函数用于调用旋转动画
playerid：播放动画的目标玩家
uiid：界面id
elementid：元件id
from_angle：起点角度坐标
to_angle：终点角度坐标
time：动画播放时间，以秒为单位，精度为0.05s
]]
function rotate_animation(playerid,uiid,elementid,from_angle,to_angle,time) --旋转动画
    local step_angle = (to_angle-from_angle)/(time/0.05) --设置步长，即单位时间内旋转的角度
    local angle = from_angle --设置初始角度
    for i = 1,time/0.05 do --播放每一帧动画
        angle = angle+step_angle --设置下一帧角度
        Customui:rotateElement(playerid,uiid,elementid,angle) --修改元件旋转角度
        Trigger:wait(0.05) --等待0.05s后继续运行脚本
    end
end
--调用示例
function play(event)
    --调用"rotate_animation"旋转动画函数，由0&deg;旋转到360&deg;，播放1s
    rotate_animation(event.eventobjid,"7082053565458168217","7082053565458168217_3",0,360,1)
end
ScriptSupportEvent:registerEvent("UI.Button.Click",play) --注册一个监听器，当按钮被点击时运行play函数</code></pre>
<p><img style="max-width: 100%;" src="https://kfz-static.mini1.cn/college/dev_college_oa/1649331666831" alt="旋转动画" width="700" /></p>

### 缩放动画
<pre class="language-lua"><code>--[[
定义一个名为"size_animation"的函数用于调用缩放动画
playerid：播放动画的目标玩家
uiid：界面id
elementid：元件id
from_width：起始宽度
from_height：起始高度
to_width：结束宽度
to_height：结束高度
time：动画播放时间，以秒为单位，精度为0.05s
]]
function size_animation(player,uiid,elementid,from_width,from_height,to_width,to_height,time) --缩放动画
    local step_width = (to_width-from_width)/(time/0.05) --设置步长，即单位时间内缩放的宽高
    local step_height = (to_height-from_height)/(time/0.05)
    local width = from_width --设置初始宽高
    local height = from_height
    for i = 1,time/0.05 do --播放每一帧动画
        width = width+step_width --设置下一帧宽高
        height = height+step_height
        Customui:setSize(playerid,uiid,elementid,width,height) --修改元件宽高
        Trigger:wait(0.05) --等待0.05s后继续运行脚本
    end
end
--调用示例
function play(event)
    --调用2次"size_animation"缩放动画函数，第一次放大，第二次缩小，分别播放1s
    size_animation(event.eventobjid,"7082053565458168217","7082053565458168217_3",200,200,300,300,1)
    size_animation(event.eventobjid,"7082053565458168217","7082053565458168217_3",300,300,200,200,1)
end
ScriptSupportEvent:registerEvent("UI.Button.Click",play) --注册一个监听器，当按钮被点击时运行play函数</code></pre>
<p><img style="max-width: 100%;" src="https://kfz-static.mini1.cn/college/dev_college_oa/1649331666821" alt="缩放动画" width="700" /></p>

### 透明动画
<pre class="language-lua"><code>--[[
定义一个名为"alpha_animation"的函数用于调用透明动画
playerid：播放动画的目标玩家
uiid：界面id
elementid：元件id
from_alpha：起始透明度
to_alpha：结束透明度
time：动画播放时间，以秒为单位，精度为0.05s
]]
function alpha_animation(player,uiid,elementid,from_alpha,to_alpha,time) --透明动画
    local step_alpha = (to_alpha-from_alpha)/(time/0.05) --设置步长，即单位时间内透明度改变量
    local alpha = from_alpha --设置初始透明度
    for i = 1,time/0.05 do --播放每一帧动画
        alpha = alpha+step_alpha --设置下一帧动画
        Customui:setAlpha(playerid,uiid,elementid,alpha) --修改元件透明度
        Trigger:wait(0.05) --等待0.05s后继续运行脚本
    end
end
--调用示例
function play(event)
    --调用2次"alpha_animation"透明动画函数，第一次为逐渐消失，第二次为逐渐出现，分别播放1s
    alpha_animation(event.eventobjid,"7082053565458168217","7082053565458168217_3",100,0,1)
    alpha_animation(event.eventobjid,"7082053565458168217","7082053565458168217_3",0,100,1)
end
ScriptSupportEvent:registerEvent("UI.Button.Click",play) --注册一个监听器，当按钮被点击时运行play函数</code></pre>
<p><img style="max-width: 100%;" src="https://kfz-static.mini1.cn/college/dev_college_oa/1649331666824" alt="透明动画" width="700" /></p>

## 组合动画
<p>ps：在组合动画讲解中将会省略定义动画函数以及注册监听器</p>
<p>只是简单的平移旋转的动画可能并不是特别美观，这时就需要用到组合动画</p>
<p>首先是一个很简单的例子，实现一个简单的组合动画</p>
<pre class="language-lua"><code>function play(event)
  local playerid = 158087577 --设置玩家
  local uiid = "7082053565458168217" --设置界面
  local elementid = "7082053565458168217_3" --设置元件
  linear_animation(playerid,uiid,elementid,300,200,800,200,1) --平移动画
  rotate_animation(playerid,uiid,elementid,0,360,1) --旋转动画
  size_animation(playerid,uiid,elementid,200,200,300,300,1) --缩放动画
  alpha_animation(playerid,uiid,elementid,100,0,1) --透明动画
end</code></pre>
<p>大致效果是：先平移，再旋转，再放大，最后淡出</p>
<p>效果如下图所示：</p>
<p><img style="max-width: 100%;" src="https://s1.ax1x.com/2022/04/16/LJfxpR.gif" alt="简单的组合动画" width="700" /></p>
<p>同一时间只有单个动画的组合动画并不能算真正意义上的组合动画，那么要怎么实现在同一时间播放多个动画呢？</p>
<p>这时就需要用到多线程函数threadpool:work()</p>
<p>在使用这个函数之前，首先你需要知道什么是线程以及多线程</p>
<p>举个例子，当你排队做核酸的时候，一条队伍就相当于一个线程，必须要等待前面的人做完核酸，后面的人才能坐，脚本也是如此</p>
<p>前面说到等待时间函数会卡住线程，导致后面的动画无法播放，要解决这个问题，就需要用到多线程，那么什么是多线程呢？</p>
<p>举个例子，排队做核酸的队伍有时并不是一条，而是多条，这样就可以实现同一时间多人一起做核酸，脚本也是如此</p>
<p>这里简单讲解一下多线程函数的用法</p>
<p>threadpool:work(没有参数的函数)</p>
<p>例子：</p>
<pre class="language-lua"><code>--方法一
  function play(event)
      local playerid = 158087577 --设置玩家
      local uiid = "7082053565458168217" --设置界面
      local elementid = "7082053565458168217_3" --设置元件
      threadpool:work(function()linear_animation(playerid,uiid,elementid,300,200,800,200,1)end) --为平移动画创建一个新的线程
      alpha_animation(playerid,uiid,elementid,0,100,1)
  end
  
  --方法二
  function play(event)
      local playerid = 158087577 --设置玩家
      local uiid = "7082053565458168217" --设置界面
      local elementid = "7082053565458168217_3" --设置元件
      function func1()
          linear_animation(playerid,uiid,elementid,300,200,800,200,1) --平移动画
      end
      threadpool:work(func1) --为func1函数创建一个新的线程
      alpha_animation(playerid,uiid,elementid,0,100,1)
  end</code></pre>
<p>大致效果是：在平移的同时淡入</p>
<p>当然，你也可以选择直接将threadpool:work()内嵌到定义函数中</p>
<p>效果如下图所示：</p>
<p><img style="max-width: 100%;" src="https://s1.ax1x.com/2022/04/16/LJhpX6.gif" alt="平移淡入动画" width="700" /></p>
<p>那么结合以上内容，通过简单的叠加几个函数，即可实现以下动画：</p>
<pre class="language-lua"><code>function play(event)
  local playerid = 158087577 --设置玩家
  local uiid = "7082053565458168217" --设置界面
  local elementid = "7082053565458168217_3" --设置元件
  local function func1() --旋转动画
    rotate_animation(playerid,uiid,elementid,0,360,2) --2s内顺时针旋转360°
    rotate_animation(playerid,uiid,elementid,360,0,4) --4s内逆时针旋转360°
  end
  threadpool:work(func1)
  threadpool:work(function()alpha_animation(playerid,uiid,elementid,0,100,1)end) --淡入
  threadpool:work(function()size_animation(playerid,uiid,elementid,100,100,200,200,6)end) --6s内逐渐放大
  linear_animation(playerid,uiid,elementid,200,100,1000,200,2) --平移
  linear_animation(playerid,uiid,elementid,1000,200,200,600,2)
  threadpool:work( --淡出
      function()
          Trigger:wait(1) --等待1s
          alpha_animation(playerid,uiid,elementid,100,0,1) --淡出
      end)
  linear_animation(playerid,uiid,elementid,200,600,800,600,2) --平移
end</code></pre>
<p>效果如下图所示：</p>

<p><img style="max-width: 100%;" src="https://s1.ax1x.com/2022/04/16/LJhkAe.gif" alt="复杂组合动画" width="700" /></p>

### 原地缩放动画
<pre class="language-lua"><code>function play(event)
    threadpool:work(function()size_animation(event.eventobjid,"7082053565458168217","7082053565458168217_3",200,200,300,300,1)end)
    linear_animation(event.eventobjid,"7082053565458168217","7082053565458168217_3",300,200,250,150,1)
    threadpool:work(function()size_animation(event.eventobjid,"7082053565458168217","7082053565458168217_3",300,300,200,200,1)end)
    linear_animation(event.eventobjid,"7082053565458168217","7082053565458168217_3",250,150,300,200,1)
end</code></pre>
<p><img style="max-width: 100%;" src="https://s1.ax1x.com/2022/04/16/LJhEhd.gif" alt="原地缩放动画" width="700" /></p>

## 高级动画
<p>高级动画需要用到比较复杂的计算过程，例如减速平移、翻转、函数图像轨迹等等（感兴趣可以仔细研究函数内容，如果只是单纯想用高级动画，直接看调用示例即可）</p>

### 减速平移动画
<pre class="language-lua"><code>--[[
定义一个名为"decelerate_animation"的函数用于调用减速平移动画
playerid：播放动画的目标玩家
uiid：界面id
elementid：元件id
from_x：起点X坐标
from_y：起点Y坐标
to_x：终点X坐标
to_y：终点Y坐标
time：动画播放时间，以秒为单位，精度为0.05s
]]
function decelerate_animation(player,uiid,elementid,from_x,from_y,to_x,to_y,time) --减速平移动画
    local longth_x = to_x-from_x --设置位移
    local longth_y = to_y-from_y
    local a_x = 2*longth_x/(time^2) --设置加速度
    local a_y = 2*longth_y/(time^2)
    local x = {} --初始化坐标表
    local y = {}
    for i = 1,time/0.05 do --将每一帧坐标储存在坐标表内
        x[i]=to_x-0.5*a_x*(0.05*i)^2
        y[i]=to_y-0.5*a_y*(0.05*i)^2
    end
    for i = 1,time/0.05 do --播放每一帧动画
        Customui:setPosition(playerid,uiid,elementid,x[time/0.05-i],y[time/0.05-i]) --修改元件坐标
        Trigger:wait(0.05)  --等待0.05s后继续运行脚本
    end
end
--调用示例
function play(event)
    --调用"decelerate_animation"减速平移动画函数，由(200,100)减速移动到(800,400),播放2s
    decelerate_animation(event.eventobjid,"7082053565458168217","7082053565458168217_3",200,100,800,400,2)
end
ScriptSupportEvent:registerEvent("UI.Button.Click",play) --注册一个监听器，当按钮被点击时运行play函数</code></pre>
<p><img style="max-width: 100%;" src="https://s1.ax1x.com/2022/04/16/LJhAtH.gif" alt="减速平移动画" width="700" /></p>
<p>ps：更多内容请等待下次更新！</p>
