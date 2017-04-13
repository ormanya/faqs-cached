<h1>Sonarr - Basic Setup</h1>

        
In SSH do the commands described in this FAQ. If you do not know how to SSH into your slot use this FAQ: <a href="https://www.feralhosting.com/faq/view?question=12">SSH Guide - The Basics</a><br>
<br>
Your FTP &#x2F; SFTP &#x2F; SSH login information can be found on the Slot Details page for the relevant slot. Use this link in your Account Manager to access the relevant slot:<br>
<br>
<img src="https://raw.github.com/feralhosting/feralfilehosting/master/Feral%20Wiki/0%20Generic/slot_detail_link.png"><br>
<br>
You login information for the relevant slot will be shown here:<br>
<br>
<img src="https://raw.github.com/feralhosting/feralfilehosting/master/Feral%20Wiki/0%20Generic/slot_detail_ssh.png"><br>
<br>
<h2>Installing Mono and then Sonarr</h2><br>
Use this command to create the <code>~&#x2F;bin</code> directory and reload your shell for this change to take effect.<br>
<br>
<pre><code>mkdir -p ~&#x2F;bin &amp;&amp; bash</code></pre><br>
Then you need to install <code>libtool</code><br>
<br>
<pre><code>wget -qO ~&#x2F;libtool.tar.gz <a href="http://ftpmirror.gnu.org/libtool/libtool-2.4.6.tar.gz">http:&#x2F;&#x2F;ftpmirror.gnu.org&#x2F;libtool&#x2F;libtool-2.4.6.tar.gz</a>
tar xf ~&#x2F;libtool.tar.gz &amp;&amp; cd ~&#x2F;libtool-2.4.6
.&#x2F;configure --prefix=$HOME &amp;&amp; make &amp;&amp; make install &amp;&amp; cd &amp;&amp; rm -rf libtool{-2.4.6,.tar.gz}</code></pre><br>
Then you can install mono locally:<br>
<br>
 <strong>Important notes:</strong> mono takes a long time to install. Once you do the last command expect to wait up to 20 minutes for the process to complete.<br>
<br>
<pre><code>wget -qO ~&#x2F;mono.tar.gz <a href="http://download.mono-project.com/sources/mono/mono-4.2.1.102.tar.bz2">http:&#x2F;&#x2F;download.mono-project.com&#x2F;sources&#x2F;mono&#x2F;mono-4.2.1.102.tar.bz2</a>
tar xf ~&#x2F;mono.tar.gz &amp;&amp; cd ~&#x2F;mono-4.2.1
.&#x2F;autogen.sh --prefix=&quot;$HOME&quot; &amp;&amp; make monolite_url=<a href="http://download.mono-project.com/monolite/monolite-138-latest.tar.gz">http:&#x2F;&#x2F;download.mono-project.com&#x2F;monolite&#x2F;monolite-138-latest.tar.gz</a> get-monolite-latest
make &amp;&amp; make install &amp;&amp; cd &amp;&amp; rm -rf mono{-4.2.1,.tar.gz}</code></pre><br>
<strong>Important:</strong> If on executing the .&#x2F;autogen.sh stage you get a message <code>**Error**: You must have `libtool&#x27; installed to compile Mono.</code> please execute <code>PATH=~&#x2F;bin:$PATH</code> try again from the .&#x2F;autogen command.<br>
<br>
Then install and run Sonarr&#x2F;NzbDrone:<br>
<br>
<pre><code>wget -qO ~&#x2F;NzbDrone.tar.gz <a href="http://update.sonarr.tv/v2/master/mono/NzbDrone.master.tar.gz">http:&#x2F;&#x2F;update.sonarr.tv&#x2F;v2&#x2F;master&#x2F;mono&#x2F;NzbDrone.master.tar.gz</a>
tar xf ~&#x2F;NzbDrone.tar.gz
mkdir -p ~&#x2F;.config&#x2F;NzbDrone
wget -qO ~&#x2F;.config&#x2F;NzbDrone&#x2F;config.xml <a href="http://git.io/vcCvh">http:&#x2F;&#x2F;git.io&#x2F;vcCvh</a>
sed -i &#x27;s|&lt;Port&gt;8989&lt;&#x2F;Port&gt;|&lt;Port&gt;&#x27;$(shuf -i 10001-32001 -n 1)&#x27;&lt;&#x2F;Port&gt;|g&#x27; ~&#x2F;.config&#x2F;NzbDrone&#x2F;config.xml
sed -i &#x27;s|&lt;UrlBase&gt;&lt;&#x2F;UrlBase&gt;|&lt;UrlBase&gt;&#x2F;&#x27;&quot;$(whoami)&quot;&#x27;&#x2F;sonarr&lt;&#x2F;UrlBase&gt;|g&#x27; ~&#x2F;.config&#x2F;NzbDrone&#x2F;config.xml</code></pre><br>
This will now be the URL you need to connect until you activate the proxypass.<br>
<br>
<pre><code>echo -e &quot;\n<a href="http://">http:&#x2F;&#x2F;</a>$(hostname -f):$(sed -rn &#x27;s|(.*)&lt;Port&gt;(.*)&lt;&#x2F;Port&gt;|\2|p&#x27; ~&#x2F;.config&#x2F;NzbDrone&#x2F;config.xml)&#x2F;$(whoami)&#x2F;sonarr&#x2F;\n&quot;</code></pre><br>
Start Sonarr<br>
<br>
<br>
<pre><code>screen -dmS sonarr && screen -S sonarr -X stuff 'export TMPDIR=~/tmp; ~/bin/mono --debug ~/NzbDrone/NzbDrone.exe'$(echo -ne '\015')</code></pre><br>
Attach to this screen at any time by using this command:<br>
<br>
<pre><code>screen -r sonarr</code></pre><br>
Then press and hold <code>CTRL</code> and <code>a</code> then press <code>d</code> to detach from the screen. This leaves it running in the background.<br>
<br>
<h2>Configuration</h2><br>
You configuration will be saved here:<br>
<br>
<pre><code>~&#x2F;.config&#x2F;NzbDrone&#x2F;config.xml</code></pre><br>
Change the port manually:<br>
<br>
<pre><code>sed -i &#x27;s|&lt;Port&gt;8989&lt;&#x2F;Port&gt;|&lt;Port&gt;&#x27;$(shuf -i 10001-32001 -n 1)&#x27;&lt;&#x2F;Port&gt;|g&#x27; ~&#x2F;.config&#x2F;NzbDrone&#x2F;config.xml</code></pre><br>
Read the&nbsp; port:<br>
<br>
<pre><code>sed -rn &#x27;s|(.*)&lt;Port&gt;(.*)&lt;&#x2F;Port&gt;|\2|p&#x27; ~&#x2F;.config&#x2F;NzbDrone&#x2F;config.xml</code></pre><br>
<h2>Proxypass</h2><br>
<h3>Apache - Run these commands:</h3><br>
<pre><code>wget -qO ~&#x2F;.apache2&#x2F;conf.d&#x2F;sonarr.conf <a href="http://git.io/vcCGM">http:&#x2F;&#x2F;git.io&#x2F;vcCGM</a>
sed -i &#x27;s|PORT|&#x27;&quot;$(sed -rn &#x27;s|(.*)&lt;Port&gt;(.*)&lt;&#x2F;Port&gt;|\2|p&#x27; ~&#x2F;.config&#x2F;NzbDrone&#x2F;config.xml)&quot;&#x27;|g&#x27; ~&#x2F;.apache2&#x2F;conf.d&#x2F;sonarr.conf
&#x2F;usr&#x2F;sbin&#x2F;apache2ctl -k graceful 2&gt;&#x2F;dev&#x2F;null</code></pre><br>
This will now be the URL you need to connect to.<br>
<br>
<pre><code>echo -e &quot;\n<a href="https://">https:&#x2F;&#x2F;</a>$(hostname -f)&#x2F;$(whoami)&#x2F;sonarr&#x2F;\n&quot;</code></pre><br>
<h3>nginx - Run these commands:</h3><br>
<pre><code>wget -qO ~&#x2F;.nginx&#x2F;conf.d&#x2F;000-default-server.d&#x2F;sonarr.conf <a href="http://git.io/vcC03">http:&#x2F;&#x2F;git.io&#x2F;vcC03</a>
sed -i &#x27;s|username|&#x27;&quot;$(whoami)&quot;&#x27;|g&#x27; ~&#x2F;.nginx&#x2F;conf.d&#x2F;000-default-server.d&#x2F;sonarr.conf
sed -i &#x27;s|PORT|&#x27;&quot;$(sed -rn &#x27;s|(.*)&lt;Port&gt;(.*)&lt;&#x2F;Port&gt;|\2|p&#x27; ~&#x2F;.config&#x2F;NzbDrone&#x2F;config.xml)&quot;&#x27;|g&#x27; ~&#x2F;.nginx&#x2F;conf.d&#x2F;000-default-server.d&#x2F;sonarr.conf
&#x2F;usr&#x2F;sbin&#x2F;nginx -s reload -c ~&#x2F;.nginx&#x2F;nginx.conf 2&gt;&#x2F;dev&#x2F;null</code></pre><br>
This will now be the URL you need to connect to.<br>
<br>
<pre><code>echo -e &quot;\n<a href="https://">https:&#x2F;&#x2F;</a>$(hostname -f)&#x2F;$(whoami)&#x2F;sonarr&#x2F;\n&quot;</code></pre><br>
<h2>Configuring with rTorrent</h2><br>
To use Sonarr with rTorrent, first you need to <a href="https://www.feralhosting.com/faq/view?question=231">switch to nginx</a> to enable to rTorrent RPC. Information should be supplied as follows (see below for an explanation of the variables, which begin with $):<br>
<br>
<pre><code>Name: (This doesn&#x27;t matter)
Enable: Yes
host: $server.feralhosting.com
port: 443
urlpath: &#x2F;$username&#x2F;rtorrent&#x2F;rpc
use SSL: yes
username: rutorrent
password: $password</code></pre><br>
In the above list, $server is your server here at feral, e.g. Metis or Brontes. $username is your Feral username and $password is the one you use to access ruTorrent (not necessarily the one listed on your account if you&#x27;ve changed it, but the one you actually use!).<br>
<br>
Please note that the RPC username is given as rutorrent in the list above - this is correct. It&#x27;s the word <code>rutorrent</code> and not your Feral username for this field.<br>
<h2>Configuring with Deluge</h2><br>
To use Sonarr with Deluge:<br>
<br>
1) Make sure your Deluge client allows remote connections (Preferences -&gt; Daemon -&gt; Allow remote connections). <br>
<br>
2) Set Sonarr to show advanced settings (you need this to display the URL base field).<br>
<br>
Information should be supplied as follows in Sonarr (see below for an explanation of the variables, which begin with $):<br>
<br>
<pre><code>Name: (This doesn&#x27;t matter)
Enable: Yes
host: $server.feralhosting.com
port: 80
URL Base: &#x2F;$username&#x2F;deluge
use SSL: no
password: $password</code></pre><br>
In the above list, $server is your server here at feral, e.g. Metis or Brontes. $password is the one you use to access Deluge (not necessarily the one listed on your account if you&#x27;ve changed it, but the one you actually use!).<br>
<h2>Restart Sonarr</h2><br>
To shut-down Sonarr:<br>
<br>
<pre><code>killall mono</code></pre><br>
If it won&#x27;t go down then use this:<br>
<br>
 <strong>Important notes:</strong> You may lose settings this way, make sure to save first:<br>
<br>
<pre><code>killall -9 mono</code></pre><br>
Then restart like this:<br>
<br>
<pre><code>screen -dmS sonarr && screen -S sonarr -X stuff 'export TMPDIR=~/tmp; ~/bin/mono --debug ~/NzbDrone/NzbDrone.exe'$(echo -ne '\015')</code></pre><br>
Attach to this screen at any time by using this command:<br>
<br>
<pre><code>screen -r sonarr</code></pre><br>
Then press and hold <code>CTRL</code> and <code>a</code> then press <code>d</code> to detach from the screen. This leaves it running in the background.<br>
<br>
