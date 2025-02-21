## OPENVPN 设置

1. get script:

login vps:  run

curl -O https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh
chmod +x openvpn-install.sh


or  login vps, scp openvpn-install.sh to vps

2. run script using default option to the end

3. scp ovpn file to client device

## 多个设备需要多个ovpn文件，配置方法：


为每台设备创建独立的 OpenVPN 配置文件
我们需要为 client2 生成一个独立的证书和密钥，然后创建一个新的 .ovpn 配置文件。以下是完整操作步骤。

🔹 1. 进入 OpenVPN 的 easy-rsa 目录
sh
Copy
Edit
cd /etc/openvpn/easy-rsa
如果 easy-rsa 目录不在 /etc/openvpn/ 下，可能在 /etc/openvpn/server/easy-rsa/，请根据你的系统调整路径。

🔹 2. 生成 client2 的证书和密钥
执行以下命令：


./easyrsa build-client-full client2 nopass

client2：这是新的客户端名称，你可以换成其他名称（比如 iphone 或 laptop）。

nopass：表示不为私钥设置密码（如果需要密码保护，可以去掉 nopass）。

🔸 生成的文件（存储在 pki 目录下）：

pki/private/client2.key（客户端私钥）

pki/issued/client2.crt（客户端证书）

pki/ca.crt（CA 证书，所有客户端共享）

pki/ta.key（TLS 认证密钥，所有客户端共享）

🔹 3. 复制 client.ovpn 并创建 client2.ovpn

cp /etc/openvpn/client.ovpn /etc/openvpn/client2.ovpn
然后，编辑 client2.ovpn：


nano /etc/openvpn/client2.ovpn
或使用 vi 编辑：

vi /etc/openvpn/client2.ovpn

🔹 4. 替换 client2 的证书和密钥

找到 .ovpn 配置中的以下部分：


< cert >
  
-----BEGIN CERTIFICATE-----
  
 这里是原 client1 的证书

-----END CERTIFICATE-----

< /cert >


<key>
-----BEGIN PRIVATE KEY-----
  
这里是原 client1 的私钥

-----END PRIVATE KEY-----

</key>



然后：

替换 <cert> 中的内容（来自 pki/issued/client2.crt）

替换 <key> 中的内容（来自 pki/private/client2.key）

保持 <ca> 和 <tls-auth> 不变，因为这些是所有客户端通用的。
