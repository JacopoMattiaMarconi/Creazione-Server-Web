# Controllare che tutte le modifiche apportate siano correttamente eseguite :rocket:
## apache2

1. controllare l'esatta modifica delle impostazioni di rete
>ip address
>
>ping su altra macchina per verificare connettività
>

--------------------------------------------------------------

2. controllare connettività dopo aver installato APACHE2
>digitare ip su barra di ricerca nel browser
>

--------------------------------------------------------------

3. controllo finale su raggiungimento sitoX
>digitare sul browser sitoa-113.virtual.marconi
>
>apache2 is not active -> sudo systemctl enable apache2
>
>history | grep enable
>
>sudo systemctl restart apache2.service
>
>sudo systemctl reload apache2
>

--------------------------------------------------------------

4. controllare inserimento corretto per accesso unico alla cartella SitoA
>exit
>
>         accedere con user e passwd insirite
>
