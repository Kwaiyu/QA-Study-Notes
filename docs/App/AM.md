<h2 id="一概述">一、概述</h2>
<p>作为一名开发者，相信对adb指令一定不会陌生。那么在手机连接adb后，可通过am命令做很多操作：</p>
<h4 id="拨打电话">拨打电话</h4>
<p>通过adb，可以直接拨打电话10086</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>adb shell am start -a android.intent.action.CALL -d tel:10086
</code></pre></div></div>

<h4 id="打开网站">打开网站</h4>
<p>比如，打开网站www.gityuan.com</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>adb shell am start -a android.intent.action.VIEW -d  http://gityuan.com
</code></pre></div></div>

<h4 id="启动应用">启动应用</h4>
<p>比如，启动包名为com.gityuan.app，主Activity为<code class="language-plaintext highlighter-rouge">.MainActivity</code>，且extra数据以”website”为key, “gityuan.com”为value。通过java代码要完成该功能虽然不复杂，但至少需要一个android环境，而通过adb的方式，只需要在adb窗口，输入如下命令便可完成:</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>am start -n com.gityuan.app/.MainActivity -es website gityuan.com
</code></pre></div></div>

<p>am命令还可以启动Service、Broadcast，杀进程，监控等功能，这些功能都非常便捷调试程序，接下来讲述关于am更多更详细的功能。</p>
<h2 id="二am命令">二、Am命令</h2>
<h3 id="21-命令列表">2.1 命令列表</h3>
<p>命令格式如下：</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>am [subcommand] [options]
</code></pre></div></div>

<p>命令列表如下：</p>
<table>
  <thead>
    <tr>
      <th>命令</th>
      <th>功能</th>
      <th>实现方法</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>am start  <code class="language-plaintext highlighter-rouge">[options</code>]  <code class="language-plaintext highlighter-rouge"><INTENT</code>></td>
      <td>启动Activity</td>
      <td>startActivityAsUser</td>
    </tr>
    <tr>
      <td>am startservice  <code class="language-plaintext highlighter-rouge"><INTENT</code>></td>
      <td>启动Service</td>
      <td>startService</td>
    </tr>
    <tr>
      <td>am stopservice  <code class="language-plaintext highlighter-rouge"><INTENT</code>></td>
      <td>停止Service</td>
      <td>stopService</td>
    </tr>
    <tr>
      <td>am broadcast  <code class="language-plaintext highlighter-rouge"><INTENT</code>></td>
      <td>发送广播</td>
      <td>broadcastIntent</td>
    </tr>
    <tr>
      <td>am kill  <code class="language-plaintext highlighter-rouge"><PACKAGE</code>></td>
      <td>杀指定后台进程</td>
      <td>killBackgroundProcesses</td>
    </tr>
    <tr>
      <td>am kill-all</td>
      <td>杀所有后台进程</td>
      <td>killAllBackgroundProcesses</td>
    </tr>
    <tr>
      <td>am force-stop  <code class="language-plaintext highlighter-rouge"><PACKAGE</code>></td>
      <td>强杀进程</td>
      <td>forceStopPackage</td>
    </tr>
    <tr>
      <td>am hang</td>
      <td>系统卡住</td>
      <td>hang</td>
    </tr>
    <tr>
      <td>am restart</td>
      <td>重启</td>
      <td>restart</td>
    </tr>
    <tr>
      <td>am bug-report</td>
      <td>创建bugreport</td>
      <td>requestBugReport</td>
    </tr>
    <tr>
      <td>am dumpheap <code class="language-plaintext highlighter-rouge"><pid</code>> <code class="language-plaintext highlighter-rouge"><file</code>></td>
      <td>进程pid的堆信息输出到file</td>
      <td>dumpheap</td>
    </tr>
    <tr>
      <td>am send-trim-memory  <code class="language-plaintext highlighter-rouge"><pid</code>> <code class="language-plaintext highlighter-rouge"><level</code>></td>
      <td>收紧进程的内存</td>
      <td>setProcessMemoryTrimLevel</td>
    </tr>
    <tr>
      <td>am monitor</td>
      <td>监控</td>
      <td>MyActivityController.run</td>
    </tr>
  </tbody>
</table>

<p>am命令实的实现方式在Am.java，最终几乎都是调用<code class="language-plaintext highlighter-rouge">ActivityManagerService</code>相应的方法来完成。</p>
<ul>
  <li>比如命令<code class="language-plaintext highlighter-rouge">am start -a android.intent.action.VIEW -d  http://gityuan.com</code>， 启动Acitivty最终调用的是ActivityManagerService类的startActivityAsUser()方法来完成的。</li>
  <li>再比如 <code class="language-plaintext highlighter-rouge">am kill-all</code>命令，最终的实现工作是由ActivityManagerService的killBackgroundProcesses()方法完成的。</li>
</ul>

<p>接下来，说说<code class="language-plaintext highlighter-rouge">[options</code>]和 <code class="language-plaintext highlighter-rouge"><INTENT</code>>参数含义和使用。</p>
<h3 id="22-activity启动命令">2.2 Activity启动命令</h3>
<p>先来介绍一下启动Activity命令<code class="language-plaintext highlighter-rouge">am start [options] <INTENT></code>使用options参数，接下来列举Activity命令的[options]参数：</p>
<ul>
  <li>-D: 允许调试功能</li>
  <li>-W: 等待app启动完成</li>
  <li>-R <code class="language-plaintext highlighter-rouge"><COUNT</code>>: 重复启动Activity COUNT次</li>
  <li>-S: 启动activity之前，先调用forceStopPackage()方法强制停止app.</li>
  <li>–opengl-trace: 运行获取OpenGL函数的trace</li>
  <li>–user <code class="language-plaintext highlighter-rouge"><USER_ID</code>> <code class="language-plaintext highlighter-rouge">|</code> current: 指定用户来运行App,默认为当前用户。</li>
  <li>–start-profiler <code class="language-plaintext highlighter-rouge"><FILE</code>>: 启动profiler，并将结果发送到 <code class="language-plaintext highlighter-rouge"><FILE</code>>;</li>
  <li>-P <code class="language-plaintext highlighter-rouge"><FILE</code>>: 类似 –start-profiler，不同的是当app进入idle状态，则停止profiling</li>
  <li>–sampling INTERVAL: 设置profiler 取样时间间隔，单位ms;</li>
</ul>

code

code

code

code

code

code

code

code

code

code

code

code