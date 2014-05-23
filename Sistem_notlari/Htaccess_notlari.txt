* http isteklerini https'e yonlendirmek;
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} 
default olarak emrah'in powerdns yazisindakine ek olarak;
Options FollowSymLinks

htaccess ile basic auth. yaptiginda su hatayla karsilasirsan;
Options FollowSymLinks or SymLinksIfOwnerMatch is off which implies that RewriteRule directive is forbidden
htaccess'in options'una her iki ozelligi de ekle (FollowSymLinks or SymLinksIfOwnerMatch, sunucuyu restart et.)
