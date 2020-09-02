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

<p>启动Activity的实现原理： 存在-W参数则调用startActivityAndWait()方法来运行，否则startActivityAsUser()。</p>
<h3 id="23-trim-memory命令">2.3 trim-memory命令</h3>
<p>收紧内存命令</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>am send-trim-memory  <pid> <level>
</code></pre></div></div>

<p>例如： 向pid=12345的进程，发出level=RUNNING_LOW的收紧内存命令</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>am send-trim-memory 12345 RUNNING_LOW。
</code></pre></div></div>

<p>那么level取值范围为： HIDDEN、RUNNING_MODERATE、BACKGROUND、RUNNING_LOW、MODERATE、RUNNING_CRITICAL、COMPLETE。</p>
<h3 id="24-intent参数">2.4 Intent参数</h3>
<p>Intent的参数和flags较多，本文为方便起见，分为3种类型参数，常用参数，Extra参数，Flags参数。</p>
<h4 id="241-常用参数">2.4.1 常用参数</h4>
<ul>
  <li><code class="language-plaintext highlighter-rouge">-a <ACTION></code>: 指定Intent action， 实现原理Intent.setAction()；</li>
  <li><code class="language-plaintext highlighter-rouge">-n <COMPONENT></code>: 指定组件名，格式为{包名}/.{主Activity名}，实现原理Intent.setComponent(）；</li>
  <li><code class="language-plaintext highlighter-rouge">-d <DATA_URI></code>: 指定Intent data URI</li>
  <li><code class="language-plaintext highlighter-rouge">-t <MIME_TYPE></code>: 指定Intent MIME Type</li>
  <li><code class="language-plaintext highlighter-rouge">-c <CATEGORY> [-c <CATEGORY>] ...]</code>:指定Intent category，实现原理Intent.addCategory()</li>
  <li><code class="language-plaintext highlighter-rouge">-p <PACKAGE></code>: 指定包名，实现原理Intent.setPackage();</li>
  <li><code class="language-plaintext highlighter-rouge">-f <FLAGS></code>: 添加flags，实现原理Intent.setFlags(int )，紧接着的参数必须是int型；</li>
</ul>

<p>实例</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>am start -a android.intent.action.VIEW
am start -n com.gityuan.app/.MainActivity
am start -d content://contacts/people/1
am start -t image/png
am start -c android.intent.category.APP_CONTACTS
</code></pre></div></div>

<h3 id="242-extra参数">2.4.2 Extra参数</h3>
<p><strong>(1). 基本类型</strong></p>
<table>
  <tbody>
    <tr>
      <td>参数</td>
      <td>-e/-es</td>
      <td>-esn</td>
      <td>-ez</td>
      <td>-ei</td>
      <td>-el</td>
      <td>-ef</td>
      <td>-eu</td>
      <td>-ecn</td>
    </tr>
    <tr>
      <td>类型</td>
      <td>String</td>
      <td>(String)null</td>
      <td>boolean</td>
      <td>int</td>
      <td>long</td>
      <td>float</td>
      <td>uri</td>
      <td>component</td>
    </tr>
  </tbody>
</table>

<p>比如参数es是Extra String首字母简称，实例：</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>am start -n com.gityuan.app/.MainActivity -es website gityuan.com
</code></pre></div></div>

<p>此处<code class="language-plaintext highlighter-rouge">-es website gityuan.com</code>，等价于Intent.putExtra(“website”, “gityuan.com”);</p>
<p><strong>(2). 数组类型</strong></p>
<table>
  <tbody>
    <tr>
      <td>参数</td>
      <td>-esa</td>
      <td>-eia</td>
      <td>-ela</td>
      <td>-efa</td>
    </tr>
    <tr>
      <td>数组类型</td>
      <td>String[]</td>
      <td>int[]</td>
      <td>long[]</td>
      <td>float[]</td>
    </tr>
  </tbody>
</table>

<p>比如参数eia，是Extra int array首字母简称，多个value值之间以逗号隔开，实例：</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>am start -n com.gityuan.app/.MainActivity -ela weekday 1,2,3,4,5
</code></pre></div></div>

<p>此处<code class="language-plaintext highlighter-rouge">-ela weekday 1,2,3,4,5</code>，等价于Intent.putExtra(“weekday”, new int[]{1,2,3,4,5});</p>
<p><strong>(3). ArrayList类型</strong></p>
<table>
  <tbody>
    <tr>
      <td>参数</td>
      <td>-esal</td>
      <td>-eial</td>
      <td>-elal</td>
      <td>-efal</td>
    </tr>
    <tr>
      <td>List类型</td>
      <td>String</td>
      <td>int</td>
      <td>long</td>
      <td>float</td>
    </tr>
  </tbody>
</table>

<p>比如参数efal，是Extra float Array List首字母简称，多个value值之间以逗号隔开，实例：</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>am start -n com.gityuan.app/.MainActivity -efal nums 1.2,2.2
</code></pre></div></div>

<p>此处<code class="language-plaintext highlighter-rouge">-efal nums 1.2,2.2</code>，等价于先构造ArrayList变量，再通过putExtra放入第二个参数。</p>
<h3 id="243-flags参数">2.4.3 Flags参数</h3>
<p>在参数类型1中，提到有<code class="language-plaintext highlighter-rouge">-f <FLAGS></code>，是通过<code class="language-plaintext highlighter-rouge">Intent.setFlags(int )</code>方法，来设置Intent的flags.本小节也是关于flags，是通过<code class="language-plaintext highlighter-rouge">Intent.addFlags(int )</code>方法。如下所示，所有的flags参数。</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>[--grant-read-uri-permission] [--grant-write-uri-permission]
[--grant-persistable-uri-permission] [--grant-prefix-uri-permission]
[--debug-log-resolution]
[--exclude-stopped-packages] [--include-stopped-packages]
[--activity-brought-to-front] [--activity-clear-top]
[--activity-clear-when-task-reset] [--activity-exclude-from-recents]
[--activity-launched-from-history] [--activity-multiple-task]
[--activity-no-animation] [--activity-no-history]
[--activity-no-user-action] [--activity-previous-is-top]
[--activity-reorder-to-front] [--activity-reset-task-if-needed]
[--activity-single-top] [--activity-clear-task]
[--activity-task-on-home]
[--receiver-registered-only] [--receiver-replace-pending]
</code></pre></div></div>

<p>例如，发送action=”broadcast.demo”的广播，并且对于forceStopPackage()的应用不允许接收该广播，命令如下：</p>
<div class="language-plaintext highlighter-rouge"><div class="highlight"><pre class="highlight"><code>am broadcast -a broadcast.demo --exclude-stopped-packages
</code></pre></div></div>
