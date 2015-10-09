python manage.py shell --settings=settings.<calistiralacak_settings>
from django.core.mail import EmailMessage
email = EmailMessage('<kelime1>', '<kelime2>', to=['<kullanici_adi>@<mail_sunucu_adresi>'])
email.send()
