# Install FreeTDS
## Download file
```bash
#download freetds
wget http://www.freetds.org/files/stable/freetds-1.00.82.tar.gz
tar xvf freetds-1.00.82.tar.gz

#download iconv for UTF-8 encode
wget https://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.15.tar.gz
tar libiconv-1.15.tar.gz
```
## Cross Compile

1. Cross  compile __iconv__
2. Cross  compile __freetds__
3. Copy file to ARM
```bash 
#cross compile iconv
cd libiconv-1.15 
./configure --prefix=/usr/local/iconv-arm --host=arm-linux-gnueabihf --enable-static
make 
sudo make install 
#confrim successful cross compile
file /usr/local/iconv-arm/bin/iconv

#cross compile freetds
cd ../freetds-1.00.82
./configure --prefix=/usr/local/freetds-arm --host=arm-linux-gnueabihf --enable-msdblib --with-libiconv-prefix=/usr/local/iconv-arm
make 
sudo make install
#confrim successful cross compile
file /usr/local/freetds-arm/bin/tsql

#Copy file to ARM
scp -r /usr/local/freetds-arm /usr/local/iconv-arm user_name@arm_ip:/usr/local
```
> __Note:__ if `/usr/local` not exist on ARM, use `mkdir /usr/local`  create it before.
## Install
```bash
#remote login ARM
ssh user_name@arm_ip
#copy Shared Library to /usr/lib
cp -r /usr/local/freetds-arm/lib/* /usr/local/iconv-arm/lib/* /usr/lib

#enable UTF-8 encode and modify time format on freetds
vi /usar/local/freetds-arm/etc/freetds.conf
```
add `client charset = UTF-8`
```bash
vi /usar/local/freetds-arm/etc/locales.conf
```
In `[default]` section  
replace `date format = %b %e %Y %I:%M:%S:%z%p` to `date format = %Y-%m-%d %H:%M:%S`
