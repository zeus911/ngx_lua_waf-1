# ngx_lua_waf
https://github.com/loveshell/ngx_lua_waf 
鉴于原作者不提供后续维护，本人不才接着用，欢迎各位大佬来完善

## 更新内容

- 优化了访问频繁,被列黑名单ip的访问提示
- 采用issues/132  修改获取客户端真实ip的代码，支持代理
- 采用/issues/73  解决在GBK和UTF-8多编码环境下的乱码
- 采用/pull/138   添加2行div使提示居中显示
- 采用/pull/83    修复yii框架等pathmode模式拿不到get参数的问题
- <del>采用/pull/13    同一IP不能频繁请求uriChecklist={"/users/sign_in", "/users/sign_up"}列表中的地址</del>无效果已去除





-----------------------------------------------------------------
-----------------------------------------------------------------
---------------后面都是原样搬运可忽略--------------------------------
-----------------------------------------------------------------
-----------------------------------------------------------------

### 安装说明：

nginx安装路径假设为:/usr/local/nginx/conf/

把ngx_lua_waf下载到conf目录下,解压命名为waf

在nginx.conf的http段添加

        lua_package_path "/usr/local/nginx/conf/waf/?.lua";
        lua_shared_dict limit 10m;
        init_by_lua_file  /usr/local/nginx/conf/waf/init.lua; 
        access_by_lua_file /usr/local/nginx/conf/waf/waf.lua;

配置config.lua里的waf规则目录(一般在waf/conf/目录下)

        RulePath = "/usr/local/nginx/conf/waf/wafconf/"

绝对路径如有变动，需对应修改

然后重启nginx即可


### 配置文件详细说明：

    	RulePath = "/usr/local/nginx/conf/waf/wafconf/"
        --规则存放目录
        attacklog = "off"
        --是否开启攻击信息记录，需要配置logdir
        logdir = "/usr/local/nginx/logs/hack/"
        --log存储目录，该目录需要用户自己新建，切需要nginx用户的可写权限
        UrlDeny="on"
        --是否拦截url访问
        Redirect="on"
        --是否拦截后重定向
        CookieMatch = "on"
        --是否拦截cookie攻击
        postMatch = "on" 
        --是否拦截post攻击
        whiteModule = "on" 
        --是否开启URL白名单
        black_fileExt={"php","jsp"}
        --填写不允许上传文件后缀类型
        ipWhitelist={"127.0.0.1"}
        --ip白名单，多个ip用逗号分隔
        ipBlocklist={"1.0.0.1"}
        --ip黑名单，多个ip用逗号分隔
        CCDeny="on"
        --是否开启拦截cc攻击
        CCrate = "100/60"
        --设置cc攻击频率，单位为秒.
        --默认1分钟同一个IP只能请求同一个地址100次
        html=[[Please go away~~]]
        --警告内容,可在中括号内自定义
        备注:不要乱动双引号，区分大小写
        
### 检查规则是否生效

部署完毕可以尝试如下：        

        http://xxxx/phpmyadmin

注意:默认，本机在白名单不过滤，可自行调整config.lua配置


### 一些说明：

	过滤规则在wafconf下，可根据需求自行调整，每条规则需换行,或者用|分割
	
		args里面的规则get参数进行过滤的
		url是只在get请求url过滤的规则		
		post是只在post请求过滤的规则		
		whitelist是白名单，里面的url匹配到不做过滤		
		user-agent是对user-agent的过滤规则
	

	默认开启了get和post过滤，需要开启cookie过滤的，编辑waf.lua取消部分--注释即可
	
	日志文件名称格式如下:虚拟主机名_sec.log


## Copyright

	
感谢作者loveshell

