## kong

### install luasec failed

	安装失败
	
	```
	Installing https://luarocks.org/luasec-0.7-1.src.rock

	Error: Could not find header file for OPENSSL
	  No file openssl/ssl.h in /usr/local/include
	  No file openssl/ssl.h in /usr/include
	  No file openssl/ssl.h in /include
	You may have to install OPENSSL in your system and/or pass OPENSSL_DIR or OPENSSL_INCDIR to the luarocks command.
	Example: luarocks install luasec OPENSSL_DIR=/usr/local
	```
	
	
	- 解决：查看本机openssl安装地址，修改安装命令如下
	
	luarocks install luasec OPENSSL_LIBDIR=/usr/local/opt/openssl/lib OPENSSL_DIR=/usr/local/opt/openssl
	
	编译
	luarocks make --lua-dir=/usr/local/opt/lua@5.1 OPENSSL_LIBDIR=/usr/local/opt/openssl/lib OPENSSL_DIR=/usr/local/opt/openssl CRYPTO_DIR=/usr/local/opt/openssl
	
### 本地启动 kong

	启动命令
	
	> /usr/local/openresty/nginx/sbin/nginx -p /usr/local/kong -c nginx.conf
	
### luarocks 切换lua版本

	> luarocks --lua-dir=/usr/local/opt/lua@5.1
	
### kong docker启动添加自定义插件参数

```
docker run -d --name kong \
     --network=kong-net \
     -e "KONG_DATABASE=postgres" \
     -e "KONG_PG_HOST=kong-database" \
     -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
     -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
     -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
     -e "LUA_PATH=/kong-plugin/?.lua;/kong-plugin/?/init.lua;;" \
     -e "KONG_CUSTOM_PLUGINS=auth-hll, hello-world, redis-session" \
     -p 8000:8000 \
     -p 8443:8443 \
     -p 8001:8001 \
     -p 8444:8444 \
     -v /Users/aiyuan/kong-plugin:/kong-plugin \
     kong:0.14.1
```

### kong 部署

## jenkins

```
<useSecurity>true</useSecurity>
  <authorizationStrategy class="hudson.security.FullControlOnceLoggedInAuthorizationStrategy">
    <denyAnonymousReadAccess>true</denyAnonymousReadAccess>
  </authorizationStrategy>
  <securityRealm class="hudson.security.HudsonPrivateSecurityRealm">
    <disableSignup>true</disableSignup>
    <enableCaptcha>false</enableCaptcha>
  </securityRealm>
```