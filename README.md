# aws_server
第一次架設自己的伺服器，目的是為了拿來儲存一小孩的相關資料，
不過從來都沒有架設的經驗，不過在公司有一些管理伺服器的經驗，
所以打算使用AWS + LAMP + phpmyadmin來架設自己的伺服器。
下面是一些簡易的流程，確保以後忘記的時候可以依靠這個回憶起來。
以下是使用mac的操作流程，windows的可能需要轉換一些步驟才行。
1. 註冊或者登入AWS帳號。
2. 選擇創建EC2虛擬主機，並選擇最新版本的ubuntu版本的虛擬伺服器。
3. 建立虛擬伺服器時的各項設定都可以維持預設值，不需要更動。
4. 建立完成後會建立連線用的金鑰。
5. 檢查網路和安全裡的安全群組，找到伺服器正在使用的安全群組，確認傳入規則是否有開放http的80埠和https的443埠以及mysql的3306埠，如果沒有請編輯規則並開放。
6. 在使用金鑰登入伺服器之前，需要先修改金鑰的權限。
7. 用finder找到金鑰後點右鍵開啟列表後按下option鍵，拷貝選項就會變成複製路徑。
8. 打開終端機輸入chmod 0400 貼上複製的路徑後執行。
9. 在終端機輸入ssh -i 貼上複製的路徑 ubuntu@伺服器的公有IPv4地址後執行。
10. 登入成功後輸入sudo apt update && sudo apt upgrade && sudo apt dist-upgrade把版本升級到最新版。
11. 安裝apache2，輸入sudo apt install apache2
12. 確認apache2是否正常運行，輸入sudo systemctl status apache2
13. 安裝php，輸入sudo apt install php7.4 php7.4-mysql php-common php7.4-cli php7.4-json php7.4-common php7.4-opcache libapache2-mod-php7.4
14. 確認php安裝版本，輸入php --version
15. 重新啟動apache，輸入sudo systemctl restart apache2
16. 建立頁面，輸入echo '<?php phpinfo(); ?>' | sudo tee -a /var/www/html/phpinfo.php > /dev/null
17. 在瀏覽器中輸入http://你的伺服器公開IP位置/phpinfo.php確認網頁是否順利執行。
18. 確認完畢後需要刪除所建立的頁面的話，輸入sudo rm /var/www/html/phpinfo.php
19. 安裝MariaDB資料庫，輸入sudo apt install mariadb-server mariadb-client
20. 確認MariaDB安裝版本，輸入sudo systemctl status mariadb
21. 輸入sudo mysql_secure_installation來進行資料庫的基本安全設定
22. 第一個問題是詢問是否移除匿名登入的功能，建議移除
23. 第二個問題是詢問是否禁用遠程root登入，建議保留
24. 第三問題是詢問是否移除test資料庫，建議移除
25. 第四個問題是詢問是否將以上修改保存，選擇Ｙ
26. 設定遠端連入資料庫I，輸入cd /etc/mysql/mariadb.conf.d，輸入sudo vi 50-server.cnf，找到bind-address 127.0.0.1這一行，在前面加上#後儲存並離開。 
27. 設定遠端連入資料庫II，輸入sudo mysql -u root -p，再輸入密碼後輸入grant all privileges on *.* to 'root'@'%' identified by 'password' with grant option;
28. 設定遠端連入資料庫III，password要使用跟登入資料庫一樣的密碼才行，之後輸入flush privileges;再輸入shutdown -r now來重新開機。
29. 使用DBeaver或者Navicat等資料庫管理軟體來連接資料庫，如果成功就表示設定成功了。
30. 使用指令sudo chown ubuntu:ubuntu -R /var/www/html來修改/var/www/html的擁有權限，從root改成ubuntu這樣一來檔案就可以使用ftp軟體上傳到html資料夾了。
以上就是部署資料庫的流程。
