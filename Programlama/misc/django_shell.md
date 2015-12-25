#### Mail test etme
```
python manage.py shell --settings=settings.<calistiralacak_settings>
from django.core.mail import send_mail
send_mail('baslik', 'icerik', 'from@example.com', ['to@example.com'], fail_silently=False)
```

* alternatif
```
from django.core.mail import EmailMessage
email = EmailMessage('<kelime1>', '<kelime2>', to=['<kullanici_adi>@<mail_sunucu_adresi>'])
email.send()
```
