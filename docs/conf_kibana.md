#Operasional Kibana

## Otentikasi pada kibana
Setelah kita enable otentikasi pada elasticsearch, kita harus menambahkan username password untuk elasticsearch pada konfigurasi kibana pada `/etc/kibana/kibana.yml`.

    elasticsearch.username: "username"
    elasticsearch.password: "password"

    # Enables you to specify a file where Kibana stores log output.
    logging.dest: /var/log/kibana/kibana.log

    # Set the value of this setting to true to suppress all logging output.
    logging.silent: false

    # Set the value of this setting to true to suppress all logging output other than error messages.
    logging.quiet: false

    # Set the value of this setting to true to log all events, including system usage information
    # and all requests.
    logging.verbose: false


dan kemudian kita enbale xpack untuk kibana dengan menambah konfigurasi

    xpack.security.enabled: true
    xpack.ingestManager.fleet.tlsCheckDisabled: true
    xpack.encryptedSavedObjects.encryptionKey: "1234567890qwertyuiopasdfghjklzxc"


restart kibana dan coba akses dari browser

    systemctl restart kibana