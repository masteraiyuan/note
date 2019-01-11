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