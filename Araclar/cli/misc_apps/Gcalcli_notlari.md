Islevi: cli ile google calendar yonetmeye yariyor.
girilmis tum kayitlari listeleme
*  gcalcli agenda

* su anla 2:30 arasinda takvimde bir kayit varsa listeler.
gcalcli --nocolor --nostarted agenda "`date`" 2:29pm | 
 grep -v 'No Events' | head -2 | tail -1 | grep -v '^$'

