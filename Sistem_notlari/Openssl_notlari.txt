* yuklu sertifikalari gorme
openssl s_client -connect localhost:443 -showcerts |less

* sertifika suresinin ne zaman dolacak
openssl x509 -enddate -in /etc/apache2/ssl/*.crt -noout
