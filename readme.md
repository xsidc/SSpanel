<h2 id="搭建环境介绍"><span id="i">搭建环境介绍</span></h2>
<p>本文所使用的机器配置：</p>
<ul>
  <li>Provider: 谷歌云</li>
  <li _hover-ignore="1">Operating system: Centos 7 x86_64 bbr</li>
  <li>RAM: 1.7GB</li>
  <li>Disk: 20GB</li>
</ul>
<p>SSRPanel 环境要求：</p>
<ul>
  <li>PHP 7.1 （必须）</li>
  <li>MySQL 5.5 （推荐 5.6+）</li>
  <li>内存 1G+</li>
  <li>磁盘空间 10G+</li>
</ul>
<h2 id="系统初始配置"><span id="i-2">系统初始配置</span></h2>
<div>
  <div id="crayon-5d72816a22383293361959" data-settings=" minimize scroll-mouseover">
    <div></div>
    <div>
      <table>
        <tbody>
          <tr>
            <td><div>
              <div id="crayon-5d72816a22383293361959-1">yum -y install epel-release</div>
              <div id="crayon-5d72816a22383293361959-2">yum -y update</div>
              <div id="crayon-5d72816a22383293361959-3">yum -y groupinstall &quot;Development Tools&quot;</div>
            </div></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</div>
<h2 id="宝塔面板的安装及配置"><span id="i-3">宝塔面板的安装及配置</span></h2>
<h3 id="使用宝塔面板的好处"><span id="i-4">使用宝塔面板的好处</span></h3>
<ul>
  <li>可视化管理；</li>
  <li>一键安装网站环境；</li>
  <li>自动更改时区以及校时（SSRPanel 要求多节点之间时区相同，时间一样）；</li>
  <li>端口管理方便。</li>
</ul>
<h3 id="安装宝塔面板"><span id="i-5">安装宝塔面板</span></h3>
<p>首先要安装宝塔面板啦，复制下面这条命令在控制台中执行就行了。</p>
<div id="crayon-5d72816a22390497249317" data-settings=" minimize scroll-mouseover">
  <div>
     </div>
  <div>
    <table>
      <tbody>
        <tr>
          <td data-settings="show"><div>
            <div data-line="crayon-5d72816a22390497249317-1">1</div>
          </div></td>
          <td><div>
            <div id="crayon-5d72816a22390497249317-1">yum install -y wget &amp;&amp; wget -O install.sh http://download.bt.cn/install/install_6.0.sh &amp;&amp; bash install.sh</div>
          </div></td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
<p> </p>
<p>安装完成之后你会得到一个访问网址，以及用户名和密码，在浏览器中打开给定网址就能访问宝塔面板的控制台了。</p>
<h3 id="安装LNMP"><span id="_LNMP">安装 LNMP</span></h3>
<p>第一次打开宝塔控制台时会提示你安装网站运行环境，按图选择即可（稍后补图）。</p>
<p>PHP 版本选择 7.1。</p>
<p>SSRPanel 推荐使用MySQL 5.6+，低内存服务器就老老实实选择MySQL 5.5吧。</p>
<h3 id="添加网站"><span id="i-6">添加网站</span></h3>
<p>在<strong>宝塔面板 -&gt; 网站 -&gt; 添加站点</strong>中来添加一个新的站点。配置的信息都很重要，要记下来。</p>
<ul>
  <li>域名：填写你的域名；</li>
  <li>根目录：你的网站文件在服务器上的位置，要记住自己网站的根目录；</li>
  <li>FTP：是否创建 FTP 用户，可根据需求选择；</li>
  <li>数据库：类型选择MySQL，编码选择utf8mb4；</li>
  <li>数据库设置：数据库的用户名和密码；</li>
  <li>PHP 版本：PHP-71。</li>
</ul>
<h3 id="导入数据库"><span id="i-7">导入数据库</span></h3>
<p>在<strong>宝塔面板 -&gt; 数据库</strong>中，找到你刚刚创建的数据库，点击<strong>导入 -&gt; 从本地上传</strong>。</p>
<p>数据库文件位于sql/db.sql，你可以从 Github 上下载到。</p>
<h3 id="SWAP配置"><span id="SWAP">SWAP 配置</span></h3>
<p>SSRPanel 依赖 phpfileinfo 扩展，phpfileinfo 的安装对内存容量有一定的要求，如果内存太小的话会安装失败，所以小内存机器可以通过添加 Swap 的方式增大可用内存容量。</p>
<p>在<strong>宝塔面板 -&gt; 首页 -&gt;Linux 工具箱 -&gt;Swap / 虚拟内存</strong>中，添加 Swap 为2048 MB。</p>
<h3 id="安装phpfileinfo扩展"><span id="_phpfileinfo">安装 phpfileinfo 扩展</span></h3>
<p>在<strong>宝塔面板 -&gt; 软件管理 -&gt;PHP-7.1-&gt; 安装扩展</strong>中，找到名为fileinfo的扩展并安装。</p>
<h3 id="删除禁用函数"><span id="i-8">删除禁用函数</span></h3>
<p>在<strong>宝塔面板 -&gt; 软件管理 -&gt;PHP-7.1-&gt; 禁用函数</strong>中，删除 proc_开头的所有函数。</p>
<h2 id="网站配置"><span id="i-9">网站配置</span></h2>
<h3 id="拉取文件"><span id="i-10">拉取文件</span></h3>
<p>在之前，我们添加网站的时候已经设置了网站的根目录，现在我们在服务器控制台里输入命令，从 Github 拉取 SSRPanel 的文件。</p>
<div>
  <div id="crayon-5d72816a22394064076333" data-settings=" minimize scroll-mouseover">
    <div>
       </div>
    <div>
      <table>
        <tbody>
          <tr>
            <td><div>
              <div id="crayon-5d72816a22394064076333-1"># 进入网站根目录</div>
              <div id="crayon-5d72816a22394064076333-2"># 注意替换你自己的网站路径</div>
              <div id="crayon-5d72816a22394064076333-3">cd www/wwwroot/baidu.com</div>
              <div id="crayon-5d72816a22394064076333-4"> </div>
              <div id="crayon-5d72816a22394064076333-5"># 拉取代码</div>
              <div id="crayon-5d72816a22394064076333-6">git clone https://github.com/marisn2017/ssrpanel_resource.git tmp</div>
              <div id="crayon-5d72816a22394064076333-7">mv tmp/.git .</div>
              <div id="crayon-5d72816a22394064076333-8">rm -rf tmp</div>
              <div id="crayon-5d72816a22394064076333-9">git reset --hard</div>
              <div id="crayon-5d72816a22394064076333-10"> </div>
              <div id="crayon-5d72816a22394064076333-11"># 更改权限</div>
              <div id="crayon-5d72816a22394064076333-12">chown -R www:www storage/</div>
              <div id="crayon-5d72816a22394064076333-13">chmod -R 755 storage/</div>
              <div id="crayon-5d72816a22394064076333-14"> </div>
              <div id="crayon-5d72816a22394064076333-15"># 安装依赖</div>
              <div id="crayon-5d72816a22394064076333-16">php composer.phar install</div>
              <div id="crayon-5d72816a22394064076333-17">php artisan key:generate</div>
            </div></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</div>
<h3 id="编写配置文件"><span id="i-11">编写配置文件</span></h3>
<p>因为网站需要用到 MySQL 数据库，这一步我们配置数据库连接信息。</p>
<p>把.env.example复制一份命名为.env，按需更改配置。</p>
<p>可以在<strong>宝塔面板 -&gt; 文件</strong>里操作，也可以在服务器控制台操作。</p>
<div>
  <div id="crayon-5d72816a2239c734393897" data-settings=" minimize scroll-mouseover">
    <div>
       </div>
    <div>
      <table>
        <tbody>
          <tr>
            <td><div>
              <div id="crayon-5d72816a2239c734393897-1">cp .env.example .env</div>
              <div id="crayon-5d72816a2239c734393897-2">vi .envcp .env.example .env</div>
              <div id="crayon-5d72816a2239c734393897-3">vi .env</div>
            </div></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</div>
<ul>
  <li>DB_HOST: 数据库地址，如果在本机就是 127.0.0.1；</li>
  <li>DB_PORT: 数据库端口，默认 3306；</li>
  <li>DB_DATABASE: 数据库名；</li>
  <li>DB_USERNAME: 数据库用户名；</li>
  <li>DB_PASSWORD: 数据库密码；</li>
  <li>REDIRECT_HTTPS: 是否启用 HTTPS。</li>
</ul>
<h3 id="运行目录"><span id="i-12">运行目录</span></h3>
<p><strong>宝塔面板 -&gt; 网站 -&gt; 设置 -&gt; 运行目录 -&gt; 选择/public</strong>。</p>
<h3 id="伪静态"><span id="i-13">伪静态</span></h3>
<p><strong>宝塔面板 -&gt; 网站 -&gt; 设置 -&gt; 伪静态 -&gt; 选择laravel5-&gt; 保存</strong>。</p>
<h3 id="定时任务"><span id="i-14">定时任务</span></h3>
<p>SSRPanel 需要定时任务来完成自动维护。</p>
<div>
  <div id="crayon-5d72816a2239e154552063" data-settings=" minimize scroll-mouseover">
    <div>
       </div>
    <div>
      <table>
        <tbody>
          <tr>
            <td><div>
              <div id="crayon-5d72816a2239e154552063-1"># 必须给 www 用户创建一个目录，否则没法用其身份去运行crontab</div>
              <div id="crayon-5d72816a2239e154552063-2">mkdir /home/www &amp;&amp; chown -R www:www /home/www/</div>
              <div id="crayon-5d72816a2239e154552063-3"> </div>
              <div id="crayon-5d72816a2239e154552063-4"># 注意运行权限，必须跟SSRPanel项目权限一致，否则出现各种莫名其妙的错误</div>
              <div id="crayon-5d72816a2239e154552063-5"># 宝塔默认生成的用户是www</div>
              <div id="crayon-5d72816a2239e154552063-6">crontab -e -u www</div>
              <div id="crayon-5d72816a2239e154552063-7"> </div>
              <div id="crayon-5d72816a2239e154552063-8"># crontab加入下面的命令，自行替换路径</div>
              <div id="crayon-5d72816a2239e154552063-9">* * * * * php /www/wwwroot/baidu.com/artisan schedule:run &gt;&gt; /dev/null 2&gt;&amp;1</div>
            </div></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</div>
<h2 id="打开网站"><span id="i-15">打开网站</span></h2>
<p>在打开网站之前，要先<strong>重启一下服务器</strong>，使网站环境重载配置。</p>
<p>如果以上步骤都没出错，那么现在在浏览器输入你的域名就能访问网站了。</p>
<p>默认登录账号是admin，密码是123456。</p>
<h3 id="修改管理员密码"><span id="i-16">修改管理员密码</span></h3>
<p>点击<strong>右上角头像 -&gt; 个人设置 -&gt; 修改密码</strong>。</p>
<h3 id="添加节点"><span id="i-17">添加节点</span></h3>
<p><strong>SSRPanel-&gt; 管理面板 -&gt; 节点管理 -&gt; 添加节点</strong>。</p>
<ul>
  <li>基础信息
      <ul>
        <li>节点名称：起个名字；</li>
        <li>绑定域名：如果用域名解析可以填，不过没有可以忽略；</li>
        <li>SSH 端口：填服务器的 SSH 端口，用于 TCP 阻断检测；</li>
        <li>IPV4 地址：填 SSR 服务器的 IP 地址；</li>
        <li>标签：起一个标签名字，<strong>用户是通过标签与服务器关联起来的</strong>。</li>
      </ul>
  </li>
  <li>扩展信息
      <ul>
        <li>类型：Shadowsocks(R)；</li>
        <li>加密方式、协议、协议参数、混淆、混淆参数：可以改，但请务必<strong>与后端保持一致</strong>；</li>
      </ul>
  </li>
</ul>
<p>填好之后保存，在节点列表你可以看到<strong>节点 id</strong>，记下来，待会儿写在后端配置文件里。</p>

SSR节点配置：https://garygeng.com/buildsite/ssrpanel-ssr/


<h2><span id="SSRPanel">SSRPanel一键脚本</span></h2>
<p>https://github.com/marisn2017/ssrpanel</p>
<p>一键脚本【仅支持Centos 7.x 64位系统】：</p>
<p>稳定版：</p>
<div id="crayon-5d72816a223a1209410596" data-settings=" minimize scroll-mouseover">
  <div>
     </div>
  <div>
    <table>
      <tbody>
        <tr>
          <td><div>
            <div id="crayon-5d72816a223a1209410596-1">yum install screen wget -y &amp;&amp;screen -S ssrpanel</div>
            <div id="crayon-5d72816a223a1209410596-2">wget --no-check-certificate https://raw.githubusercontent.com/marisn2017/ssrpanel/master/stable-script.sh&amp;&amp;chmod +x stable-script.sh&amp;&amp;bash stable-script.sh</div>
          </div></td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
<p>开发版：</p>
<div id="crayon-5d72816a223a2320997769" data-settings=" minimize scroll-mouseover">
  <div>
     </div>
  <div>
    <table>
      <tbody>
        <tr>
          <td><div>
            <div id="crayon-5d72816a223a2320997769-1">yum install screen wget -y &amp;&amp;screen -S ssrpanel</div>
            <div id="crayon-5d72816a223a2320997769-2">wget --no-check-certificate https://raw.githubusercontent.com/marisn2017/ssrpanel/master/dev-script.sh&amp;&amp;chmod +x dev-script.sh&amp;&amp;bash dev-script.sh</div>
          </div></td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
<p><strong>胖虎的ssrpanel一键包</strong></p>
<p>1.多节点账号管理面板，兼容SS、SSRR<br />
  2.需配合SSR或SSRR版后端使用<br />
  3.强大的管理后台、美观的界面、简单易用的开关、支持移动端自适应<br />
  4.内含简单的购物、卡券、邀请码、推广返利&amp;提现、文章管理、工单（回复带邮件提醒）等模块<br />
  5.用户、节点标签化，不同用户可见不同节点<br />
  6.SS配置转SSR(R)配置，轻松一键导入SS账号<br />
  7.单机单节点日志分析功能<br />
  8.账号、节点24小时和近30天内的流量监控<br />
  9.邮件、serverChan投递都有记录<br />
  10.账号临近到期、流量不够会自动发邮件提醒，自动禁用到期、流量异常的账号，自动清除日志等各种强大的定时任务<br />
  11.后台一键添加加密方式、混淆、协议、等级<br />
  12.强大的后台一键配置功能<br />
  13.屏蔽常见爬虫、屏蔽机器人<br />
  14.支持单端口多用户<br />
  15.支持节点订阅功能，可自由更换订阅地址、封禁账号订阅地址<br />
  16.节点宕机提醒（邮件、ServerChanWeChat提醒）<br />
  17.支持多国语言，自带英文语言包<br />
  18.订阅防投毒机制<br />
  19.自动释放端口机制，防止端口被大量长期占用<br />
  20.封特定国家、地区、封IP段<br />
  21.有赞云支付<br />
  22.开放API，方便自行定制改造客户端</p>
