eJet��һ����GitHub�Ͽ�Դ��Web�����������ص�ַΪ [https://github.com/kehengzhong/ejet](https://github.com/kehengzhong/ejet)������[adif���ݽṹ���㷨��](https://github.com/kehengzhong/adif) �� [ePump���](https://github.com/kehengzhong/epump)������Ƕ��ʽWeb�������������������Web Cacheϵͳ�����Կ����ʽǶ�뵽Ӧ�ó����У��ṩWeb�����ܡ�

# һ. eJet��ʲô��

eJet Web������������GitHub�ϵĿ�Դ��Ŀ [adif���ݽṹ���㷨��](https://github.com/kehengzhong/adif) �� [ePump���](https://github.com/kehengzhong/epump)����C���Կ�����һ���¼�����ģ�͡����̡߳��󲢷����ӵ��������ĸ�����Web��������֧��HTTP/1.0��HTTP/1.1Э�飬��֧��HTTP Proxy��Tunnel�ȹ��ܡ�

��Linux�£�eJet Web����������ɶ�̬���̬��Ĵ�СԼΪ300K���ɼ���Ƕ�뵽�κ�Ӧ�ó����У�����Ӧ�ó���ʹ��HTTPͨ�źͷ�����ص�������ʹ��߱���Nginx������һ��ǿ���Web���ܡ�

eJet Web��������ȫ������ePump���֮�ϣ�����ePump��ܵĶ��߳��¼�����ģ�ͣ�ʵ��������HTTP����<-->HTTP��Ӧ�������̡�eJet��û�д������̻��̣߳�����ePump��ܵ��¼��������̣߳���Ч�����÷�������CPU����������

eJet���պʹ����TCP�����ϵ�HTTP����ͷ�������壬����������У�顢������ʵ�����ȴ���ִ��HTTP���󣬻��ȡWeb�������ض�Ŀ¼�µ��ļ��������ͻ��˷�����ԴHTTP�����������󣬻�HTTP����ͨ��FastCGI�ӿ�ת����CGI���������򽫿ͻ���HTTP���󽻸��ϲ����õĻص���������ȡ����д�������������HTTP��Ӧ��ʽ������HTTP��Ӧͷ����Ӧ�壬ͨ���ͻ��˽�����TCP���ӣ����ظ��ͻ��ˡ���TCP���ӿ���Pipe-line��ʽ�������ͺͽ��ն��HTTP�������Ӧ��

eJet�������ṩ����ΪWeb�������������������ܣ���������TLS/SSL�İ�ȫ�ͼ��ܴ��䡢������������Դλ��Location�ĸ���ƥ����ԡ�������URIִ�ж�̬�ű�ָ�����rewrite��reply��return��try_files�ȣ����������ļ���ʹ��HTTP�������������ͷ������HTTP Proxy��FastCGI��HTTP Proxy Cache���ܡ�HTTP Tunnel��MultiPart�ļ��ϴ�����̬��ص���ӿں����ص����ơ�HTTP��־���ܡ�CDN�ַ��ȡ�

eJet Web����������JSon��ʽ�������ļ�������ϵͳ���ù�����JSon�﷨����һ������չ��ʹ��JSon֧��include�ļ�ָ�֧��Ƕ��Script�ű��������ԡ�ʹ����չJSon���ܵ������ļ����ɸ������������չWeb�����ܡ�

eJetϵͳ����������Zero-Copy���ڴ�ء�����ȼ�����������Web�������������ܺ�Ч�ʣ��ӿ���������Ӧ�Ĵ����ٶȣ�֧�Ÿ����ģ�Ĳ�������������֧�ָ����ģ���������������ȡ�

eJet Web�������ȿ����������Ա��ϵͳ�ܹ�ʦ�ṩӦ�ó��򿪷��ӿڻ�ֱ��Ƕ�뵽����ϵͳ�У�Ҳ����������ά����ʦ������ȫ����Nginx Web��������Web Cache��CDN��Դ����ҵ����ϵͳ�������������Ա�ṩѧϰ���о�������ܡ�ͨ��ϵͳ�ȵ�����ƽ̨��

����eJet Web��������ԭ���Ǿ����ܲ������ڵ���������Ϳ⣬���Ͱ�Ȩ�͸��Ӳ�������ش�����Ǳ�ڷ��ա�ϵͳʹ�õĵ�������������ҪΪ��OpenSSL�⡢Linuxϵͳ�Դ��ķ���POSIX��׼��������ʽregex�⡣gzipѹ����Ҫ����zlib��Դ�⣬Ŀǰû����ӽ���������eJet Web��������ʱ���ṩgzip��deflate��ѹ��֧�֡�

# ��. JSon��ʽ�������ļ�

## 2.1 JSON�﷨�ص�

JSON��ȫ����JavaScript Object Notation����һ�������������ݽ�����ʽ��JSON���ı���ʽ�����ڱ�����ԣ�����name:value�Դ洢���ƺ����ݣ����Ա������֡��ַ������߼�ֵ�����顢������������ͣ�����������ݽ����﷨��ʽ����������������չ���Ķ��ͱ�д��Ҳ���ڳ�����������ɡ�

��������JSon�﷨�ļ򵥺�ǿ��չ�ԡ����ÿɱ�������������͵�name/value���﷨����Ƕ��JSON�Ӷ�������ԣ��������ļ������������ر��Ǻϣ����ԣ�eJetϵͳʹ��JSon��ʽ�����桢���ݡ�����ϵͳ�����ļ���

## 2.2 eJet�����ļ���JSON����չ

### 2.2.1 �ָ���

eJetϵͳʹ��adif�е�JSon�������������������ļ���Ϣ��JSon�﷨ȱʡ��ʽ��ð��(:)���ָ�name��value���Ե�����(')��˫����(")������name��value�����Զ���(,)��Ϊname/value�Եķָ�������������\[\]��ʾ���飬�Դ�����{}��ʾ����

eJetϵͳ����JSon��Ϊ�����ļ��﷨�淶��Ϊ�˼��ݴ�ͳ�����ļ��ı�дϰ�ߣ���JSon�����﷨����һЩ��չ�����ָ�name��value��ð��(:)���ɵ��ں�(=)���ָ�name/value��֮��Ķ���(,)���ɷֺ�(;)�����������﷨���䡣

### 2.2.2 includeָ��

����������Ϣ���ݽϴ���Ҫʹ�ò�ͬ���ļ������治ͬ��������Ϣ�����C����/PHP���Ե�include��ָ�eJetϵͳ��JSon�﷨������includeָ���չ�﷨�н���"include"��ΪJSon�﷨�Ĺؼ��֣����ᱻ�����������ƺ�ֵ����������������ΪǶ������һ���ļ�����ǰλ�ý��к������������ָ����﷨�淶���£�

```
include <�����ļ���>;
```

����JSon����ʱ���������includeָ��ͽ�includeָ�������ļ����ݼ��ص���ǰָ��λ�ã���Ϊ��ǰ�ļ����ݵ�һ���֣����н�������

### 2.2.3 ����ע�ͺͶ���ע��

Ϊ�����������ļ��д���Ŀɶ��ԣ���Ҫ����صĶ��������ϸ˵����ע������ݣ�����ʹ����Ա�����Ķ�����⡣

Ϊ֧��ע�͹��ܣ�eJetϵͳ�������ļ���JSON�﷨������Ӧ��չ�������˵���ע�ͷ���#�Ͷ���ע��(/* */)�����﷨�淶���£�

```
# ���ǵ���ע�ͣ��������(#)����JSonĳ��Key-Value�Ե��������棬��ô�Ծ��ſ�ͷ�����ź�������ݶ���ע��

/* ע�⣺����ע����������һ���/��*��ʼ
         ������һ���*��/��β���м�����ݶ���ע��
   ����ע�Ϳ��շ��ţ����벻����Key-Value�Ե���������
 */
```
ע�͵������ڽ���ʱֱ�Ӻ������������ᱻϵͳ�����ʹ���

### 2.2.4 script�﷨

ʹ��JSON��ʽ�����ݶ�����name/value�Թ��ɣ�eJetϵͳ����Ҫ�������ļ���֧��Script�ű�������̬�ش���HTTP����

eJet�����ļ���JSON�﷨��ʽ��չ��һ�̶ֹ����Ƶ�script���󣬽�����"script"��Ϊ�����������ƹؼ��֣�����scriptΪ���ƵĶ��������ݲ�����ΪJSON�Ӷ�����������ΪScript�ű��������ݣ�����ڶ�����Ϊscript�Ķ����С����﷨�淶���£�

```
script = {
    if ($request_uri ~* '^/topic/[0-9](*)/(.*)\.mp4$') {
        set $video_flag 1;
    }
};
```

��ͬһ��JSon�����£������ж��script�����Զ�����script�������顣

���⣬ʹ������Ŀ��ձ�ǩ<script>��</script>��Ҳ���Զ���ű����������������ձ�ǩ�м�����ݣ�����Script�ű����򣬲�����Щ���ݴ洢�������ļ����������name���ƶ����У����﷨�淶���£�

```
cache file = <script>
       if ($request_uri ~* 'laoooke')
           return "${host_name}_${server_port}${req_path_only}${req_file_only}";
       else if (!-f $root$request_path) {
           return "${host_name}_${server_port}${req_path_only}${index}";
       } else if (!-x $root$request_path) {
           return "$root$request_path is not an executable file";
       } else
           return "${request_header[host]}${req_path_only}else.html";
     </script>;
```

������"cache file"��������ݾ���һ�νű�������Ҫ�ڽ���ִ�е�����ʱ������������ʵ�����ݡ�

# ��. eJet��Դ����ܹ�

## 3.1 ������Դ��λ�ܹ�

eJet Web����������Դ����ṹ�ֳ����㣺

-   **HTTP��������HTTPListen**\- ��Ӧ���Ǽ�������IP��ַ�Ͷ˿ں��TCP����
-   **HTTP��������HTTPHost**\- ��Ӧ����������������domain
-   **HTTP��Դλ��HTTPLoc**\- ��Ӧ���������µĸ�����ԴĿ¼

һ��eJet Web��������������һ���������������HTTPListen��һ�����������¿�������һ�������HTTP����������һ�����������¿������ö����Դλ��HTTPLoc������ġ������û���������ƣ�ȡ����ϵͳ��������ں���Դ���ơ�

## 3.2 HTTP�������� - HTTPListen

HTTP��������HTTPListen��ָeJet Web������������ʱ����Ҫ�󶨱���ĳ��������IP��ַ��ĳ���˿ں�����TCP�������񣬵Ⱥ���տͻ��˷���TCP���Ӻ�HTTP�������ݣ�ÿ�����ܵ�HTTPCon����һ������ĳ��HTTP��������HTTPListen���ϸ���˵��HTTPListen�������HTTPCon���ӣ������������ݴ洢��HTTPCon�Ľ��ջ����������Լ��������Ӧ����TC������Դ��������Ӧ����������Դ��domain�Ͷ˿ڡ�

HTTP���������������Ϣ��ʽ�ο����£�

```
listen = {
    local ip = *; #192.168.1.151
    port = 443;
    forward proxy = on;

    ssl = on;
    ssl certificate = cert.pem;
    ssl private key = cert.key;
    ssl ca certificate = cacert.pem;

    request process library = reqhandle.so

    script = {
        #reply 302 https://ke.test.ejetsrv.com:8443$request_uri;
        addResHeader X-Nat-IP $remote_addr;
    }

    host = {.....}
    host = {.....}
    host = {.....}
}
```

һ̨������������԰�װ���������ÿ����������һ������IP��ַ��HTTP����������Լ���ĳһ��IP��ַ�ϵ�ĳ���˿ڣ�Ҳ���Լ�������IP��ַ�ϵ�ͬһ���˿ڡ���������������Ķ˿�������������65536��������С��1024�Ķ˿���Ҫ��root����Ȩ�޲��ܼ�����

HTTP��������HTTPListen�����ڵײ�ePump��ܵ�eptcp_mlisten�ӿں�����ͨ���ýӿڣ���ÿһ��epump�����̶߳�ȥ����ָ��IP��ַ�Ͷ˿��ϵ��������������������񡣶���֧��REUSEPORT�Ĳ���ϵͳ�ںˣ������ͻ��˷���Ĳ������ӣ�����ͨ���ں�acceptϵͳ���þ���ط�̯����epump�̴߳������ڲ�֧��REUSEPORT�Ĳ���ϵͳ��ePump��ܸ���󲢷������ڸ������̼߳�ĸ��ؾ��⡣

HTTP��������HTTPListen�������õ�ǰ����Ϊ��ҪSSL�İ�ȫ���ӣ�������SSL���������˽Կ��֤��ȡ�����ΪSSL��ȫ���Ӽ�������󣬿ͻ��˷����HTTP���󶼱������� https:// ��ͷ��URL��

��HTTP��������HTTPListen���������Script�ű�����ִ�и�������������ݽ���Ԥ�жϺ�Ԥ�����ָ���Щ�ű������ִ��ʱ�������յ�������HTTP����ͷ����еġ�

eJetϵͳ�ṩ�˶�̬��ص����ƣ�ʹ�ö�̬��ص����ȿ�����չeJet Web������������Ҳ���Խ�С��Ӧ��ϵͳ������eJet Web�������ϣ�����ͻ��˷����HTTP����

HTTP��������HTTPListen�¿ɹ�������������HTTPHost��������������Ϊ����������hashtab��������������������������ǰ��������Ķ˿��յ�TCP��������ݺ󣬸���Host����ͷ���������ƣ�����ȷƥ�䶨λ���������HTTP��������HTTPHost��

## 3.3 HTTP�������� - HTTPHost

��HTTPListen���������£��������ö��������������������HTTPHost��eJet Web��������Դ������ϵ�ĵڶ��㣬��HTTPCon�����������ݽ��н���������HTTPMsg������������HTTP�������ݣ�HTTPЭ��淶�У�����ͷHostЯ����ֵ������URL��domain��Ϣ������HTTP��������HTTPHost����Ӧ�ľ����������������߾���һ����վ��һ����������HTTPListen�¿��Լ��޴�����ͨ����������HTTPHost���������վ��

HTTP����������������Ϣ��ʽ�ο����£�

```
host = {
    host name = *; #www.ejetsrv.com
    type = server | proxy | fastcgi;
    gzip = on;

    ssl certificate = cert.pem;
    ssl private key = cert.key;
    ssl ca certificate = cacert.pem;

    script = {
        #reply 302 https://ke.test.ejetsrv.com:8443$request_uri;
        addResHeader X-Nat-IP $remote_addr;
    }

    error page = {
        400 = 400.html;
        504 = 504.html;
        root = /opt/ejet/errpage;
    }

    root = /home/hzke/sysdoc;

    location = {...}
    location = {...}
    location = {...}
}
```

HTTP��������������һ����������ʽ�����༶������ϵ�����������������������������������ȣ�ͨ��DNSϵͳ������������������ǰeJet Web���������ڵ�IP��ַ�ϣ�����ڸ�IP��ַ������HTTPListen������ô����ʹ�ø����������󶼻�ָ�򵽶�Ӧ��HTTPHost����������

eJetϵͳ���ݹ��ܷ�����ʽ�����������������˼������ͣ�Server��Proxy��FastCGI�ȣ��⼸�����Ϳ���ͬʱ���棬�ɻ���һ��

��������HTTPHost�¿���������Դ��ȱʡĿ¼����������Դλ��HTTPLoc�����Ը�������������ȱʡĿ¼��

�����ǰ��������HTTPHost���ϼ����������ǽ����ڰ�ȫ����SSL�ϣ���ô���ж����վ�����������������£���ҪΪÿ����վ�������ڸ���վ������֤�顢˽Կ�Ȱ�ȫ��ݱ�ʶ��Ϣ���ͻ�������ͬһ����������������󣬲���TLS SNI���ƺ�eJet��ʵ�ֵ�SSL����ѡ��ص��������������֤���ѡ��

HTTPHost���������¿�������Script�ű��������������µĽű�����ִ��ʱ�����ڴ���HTTPMsgʵ������������DocURI��ʼִ����Դλ��ʵ�������̣��ڸ������зֱ�ִ��HTTPListen��Script�ű���HTTPHost��Script�ű���HTTPLoc��Script�ű����ű������ִ�а����������ȼ������У�ʹ�ýű������ָ����Ԥ����HTTP����ĸ������ݡ�

һ����������HTTPHost�¿������ö����Դλ��HTTPLoc��������ʵ�ǰ�����µĲ�ͬĿ¼����������HTTPHost���ö��ַ�ʽ������������Դλ��HTTPLocʵ������Ҫ�������֣�

-   ��ȷƥ������·�������������� \- ������·������Ϊ��������Դλ��������
-   ������·��ǰ׺ƥ������������� \- ������·��ǰ׺����Ϊ��������Դλ���ֵ���
-   ������·������������ʽ��������������� \- ��������ʽ�ַ���Ϊ������������Դλ���б�

���뵱ǰ���������󣬵��ײ����ĸ���Դλ��HTTPLoc��ƥ������˳���ǰ��������б�����������еģ����ȸ���HTTP�����·��������Դλ���������о�׼ƥ�䣬���û�У��������·������ǰ׺����Դλ���ֵ����н���ƥ������������û��ƥ���ϣ�������Դλ���б��е�ÿ��HTTPLoc��������������ʽ�ַ�����ȥƥ�䵱ǰ����·�������������û��ƥ�����Դλ��HTTPLoc����ôʹ�õ�ǰ����������ȱʡ��Դλ�á�

## 3.4 HTTP��Դλ�� - HTTPLoc

HTTP��Դλ��HTTPLoc�������������Դ��ĳ�����������µ�ĳ�������������Ŀ¼λ�ã�HTTPLoc�����������·��������HTTPMsg�еĿͻ����������ݣ����ջ��ڸ�����Դƥ������ҵ�HTTPListen��HTTPHost��HTTPLoc�󣬻���ȷ���˵�ǰ�������Դλ�á�����ʽ�ȡ�һ����վ��Ӧ�����������£������ж��ֹ��ܺ���Դ������Դλ��HTTPLoc����ͼ���ļ�������imageΪ����Ŀ¼�£�PHP�ļ���Ҫ����FastCGIת����php-fpm�������ȡ�

HTTP��Դλ�õ�������Ϣ��ʽ�ο����£�

```
location = {
    type = server;
    path = [ "\.(h|c|apk|gif|jpg|jpeg|png|bmp|ico|swf|js|css)$", "~*" ];

    root = /opt/ejet/httpdoc;
    index = [ index.html, index.htm ];
    expires = 30D;

    cache_file = <script>
           if ($request_uri ~* 'laoke')
               return "${host_name}_${server_port}${req_path_only}${req_file_only}";
           else if (!-f $root$request_path) {
               return "$root$request_path is not a regular file";
           } else if (!-x $root$request_path) {
               return "$root$request_path is not an executable file";
           } else
               return "${request_header[host]}${req_path_only}else.html";
         </script>;
}

location = {
    path = [ '^/view/([0-9A-Fa-f]{32})$', '~*' ];
    type = proxy;
    passurl = http://cdn.ejetsrv.com/view/$1;

    root = /opt/cache/;
    cache = on;
    cache file = /opt/cache/${request_header[host]}/view/$1;
}

location = {
    type = fastcgi;
    path = [ "\.(php|php?)$", '~*'];

    passurl = fastcgi://localhost:9000;

    index = [ index.php ];
    root = /opt/ejet/php;
}

location = {
    path = [ '/' ];
    type = server;

    script = {
        try_files $uri $uri/ /index.php?$query_string;
    };

    index = [ index.php, index.html, index.htm ];
}
```

HTTP��Դλ��HTTPLoc��ͨ��·����path��ƥ������matchtype����Ϊ���ʶ��·����Ϊ���������õ����ƣ��ͻ��������·����ͨ��ƥ�����Ͷ����ƥ������������õ�·��������ƥ�䣬�������ƥ�䣬�������ʹ�ô���Դλ��HTTPLoc��

ƥ�����matchtypeһ�㶨���������ļ���path������ĵڶ����Ϊ���¼��֣�

-   ��׼ƥ�䣬ʹ�õ��ں�'='
-   ǰ׺ƥ�䣬ʹ��'^~'����������
-   ���ִ�Сд��������ʽƥ�䣬ʹ��'~'����
-   �����ִ�Сд��������ʽƥ�䣬ʹ��'~*'����������
-   ͨ��ƥ�䣬ʹ��'/'���ţ����û������ƥ�䣬�κ����󶼻�ƥ�䵽

ƥ������ȼ�˳��Ϊ�� (location =) > (location ����·��) > (location ^~ ·��) > (location,* ����˳��) > (location ������ʼ·��) > (/)

eJetϵͳ���ݹ��ܷ�����ʽ������Դλ��HTTPLoc�����˼������ͣ�Server��Proxy��FastCGI�ȣ�ͨ������£�һ����Դλ��HTTPLocֻ����һ�����͡�

HTTP��Դλ��HTTPLoc����Ҫһ��ȱʡ�ĸ�Ŀ¼��ָ��ǰ��Դ���ڵĸ�·�����ͻ��������·����������ڵ�ǰHTTPLoc�µ�root��Ŀ¼����λ�ļ���Դ�ġ�����Proxyģʽ����Ŀ¼һ��䵱�����ļ��ĸ�Ŀ¼������Ҫ��Proxy����������������ݻ���ʱ���������ڵ�ǰHTTPLoc�µ�rootĿ¼�С�

ÿ��HTTPLoc�¶�����ȱʡ�ļ�ѡ��������ö��ȱʡ�ļ���һ������Ϊindex.html�ȡ�ʹ��ȱʡ�ļ��������ǿͻ��˷��������ֻ��Ŀ¼��ʽ����`http://www.xxx.com/`����ʱ��������ʵ���HTTPLoc�ĸ�Ŀ¼��eJetϵͳ���Զ�������Ѱ�ҵ�ǰ��Ŀ¼�µĸ���ȱʡ�ļ��Ƿ���ڣ�������ھͷ���ȱʡ�ļ����ͻ��ˡ�������Ҫע����ǣ�eJetϵͳ�����������������DocURIʱ����ġ�

HTTP��Դλ�������Proxy���ͻ�FastCGI���ͣ����������ת����ַpassurl��ת����ַpassurlһ�㶼Ϊ����URL��ַ������ָ��������������domain������passurl����ʽȡ��HTTPLoc��Դ���͡�

�������Reverse Proxy�����ǽ�HTTPLoc����Դ��������ΪProxyģʽ��ͨ������passurlָ��Ҫ�����Զ�̷�����URL��ַ����ʵ�ַ�������ܡ��ڷ������ģʽ�£�passurl�����Ǻ���ƥ����������URL��ַ�������ַָ����Ǵ�ת������һ��Origin��������ƥ��������Ϊ$1��$2�����ֱ���������ʾ����������ʽƥ��·��ʱ���ѵ�һ����ڶ���ƥ���ַ�����Ϊpassurl��һ���֡���Ȼpassurl���԰����κ�ȫ�ֱ��������ñ�����ʹ����Щ�������Ը�����ش���ת�����ݡ�

�ڷ������ģʽ�£�HTTPLoc��Դλ������һ��cache���أ��������cache=on����Cache���ܣ�����Ҫ�ڵ�ǰHTTPLoc������cachefile�����ļ��������ڲ�ͬ�������ַ��cachefile������������·��������ı仯���仯������cachefile��ȡֵ������Ҫ����HTTP����������ʹ��Script�ű�����̬����cachefile��ȡֵ��

HTTPLoc��һ�㶼�Ჿ��Script�ű����򣬰���rewrite��reply��try_files�ȣ���������·�����������������ͷ��Դ��ַ����Ϣ��������ǰ��Դλ���Ƿ���Ҫ��д���Ƿ���Ҫת�Ƶ�������ַ����ȡ�

# ��. HTTP����

## 4.1 HTTP�����Ķ���

HTTP������ָ��eJet Web�����������ڼ䣬�ܶ�̬�ط���HTTP����HTTP��Ӧ��HTTPȫ�ֹ����ʵ�������еĴ洢�ռ�������ݣ����߷���HTTP�����ļ����������ݵȵȣ������Щ�洢�ռ�ķ��ʣ���������������ƽ���HTTP������

���������ñ�����$��ͷ�������������������������滹����������������ַ�����������{}������ס���������������ʽΪ��$�������ƣ� ${��������}�� ${ �������� }���ȵ�

## 4.2 HTTP������Ӧ��

ʹ��HTTP�����ĳ�����Ҫ��JSon��ʽ�������ļ��У�������������Ŀ���Ӷ�̬�Ŀɱ�̽ӿڣ�����Ҫ���ڲ�ͬ��HTTP�������Ϣ�����жϡ��Ƚϡ���ֵ�����������ӵȲ�������Щ���벻����������Ҫ��ͬ�ı�����ȥ���ʲ�ͬHTTP�����еĲ�ͬ��Ϣ���ݣ�ͨ��������ʹ�ñ��������ʱ�����ֵ�����������жϡ��Ƚϡ�ƥ�䡢�Ӽ��˳�����ֵ�ȡ�������ʹ�������ɲο����£�

```
access log = {
    log2file = on;
    log file = /var/log/access.log;
    format = [ '$remote_addr', '-', '[$datetime[stamp]]', '"$request"', '"$request_header[host]"',
               '"$request_header[referer]"', '"$http_user_agent"', '$status', '$bytes_recv', '$bytes_sent' ];
}

script = {
    reply 302 https://ke.test.ejetsrv.com:8443$request_uri;
}

cache file = /opt/cache/${request_header[host]}/view/$1;

params = {
    SCRIPT_FILENAME   = $document_root$fastcgi_script_name;
    QUERY_STRING      = $query_string;
    REQUEST_METHOD    = $request_method;
    CONTENT_TYPE      = $content_type;
    CONTENT_LENGTH    = $content_length;
}

script = {
    if ($query[fid])
        cache file = $real_path$query[fid]$req_file_ext;
    else if ($req_file_only)
        cache file = $real_path$req_file_only;
    else if ($query[0])
        cache file = ${real_path}${query[0]}$req_file_ext;
    else
        cache file = ${real_path}index.html;
}
```

## 4.3 HTTP���������ͺ�ʹ�ù���

eJetϵͳ�У�������������HTTP�������ͣ��ֱ�Ϊ��

-   ƥ����� \- ������Դλ��HTTPLocģʽ��ƥ��HTTP����·��ʱƥ�䴮��ͨ�����ֱ��������ʣ���$1,$2�ȣ�
-   �ֲ����� \- ��script�ű���ִ�й�������setָ���ֵ���š�=�����õı�����
-   ���ñ��� \- �����ļ���Listen��Host��Location�¶����JSon Key��������ϵͳ��ʹ�õ��ĳ�������Ϊ����
-   �������� \- ����������ϵͳԤ�ȶ��塢��ֵ��������HTTPMsg�����󱻸�ֵ�ı���������������ֵ��ֻ������д��

������ʹ�ù�����ϸ߼����Ե�Լ��������ͬ��������ȡֵʱ���ȼ�˳��Ϊ�� $ƥ����� > $�ֲ����� > $���ñ��� \> $��������

HTTP������ֵ�����������ͣ����ݸ�ֵ������Ĺ���������Ļ����ı仯����ȷ����ʹ��ʱ�����������͡��ַ��͵ȡ�����ƥ������⣬�������������Ʊ����Ǵ�Сд��ĸ���»���_��϶��ɣ������ַ������ڱ���������ñ���һ���ǷǷ���Ч�����������Ķ���ܼ򵥣�ǰ�������Ԫ����$������ʹ�ñ������ƣ�ϵͳ�ͻ���Ϊ��HTTP��������Ԫ���ź�ı�������Ҳ����ͨ��������{}�����������ַ���������

���������ֵ���ݰ����������ô�ñ�����������������������ͨ��������\[\]�������±��������������ĸ���Ԫ�أ���$query\[1\]��������������еĵ�һ��������ֵ��

ƥ�����������Ϊ���֣�����Ԫ��$��ͷ����$1,$2...�������ִ������ʹ��HTTPLoc�����·��ģʽ����ȥƥ�䵱ǰHTTP����·��ʱ����ƥ��ɹ��Ķ���Ӵ���������š�ƥ�����������������HTTPMsgʵ�����ɹ���׼ȷ�ҵ�HTTPLoc��Դλ��ʵ����ʼ����HTTP��Ӧ���ɹ��ط��͵��ͻ��˺�HTTPMsg��Ϣ������ʱΪֹ��

�ֲ���������������ĸ���»�����ɣ���script�ű���ִ�й�������setָ���ֵ���š�=�����õı����������������Ǵӱ���������֮�󵽸�HTTPMsg����������ڼ䣬��HTTPMsg�����û�HTTP���󵽴�ʱ�������ɹ�����Response�󱻴ݻ١�

���ñ�����JSon��ʽ�������ļ��ж����Key-Value���У���KeyΪ���Ƶı�����������ֵ�����õ�Value���ݡ��������ļ���λ��Location��Host��Listen�¶����Key-Value��ֵ���ԣ����Ϊ���������Ҳ�Ϊ����ֵ����$���ſ���ֱ��������Щ������������ݣ���Listen��Host��Location�¶�������ñ�������Ҫ����ϵͳ�п���ʹ�õ��ĳ�������Ϊ������Щ��������Ҳ����ʹ��script�ű�����̬�����䳣��ֵ�����⣬�û����Զ��ⶨ��ϵͳ�����з�ȱʡ���������ǳ�֮Ϊ��̬���ñ�����

����������ϵͳԤ������й̶����Ƶ�һ�ֱ������ͣ���������һ��ָ��HTTP����ĸ�����Ϣ��eJetϵͳ�����ȫ�ֱ����ȡ�����������������eJetϵͳԤ�ȶ��岢���������󲿷ֱ����������Ǹ�HTTP����HTTPMsg��صģ�����ͬ������HTTPMsg��������������ֵҲ�����ű仯�ġ�һ��Ҫ�󣬲���������ֻ������д������������������ֵ���ܱ��ű�����ı䣬ֻ�ܶ�ȡ���ʡ�

## 4.4 Ԥ����Ĳ��������б��ʵ��ԭ��

����������ֱ��������������Ǳ�ʹ����ࡢ���з��ʼ�ֵ�ı���������������ϵͳԤ�ȶ���Ĺ̶����Ʊ�����������ֵ������HTTP����HTTPMsg�Ĳ�ͬ����ͬ��ͨ�����������������ļ��п��Ը����������Ϣ����̬�ؾ����������ѡ��ĸ�ֵ���ݣ��Ӷ���չeJet����������������������⹦����չ����eJetϵͳ�Ķ��ƿ�����

��������һ����eJetϵͳԤ�ȶ��巢�����������ֵ�����Ǹ���HTTP����HTTPMsg�ı仯���仯��������������ȫ��ͳһͨ�ã����Բ�������Ҳ��ʱ��Ϊȫ�ֱ�����

eJetϵͳԤ����Ĳ����������£�

-   **remote_addr**\- HTTP�����ԴIP��ַ
-   **remote_port**\- HTTP�����Դ�˿�
-   **server_addr**\- HTTP����ķ�����IP��ַ
-   **server_port**\- HTTP����ķ������˿�
-   **request_method**\- HTTP����ķ�������GET��POST��
-   **scheme**\- HTTP�����Э�飬��http��https��
-   **host_name**\- HTTP�������������
-   **request_path**\- HTTP�����·��
-   **query_string**\- HTTP�����Query������
-   **req\_path\_only**\- HTTP�����ֻ��Ŀ¼��·����
-   **req\_file\_only**\- HTTP����·���е��ļ�����
-   **req\_file\_base**\- HTTP����·���е��ļ�������
-   **req\_file\_ext**\- HTTP����·�����ļ���չ��
-   **real_file**\- HTTP�����Ӧ����ʵ�ļ�·����
-   **real_path**\- HTTP�����Ӧ����ʵ�ļ�����Ŀ¼��
-   **bytes_recv**\- HTTP������յ��Ŀͻ����ֽ���
-   **bytes_sent**\- HTTP��Ӧ���͸��ͻ��˵��ֽ���
-   **status**\- HTTP��Ӧ��״̬��
-   **document_root**\- HTTP�������Դλ�ø�·��
-   **fastcgi\_script\_name**\- HTTP�����о����ű����к��DocURI��·����
-   **content_type**\- HTTP���������MIME����
-   **content_length**\- HTTP����������ݳ���
-   **absuriuri**\- HTTP����ľ���URI
-   **uri**\- HTTP����ԴURI��·����
-   **request_uri**\- HTTP����ԴURI����
-   **document_uri**\- HTTP���󾭹��ű����к��DocURI����
-   **request**\- HTTP������
-   **http\_user\_agent**\- HTTP�����û�����
-   **http_cookie**\- HTTP�����Cookie��
-   **server_protocol**\- HTTP�����Э��汾
-   **ejet_version**\- eJetϵͳ�İ汾��
-   **request_header**\- HTTP�����ͷ��Ϣ���飬ͨ�����������±������ͷ���Ƶ�������������
-   **cookie**\- HTTP�����Cookie���飬ͨ�����������±��Cookie���Ƶ�������������
-   **query**\- HTTP�����Query�������飬ͨ�����������±��������Ƶ�������������
-   **response_header**\- HTTP��Ӧ��ͷ��Ϣ���飬ͨ�����������±����Ӧͷ���Ƶ�������������
-   **datetime**\- ϵͳ����ʱ�����飬������������ϵͳʱ�䣬��createtime��stamp�������������HTTPMsg����ʱ������ʱ��
-   **date**\- ϵͳ�������飬ͬ��
-   **time**\- ϵͳʱ�䣬ͬ��

����Ӧ�ó�������չ��������Ҫ��������չ�����������ƵĲ���������������˵��ʹ�����������������������Է���HTTP������ص�������Ϣ����������󲿷ֳ���������

ϵͳ��Ԥ����Ĳ�������������ָ���ض��Ļ������ݽṹ��ĳ����Ա�������ڸ����ݽṹʵ���������Ա�����ĵ�ַָ��ͻᱻ��̬�ظ�ֵ��Ԥ����Ĳ����������Ӷ�����ַָ��ָ������ݹ��������������ϡ�

������Ԥ�������������ʱ��һ����Ҫ���ù��������ݽṹ�����ݽṹ�ĳ�Ա������ַ��λ�á���Ա�������ͣ��ַ��������������������������ַ������ַ�ָ�롢frame\_t�����������͡��洢���ȵȣ�eJetϵͳ��ά��һ�������Ĳ����������飬�ֱ���ɲ����������ݵĳ�ʼ����ͨ��hashtab\_t�����ٶ�λ�Ͷ�д���������еĲ���������

��ȡ����������ʵ��ֵʱ����Ҫ����HTTPMsg������ݽṹ��ʵ��ָ�룬���ݲ��������������ҵ�������������Ĳ�������ʵ�������ݲ�����������Ϣ���ʹ����ʵ��ָ�룬��λ����ʵ�ʳ�Ա�������ڴ�ָ��ʹ�С�����ڴ���ȡ���ó�Ա������ֵ��

# ��. HTTP Script�ű�

## 5.1 HTTP Script�ű�����

eJetϵͳ�������ļ�����չ��Script�ű����Ե��﷨���壬��JSon�﷨�淶������չ��������һ�׷���JavaScript��C���Եı���﷨�����ṩScript�ű���������ʵ��һ���ı�̺ͽ���ִ�й��ܡ�

Script�ű�����һϵ�з��϶�����﷨�������д�Ĵ��������ɣ��������������Javascript��C���ԣ�ÿ�������һ������ָ��ɣ����Էֺ�;��β��

## 5.2 Script�ű�Ƕ��λ��

HTTP Script�ű������Ƕ��λ�ã��������֡���һ��Ƕ��λ�����������ļ���Listen��Host��Location�£�ͨ������JSon����script�����ű�������Ϊscript��������ݣ���ʵ�������ļ���Ƕ��ű���̹��ܡ�������λ���У�����script�ű�������﷨�����������֣�

```
  script = {....};
  script = if()... else...;
  <script> .... </script>
```

����һ��Ƕ��Script�ű������λ�ã�����JSon�е�Key-Value���У���Value����������պϱ�ǩ<script> Script Codes </script>���ڱ�ǩ����Ƕ��Script�ű����룬ִ�������󷵻ص����ݣ���ΪKey��ֵ�����ַ�ʽʹ��JSon�淶��Key��ֵ���Զ�̬����Script�ű���������������Listen��Host��Location�ĳ�����ֵ�У�Value���ݿ�����script�ű�����

```
  cache file = <script> if ()... return... </script>
```

��adif �������е�json.c�ļ������޸���չ��ʹ��Json������֧��script�ű�������⼸���﷨�����ĳ��������������Ϊscript�����������Ϊ���������µ�ValueֵΪ�ű����ݡ���ͽ�����script��ΪJson��ȱʡ���������ˣ�ʹ��ʱ���ײ�Ҫʹ��script��Ϊ��������

## 5.3 Script�ű�����

HTTP Script�ű�����ʾ�����£�

```
 script = {
     if ($query[fid]) "cache file" = $req_path_only$query[fid]$req_file_ext;
     else if ($req_file_only) "cache file" = ${req_path_only}index.html;
     else "cache file" = $req_path_only$req_file_only; 
 }

 cache file = <script> if ($query[fid]) return $req_path_only$query[fid]$req_file_ext;
                        else if ($req_file_only) return ${req_path_only}index.html;
                        else return $req_path_only$req_file_only; 
              </script>

 <script>
     if ($query[fid]) "cache file" = $req_path_only$query[fid]$req_file_ext;
     else if ($req_file_only) "cache file" = ${req_path_only}index.html;
     else "cache file" = $req_path_only$req_file_only; 
 </script>

 <script>
     if ($scheme == "http://") rewrite ^(.*)$  https://$host$1;
 </script>
```

HTTP Script�ű�����Ľ���ִ�У����ڴ���HTTPMsgʵ����������DocURI�󣬿�ʼִ����Դλ��ʵ�������̣���ʵ���������У��ֱ�ִ��HTTPListen��Script�ű���HTTPHost��Script�ű���HTTPLoc��Script�ű���

## 5.4 Script�ű����

script�ű�����һϵ����乹�ɵĳ����﷨������JavaScript��C��������Ҫ����������䣺

### 5.4.1 �������

���������Ҫ��if��else if��else��ɣ������﷨Ϊ��

```
  if (�ж�����) { ... } else if (�ж�����) { ... } else { ... }
```

�ж��������ٰ���һ������������ͨ����һ������������ֵ�����жϻ�Ƚϣ�ȡ�����ΪTRUE��FALSE��������ִ�з�֧���ж������������¼��������

-   (a) �ж�������ֻ����һ��������
-   (b) �ж������а���������������
-   (c) �ļ���Ŀ¼���Ե��жϣ�

�жϱȽϲ�����Ҫ������

-   (a) ����1 == ����2���ж��Ƿ���ȣ���������ֵ������ͬΪTRUE������ΪFALSE
-   (b) ����1 != ����2���жϲ���ȣ���������ֵ���ݲ���ͬΪTRUE������ΪFALSE
-   (c) ���������жϱ���ֵ�����������ˡ��ұ���ֵ��ΪNULL���ұ���ֵ��Ϊ0����ΪTRUE������ΪFALSE
-   (d) !������������ֵȡ���жϣ�����δ���壬�����ֵΪNULL�������ֵΪ0����ΪTRUE������ΪFALSE
-   (e) ����1 ^~ ����2������1�е���ʼ�������Ա���2��ͷ����ΪTRUE������ΪFALSE
-   (f) ����1 ~ ����2���ڱ���1�в��ұ���2�е����ִ�Сд������ʽ�����ƥ����ΪTRUE������ΪFALSE
-   (g) ����1 ~* ����2���ڱ���1�в��ұ���2�еĲ����ִ�Сд������ʽ�����ƥ����ΪTRUE������ΪFALSE
-   (h) -f ������ȡ����ֵ�ַ�����Ӧ���ļ����ڣ���ΪTRUE������ΪFALSE
-   (i) !-f ������ȡ����ֵ�ַ�����Ӧ���ļ������ڣ���ΪTRUE������ΪFALSE
-   (j) -d ������ȡ����ֵ�ַ�����Ӧ��Ŀ¼���ڣ���ΪTRUE������ΪFALSE
-   (k) !-d ������ȡ����ֵ�ַ�����Ӧ��Ŀ¼���ڣ���ΪTRUE������ΪFALSE
-   (l) -e ������ȡ����ֵ�ַ�����Ӧ���ļ���Ŀ¼�������ļ����ڣ���ΪTRUE������ΪFALSE
-   (m) !-e ������ȡ����ֵ�ַ�����Ӧ���ļ���Ŀ¼�������ļ������ڣ���ΪTRUE������ΪFALSE
-   (n) -x ������ȡ����ֵ�ַ�����Ӧ���ļ����ڲ��ҿ�ִ�У���ΪTRUE������ΪFALSE
-   (o) !-x ������ȡ����ֵ�ַ�����Ӧ���ļ������ڻ򲻿�ִ�У���ΪTRUE������ΪFALSE

### 5.4.2 ��ֵ���

��ֵ�����Ҫ��set��乹�ɣ�eJetϵͳ�оֲ������Ĵ����͸�ֵ��ͨ��set�������ɵġ����﷨���£�

```
  set $������  value;
```

### 5.4.3 �������

�������Ҳ����return��䣬��script�պϱ�ǩ��Ƕ���Scirpt�ű�����ִ�������Ľ������Key-Value����Value��Ƕ�Ľű����򣬽���ִ�к�Ľ�����ظ�Key�����������﷨Ϊ��

```
  return $������;
  return ����;
```

��ʹ����̬���£�

```
  cache file = <script> if ($user_agent ~* "MSIE") return $real_file; </script>;
```

### 5.4.4 ��Ӧ���

��Ӧ���Ҳ����reply��䣬ִ�и�����eJetϵͳ����ֹ��ǰHTTP����HTTPMsg���κδ���ֱ�ӷ���HTTP��Ӧ���ͻ��ˣ����﷨���£�

```
  reply  ״̬��  [ URL����Ӧ��Ϣ�� ];
```

������ص�״̬���� 444����ֱ�ӶϿ� TCP ���ӣ��������κ����ݸ��ͻ��ˡ�

����Replyָ��ʱ������ʹ�õ�״̬���У�204��400��402-406��408��410, 411, 413, 416 �� 500-504���������״̬��ֱ�ӷ��� URL ����Ϊ 302����ʹ����̬���£�

```
  if ($http_user_agent ~ curl) {
      reply 200 'COMMAND USER\n';
  }   
  if ($http_user_agent ~ Mozilla) {
      reply 302 http://www.baidu.com?$args;
  }      
  reply 404;
```

eJetϵͳ�ڽ���������ִ��Script����ʱ����ִ��Listen�µ�script�ű�����ִ��Host�µ�script�ű��������ִ��Location�µ�script�ű�����ִ����һ���ű�֮ǰ�����жϸո�ִ�е�script�ű��Ƿ��Ѿ�Reply�˻����Ѿ��رյ�ǰHTTPMsg�ˡ����Reply�˻�رյ�ǰ��Ϣ�ˣ���ֱ�ӷ��أ��������������ִ�к�����script�ű�����

### 5.4.5 rewrite���

eJetϵͳ�е�URL��д��ͨ��Script�ű���ʵ�ֵģ��ֱ�����Apache��Nginx�ĳɹ����顣

rewrite���ʵ��URL��д���ܣ����ͻ�HTTP���󵽴�Web Server������HTTPMsg�󣬷ֱ�����ִ��Listen��Host��Location�µ�script�ű�����rewrite���λ����Щscript�ű�����֮�У�rewrite����ı�����DocURL��һ���ı�����DocURL��������ִ������Щscript�ű�����֮�󣬼��������µ�DocURLȥƥ���µ�Host��Location������������ִ�и�Host��Location�µ�script�ű��������ѭ�����Ƿ����ѭ��ִ�У�ȡ����rewrite��flag��ǡ�

rewrite�����﷨���£�

```
  rewrite regex replacement [flag];
```

ִ�и����ʱ����regex��������ʽȥƥ��DocURI������ƥ�䵽��DocURI�滻���µ�DocURI��replacement��������ж��rewrite��䣬�����µ�DocURI������ִ����һ����䡣

flag��ǿ�������Nginx��Ƶ�4������⣬��������proxy��forward��ǡ����Ƕ������£�

-   (a) last
    -   ֹͣ����rewrite���ָ�ʹ���µ�URI����Locationƥ�䡣
-   (b) break
    -   ֹͣ����rewrite���ָ����ټ����µ�URI����Locationƥ�䣬ֱ��ʹ�õ�ǰURI����HTTP����
-   (c) redirect
    -   ʹ��replacement�е�URI����302�ض��򷵻ظ��ͻ��ˡ�
-   (d) permament
    -   ʹ��replacement�е�URI����301�ض��򷵻ظ��ͻ��ˡ�
-   (e) proxy | forward
    -   ʹ��replacement�е�URI����Origin����������Proxy�������󣬲���Origin���󷵻ص���Ӧ������ظ��ͻ��ˡ�

����reply��书�ܺ�ǿ��rewrite�е�redirect��permament����������ʵ�ֵĹ��ܣ���������reply��ʵ���ˣ������������ʵû�ж���Ҫ��

rewriteʹ�ð������£�

```
  addReqHeader  <header name>  <header value>;
```

�����ǿո��ַ�������ĸ��ͷ�����ĸ�����ֺ��»���_���ַ�����������˫����Ȧ�������� �������ַ��������������Ű����������ַ����пɰ���������

ʹ�ð������£�

```
if ($proxied) {
    addReqHeader X-Real-IP $remote_addr;
    addReqHeader X-Forwarded-For $remote_addr;
}
```

### 5.4.7 addResHeader���

������﷨Ϊ��

```
 addResHeader  <header name>  <header value>;
```

### 5.4.8 delReqHeader���

������﷨Ϊ��

```
 delReqHeader  <header name>;
```

### 5.4.9 delResHeader���

������﷨Ϊ��

```
  delResHeader  <header name>;
```

### 5.4.10 try_files ���

try_files ��һ����Ҫ��ָ�����λ��Location��Host���档ʹ�ø�ָ����β����б��е��ļ��Ƿ���ڣ����ھͽ�������DocURI���粻�����ڣ�������URI����ΪDocURI������ͻ��˷���״̬��code��

try_files�����﷨���£�

```
chunked body = chunk-size[; chunk-ext-nanme [= chunk-ext-value]]\r\n
               ...
               0\r\n
               [footer]
               \r\n
```

chunk size����16���Ʊ�ʾ�ĳ��ȣ�footerһ������\\r\\n��β��entity-header��һ�㶼���Ե���

eJetϵͳʹ��HTTPChunk���ݽṹ������chunk�ֿ鴫��������Ϣ�����ݣ�ʹ��chunk\_t���ݽṹ������ֿ鴫����롣HTTPChunk���ݽṹ����chunk\_t��Աʵ�������ڴ洢�����ɹ���Chunk���ݿ飬ÿһ��Chunk���ݿ����״̬����Ϣ��ChunkItem���洢����HTTPChunk����item_list��������ChunkItem��

����chunk�ֿ鴫��������Ϣ�壬ʵ�������һ�ߴ���һ�߽�������������Ҫ��ֿ��ǵ���ǰ���ջ��������ݵĲ������ԣ�������HTTPChunk���http\_chunk\_add_bufptr��ʵ�ֵģ������������£�

```c
int http_chunk_add_bufptr (void * vchk, void * vbgn, int len, int * rmlen);
```
vbgn��lenָ�����������Ϣ�����ݣ�rmlen�ǽ����ɹ���ʵ������chunk�ֿ鴫�������ֽ�������

eJet������chunk�ֿ鴫��������Ϣ��ʱ��ÿ���յ����¼����ͽ����ݶ�ȡ�������������������������ݽ�������������������������ص�rmlenֵ�Ǳ������ʹ�����ֽ�����������������ݴӻ������Ƴ�����ͨ��http\_chunk\_gotall���ж��Ƿ���յ�ȫ��chunk�ֿ鴫�������������ݣ����û�У�ѭ�������½��յ����ݵ��øú����������ʹ���ֱ���ɹ�������ϡ�

# ��. ���������������

## 9.1 �ж��Ƿ�Ϊ��������

��������ǽ���ͬ��Origin������������ͻ��ˣ��ͻ��˲����κδ������÷���������HTTP���󵽷��������������������������������õ�·�����򣬴�����ʲ�ͬ��Origin������������Ӧ������ظ��ͻ��ˣ��ÿͻ�����Ϊ��������������������ʵ�Origin��������

���������Ҫ��ͻ���������������������ַ����ȷ����Origin��������ַ��Ҫ���������������������Origin������ת���������Ӧ��

���������ķ����������������������eJet Web�����������˳䵱Web�����������⣬�����Գ䵱�������������ͷ�������������

eJetϵͳ��HTTPMsgʵ������ɺ�����Ҫ�����ǵ�ǰ�����Ƿ�ΪProxy��������:

-   �Ƿ���rewriteʱ����forward��һ���µ�Origin�������Ķ���������������ת�����µ�URL
-   �Ƿ�Ϊ��������������������ַrequest URI�Ǿ���URI������������ת��������URI��
-   �жϵ�ǰ��Դλ��HTTPLoc�Ƿ������˷�������Լ��������ָ���Origin������������ǣ����ݹ������ɷ���Origin��������URL��ַ

������������У���һ�ֺ͵�����Ϊ��������ڶ���Ϊ���������Ӧ�������������£�

```
location = { #rewrite ... forward
    type = server;
    path = ['/5g/', '^~' ];
    script = {
        rewrite ^/5g/.*tpl$ http://temple.ejetsrv.com/getres.php forword;
    }
}

# HTTP�������Ǿ���URI��ַ
GET http://cdn.ejetsrv.com/view/23C87F23D909B47E2187A0DB83AF07D3 HTTP/1.1
....

location = { # �����������
    path = [ '^/view/([0-9A-Fa-f]{32})$', '~*' ];
    type = proxy;
    passurl = http://cdn.ejetsrv.com/view/$1;
......
}
```

����������������Ƿ���������ת������Ĳ������̻������ƣ�������ȷָ����Origin��������URL��ַ����Ϊ��һ��ת����ַ������������Origin��������HTTPCon���ӣ���װ�µ�HTTPMsg���󣬷������󲢵Ⱥ���Ӧ������Ӧ���ת����ԴHTTPMsg�У����͸��ͻ��ˡ�

����Ǵ������󣬰����������������eJet��Ҫ��Proxy����ת������

## 9.2 ���������ʵʱת��

��Ҫ�ص���ܵ���ʵʱת��Դ����Origin�����������̡�����ת��ʱ�ȴ���һ������ת����HTTPMsgʵ������Դ����HTTPMsgʵ�����������ݸ��Ƶ���������HTTPMsg�У����HTTP������������Ϣ��ʱ������ת������������ʵ�ַ�ʽ��

-   һ�ַ�ʽ�Ǵ洢ת���������������е�HTTP������Ϣ����ٸ��Ƶ�����ת��HTTPMsg�У�����ͳ�ȥ
-   ��һ�ַ�ʽʵʱת����������һ������Ϣ��ͷ���һ������Ϣ�壬ֱ��ȫ���������

Ϊ��ȷ������ת��Ч�ʺͽ��ʹ洢���ģ�eJetϵͳ����ʵʱת��ģʽ��

Դ�������Ϣ�����ݱ�����HTTPCon��rcvstream�У���ӦIOE\_READ�¼�ʱ���������ݶ�ȡ���û������󣬾�Ҫ����http\_proxy\_srv\_send��ʵʱת����ת�������ݰ�����������ͷ���ϴ�δ���ͳɹ�����Ϣ�塢������λ��HTTPCon�������е�rcvstream���ϸ��ս��յ�˳�������͡�

ÿ��δ���ͳɹ�����Ϣ�壬�����HTTPCon��rcvstream�п���������ת�浽��������HTTPMsg�е�req\_body\_stream�У���Ϊ��ʱ�����������۴�δ�ܷ��͵���Ϣ�塣����ԴHTTPCon�н��յ������ݡ���Origin��������Ŀ��HTTPCon�п�д����ʱ����������http\_proxy\_srv\_send��ʵʱ�������̣������ȷ��͵���Ϣ����Ǵ���������req\_body_stream�е����ݡ�

Դ�������Ϣ�������������

-   û����Ϣ������
-   ������Content-Length����ʶ��С����Ϣ������
-   ������Transfer-Encoding��ʶ�ֿ鴫��������Ϣ������

ʵʱת����Ҫ�������������������ͨ��http\_con\_writev�����͸��Է������Ͳ��ɹ���ʣ�����ݣ���Ҫ��ԴHTTPCon�п�������������HTTPMsg�е�req\_body\_stream�С�

ʵʱת�����������ӵ�����⣬��ԴHTTPCon�ϵ��������ݷ����ٶȺܿ죬����Origin��������Ŀ��HTTPCon���ӵķ����ٶȱȽ��������´��������ݻ�ѻ���������ϢHTTPMsg��req\_body\_stream�У����Ĵ����ڴ棬����ʱ�ᵼ���ڴ����Ĺ���ϵͳ������

������Ϣʵʱת��ģʽ��ӵ�������Դ����������·�����ٶȲ��Եȵ��£�ֻҪ���Ͳ��ٶȴ��ڽ��ղ��ٶȣ�ӵ������ͻ���֡����ӵ���������Դͷ�����ǣ��ж��Ƿ�ӵ���ı�׼�Ƕѻ����ڴ滺��������һ������ֵ��һ���ڴ�ѻ�������ֵ���Ͷ϶�Ϊӵ���������ƿͻ��˼��������κ����ݣ�ֱ�����ӵ����������͡�

## 9.3 ������Ӧ��ʵʱת��

��������ת����Origin�������󣬻᷵����Ӧ��Ϣ��������Ӧͷ����Ӧ�壬eJet������Ӧͷ�Ľ��պʹ�����롣

��HTTP������Ϣ��ʵʱת�����ƣ�������Ϣ����ӦҲ��Ҫʵʱת�����ͻ��ˡ�

���ݴ���HTTPMsg�ڲ���Աproxiedl���жϵ�ǰ��Ϣ�Ƿ�Ϊ������Origin���ص���Ӧͷ��Ϣ����Ԥ����

-   �����301/302��ת����ǰ������Ϣ�Ƿ����������ϵͳ�����Զ��ض����������·����ض�������
-   �����Ҫ���浽���ش洢ϵͳ�����û��洦�����̣���4.20�½�
-   �������ξͰ��մ�����Ӧ������
    

�������е���Ӧ״̬�����Ӧͷ��ԴHTTPMsg�У�������ӦHTTPCon�Ľ��ջ�����rcvstream����ʵʱת����ԴHTTPCon�У�ͬ���أ�HTTPCon��û�з��Ͳ��ɹ������ݣ�ת�浽ԴHTTPMsg�е�res\_body\_stream����ʱ����������ÿ�ε�ԴHTTPCon��д�����������HTTPCon�����ݿɶ�����ȡ�ɹ��󣬶������http\_proxy\_cli\_send�����ȷ��͵��Ƕѻ���res\_body_stream�е����ݡ�

����������������������Ϣ��ʵʱת����

# ʮ. FastCGI���ƺ�����PHP������

## 10.1 FastCGI������Ϣ

FastCGI��CGI��Common Gateway Interface���Ŀ���ʽ��չ�淶���似���淶����ַ [http://www.mit.edu/~yandros/doc/specs/fcgi-spec.html](http://www.mit.edu/~yandros/doc/specs/fcgi-spec.html)

�Ծ�̬HTMLҳ����Ƕ�붯̬�ű���������ݣ���PHP��ASP�ȣ���Ҫ���ض��Ľű����������������У�����̬�����µ�ҳ�棬���������ҪeJet Web�������ͽű����������֮����һ�����ݽ����ӿڣ�����ӿھ���CGI�ӿڣ����ǵ����ܾ��ޣ����ڵĶ�������ģʽ��CGI�ӿڷ�չ��FastCGI�ӿڹ淶��ϰ�ߵأ����ǰѽ�������֮ΪCGI��������

ʹ��CGI�ӿڹ淶��ҳ��ű��������ʹ���κ�֧�ֱ�׼����STDIN����׼���STDOUT�����������ı����������д����PHP��Perl��Python��TCL�ȡ��ڴ�ͳCGI�淶��fork-and-executeģʽ�У�Web��������Ϊÿ��HTTP���󣬴���һ���½��̡�����ִ�С�������Ӧ�����ٽ��̣����Ǹ����صĹ������̡�

FastCGI��CGI������ģʽ�����˼򻯣��ű���������Web������֮��Ľ�����ͨ��Unix Socket��TCPЭ����ʵ�֣�Web�������յ���Ҫ����ִ�е�HTTP����ʱ��������ά��ͨ�����ӵ�CGI������������FastCGIͨ�Ź淶�������󣬲�������Ӧ������������CGIģʽ��������������ܺͲ�������������

PHP����������Ϊphp-fpm��php FastCGI Processor Manager������ΪFastCGIͨ�ŷ�������������Web���������������󣬲����������ϵ����ݣ����н���������ִ�к󣬷�����Ӧ��Web�������ˡ�php-fpm���������У�������������

```
; The address on which to accept FastCGI requests.
; Valid syntaxes are:
;   'ip.add.re.ss:port'    - to listen on a TCP socket to a specific IPv4 address on
;                            a specific port;
;   '[ip:6:addr:ess]:port' - to listen on a TCP socket to a specific IPv6 address on
;                            a specific port;
;   'port'                 - to listen on a TCP socket to all addresses
;                            (IPv6 and IPv4-mapped) on a specific port;
;   '/path/to/unix/socket' - to listen on a unix socket.
; Note: This value is mandatory.
listen = /run/php-fpm/www.sock
;listen = 9000
```

## 10.2 eJet�������FastCGI

eJet�յ��ͻ��˵�HTTP���󲢴���HTTPMsg�����HTTPMsgʵ�����󣬸�����Դλ��HTTPLoc�Ƿ���Դ��������ΪFastCGI������������ָ��CGI��������ַ��passurl�������������������������ǰ����ᱻ����FastCGI����ת����CGI��������

����FastCGI�Ĳ����������£�

```
location = {
    type = fastcgi;
    path = [ "\.(php|php?)$", '~*'];

    passurl = fastcgi://127.0.0.1:9000;
    #passurl = unix:/run/php-fpm/www.sock;

    index = [ index.php ];
    root = /data/wwwroot/php;
}
```

ֻҪ������DocURL��·����������.php��.php5�Ƚ�β����ǰ���󶼻ᱻFastCGIת����

�ڻ�ȡת��URL��ַʱ���Ǹ��������е�passurl��ַ����CGI��������ַ�����ܰ�HTTP�����е�·����query������Ϣ��������ת��URL���档ת����ַ��������̬��

-   ����TCPЭ���CGI��������ַ����fastcgi://��ͷ�����IP��ַ�Ͷ˿ڣ��������Ͷ˿ڣ�
-   ����Unix Socket��CGI��������ַ����unix:��ͷ�����Unix Socket��·���ļ�����
    

passurl��ַָ��CGI��������eJet����������֧�ֺܶ��CGI��������

eJet��ȡ��FastCGIת����ַ�󣬸��ݸõ�ַ�������CGI������FcgiSrv����ʵ��������TCP���ӻ�Unix Socket���ӵ��÷�������FcgiConʵ����Ϊ��ǰHTTP���󴴽�FcgiMsg��Ϣʵ������HTTP������Ϣ����FastCGI�淶��װ��FcgiMsg�У��������������̣��������͵�CGI��������

## 10.3 FastCGI��ͨ�Ź淶

FastCGIͨ��������C/Sģʽ�Ŀɿ�����ʽ�����ӣ�Э�鶨����ʮ��ͨ��PDU��Protocol Data Unit�����ͣ�ÿ��PDU������������ɣ�һ������FastCGI Headerͷ������һ������FastCGI��Ϣ�壬FastCGI��PDU���ϸ�8�ֽڶ��룬PDU�ܳ��Ȳ���8�ı�������Ҫ���Padding����8�ֽڶ��롣FastCGI��PDUͷ��ʽ���£�

```c
typedef struct fastcgi_header {
    uint8           version;
    uint8           type;
    uint16          reqid;
    uint16          contlen;
    uint8           padding;
    uint8           reserved;
} FcgiHeader, fcgi_header_t;
```

���涨���Э��ͷ��ʽ�У�version�汾��1���ֽڣ�ȱʡֵΪ1��typeΪPDU����1���ֽڣ����ƶ�����10�����ͣ�reqidΪPDU����ţ����ֽ�BigEndian������contlen��PDU��Ϣ������ݳ��ȣ����ֽ�BigEndian������1�ֽڵ�padding��PDU��Ϣ�岻��8�ֽڵı���ʱ����Ҫ����8�ֽڶ����������ֽ���������1�ֽڡ�

����PDU���͹���ʮ�֣��ֱ������£�

```c
/* Values for type component of FCGI_Header */
#define FCGI_BEGIN_REQUEST       1
#define FCGI_ABORT_REQUEST       2
#define FCGI_END_REQUEST         3
#define FCGI_PARAMS              4
#define FCGI_STDIN               5
#define FCGI_STDOUT              6
#define FCGI_STDERR              7
#define FCGI_DATA                8
#define FCGI_GET_VALUES          9
#define FCGI_GET_VALUES_RESULT  10
#define FCGI_UNKNOWN_TYPE       11
#define FCGI_MAXTYPE            (FCGI_UNKNOWN_TYPE)
```

���д�Web���������͸�CGI��������PDU����Ϊ��BEGIN\_REQUEST��ABORT\_REQUEST��PARAMS��STDIN��GET\_VALUES�ȣ���CGI���������ظ�Web��������PDU����Ϊ��END\_REQUEST��STDOUT��STDERR��GET\_VALUES\_RESULT�ȡ�

����PDU typeֵ��PDU��Ϣ���ʽҲ����һ�����ֱ���Ϊ��

```c
typedef struct {
    uint8  roleB1;
    uint8  roleB0;
    uint8  flags;
    uint8  reserved[5];
} FCGI_BeginRequest;

/* Values for role component of FCGI_BeginRequest */
#define FCGI_RESPONDER           1
#define FCGI_AUTHORIZER          2
#define FCGI_FILTER              3
```

BEGIN_REQUEST�Ƿ������ݵ�CGI������ʱ����һ�����뷢�͵�PDU�����еĽ�ɫrole�������ֽ���ɣ���λ��ǰ����λ�ں�һ�����roleֵΪRESPONSER����Ҫ��CGI�������䵱Responder�����������PARAMS��STDIN�������ݡ��ֶ�flags��ָ��ǰ����keep-alive���Ƿ������ݺ������رա�

�ڶ����跢�͵�CGI��������PDU��PARAMS�����ʽ����FcgiHeader���ϴ��г��ȵ�name/value����ɣ�PDU��Ϣ���ʽ���£�

```c
typedef struct {
    uint8    namelen;    //namelen < 0x80
    uint32   lnamelen;   //namelen >= 0x80
    uint8    valuelen;   //valuelen < 0x80
    uint32   lvaluelen;  //valuelen >= 0x80
    uint8  * name;       //[namelen];
    uint8  * value;      //[valuelen];
} FCGI_PARAMS;
```

FastCGI�е�PARAMS PDU�ǽ�HTTP����ͷ��Ϣ��Ԥ�����Key-Valueͷ��Ϣ���͸�CGI����������Щ��Ϣ����Key-Value��ֵ�ԡ����key��value�����ݳ�����128�ֽ����ڣ��䳤���ֶ�ֻ��һ���ֽڼ��ɣ�������ڻ����128�ֽڣ����䳤���ֶα�����BigEndian��ʽ��4�ֽڡ��ڶ�HTTP����ͷ��Ԥ����ͷKey-Value����Ϣ��װ�����PARAMS PDUʱ��ÿ��Header�ֶεı����ʽΪ������Header��name���ȣ�����value���ȣ������name���ȵ�name�������ݣ������value���ȵ�value�������ݡ�

```
1�ֽ�namelen��4�ֽ�namelen + 1�ֽ�valuelen��4�ֽ�valuelen + name + value
```

����ͷ��Ϣ�������������ʽ�����ɺ��ܳ����������8�ı����������費ȫ8�ֽڶ����padding����������Щ������䵽FcgiHeader�С�

������Ҫ���͵�CGI��������PDU��STDIN��STDIN PDU����FcgiHeader����ʵ��������ɡ�ע�����STDIN���ݳ��Ȳ��ܴ���65535�����HTTP��������Ϣ�����ݴ���65535����Ҫ����Ϣ���ֳɶ��STDIN����ʹ��ÿ��STDIN PDU����Ϣ�峤�ȶ���65536�ֽ����¡���Ҫ�ر�ע����ǣ������������ݲ�ֳɶ��STDIN PDU��ɺ������Ҫ���һ����Ϣ�峤��Ϊ0��STDIN PDU����ʾ���е�STDIN���ݷ�����ϡ�

��eJetϵͳ�յ�HTTP������ҪFastCGIת���ǣ����������������ݰ�Э���ʽ����HTTP��������װ�������ͳɹ��󣬾͵ȵ�CGI�������Ĵ������Ӧ�ˡ�

CGI���������ص�PDUһ�����£�

������������ʽ������������󣬻᷵��STDERR���ݣ�����Ϣ���Ǵ������ݣ�����������ȡ��������ֱ�ӷ��ظ��ͻ��ˡ�

��������£�CGI�������᷵��һ�������STDOUT PDU��STDOUT����Ϣ����ʵ�ʵ��������ݣ���󳤶�С��65536����Ҫ����ЩSTDOUT������������һ����ΪHTTP��Ӧ���ݡ���ע�����STDOUT�����У�Ҳ��������HTTP��Ӧͷ��Ϣ�����ʽ��ѭHTTP�淶��ÿ����Ӧͷ��key-value�Թ��ɣ���\\r\\n���з���������Ӧͷ����Ӧ��֮�����һ������\\r\\n��

ȫ��STDOUT���ݽ����󣬽����ŷ��ص���END_REQUEST PDU�����ʽ��8�ֽڵ�FcgiHeader������8�ֽڵ���Ϣ�壬����Ϣ�嶨�����£�

```c
typedef struct {
    uint32     app_status;
    uint8      protocol_status;
    uint8      reserved[3];
} FCGI_EndRequest;

/* Values for protocolStatus component of FCGI_EndRequest */
#define FCGI_REQUEST_COMPLETE    0
#define FCGI_CANT_MPX_CONN       1
#define FCGI_OVERLOADED          2
#define FCGI_UNKNOWN_ROLE        3
```

eJet�������յ�END_REQUESTʱ���ͱ�ʾCGI�������Ѿ�����ȫ������Ӧ�����ˣ�����Щ���ݷ��͸��ͻ��ˣ����ɽ�����ǰ����

## 10.4 FastCGI��Ϣ��ʵʱת��

eJetϵͳ��HTTP����ʵʱת����CGI���������������̸�Proxy����ת�����ƣ�����ʵʱת��������ӵ�����Ƶȡ�

�����ڽ���CGI����������Ӧ����ʱ����Ҫ��������ʽ���ص�STDOUT PDU�����ݣ�����Ӧ���ݵ��ܳ��Ȳ�δ���أ�eJet����Щ��Ӧ���ݵ�ʵʱת���ǲ���Transfer-Encoding�ֿ鴫�����ģʽ��Ϊ�˼�����Ӧ���ݵĶ�ο�����FcgiCon��ÿ�����ݶ�����ʱ������rcvstream�����������ݣ���ͬrcvstreamһ�����뵽����HTTP�����ԴHTTPMsg�ڵ�res\_rcvs\_list�б��У����������ɹ�������ָ����뵽res\_body\_chunk����ƿͻ��˷��ʱ����ļ�һ����ͨ��http\_cli\_send���͸��ͻ��ˡ�

# ʮһ. HTTP Cacheϵͳ

## 11.1 HTTP Cache��������

HTTP Cache��ָWeb�������䵱HTTP Proxy����������������������ͷ��������ͨ��HTTPЭ����Origin�����������ļ���Ȼ��ת�����ͻ��ˣ���Щ�ļ���ת�����ͻ��˵�ͬʱ�������ڴ���������ı��ش洢�У��´�������ͬ����ʱ��������ػ�����Ծ��������ļ��Ƿ����У�������У��������������Origin�������������أ�ֱ�ӽ����������е��ļ���ȡ�������ظ��ͻ��ˣ��Ӷ���ʡ���翪����

�������ļ��������������������ĵط��������Կ���cache���ܣ����������ýű���̬���û����ļ����Ȼ���ѡ�

```
location = {
    path = [ '^/view/([0-9A-Fa-f]{32})$', '~*' ];
    type = proxy;
    passurl = http://cdn.yunzhai.cn/view/$1;

    # ����������û���ѡ��
    root = /opt/cache/;
    cache = on;
    cache file = /opt/cache/${request_header[host]}/view/$1;
}

send request = {
    max header size = 32K;

    /* ����������õĻ���ѡ�� */
    root = /opt/cache/fwpxy;
    cache = on;
    cache file = <script>
                   if ($req_file_only)
                       return "${host_name}_${server_port}${req_path_only}${req_file_only}";
                   else if ($index)
                       return "${host_name}_${server_port}${req_path_only}${index}";
                   else
                       return "${host_name}_${server_port}${req_path_only}index.html";
                 </script>;
}
```

�������������˻��湦�ܺ󣬻�Ҫ����Origin���������ص���Ӧͷָ���Ļ�����ԣ���������ǰ�����ļ��Ƿ񱣴��ڱ��ء������ļ�����೤ʱ��ȡ�HTTP��Ӧͷ���м���ͷ�Ǹ��𻺴���Եģ�

```
Expires: Wed, 21 Oct 2020 07:28:00 GMT            (Response Header)
Cache-Control: max-age=73202                      (Response Header)
Cache-Control: public, max-age=73202              (Response Header)

Last-Modified: Mon, 18 Dec 2019 12:35:00 GMT      (Response Header)
If-Modified-Since: Fri, 05 Jul 2019 02:14:23 GMT  (Request Header)
 
ETag: 627Af087-27C8-32A9E7B10F                    (Response Header)
If-None-Match: 627Af087-27C8-32A9E7B10F           (Request Header)
```

Proxy�����������Ҫ����Origin���������ص���Ӧͷ����Ҫ��Expires��Cache-Control��Last-Modified��ETag�ȡ�����Cache-Control�Ļ�����Ծ�����ǰ�ļ��Ƿ񻺴棺�����no-cache��no-store�������趨��max-age=0�������趨��must-revalidate�ȶ����ܽ���ǰ�ļ����浽�����ļ��С����������max-age����0�����max-ageֵ��Expiresֵ��Last-Modifiedֵ��ETagֵ���ж��´������Ƿ�ʹ�øû����ļ���

## 11.2 eJetϵͳCache�洢�ܹ�

eJetϵͳ�Ƿ�����������������Ϣ���趨������Ƿ������HTTP�����Ӧ��HTTPLoc�µķ��������cache�Ƿ�������cache=on��cache file���Ƿ����ã��������Ƿ��������湦�ܣ���������������send requestѡ���У��Ƿ�����cache���Լ�cache file���������Ƿ����ã������Ƿ������������

������cache���ܣ�����Ҫ���ݵ�ǰ����ת����Origin�󣬷��ص���Ӧͷ�У��Ƿ���Cache�����ͷ��Ϣ����ȷ����ǰ���ص���Ӧ���Ƿ񻺴棬�Լ�ȷ����ǰ����������Ϣ��

�����Raw�ļ����ݴ洢��������������cache file�������ļ��У����ļ���������ȫ�����ز��洢����ǰ���ļ�������Ҫ������չ��.tmp���Ա�ʾ��ǰ�洢�ļ����������У�������һ���������ļ������Ѿ��������������Ա�����ʹ�á�

cache������Ϣ��洢�ڻ�����Ϣ�����ļ���Cache Information Management File���У����ΪCacheInfo�ļ���CacheInfo�ļ��Ĵ洢λ����Raw�����ļ�����Ŀ¼�½���һ������Ŀ¼.cacheinfo��CacheInfo�ļ��ʹ�Ÿ�����Ŀ¼�£�CacheInfo�ļ�������Raw�洢�ļ������Ӻ�׺.cacinf��Ʃ��Raw�����ļ�Ϊfoo.jpg���򻺴���Ϣ�����ļ�·��Ϊ�� .cacheinfo/foo.jpg.cacinf

CacheInfo�ļ��Ľṹ���������֣�Cacheͷ��Ϣ��96�ֽڣ���Raw�洢��Ƭ������Ϣ��Cacheͷ��Ϣ�ǹ̶���96�ֽڣ���ṹ���£�

```c
/* 96 bytes header of cache information file */
typedef struct cache_info_s {
    char         * cache_file;
    void         * hcache;
    char         * info_file;
    void         * hinfo;

    uint8          initialized;
 
    uint32         mimeid;
    uint8          body_flag;
    int            header_length;
    int64          body_length;
    int64          body_rcvlen;
 
    /* Cache-Control: max-age=0, private, must-revalidate
       Cache-Control: max-age=7200, public
       Cache-Control: no-cache */
    uint8          directive;     //0-max-age  1-no cache  2-no store
    uint8          revalidate;    //0-none  1-must-revalidate
    uint8          pubattr;       //0-unknonw  1-public  2-private(only browser cache)
 
    time_t         ctime;
    time_t         expire;
    int            maxage;
    time_t         mtime;
    char           etag[36];
 
    FragPack     * frag;
 
} CacheInfo;
```

��ͷ��Ϣ֮���ŵ��Ǵ洢������Ƭ������Ϣ��ÿ����Ƭ��ԪΪ8�ֽڣ�

```c
typedef struct frag_pack {
    int64    offset;
    int64    length;
} FragPack;
```

�ڴ��в��ö�̬��������������ÿһ����Ƭ�飬���ڿ����Ҫ�ϲ���һ���飬�����ļ�ֻ��һ���顣����Щ��Ƭ����Ϣ����8�ֽ�˳��洢����������С�ÿ���ļ���������д��ʱ���ڴ���Ƭ������Ҫ��ɺϲ��ȸ��£��������½�����µ����������Ƭ����Ϣ�������Raw�洢�ļ��д�Origin���������ز�ʵ�ʴ洢�����ݴ洢״̬��ÿ������ƫ�����ͳ�����Ψһ��ʶ�����ڵ���Ƭ��ϲ��������ļ�ֻ��һ����Ƭ�顣

## 11.3 eJetϵͳ���洦������

eJetϵͳ��Ϊ��������������������ʵ�ֱ����ر߻��桢��������ʱ�������ת��ֱ�ӷ��ػ������ݸ��ͻ��˵ȹ��ܣ�����ʵ�ֶԴ��СС��Origin�ļ���ʵʱ���湦�ܣ�������Ƭ�洢������洢�ȡ�

### 11.3.1 ȫ�ֹ���CacheInfo����

ϵͳά��һ��ȫ�ֵ�CacheInfo�����ϣ����Raw�����ļ�����ΪΨһ��ʶ��������������ڶ���û�����ͬһ����Ҫ�����Origin�ļ�ʱ��ֻ�򿪻򴴽�һ��CacheInfo���󣬸ö����Ա�ɻ���������������ÿ����ͬһOrigin�ļ���HTTP��������λ�á�ƫ��������дRaw�����ļ��ľ���ȶ������ڸ��Ե�HTTPMsgʵ�������С�

CacheInfo�����ǹ���ʹ��Raw�����ļ��ĸ���Ԫ��Ϣ�����Ⱪ¶����Ҫ�ӿ��ǣ�`cache_info_open, cache_info_create, cache_info_close, cache_info_add_frag��`

�û�����Origin�ļ�����ʱ���ȵ���cache\_info\_open��CacheInfo������������ڣ������յ�Origin�ĳɹ���Ӧ�󣬵���cache\_info\_create����CacheInfo����ÿ�ε���cache\_info\_openʱ�����CacheInfo�����Ѿ����ڴ��У���count������1��ֻ��count����Ϊ0ʱ�ſ���ɾ���ͷ�CacheInfo���󡣵�HTTPMsg�ɹ����ظ��û�����Ҫ�ر�CacheInfo���󣬵���cache\_info\_close�����Ƚ�count������1�����count����0��ֱ�ӷ��ز�����Դ�ͷš�

### 11.3.2 ��Origin������ת��Proxy��������

eJet�յ�HTTP�ͻ�����ʱ�������Proxy���������http\_proxy\_cache\_open��Ⲣ�򿪻��棬�ȸ�������URL��Ӧ��HTTPLoc������Ϣ����������Ӧ��send request������Ϣ��������ǰ����ģʽ�µ�HTTP�����Ƿ�������Cache���ܣ����������Cache���ܣ�����Cache File������������ȷ��Raw�����ļ��������û����ļ���������HTTPMsg�����res\_file_name�С�

���û����ļ��Ƿ���ڣ����������ֱ�ӽ��û����ļ����ظ��ͻ��˼��ɡ�ע����û���յ�ȫ���ֽ�����֮ǰRaw�����ļ�����ʵ�ʻ����ļ����.tmp����չ����

������ļ������ڣ��Ըû����ļ���Ϊ����������cache\_info\_open��CacheInfo������������ڻ�����Ϣ����CacheInfo���򷵻ز�ֱ�ӽ��ͻ�������ת����Origin��������

�������CacheInfo����Ҳ���Ǵ�����.tmpΪ��չ����Raw�����ļ�����.cacinfΪ��չ���Ļ�����Ϣ�ļ������жϵ�ǰ��������ݣ�Range�淶ָ�������������Ƿ�ȫ��������Raw�����ļ��У���������ˣ���ֱ�ӽ��ò������ݷ��ظ��ͻ��ˣ�������Origin����������HTTP�����������������������Ҫ��Origin�������������󣬵����ػ������Ѿ��е����ݲ����������󣬶��ǽ��ͻ������������Range�淶ָ���ķ�Χ������δ���浽���ص���ʼλ�úͳ��ȼ������������µ�Range�淶����Origin����HTTP����

### 11.3.3 ����Origin���������ص���Ӧͷ

��HTTP����ת����Origin��������������Ӧ����������ǽ�Proxy��������HTTPMsg�����е���Ӧͷȫ������һ�ݵ�Դ����HTTPMsg����Ӧͷ�У�����״̬��Ҳ���ƹ�ȥ��

������������Cache=on����CacheInfoҲ�Ѿ��򿪵����������Ҫ����Դ����HTTPMsg����Ӧͷ��������http\_cache\_response_header����ɣ�ɾ��������Ҫ����Ӧͷ������HTTP��Ӧ������ݴ����ʽ����ѡ��Content-Length��ʽ����Transfer-Encoding: chunked��ʽ������״̬���޸ĳ�206����200���޸�Content-Range��ֵ���ݣ���ΪԴ�����Range����Origin�����������Proxy���������Range��һ����һ�µġ�������CacheInfo��Ϣ�����Ƿ�����Expires��Cache-Control����Ӧͷ���ȵ�

��󣬶�Origin���������ص�HTTP��Ӧͷ���н���������http\_proxy\_cache_parse����ɣ��ֱ����Expires��ETag��Last-Modified��Cache-Control����Ӧͷ��������Щ��Ӧͷ��Ϣ���ٴ��жϵ�ǰ��Ӧ�����Ƿ���Ҫ����Cache=on��

�������Ҫ���棺��Cache����Ϊoff�����ر��Ѿ��򿪵�CacheInfo������ɾ����CacheInfo�ļ���Raw�����ļ���������Ҫ���Ǽ��Դ�����Range��Χ��Proxy���������Range��Χ�Ƿ�һ�£������һ�£�����Ҫ���½�ԴHTTP����ԭ���ٷ���һ�Σ��������ǰProxy���������������Ϣ�����ڽ�ԴHTTP����HTTPMsg��Cache����Ϊoff�ˣ��������·��͵�Proxy�������󽫲����û��湦�ܣ�ֱ��ʹ��ʵʱת��ģʽ��������������Rangeһ�£���ֱ�ӽ���ǰ�����������Ӧ�����ݲ���ʵʱת��ģʽ�����͸��ͻ��ˡ�

�����Ҫ���棺��������Ӧͷ�е�Content-Range�е���Ϣ�����֮ǰ��cache\_info\_open��CacheInfo����ʧ�ܣ����ʱ�����cache\_info\_create������CacheInfo�����������ʧ�ܣ��ڴ治����Ŀ¼�����ڵȣ���رջ��湦�ܣ���ʵʱת��ģʽ������Ӧ�������ȡ�˴���Ӧ����Ϣ�������浽CacheInfo�����У��򿪻򴴽�Raw�����ļ�������Ҫ�ļ����ǣ��򿪻򴴽���Raw�����ļ���������Դ�����HTTPMsg�У��������ļ�seekд��λ��Range��Content-Rangeͷ��ָ����ƫ��λ���ϣ��ڴ�λ���ϴ��Proxy���������е���Ӧ�塣��󣬽�CacheInfo�������������д�뵽������Ϣ�ļ��С�

### 11.3.4 �洢Origin���������ص���Ӧ��

�κο�����Cache���ܵ�HTTP����ֻҪ��������ݲ��ڱ��ػ����У�����Ҫ��Origin��������Proxyģʽת��HTTP�����ڴ���������������Ӧͷ����Ҫ����Ӧ��洢��Raw�����ļ��ʵ�λ�ã����洢λ����Ϣ���µ�������Ϣ�ļ��У���������ͻ��˷�����Ӧ��

�洢Proxy�����������Ӧ���ǵ���http\_proxy\_srv\_cache\_store��ʵ�ֵģ�����֤��ǰԴHTTPMsg�Ƿ�Ϊpipeline�����������Ϣ���Ƿ�Cache=on�ȡ�����������HTTPcon���ջ������е�������ΪҪ�洢����Ӧ�����ݣ����м򵥽����жϣ�

- ��a�������Ӧ����Content-Length��ʽ�����㻹ʣ���������û�յ������ԱȽ��ջ��������ݡ����ʣ������Ϊ0�����Ѿ�ȫ���յ�����������ݣ��رյ�ǰHTTP������Ϣ������res\_body\_chunk����Ϊ������������кܶ�ʣ������û�յ����򽫽��ջ�����д�뵽.tmp��Raw�����ļ��У�д�ļ������ԴHTTPMsg�����У���д��ɹ����ݿ���ļ�λ�úͳ�����Ϣ��׷�ӵ�CacheInfo�����У������µ�������Ϣ�ļ������������HTTPCon���������Ѿ�д��Raw�����ļ�������ɾ������������жϣ��ղŴӻ�����׷��д�뵽�ļ��������Ƿ�ȫ�������ˣ���������ˣ��رյ�ǰHTTP������Ϣ��

- ��b�������Ӧ����Transfer-Encoding: chunked��ʽ�����ָ�ʽ����֪����Ӧ���ܳ����Ƕ��٣�Ҳ��֪��ʣ�໹�ж������ݣ����ص���Ӧ������һ��һ�����ݿ���뷽ʽ��ÿ�����ݿ�ǰ���ǵ�ǰ���ݿ鳤�ȣ�16���ƣ�����\\r\\n��ÿ�����ݿ��βҲ����\\r\\nΪ��β��ֻ���յ�һ������Ϊ0�����ݿ飬��֪��ȫ����Ӧ���Ѿ������������ˡ��������紫��ĸ����ԣ�ÿ�ν�������ʱ������һ��������������һ�����������ݿ飬������Ҫ�����ջ����������ݽ���http_chunkģ���жϣ��Ƿ�Ϊ�����顢�Ƿ��յ���β��ȡ�

������ջ���������ǰ�����ж��Ƿ�������ȫ����Ӧ�壬��������ˣ�����res\_body\_chunk����״̬���رյ�ǰ������Ϣ�������ջ�����������������ӵ�http_chunk�н����жϣ��ó���������������Щ�ǽ��������ݿ飬�Ƿ�����ȣ������ջ���������Щ�������ݿ鲿��д�뵽.tmp��Raw�����ļ��У�����д�ļ���������ԴHTTPMsg�����У������ܳ��ȣ�ɾ�����ջ��������Ѿ�д������ݣ�����д��ɹ������ݿ���ļ�λ�úͳ�����Ϣ��׷�ӵ�CacheInfo�����У������µ�������Ϣ�ļ������жϣ����ȫ�����ݿ鶼������ȫ�ˣ��رյ�ǰHTTP������Ϣ���رյ�ǰHTTP������Ϣ��ͬʱ��ʽ���㲢ȷ����ǰ�������������ݣ�����ʵ�ʵ��ļ����ȡ�

- ��c������������ͻ����ļ����ݵ��ͻ��ˡ�

### 11.3.5 ��ԴHTTPMsg�Ŀͻ��˷�����Ӧ

���͵���Ӧ������Ӧͷ��λ�ڻ����ļ��е���Ӧ�壬����http\_proxy\_cli\_cache\_send������

ͨ��HTTP�ĳ���Э��TCP����������ǰ����Ҫ�������������͵��������ݣ�һ������£������͵��������ݰ������������ݡ��ļ����ݣ������ļ����ݡ������ļ����ݵȣ���δ֪����Ҫ������������ݵȵȣ���Щ���ݵ��ܳ����п���֪����Ҳ���ܲ�֪������Щ����������һ������£���λ�ڲ�ͬ�洢λ�ã�Ʃ�����ڴ��С�Ӳ���ϡ�������ȣ����ص��Ƿֲ�ʽ�ġ��������ġ���Ƭ���ġ��������ݳ��ȷǳ��󣨴��ڴ涼������ȫ�����ɵļ����������������Щ�������ġ���Ƭ�������������ͷ���ݣ��������ݽṹchunk_t��ʵ�ֵġ�

chunk\_t���ݽṹ�ṩ�˸��๦�ܽӿڣ�������Ӹ������ݣ��ڴ�顢�ļ������ļ����������ļ�ָ��ȣ�����������ͳһ����������ȷ��ʽӿڣ�����Ҫ�Ĺ����Ǹ����ݽṹ����˲�ͬ�������������һ��ģ���Ϊ��һ���󻺳����������������ݶ�д���������ľ޶����ܿ��������������ڴ����ġ�ʹ�ø����ݽṹ��ֻ�轫Ҫ���͵ĸ����������ݣ�ͨ��chunk\_t�ĸ�������׷�ӽӿڣ���ӵ������ݽṹ��ʵ�������У����ͨ��tcp\_writev��tcp\_sendfile��ʵ�����ݸ�Ч�����١��㿽����ʽ�Ĵ��䷢�͡�

���������߼�����ͻ��˷������ݵ���Ҫ��������ν�������������ӵ�ԴHTTPMsg�е�res\_body\_chunk�У�

- ��a�����ȼ����res\_body\_chunk���ۼƴ�ŵ���Ӧ�������ܳ��ȣ�����ԴHTTP�����ļ�����ʼλ�ã������Rangeȡ����ʼλ�ã����û��Range��ȱʡΪ0�����õ���ǰҪ׷�ӷ��͸��ͻ��˵������ڻ����ļ��е�λ��ƫ�������ֱ���������Ӧ������ʽ�Ĵ��������

- ��b�������Ӧ����ͨ��Content-Length����ʶ��

����HTTP��Ϣ��Ӧ�ܳ��ȼ�ȥchunk�е���Ӧ���ܳ��ȣ��ͼ����ʣ����д���ӵ����ݳ��ȡ�ͨ��CacheInfo����Ƭ���ݹ���ӿڣ���ѯ����ǰRaw�����ļ��У���(a)�м�����Ļ����ļ�ƫ����λ�ã�������õ����ݳ����ж��١�

���Raw�����ļ��д��ڿ������ݣ��Ա�ʣ�����ݳ��ȣ���ȡ���ಿ�֡�����Raw�����ļ������ļ�ƫ��λ�á���ȡ������Ŀ������ݳ��ȵ���Ϊ����������chunk������ݽӿڣ���ӵ�res\_body\_chunk�У������chunk��֮ǰ�洢��δ���ͳ�ȥ�������ǽ����ģ��ϲ����������ӵ�chunk�е������ܳ��ȴﵽ�򳬹�Դ����HTTPMsg��Ϣ����Ӧ�ܳ��ȣ���res\_body\_chunk���ý���״̬������TCP�������̡�

���Raw�����ļ��в����ڿ������ݣ����ж��Ƿ���Origin����������HTTP�������󣺵�ǰԴHTTP������û�������Ĵ���������ڡ�Raw�����ļ����ݲ�������ԴHTTP��������ݷ�Χ����Raw�����ļ��У�����������������ʱ������Ҫ��Origin����������HTTP���������������������HTTP GET���󣬿��ܸ�ԴHTTP���󷽷���һ����ֻ�ǻ�ȡ�������ݵ�ĳһ�������ݣ���Rangeֵ�Ǵ�Դ������ʼλ�ÿ�ʼ��ȥ����ʵ��Raw�����ļ��洢������ó��Ŀ�ȱ��ƫ��λ�á���HTTP��������ֻ�����������ݴ洢�����ػ����ļ�������Ӧͷ��Ϣ�������µ�������Ϣ�ļ��С�

- ��c�������Ӧ��ı����ʽΪTransfer-Encoding: chunkedʱ��

ͨ��CacheInfo����Ƭ���ݹ���ӿڣ���ѯ����ǰRaw�����ļ��У���(a)�м�����Ļ����ļ�ƫ����λ�ã�������õ����ݳ����ж��١�

���Raw�����ļ��д��ڿ������ݣ����������ݳ��Ƚس����50��1M��С�����ݿ飬��Raw�����ļ�����1M���ݿ���ʼλ�á�������Ϊ������ӵ�res\_body\_chunk�С������ӵ�chunk�е������ܳ��ȴﵽ�򳬹�Դ����HTTPMsg��Ϣ����Ӧ�ܳ��ȣ���res\_body\_chunk���ý���״̬������TCP�������̡�

���Raw�����ļ��в����ڿ������ݣ�����������b���������ơ�

- ��d�����ԴHTTPMsg��ͳ�Ʒ��͸��ͻ��˵���Ӧ�����ܳ���С��res\_body\_chunk�е��ܳ��ȣ���ʼ����chunk�е����ݡ�

### 11.3.6 ������Ӧ���ͻ��˵������Ǳ�׼ͨ�õ�����

����HTTP Proxy�Ļ������ݴ洢�����͡�������Ϣ����ά���ȹ���ȫ��ʵ����ɡ�

# ʮ��. HTTP Tunnel

HTTP Tunnel���ڿͻ��˺�Origin������֮�䣬ͨ��Tunnel���أ��������������ͨ�ŷ�ʽ��eJet���������Գ䵱HTTP Tunnel���أ��ֱ���ͻ��˺�Origin������֮�佨������TCP���ӣ���������������֮��������ݵ�ʵʱת��������RFC 2616�淶��HTTP CONNECT���󷽷��ǽ���HTTP Tunnel�Ļ�����ʽ��

HTTP Tunnel��õĳ�����HTTP Proxy������������������ת���ͻ���https�İ�ȫ��������Origin��������һ������£���Ҫ���ö˵��˵�TLS/SSL���ӣ���ʱ���ͻ��˻᳢�Է���CONNECT������HTTP���󣬽���һ��ͨ��Proxy������������Origin�����������������������TCP���Ӵ�����ʵʱת�����ݣ�ͨ������������������TLS/SSL�İ�ȫ���֡���֤����Կ���������ݼ��ܵȣ��Ӷ�ʵ�ֶ˵��˵İ�ȫ���ݴ��䡣

# ʮ��. eJet��Callback�ص�����

## 13.1 eJet�ص�����

eJetϵͳ�ṩ��HTTP������Ϣ������Ӧ�ó�����Ļص����ƣ��ص��������¼�����ģ���еײ�ϵͳ�첽�����ϲ㴦�����ı��ģʽ���ϲ�Ӧ��ϵͳ�����Ƚ�����ʵ�����õ��ײ�ϵͳ�Ļص�����ָ���С�

eJetϵͳ�ṩ�����ֻص����ƣ�һ����������eJetʱ�����õ�ȫ�ֻص���������һ������ϵͳ�����ļ���λ�ڼ��������µĶ�̬�����ûص����ơ�

## 13.2 eJetȫ�ֻص�����

ȫ�ֻص�������������������eJetϵͳʱ��Ӧ�ò����ʵ��HTTP��Ϣ������������������HTTP�����HTTPMsg�����ǳ��򼶵Ļص����ƣ���Ҫ��eJet����Ƕ�뵽Ӧ��ϵͳ����ʵ�ֻص�����

����ȫ�ֻص���API���£�

```c
int http_set_reqhandler (void * httpmgmt, RequestHandler * reqhandler, void * cbobj);
```

���У�httpmgmt��eJetϵͳ������ȫ�ֹ������HTTPMgmt����ʵ���� reqhandler��Ӧ�ò�ʵ�ֵĻص�������cbobj��Ӧ�ò�ص������ĵ�һ���ص�������eJetÿ�ε��ûص�����ʱ������Я���ĵ�һ����������cbobj��

Ӧ�ò�ص�������ԭ�����£�

```c
typedef int RequestHandler (void * cbobj, void * vmsg);
```

���У�cbobj������ȫ�ֻص�����ʱ���ݻص�������vmsg�ǵ�ǰ��װHTTP�����HTTPMsgʵ������

Ӧ�ó���ϵͳ������������ݽṹ������Ӧ�ò����á����ݿ����ӡ��û�����ȣ���װ�ã���������ʼ��һ��cbobj������Ϊ���ûص�����ʱ�Ļص�������ͨ���ص��������Ѿ�HTTPMsg������󣬿��Խ�������Ϣ��Ӧ�ó����ڵ����ݶ��������ֹ�����ϵ��

## 13.3 eJet��̬��ص�

eJetϵͳ����һ�ֻص���ʹ�ö�̬��Ļص���ʽ������������͵ġ��޸������ļ��Ϳ�����ɻص�����ķ�ʽ��Ӧ�ó�������Ķ�eJet���κδ��룬ֻ������������Ӻ���·���Ķ�̬���ļ�����������ʵ�ֻص����ܣ����ж�̬�����ʵ�������̶����Ƶĺ���������ѭeJetԼ���ĺ���ԭ�Ͷ��塣

�����ļ�����Ӷ�̬��ص���λ�ã�

```
listen = {
    local ip = *;
    port = 8181;

    request process library = reqhandle.so app.conf
......
```

eJetϵͳ�����ڼ䣬���������ļ��󣬽���������Դ�ܹ��ĵ�һ��HTTPListenʱ�����������µĶ�̬��ᱻ���أ����ع���Ϊ��

-   ����������ָ����̬���ļ���
-   ���ݺ�����http\_handle\_init����ȡ��̬���еĳ�ʼ������ָ�룻
-   ���ݺ�����http_handle����ȡ��̬���еĻص�������ָ�룻
-   ���ݺ�����http\_handle\_clean����ȡ��̬���е��������ָ�룻
-   ִ�ж�̬���ʼ�������������س�ʼ����Ļص���������

��eJetϵͳ�˳�ʱ�������http\_handle\_clean���ͷų�ʼ�����̷������Դ��

��̬����ʵ�ֻص�ʱ�����뺬����������������http\_handle\_init��http\_handle��http\_handle_clean���亯��ԭ�Ͷ������£�

```c
typedef void * HTTPCBInit     (void * httpmgmt, int argc, char ** argv);
typedef void   HTTPCBClean    (void * hcb);
typedef int    RequestHandler (void * cbobj, void * vmsg);
```

���лص�����http\_handle�ĵ�һ������cbobj����http\_handle_init���صĽ������vmsg����eJetϵͳ��HTTPMsgʵ������

## 13.4 �ص�����ʹ��HTTPMsg�ĳ�Ա����

eJetϵͳͨ������HTTPMsgʵ��������ص�������������HTTP����HTTP�����װ��HTTP�����������Ϣ���ص������ڴ�������ʱ��������Ӹ�����Ӧ���ݵ�HTTPMsg�У�������Ӧ״̬����Ӧͷ����Ӧ��ȡ�

��������ͷ��Ϣ�������Ӧ���ݵĲ������ȿ���ֱ�Ӷ�HTTPMsg�ĳ�Ա�����������ݶ�ȡ��д�룬Ҳ����ͨ������HTTPMsg���õ�ָ�뺯�������д���HTTPMsg�з�װ�˺ܶຯ�����ã�ͨ����Щ������������ʵ��eJetϵͳHTTP������ĸ��ֲ�������Щ���Ӻ������£�

```c
......
char * (*GetRootPath)     (void * vmsg);
 
int    (*GetPath)         (void * vmsg, char * path, int len);
int    (*GetRealPath)     (void * vmsg, char * path, int len);
int    (*GetRealFile)     (void * vmsg, char * path, int len);
int    (*GetLocFile)      (void * vmsg, char * p, int len, char * f, int flen, char * d, int dlen);
 
int    (*GetQueryP)       (void * vmsg, char ** pquery, int * plen);
int    (*GetQuery)        (void * vmsg, char * query, int len);
int    (*GetQueryValueP)  (void * vmsg, char * key, char ** pval, int * vallen);
int    (*GetQueryValue)   (void * vmsg, char * key, char * val, int vallen);

int    (*GetReqContentP)    (void * vmsg, void ** pform, int * plen);
 
int    (*GetReqFormJsonValueP)  (void * vmsg, char * key, char ** ppval, int * vallen);
int    (*GetReqFormJsonValue)   (void * vmsg, char * key, char * pval, int vallen);

int    (*SetStatus)      (void * vmsg, int code, char * reason);
int    (*AddResHdr)      (void * vmsg, char * na, int nlen, char * val, int vlen);
int    (*DelResHdr)      (void * vmsg, char * name, int namelen);
 
int    (*SetResEtag) (void * vmsg, char * etag, int etaglen);

int    (*SetResContentType)   (void * vmsg, char * type, int typelen);
int    (*SetResContentLength) (void * vmsg, int64 len);

int    (*AddResContent)       (void * vmsg, void * body, int64 bodylen);
int    (*AddResContentPtr)    (void * vmsg, void * body, int64 bodylen);
int    (*AddResFile)          (void * vmsg, char * filename, int64 startpos, int64 len);

int    (*Reply)          (void * vmsg);
int    (*RedirectReply)  (void * vmsg, int status, char * redurl);
......
```

eJetͨ�����ûص����������ֽӿڻ��ƣ����ͻ��˵�HTTP����ת�����ض���Ӧ�ó����������������Web�����ĸ���ǰ�˼�������չӦ�ó������û�ǰ�˵Ľ���������
