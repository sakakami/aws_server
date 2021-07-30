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
11. 安裝taskel，輸入sudo apt install taskel
12. 安裝Lamp server，輸入sudo apt install lamp-server
13. 安裝phpmyadmin，輸入sudo apt install phpmyadmin
14. 設定mysql帳號，輸入sudo mysql -u root mysql
15. 進入mysql後輸入UPDATE user SET plugin='mysql_native_password' WHERE User='root';
16. 輸入FLUSH PRIVILEGES;
17. 輸入exit;離開mysql
18. 設定mysql密碼，輸入sudo mysql_secure_installation
19. 密碼強度選擇，如果沒有特殊需求的話輸入0即可，後續的全部輸入yes，等出現All Done就表示完成了。
20. 打開瀏覽器並在網址列輸入伺服器的公有IPv4/phpmyadmin
21. 如果出現錯誤訊息如下：-NOT FOUND The requested URL /phpmyadmin was not found on this server. Apache/2.4.7 (Ubuntu) Server at localhost Port 80
22. 輸入sudo vi /etc/apache2/apache2.conf
23. 按下 i 進入編輯模式，來到最後一行並添加Include /etc/phpmyadmin/apache.conf，添加完成後按下ESC離開編輯模式，按下:並輸入wq來離開並儲存。
24. 輸入sudo /etc/init.d/apache2 restart來重啟apache
25. 再次執行步驟20應該就可以正常顯示phpmyadmin的畫面了
26. 登入phpmyadmin後選擇使用者帳號，編輯root，進入編輯後選擇登入資訊。
27. 在主機名稱那欄選擇任意主機，在密碼那欄選擇使用文字方塊，在後面輸入外部連結資料庫要用的密碼，密碼要包含大小寫，完成後點選執行，如果出現錯誤的話密碼再加上特殊服後應該就可以通過了。
28. 回到終端機輸入，sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
29. 按下i進入編輯模式後找到bind-address = 127.0.0.1，把127.0.0.1改成伺服器的私有IPv4後，按下ESC離開編輯模式並按下:和輸入wq儲存並離開。
30. 輸入sudo service mysql restart重啟資料庫。
31. 使用DBeaver或者Navicat等資料庫管理軟體來連接資料庫，如果成功就表示設定成功了。
32. 使用指令sudo chown ubuntu:ubuntu -R /var/www/html來修改/var/www/html的擁有權限，從root改成ubuntu這樣一來檔案就可以使用ftp軟體上傳到html資料夾了。
以上就是部署資料庫的流程。
