# Magento Community Edition

In this repo you could find all information for create your Magento' shop in localhost

## How to install Magento 2.4.6 on windows 10

### Download and install XAMPP
https://www.apachefriends.org/download.html 

Stop XAMPP

Enable PHP extensions & Configure php.ini [PHP] 

Remove ; from the above lines of php.ini file. 
;extension=gd ;extension=intl ;extension=soap ;extension=xsl ;extension=sockets ;extension=sodium 

Set
max_execution_time=3000 
max_input_time=2000 
memory_limit=3G 

Save PHP file.

Restart XAMPP 
============================= 

### Download and install Compopser 
https://getcomposer.org/download/ 
============================= 
 
### Download and run Elasticsearch 
https://www.elastic.co/downloads/past-releases#elasticsearch https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.16.3-windows-x86_64.zip
============================= 


### Server configuration (Create virtual host) 
Stop XAMPP 

C:\xampp\apache\conf\extra\httpd-vhosts.conf 

<VirtualHost *:80>
    DocumentRoot "C:/xampp/htdocs/shop/pub"
    ServerName shop.magento.com
</VirtualHost>
<VirtualHost *:80>
    DocumentRoot "C:/xampp/htdocs"
    ServerName localhost
</VirtualHost>

=============================  

C:\Windows\System32\drivers\etc\hosts 
file in notepad and add the below line at the bottom of the file. 
127.0.0.1 shop.magento.com 

Restart XAMPP
============================= 

### Download Magento  
composer create-project --repository-url=https://repo.magento.com/ magento/project-community-edition shop 
============================= 

### Create Database in phpmyadmin

============================= 

Find validateURLScheme function in vendor\magento\framework\Image\Adapter\Gd2.php file
at line 92. Replace function with this: 

private function validateURLScheme(string $filename) : bool
{
          $allowed_schemes = ['ftp', 'ftps', 'http', 'https'];
          $url = parse_url($filename);
          if ($url && isset($url['scheme']) && !in_array($url['scheme'], $allowed_schemes) && !file_exists($filename)) {
              return false;
          }

          return true;   
}
============================= 

Go to: C:\xampp\htdocs\shop\vendor\magento\framework\View\Element\Template\File - > Edit Validator.php using a text editor and find this line: Find this line 

strpos($realPath, $directory) 
Replace strpos($path, $directory)

=============================  

Then, Open up app/etc/di.xml in the editor, fint this line

“Magento\Framework\App\View\Asset\MaterializationStrategy\Symlink” 
replace
“Magento\Framework\App\View\Asset\MaterializationStrategy\Copy”

============================= 

### Install Magento 

php bin/magento setup:install --base-url="http://shop.magento.com/" --db-host="localhost" --db-name="shop" --db-user="root" --db-password="Admin@" --admin-firstname="admin" --admin-lastname="admin" --admin-email="yourmail@mail.it" --admin-user="AdminName" --admin-password="Admin@123456" --language="en_US" --currency="USD" --timezone="America/Chicago" --use-rewrites="1" --backend-frontname="admin" --search-engine=elasticsearch7 --elasticsearch-host="localhost" --elasticsearch-port=9200 

=================== 

### Terminal congiguration:
 
php bin/magento module:disable Magento_AdminAdobeImsTwoFactorAuth
php bin/magento indexer:reindex
php bin/magento setup:upgrade 
php bin/magento setup:static-content:deploy -f 
php bin/magento cache:flush 
php bin/magento module:disable Magento_TwoFactorAuth 