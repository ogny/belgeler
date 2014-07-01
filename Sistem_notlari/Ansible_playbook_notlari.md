playbooks are Ansible’s language in YAML format. If Ansible
modules are the tools in your workshop, playbooks are your design plans.         
* YAML syntax;         
Tum dosyalar ---  ile baslar.         
All lines begin with - Each item in the list is a list       
of key/value pairs, commonly called a “hash” or a “dictionary”         
Ansible doesn’t really use these too much,        
but you can also specify a boolean value (true/false)         
        * iki nokta ustuste barindiran satirlari tirnak icine al, ornek;         
        f̶o̶o̶:̶ ̶s̶o̶m̶e̶b̶o̶d̶y̶ ̶s̶a̶i̶d̶ ̶I̶ ̶s̶h̶o̶u̶l̶d̶ ̶p̶u̶t̶ ̶a̶ ̶c̶o̶l̶o̶n̶ ̶h̶e̶r̶e̶:̶ ̶s̶o̶ ̶I̶ ̶d̶i̶d̶         
        foo: “somebody said I should put a colon here: so I did
        * iki nokta ustusteden sonra suslu parantez ile basliyorsa tirnak icine
        al, aksi durumda ansible bunu bir dictionary olarak algilar, ornek;
        foo: "{{ variable }}"        

* satir uzuyorsa sonda bos karakter birakip yeni satiri x karakter iceriden
baslatiyoruz.
        tasks:
        - name: Copy ansible inventory file to client
        copy: src=/etc/ansible/hosts dest=/etc/ansible/hosts 
            owner=root group=root mode=0644

