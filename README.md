# sekolam
Dengan menggunkan konfigurasi `config.cfg` ini:
<pre>
admin:
    email: swdev.bali@gmail.com
    
pools:
    domain: pythonthusiast.com
        type: digitalocean
        ssh-alias: do-wp
        ssh-user: root
        pwd: ~/code/pythonthusiast
        deploy-script: deploy.sh
        keep-alive: true
       
    domain: whizkids.id
        type: digitalocean
        ssh-alias: do-whizkidsid
        ssh-user: root
        pwd: ~/code/whizkidsid
        deploy-script: deploy.sh
</pre>

Kode `main.ceb` berikut akan membuat manajemen situs-situs 
tersebut menjadi otomatis:
<pre>
import web
import admin

using config.cfg
watch for pool in pools:
    if web.down(pool):
       web.deploy(pool)
       if not web.wait_until_alive(pool, 1m):
            admin.send_alert('Web still down after redeploy', 
            pool)
</pre> 