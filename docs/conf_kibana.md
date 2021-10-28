#Operasional Kibana

## Otentikasi pada kibana
Setelah kita enable otentikasi pada elasticsearch, kita harus menambahkan username password untuk elasticsearch pada konfigurasi kibana pada `/etc/kibana/kibana.yml`.

    elasticsearch.username: "username"
    elasticsearch.password: "password"

dan kemudian kita enbale xpack untuk kibana dengan menambah konfigurasi

    xpack.security.enabled: true
    xpack.ingestManager.fleet.tlsCheckDisabled: true
    xpack.encryptedSavedObjects.encryptionKey: "1234567890qwertyuiopasdfghjklzxc"


restart kibana dan coba akses dari browser

    systemctl restart kibana