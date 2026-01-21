
# perus alku luodaan kansio ja syöteäään dataa/tekstiä tiedoston sisälle (txt) tai lokituksensa
```
┌──(kali㉿kali)-[~/Desktop]
└─$ mkdir harjoitus
                                                                                                            
┌──(kali㉿kali)-[~/Desktop]
└─$ cd harjoitus 
                                                                                                            
┌──(kali㉿kali)-[~/Desktop/harjoitus]
└─$ echo "perus harjoitus teksti" > tiedosto1.txt
                                                                                                            
┌──(kali㉿kali)-[~/Desktop/harjoitus]
└─$ echo "error: syntax error 1 lokitus meni pieleen" > loki1.log
```

# luodaan uusi kansio
```
┌──(kali㉿kali)-[~/Desktop/harjoitus]
└─$ mkdir data1    
                                                                                                            
                                                                                                            
┌──(kali㉿kali)-[~/Desktop/harjoitus]
└─$ echo "sisältöä dataa" > data1/data1.txt
```

## zippataan ja kuin pakkataan tiedosto kansio 1 yhteen pakettiin    
```
┌──(kali㉿kali)-[~/Desktop/harjoitus]
└─$ tar czf data1.tar.gz data1
                                                                                                            
┌──(kali㉿kali)-[~/Desktop/harjoitus]
```

# purkaa tiedoston
```
┌──(kali㉿kali)-[~/Desktop/harjoitus]
└─$ tar xzf data1.tar.gz      
                                                                                                            
┌──(kali㉿kali)-[~/Desktop/harjoitus]
└─$ ls
data1  data1.tar.gz  loki1.log  tiedosto1.txt
```


# etsii tiedostoja nimellä .txt
```
┌──(kali㉿kali)-[~]
└─$ find . -name "*.txt"
./Desktop/harjoitus/data1/data1.txt
./Desktop/harjoitus/tiedosto1.txt
```

# Etsi tiedostoja joissa sana "error"
```
┌──(kali㉿kali)-[~]
└─$ grep -Ri "error" .
grep: ./.cache/gstreamer-1.0/registry.x86_64.bin: binary file matches
./.xsession-errors:(polkit-mate-authentication-agent-1:1221): polkit-mate-1-WARNING **: 08:18:59.197: Failed to register client: GDBus.Error:org.freedesktop.DBus.Error.ServiceUnknown: The name org.gnome.SessionManager was not provided by any .service files
./.bashrc:# colored GCC warnings and errors
./.bashrc:#export GCC_COLORS='error=01;31:warning=01;35:note=01;36:caret=01;32:locus=01:quote=01'
grep: ./.ssh/agent/s.RDJpj98jBr.agent.HWpFt23gJj: No such device or address
./.zshrc:setopt nonomatch           # hide error message if there is no match for the pattern
./.zshrc:        ZSH_HIGHLIGHT_STYLES[bracket-error]=fg=red,bold
./Desktop/harjoitus/loki1.log:error: syntax error 1 lokitus meni pieleen
./.xsession-errors.old:(polkit-mate-authentication-agent-1:1409): polkit-mate-1-WARNING **: 08:14:50.231: Failed to register client: GDBus.Error:org.freedesktop.DBus.Error.ServiceUnknown: The name org.gnome.SessionManager was not provided by any .service files
./.xsession-errors.old:Error:            No Symbols named "classic" in the include file "us"
./.xsession-errors.old:(xfdesktop:1329): libxfce4ui-WARNING **: 08:16:45.321: ICE I/O Error
./.bashrc.original:# colored GCC warnings and errors
./.bashrc.original:#export GCC_COLORS='error=01;31:warning=01;35:note=01;36:caret=01;32:locus=01:quote=01'
```
